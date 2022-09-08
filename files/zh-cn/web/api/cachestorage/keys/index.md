---
title: CacheStorage.keys()
slug: Web/API/CacheStorage/keys
---
{{APIRef("Service Workers API")}}{{SeeCompatTable}}

{{domxref("CacheStorage")}} 接口的 **`keys()`** 方法返回一个 {{jsxref("Promise")}}对象，它使用一个数组 resolve，该数组包含 {{domxref("CacheStorage")}} 对象按创建顺序跟踪的所有命名 {{domxref("Cache")}} 对象对应的字符串。使用此方法迭代所有 {{domxref("Cache")}} 对象。

## 语法

```plain
caches.keys().then(function(keyList) {
  //对 keyList 做操作
});
```

### 返回

一个使用 {{domxref("CacheStorage")}} 对象中 {{domxref("Cache")}} 名称数组 resolve 的 {{jsxref("Promise")}}

### 参数

无。

## 示例

在此代码片段中，我们监听{{domxref("ServiceWorkerGlobalScope.onactivate", "activate")}} 事件，然后运行一个 {{domxref("ExtendableEvent.waitUntil","waitUntil()")}} 方法，该方法在新的 service worker 被激活之前清除老的、无用的 cache。 这里我们设置一个包含缓存名称的白名单。 通过使用 `keys() 方法` 来返回{{domxref("CacheStorage")}} 对象中的 keys 集合，然后检查缓存 key 是否在白名单中，如果不存在，则使用 {{domxref("CacheStorage.delete")}} 方法来删除该缓存。

```js
this.addEventListener('activate', function(event) {
  var cacheWhitelist = ['v2'];

  event.waitUntil(
    caches.keys().then(function(keyList) {
      return Promise.all(keyList.map(function(key) {
        if (cacheWhitelist.indexOf(key) === -1) {
          return caches.delete(key);
        }
      });
    })
  );
});
```

## 规范

{{Specifications}}

## 浏览器兼容

{{Compat("api.CacheStorage.keys")}}

## 亦可参考

- [Using Service Workers](/zh-CN/docs/Web/API/ServiceWorker_API/Using_Service_Workers)
- {{domxref("Cache")}}
- {{domxref("WorkerGlobalScope.caches")}}
