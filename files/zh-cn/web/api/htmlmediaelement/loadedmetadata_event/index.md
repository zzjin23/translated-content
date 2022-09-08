---
title: GlobalEventHandlers.onloadedmetadata
slug: Web/API/HTMLMediaElement/loadedmetadata_event
---
{{ ApiRef("HTML DOM") }}

{{domxref("GlobalEventHandlers")}} mixin 的 **`onloadedmetadata`** 属性是处理{{event("loadedmetadata")}}事件的{{event("Event_handlers", "event handler")}}。

`loadedmetadata`事件在元数据（metadata）被加载完成后触发。

## 语法

```plain
element.onloadedmetadata = handlerFunction;
var handlerFunction = element.onloadedmetadata;
```

`handlerFunction`应当是`null`或是由[JavaScript 函数](/en-US/docs/Web/JavaScript/Reference/Functions)声明的事件 handler。

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}

## 参考

- {{event("loadedmetadata")}}
- [DOM 事件句柄](/en-US/docs/Web/Guide/Events/Event_handlers)
