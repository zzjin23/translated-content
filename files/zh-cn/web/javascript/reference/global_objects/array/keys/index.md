---
title: Array.prototype.keys()
slug: Web/JavaScript/Reference/Global_Objects/Array/keys
---
{{JSRef}}

**`keys()`** 方法返回一个包含数组中每个索引键的 **`Array Iterator`** 对象。

{{EmbedInteractiveExample("pages/js/array-keys.html")}}

## 语法

```plain
arr.keys()
```

### 返回值

一个新的 {{jsxref("Array")}} 迭代器对象。

## 示例

### 索引迭代器会包含那些没有对应元素的索引

```js
var arr = ["a", , "c"];
var sparseKeys = Object.keys(arr);
var denseKeys = [...arr.keys()];
console.log(sparseKeys); // ['0', '2']
console.log(denseKeys);  // [0, 1, 2]
```

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}

## 相关链接

- {{jsxref("Array.prototype.values()")}}
- {{jsxref("Array.prototype.entries()")}}
- [Iteration protocols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)
