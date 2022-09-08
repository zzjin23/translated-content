---
title: 音声と動画の配信
slug: Web/Guide/Audio_and_video_delivery
---
「静的」メディアファイルからアダプティブライブストリームまで、さまざまな方法で Web 上にオーディオとビデオを配信できます。この記事は、Web ベースのメディアのさまざまな配信メカニズムおよび一般的なブラウザとの互換性を探るための出発点として意図されています。

## The Audio and Video Elements

Whether we are dealing with pre-recorded audio files or live streams, the mechanism for making them available through the browser's {{ htmlelement("audio")}} and {{ htmlelement("video")}} elements remains pretty much the same. Currently, to support all browsers we need to specify two formats, although with the adoption of MP3 and MP4 formats in Firefox and Opera, this is changing fast. You can find compatibility information in the following places:

- [Audio Codec Compatibility Table](/ja/Apps/Build/Manipulating_media/Cross-browser_audio_basics#Audio_Codec_Support)
- [Audio/Video Codec Compatibility Table](/ja/docs/Web/HTML/Supported_media_formats#Browser_compatibility)

To deliver video and audio, the general workflow is usually something like this:

1. Check what format the browser supports via feature detection (usually a choice of two, as stated above.)
2. If the browser doesn't support playback of any of the provided formats natively, provide a fallback (such as a Flash movie.)
3. Identify how you want to play/instantiate the media (e.g. a {{ htmlelement("video") }} element, or `document.createElement('video')` perhaps?)
4. Deliver the media file to the player.

### HTML Audio

```html
<audio controls preload="auto">
  <source src="audiofile.mp3" type="audio/mpeg">

  <!-- fallback for browsers that don't suppport mp3 -->
  <source src="audiofile.ogg" type="audio/ogg">

  <!-- fallback for browsers that don't support audio tag -->
  <a href="audiofile.mp3">download audio</a>
</audio>
```

The code above will create an audio player that attempts to preload as much audio as possible for smooth playback.

> **Note:** The preload attribute may be ignored by some mobile browsers.

For further info see [Cross Browser Audio Basics (HTML5 Audio In Detail)](/ja/Apps/Build/Manipulating_media/Cross-browser_audio_basics#HTML5_audio_in_detail)

### HTML Video

```html
<video controls width="640" height="480" poster="initialimage.png" autoplay muted>
  <source src="videofile.mp4" type="video/mp4">

  <!-- fallback for browsers that don't suppport mp4 -->
  <source src="videofile.webm" type="video/webm">

  <!-- specifying subtitle files -->
  <track src="subtitles_en.vtt" kind="subtitles" srclang="en" label="English">
  <track src="subtitles_no.vtt" kind="subtitles" srclang="no" label="Norwegian">

  <!-- fallback for browsers that don't support video tag -->
  <a href="videofile.mp4">download video</a>
</video>
```

The code above creates a video player of dimensions 640x480 pixels, displaying a poster image until the video is played. We instruct the video to autoplay but to be muted by default.

> **Note:** The autoplay attribute may be ignored by some mobile browsers.

For further info see [\<video> element](/ja/docs/Web/HTML/Element/video) and [Creating a cross-browser video player](/ja/Apps/Build/Manipulating_media/cross_browser_video_player).

### Audio and Video Fallback

You can create a more comprehensive Fallback using Flash. [Using flashmediaelement.swf](https://github.com/johndyer/mediaelement/blob/master/build/flashmediaelement.swf) is one way.

```html
<audio controls>
  <source src="audiofile.mp3" type="audio/mpeg">
  <source src="audiofile.ogg" type="audio/ogg">
  <!-- fallback for non supporting browsers goes here -->
  <a href="audiofile.mp3">download audio</a>
  <object width="320" height="30" type="application/x-shockwave-flash" data="flashmediaelement.swf">
    <param name="movie" value="flashmediaelement.swf" />
    <param name="flashvars" value="controls=true&isvideo=false&file=audiofile.mp3" />
  </object>
</audio>
```

The process is very similar with video — just remember to set `isvideo=true` in the `flashvars value` parameters.

[More options for Fallbacks](/ja/Apps/Build/Manipulating_media/Cross-browser_audio_basics#Fallbacks).

### JavaScript Audio

```js
var myAudio = document.createElement('audio');

if (myAudio.canPlayType('audio/mpeg')) {
  myAudio.setAttribute('src','audiofile.mp3');
} else if (myAudio.canPlayType('audio/ogg')) {
  myAudio.setAttribute('src','audiofile.ogg');
}

myAudio.currentTime = 5;
myAudio.play();
```

We set the source of the audio depending on the type of audio file the browser supports, then set the play-head 5 seconds in and attempt to play it.

> **Note:** Play will be ignored by some mobile browsers unless issued by a user-initiated event.

It's also possible to feed an {{ htmlelement("audio") }} element a base64 encoded WAV file, allowing to generate audio on the fly:

```html
<audio id="player" src="data:audio/x-wav;base64,UklGRvC..."></audio>
```

[Speak.js](https://github.com/kripken/speak.js/) employs this technique. [Try the demo](http://speak-demo.herokuapp.com).

### JavaScript Video

```js
var myVideo = document.createElement('video');

if (myVideo.canPlayType('video/mp4')) {
  myVideo.setAttribute('src','videofile.mp4');
} else if (myVideo.canPlayType('video/webm')) {
  myVideo.setAttribute('src','videofile.webm');
}

myVideo.width = 480;
myVideo.height = 320;
```

We set the source of the video depending on the type of video file the browser supports we then set the width and height of the video.

## Web Audio API

```js
  var context;
  var request;
  var source;

  try {
    context = new AudioContext();
    request = new XMLHttpRequest();
    request.open("GET","http://jplayer.org/audio/mp3/RioMez-01-Sleep_together.mp3",true);
    request.responseType = "arraybuffer";

    request.onload = function() {
      context.decodeAudioData(request.response, function(buffer) {
        source = context.createBufferSource();
        source.buffer = buffer;
        source.connect(context.destination);
        // auto play
        source.start(0); // start was previously noteOn
      });
    };

    request.send();

  } catch(e) {
    alert('web audio api not supported');
  }
```

In this example we retrieve an MP3 file via XHR, load it into a source and play it ([Try it for yourself](https://jsbin.com/facutone/1/edit?js)). Find more about Web Audio API basics in [Using the Web Audio API](/ja/docs/Web/API/Web_Audio_API/Using_Web_Audio_API).

## getUserMedia / Stream API

It's also possible to retrieve a live stream from a webcam and/or microphone using `getUserMedia` and the Stream API. This makes up part of a wider technology known as WebRTC (Web Real-Time Communications) and is compatible with the latest versions of Chrome, Firefox and Opera.

To grab the stream from your webcam, first set up a {{htmlelement("video")}} element:

```html
<video id="webcam" width="480" height="360"></video>
```

Next, if supported connect the webcam source to the video element:

```js
if (navigator.mediaDevices) {
  navigator.mediaDevices.getUserMedia({ video: true, audio: false })
  .then(function onSuccess(stream) {
    var video = document.getElementById('webcam');
    video.autoplay = true;
    video.srcObject = stream;
  })
  .catch(function onError() {
    alert('There has been a problem retreiving the streams - are you running on file:/// or did you disallow access?');
  });
} else {
  alert('getUserMedia is not supported in this browser.');
}
```

To find out more, read our {{domxref("MediaDevices.getUserMedia")}} page.

## Mediastream Recording

New standards are being rolled out to allow your browser to grab media from your mic or camera using `getUserMedia` and record it instantly using the new MediaRecorder API. You take the stream you receive from `getUserMedia`, pass it to a `MediaRecorder` object, take the resulting output and feed it to your audio or video source\*.

The main mechanism is outlined below:

```js
navigator.mediaDevices.getUserMedia({audio:true})
  .then(function onSuccess(stream) {
    var recorder = new MediaRecorder(stream);

    var data = [];
    recorder.ondataavailable = function(e) {
      data.push(e.data);
    };
    recorder.start();
    recorder.onerror = function(e) {
      throw e.error || new Error(e.name); // e.name is FF non-spec
    }
    recorder.onstop = function(e) {
      var audio = document.createElement('audio');
      audio.src = window.URL.createObjectURL(new Blob(data));
    }
    setTimeout(function() {
      rec.stop();
    }, 5000);
  })
  .catch(function onError(error) {
    console.log(error.message);
  });
```

See [MediaRecorder API](/ja/docs/Web/API/MediaRecorder_API) for more details.

## Media Source Extensions (MSE)

[Media Source Extensions](https://dvcs.w3.org/hg/html-media/raw-file/tip/media-source/media-source.html) is a W3C working draft that plans to extend {{domxref("HTMLMediaElement")}} to allow JavaScript to generate media streams for playback. Allowing JavaScript to generate streams facilitates a variety of use cases like adaptive streaming and time shifting live streams.

### Encrypted Media Extensions (EME)

[Encrypted Media Extensions](https://dvcs.w3.org/hg/html-media/raw-file/tip/encrypted-media/encrypted-media.html) is a W3C proposal to extend `HTMLMediaElement`, providing APIs to control playback of protected content.

The API supports use cases ranging from simple clear key decryption to high value video (given an appropriate user agent implementation). License/key exchange is controlled by the application, facilitating the development of robust playback applications supporting a range of content decryption and protection technologies.

One of the principle uses of EME is to allow browsers to implement DRM ([Digital Rights Management](http://en.wikipedia.org/wiki/Digital_rights_management)), which helps to prevent web-based content (especially video) from being copied.

### Adaptive Streaming

New formats and protocols are being rolled out to facilitate adaptive streaming. Adaptive streaming media means that the bandwidth and typically quality of the stream can change in real-time in reaction to the user's available bandwidth. Adaptive streaming is often used in conjunction with live streaming where smooth delivery of audio or video is paramount.

The main formats used for adaptive streaming are [HLS](/ja/Apps/Build/Manipulating_media/Live_streaming_web_audio_and_video#HLS) and [MPEG-DASH](/ja/Apps/Build/Manipulating_media/Live_streaming_web_audio_and_video#MPEG-DASH). MSE has been designed with DASH in mind. MSE defines byte streams according to [ISOBMFF](https://dvcs.w3.org/hg/html-media/raw-file/tip/media-source/isobmff-byte-stream-format.html) and [M2TS](http://en.wikipedia.org/wiki/M2ts) (both supported in DASH, the latter supported in HLS). Generally speaking, if you are interested in standards, are looking for flexibility, or wish to support most modern browsers, you are probably better off with DASH.

> **Note:** Currently Safari does not support DASH although dash.js will work on newer versions of Safari scheduled for release with OSX Yosemite.

DASH also provides a number of profiles including simple onDemand profiles that no preprocessing and splitting up of media files. There are also a number of cloud based services that will convert your media to both HLS and DASH.

For further information see [Live streaming web audio and video](/ja/Apps/Build/Manipulating_media/Live_streaming_web_audio_and_video).

## Customising Your Media Player

You may decide that you want your audio or video player to have a consistent look across browsers, or just wish to tweak it to match your site. The general technique for achieving this is to omit the `controls` attribute so that the default browser controls are not displayed, create custom controls using HTML and CSS, then use JavaScript to link your controls to the audio/video API.

If you need something extra, it's possible to add features that are not currently present in default players, such as playback rate, quality stream switches or even audio spectrums. You can also choose how to make your player responsive — for example you might remove the progress bar under certain conditions.

You may detect click, touch and/or keyboard events to trigger actions such as play, pause and scrubbing. It's often important to remember keyboard controls for user convenience and accessibility.

A quick example — first set up your audio and custom controls in HTML:

```html
  <audio id="my-audio" src="http://jPlayer.org/audio/mp3/Miaow-01-Tempered-song.mp3"></audio>
  <button id="my-control">play</button>
```

add a bit of JavaScript to detect events to play and pause the audio:

```js
window.onload = function() {

  var myAudio = document.getElementById('my-audio');
  var myControl = document.getElementById('my-control');

  function switchState() {
    if (myAudio.paused == true) {
      myAudio.play();
      myControl.innerHTML = "pause";
    } else {
      myAudio.pause();
      myControl.innerHTML = "play";
    }
  }

  function checkKey(e) {
    if (e.keycode == 32 ) { //spacebar
      switchState();
    }
  }

  myControl.addEventListener('click', function() {
    switchState();
  }, false);

  window.addEventListener( "keypress", checkKey, false );
}
```

You can [try this example out here](https://jsbin.com/jujeladu/2/edit). For more information, see [Creating your own custom audio player](/ja/Apps/Build/Manipulating_media/Cross-browser_audio_basics#Creating_your_own_custom_audio_player).

## Other tips for audio/video

### Stopping the download of media

While stopping the playback of media is as easy as calling the element's `pause()` method, the browser keeps downloading the media until the media element is disposed of through garbage collection.

Here's a trick that stops the download at once:

```js
var mediaElement = document.querySelector("#myMediaElementID");
mediaElement.removeAttribute("src");
mediaElement.load();
```

By removing the media element's `src` attribute and invoking the load() method, you release the resources associated with the video, which stops the network download. You must call `load()` after removing the attribute, because just removing the `src` attribute does not invoke the load algorithm. If the `<video>` element also has `<source>` element descendants, those should also be removed before calling `load()`.

Note that just setting the `src` attribute to an empty string will actually cause the browser to treat it as though you're setting a video source to a relative path. This causes the browser to attempt another download to something that is unlikely to be a valid video.

### Seeking through media

Media elements provide support for moving the current playback position to specific points in the media's content. This is done by setting the value of the `currentTime` property on the element; see {{ domxref("HTMLMediaElement") }} for further details on the element's properties. Simply set the value to the time, in seconds, at which you want playback to continue.

You can use the element's `seekable` property to determine the ranges of the media that are currently available for seeking to. This returns a {{ domxref("TimeRanges") }} object listing the ranges of times that you can seek to.

```js
var mediaElement = document.querySelector('#mediaElementID');
mediaElement.seekable.start(0);  // Returns the starting time (in seconds)
mediaElement.seekable.end(0);    // Returns the ending time (in seconds)
mediaElement.currentTime = 122; // Seek to 122 seconds
mediaElement.played.end(0);      // Returns the number of seconds the browser has played
```

### Specifying playback range

When specifying the URI of media for an {{ HTMLElement("audio") }} or {{ HTMLElement("video") }} element, you can optionally include additional information to specify the portion of the media to play. To do this, append a hash mark ("#") followed by the media fragment description.

A time range is specified using the syntax:

```
#t=[starttime][,endtime]
```

The time can be specified as a number of seconds (as a floating-point value) or as an hours/minutes/seconds time separated with colons (such as 2:05:01 for 2 hours, 5 minutes, and 1 second).

A few examples:

- http\://example.com/video.ogv#t=10,20
  - : Specifies that the video should play the range 10 seconds through 20 seconds.
- http\://example.com/video.ogv#t=,10.5
  - : Specifies that the video should play from the beginning through 10.5 seconds.
- http\://example.com/video.ogv#t=,02:00:00
  - : Specifies that the video should play from the beginning through two hours.
- http\://example.com/video.ogv#t=60
  - : Specifies that the video should start playing at 60 seconds and play through the end of the video.

> **Note:** The playback range portion of the media element URI specification was added to Gecko 9.0 {{ geckoRelease("9.0") }}. At this time, this is the only part of the [Media Fragments URI specification](http://www.w3.org/TR/media-frags/) implemented by Gecko, and it can only be used when specifying the source for media elements, and not in the address bar.

## Error handling

Starting in Gecko 2.0 {{ geckoRelease("2.0") }}, error handling has been revised to match the latest version of the HTML5 specification. Instead of the `error` event being dispatched to the media element itself, it now gets delivered to the child {{ HTMLElement("source") }} elements corresponding to the sources resulting in the error.

This lets you detect which sources failed to load, which may be useful. Consider this HTML:

```html
<video>
<source id="mp4_src"
  src="video.mp4"
  type='video/mp4; codecs="avc1.42E01E, mp4a.40.2"'>
</source>
<source id="3gp_src"
  src="video.3gp"
  type='video/3gpp; codecs="mp4v.20.8, samr"'>
</source>
<source id="ogg_src"
  src="video.ogv"
  type='video/ogv; codecs="theora, vorbis"'>
</source>
</video>
```

Since Firefox doesn't support MP4 and 3GP on some platforms due to their patent-encumbered nature, the {{ HTMLElement("source") }} elements with the IDs "mp4_src" and "3gp_src" will receive `error` events before the Ogg resource is loaded. The sources are tried in the order in which they appear, and once one loads successfully, the remaining sources aren't tried at all.

### Checking whether the browser supports the supplied formats

Use the following verified sources within your audio and video elements to check support.

- Audio MP3 (`type="audio/mpeg"`): <http://jPlayer.org/audio/mp3/Miaow-01-Tempered-song.mp3> ([play the MP3 audio live](https://jsbin.com/gekatoge/1/edit).)
- Audio MP4 (`type="audio/mp4"`): <http://jPlayer.org/audio/m4a/Miaow-01-Tempered-song.m4a> ([play the MP4 audio live](https://jsbin.com/gekatoge/2/edit).)
- Audio Ogg (`type="audio/ogg"`): <http://jPlayer.org/audio/ogg/Miaow-01-Tempered-song.ogg> ([play the OGG audio live](https://jsbin.com/gekatoge/4/edit).)
- Video MP4 (`type="video/mp4"`): <http://jPlayer.org/video/m4v/Big_Buck_Bunny_Trailer.m4v> ([play the MP4 video live](https://jsbin.com/gekatoge/5/edit).)
- Video WebM (`type="video/webm"`): <http://jPlayer.org/video/webm/Big_Buck_Bunny_Trailer.webm> ([play the WebM video live](https://jsbin.com/gekatoge/6/edit).)
- Video Ogg (`type="video/ogg"`): <http://jPlayer.org/video/ogv/Big_Buck_Bunny_Trailer.ogv> ([play the OGG video live](https://jsbin.com/gekatoge/7/edit).)

If these don't play then the browser you are testing doesn't support the given format. Consider using a different format or using a fallback.

If these work but the files you are supplying don't, there are two possible issues:

#### 1. The media server is not delivering the correct mime types with the file

Although this is usually supported, you may need to add the following to your media server's `.htaccess` file.

```
# AddType TYPE/SUBTYPE EXTENSION

AddType audio/mpeg mp3
AddType audio/mp4 m4a
AddType audio/ogg ogg
AddType audio/ogg oga

AddType video/mp4 mp4
AddType video/mp4 m4v
AddType video/ogg ogv
AddType video/webm webm
AddType video/webm webmv
```

#### 2. Your files have been encoded incorrectly

Your files may have been encoded incorrectly — try encoding using one of the following tools, which are proven to be pretty reliable:

- [Audacity](http://audacity.sourceforge.net/) — Free Audio Editor and Recorder
- [Miro](http://www.getmiro.com/) — Free, open-source music and video player
- [Handbrake](http://handbrake.fr/) — Open Source Video Transcoder
- [Firefogg](http://firefogg.org/) — Video and Audio encoding for Firefox
- [FFmpeg2](https://www.ffmpeg.org/) — Comprehensive command line encoder
- [Libav](https://libav.org/) — Comprehensive command line encoder
- [Vid.ly](http://m.vid.ly/) — Video player,transcoding and delivery
- [Internet Archive](https://archive.org/) — Free transcoding and storage

### Detecting when no sources have loaded

To detect that all child {{ HTMLElement("source") }} elements have failed to load, check the value of the media element's `networkState` attribute. If this is `HTMLMediaElement.NETWORK_NO_SOURCE`, you know that all the sources failed to load.

If at that point you add another source, by inserting a new {{ HTMLElement("source") }} element as a child of the media element, Gecko attempts to load the specified resource.

### Showing fallback content when no source could be decoded

Another way to show the fallback content of a video, when none of the sources could be decoded in the current browser, is to add an error handler on the last source element. Then you can replace the video with its fallback content:

```html
<video controls>
  <source src="dynamicsearch.mp4" type="video/mp4"></source>
  <a href="dynamicsearch.mp4">
    <img src="dynamicsearch.jpg" alt="Dynamic app search in Firefox OS">
  </a>
  <p>Click image to play a video demo of dynamic app search</p>
</video>
```

```js
var v = document.querySelector('video'),
    sources = v.querySelectorAll('source'),
    lastsource = sources[sources.length-1];
lastsource.addEventListener('error', function(ev) {
  var d = document.createElement('div');
  d.innerHTML = v.innerHTML;
  v.parentNode.replaceChild(d, v);
}, false);
```

## Audio/Video JavaScript Libraries

A number of audio and video JavaScript libaries exist. The most popular libraries allow you to choose a consistent player design over all browsers and provide a fallback for browsers that don't support audio and video natively. Fallbacks often use Adobe Flash or Microsoft Silverlight plugins. Other functionality such as the track element for subtitles can also be provided through media libraries.

### Audio only

- [SoundManager](http://www.schillmania.com/projects/soundmanager2/)
- [AmplitudeJS](https://521dimensions.com/open-source/amplitudejs)
- [HowlerJS](https://howlerjs.com/)

### Video only

- [flowplayer](https://flowplayer.org/): Gratis with a flowplayer logo watermark. Open source (GPL licensed.)
- [JWPlayer](http://www.jwplayer.com): Requires registration to download. Open Source Edition (Creative Commons License.)
- [SublimeVideo](http://www.sublimevideo.net/): Requires registration. Form based set up with domain specific link to CDN hosted library.
- [Video.js](http://www.videojs.com/): Gratis and Open Source (Apache 2 Licensed.)

### Audio and Video

- [jPlayer](http://jPlayer.org): Gratis and Open Source (MIT Licensed.)
- [mediaelement.js](http://mediaelementjs.com/): Gratis and Open Source (MIT Licensed.)

### Web Audio API

- [AudioContext monkeypatch](https://github.com/cwilso/AudioContext-MonkeyPatch): A polyfill for older versions of the Web Audio API; Open Source (Apache 2 Licensed.)

## Basic tutorials

- [Creating a cross-browser video player](/ja/Apps/Build/Manipulating_media/cross_browser_video_player)
  - : A guide to creating a basic cross browser video player using the {{ htmlelement ("video") }} element.
- [Video player styling basics](/ja/Apps/Build/Manipulating_media/Video_player_styling_basics)
  - : With the cross-browser video player put in place in the previous article, this article now looks at providing some basic, reponsive styling for the player.
- [Cross-browser audio basics](/ja/Apps/Build/Manipulating_media/Cross-browser_audio_basics)
  - : This article provides a basic guide to creating an HTML5 audio player that works cross browser, with all the associated attributes, properties and events explained, and a quick guide to custom controls created using the Media API.
- [Media buffering, seeking, and time ranges](/ja/Apps/Build/Manipulating_media/buffering_seeking_time_ranges)
  - : Sometimes it's useful to know how much {{ htmlelement("audio") }} or {{ htmlelement("video") }} has downloaded or is playable without delay — a good example of this is the buffered progress bar of an audio or video player. This article discusses how to build a buffer/seek bar using [TimeRanges](/ja/docs/Web/API/TimeRanges), and other features of the media API.
- [HTML5 playbackRate explained](/ja/Apps/Build/Manipulating_media/HTML5_playbackRate_explained)
  - : The `playbackRate` property allows us to change the speed or rate at which a piece of web audio or video is playing. This article explains it in detail.
- [Using the Web Audio API](/ja/docs/Web/API/Web_Audio_API/Using_Web_Audio_API)
  - : Explains the basics of using the Web Audio API to grab, manipulate and play back an audio source.

## Streaming media tutorials

- [Live streaming web audio and video](/ja/Apps/Build/Manipulating_media/Live_streaming_web_audio_and_video)
  - : Live streaming technology is often employed to relay live events such as sports, concerts and more generally TV and Radio programmes that are output live. Often shortened to just streaming, live streaming is the process of transmitting media 'live' to computers and devices. This is a fairly complex and nascent subject with a lot of variables, so in this article we'll introduce you to the subject and let you know how you can get started.
- [Setting up adaptive streaming media sources](/ja/Apps/Build/Manipulating_media/Setting_up_adaptive_streaming_media_sources)
  - : Let's say you want to set up an adaptive streaming media source on a server, to be consumed inside an HTML5 media element. How would you do that? This article explains how, looking at two of the most common formats: MPEG-DASH and HLS (HTTP Live Streaming.)
- [DASH Adaptive Streaming for HTML 5 Video](/ja/docs/Web/HTML/DASH_Adaptive_Streaming_for_HTML_5_Video)
  - : Details how to set up adaptive streaming using DASH and WebM.

## Advanced tutorials

- [Adding captions and subtitles to HTML5 video](/ja/Apps/Build/Manipulating_media/Adding_captions_and_subtitles_to_HTML5_video)
  - : This article explains how to add captions and subtitles to HTML5 {{ htmlelement("video") }}, using [Web_Video_Text_Tracks_Format](/ja/docs/Web/API/Web_Video_Text_Tracks_Format) and the {{ htmlelement("track") }} element.
- [Writing Web Audio API code that works in every browser](/ja/docs/Web/Apps/Developing/Manipulating_media/Web_Audio_API_cross_browser)
  - : A guide to writing cross browser Web Audio API code.
- [H.264 support in Firefox](/ja/Apps/Developing/Manipulating_media/H.264_support_in_Firefox)
  - : This article explains the state of support for the H.264 video format in Firefox/Firefox OS, including code examples, tips and tricks.
- [Easy audio capture with the MediaRecorder API](https://hacks.mozilla.org/2014/06/easy-audio-capture-with-the-mediarecorder-api/)
  - : Explains the basics of using the MediaRecorder API to directly record a media stream.

> **Note:** Firefox OS versions 1.3 and above support the [RTSP](http://en.wikipedia.org/wiki/Real_Time_Streaming_Protocol) protocol for streaming video delivery. A fallback solution for older versions would be to use `<video>` along with a suitable format for Gecko (such as WebM) to serve fallback content. More information will be published on this in good time.

## References

- [The video element](/ja/docs/Web/HTML/Element/video)
- [Media events API](/ja/docs/Web/Guide/Events/Media_events)
- [HTMLVideoElement API](/ja/docs/Web/API/HTMLVideoElement)
- [MediaSource API](/ja/docs/Web/API/MediaSource)
- [Web Audio API](/ja/docs/Web/API/Web_Audio_API)
- [MediaRecorder API](/ja/docs/Web/API/MediaRecorder_API)
- [getUserMedia](/ja/docs/Web/API/MediaDevices/getUserMedia)
