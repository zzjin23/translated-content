---
title: XMLHttpRequest()
slug: Web/API/XMLHttpRequest/XMLHttpRequest
---
{{APIRef('XMLHttpRequest')}}

**`XMLHttpRequest()`** 建構式會建立一個新的 {{domxref("XMLHttpRequest")}} 物件。

關於如何使用 `XMLHttpRequest` 物件的細節，請參照： [Using XMLHttpRequest](/zh-TW/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest).

## 語法

```plain
const request = new XMLHttpRequest();
```

### 參數

無。

### 回傳值

將回傳一個新的 {{domxref("XMLHttpRequest")}} 物件。{{domxref("XMLHttpRequest")}} 物件在呼叫{{domxref("XMLHttpRequest.send", "send()")}} 送出要求到伺服器之前，至少要藉著呼叫 {{domxref("XMLHttpRequest.open", "open()")}} 來準備好必需的設定。

## 非標準的 Firefox 語法

Firefox 16 added a non-standard parameter to the constructor that can enable anonymous mode (see {{Bug("692677")}}). Setting the `mozAnon` flag to `true` effectively resembles the [`AnonXMLHttpRequest()`](http://www.w3.org/TR/2012/WD-XMLHttpRequest-20120117/#dom-anonxmlhttprequest) constructor described in older versions of the XMLHttpRequest specification.

```plain
const request = new XMLHttpRequest(paramsDictionary);
```

### Parameters (non-standard)

- `objParameters`

  - : There are two flags you can set:

    - `mozAnon`
      - : Boolean: Setting this flag to `true` will cause the browser not to expose the {{Glossary("origin")}} and [user credentials](http://www.w3.org/TR/2012/WD-XMLHttpRequest-20120117/#user-credentials) when fetching resources. Most important, this means that {{Glossary("Cookie", "cookies")}} will not be sent unless explicitly added using setRequestHeader.
    - `mozSystem`
      - : Boolean: Setting this flag to `true` allows making cross-site connections without requiring the server to opt-in using {{Glossary("CORS")}}. _Requires setting `mozAnon: true`, i.e. this can't be combined with sending cookies or other user credentials. This only works in privileged (reviewed) apps ({{Bug("692677")}}); it does not work on arbitrary webpages loaded in Firefox_

## 參見

- [Using XMLHttpRequest](/zh-TW/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest)
- [HTML in XMLHttpRequest](/zh-TW/docs/Web/API/XMLHttpRequest/HTML_in_XMLHttpRequest)
