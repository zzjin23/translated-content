---
title: AbortController
slug: Web/API/AbortController
---
{{APIRef("DOM")}}

**`AbortController`** 接口表示一个控制器对象，允许你根据需要中止一个或多个 Web 请求。

你可以使用 {{domxref("AbortController.AbortController()")}} 构造函数创建一个新的 `AbortController`。使用 {{domxref("AbortSignal")}} 对象可以完成与 DOM 请求的通信。

## 构造函数

- {{domxref("AbortController.AbortController()")}}
  - : 创建一个新的 `AbortController` 对象实例。

## 属性

- {{domxref("AbortController.signal")}} {{readonlyInline}}
  - : 返回一个 {{domxref("AbortSignal")}} 对象实例，它可以用来 with/abort 一个 Web（网络）请求。

## 方法

- {{domxref("AbortController.abort()")}}
  - : 中止一个尚未完成的 Web（网络）请求。这能够中止 [fetch](/zh-CN/docs/Web/API/fetch) 请求及任何响应体的消费和流。

## 示例

> **备注：** {{domxref("AbortSignal")}} 中还有其他额外的示例。

在下面的代码片段中，我们想通过 [Fetch API](/zh-CN/docs/Web/API/Fetch_API) 下载一段视频。

我们先使用 {{domxref("AbortController.AbortController","AbortController()")}} 构造函数创建一个控制器，然后使用 {{domxref("AbortController.signal")}} 属性获取其关联 {{domxref("AbortSignal")}} 对象的引用。

当一个 [fetch request](/zh-CN/docs/Web/API/fetch) 初始化，我们把 `AbortSignal` 作为一个选项传递到到请求对象（如下 `{ signal }`）。这将 `signal` 和 `controller` 与这个 `fetch request` 相关联，然后允许我们通过调用 {{domxref("AbortController.abort()")}} 中止请求，如下第二个事件监听函数。

```js
const controller = new AbortController();
let signal = controller.signal;

const downloadBtn = document.querySelector('.download');
const abortBtn = document.querySelector('.abort');

downloadBtn.addEventListener('click', fetchVideo);

abortBtn.addEventListener('click', function() {
  controller.abort();
  console.log('Download aborted');
});

function fetchVideo() {
  //...
  fetch(url, {signal}).then(function(response) {
    //...
  }).catch(function(e) {
    reports.textContent = 'Download error: ' + e.message;
  })
}
```

> **备注：** 当 `abort()` 被调用时，这个 `fetch()` promise 将 `reject` 一个名为 `AbortError` 的 `DOMException`。

您可以在 [GitHub](https://github.com/mdn/dom-examples/tree/master/abort-api) 上找到这个示例的完整源代码（也可以[在线运行](https://mdn.github.io/dom-examples/abort-api/)）。

## 规范

{{Specifications}}

## 浏览器兼容

{{Compat}}

## 参见

- [Fetch API](/zh-CN/docs/Web/API/Fetch_API)
- [Abortable Fetch](https://developers.google.com/web/updates/2017/09/abortable-fetch) by Jake Archibald
