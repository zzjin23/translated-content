---
title: TypedArray.prototype.entries()
slug: Web/JavaScript/Reference/Global_Objects/TypedArray/entries
---
{{JSRef}}

The **`entries()`**返回新的`Array Iterator`对象，包含数组每个下标处的键值对。

## 语法

```plain
arr.entries()
```

### 返回值

新的 **`Array Iterator`** 对象。

## 示例

### `使用 for...of` 循环的迭代

```js
var arr = new Uint8Array([10, 20, 30, 40, 50]);
var eArray = arr.entries();
// 你的浏览器必须支持 for..of 循环
// 以及 for 循环中的 let 区域变量
for (let n of eArray) {
  console.log(n);
}
```

### 备选迭代

```js
var arr = new Uint8Array([10, 20, 30, 40, 50]);
var eArr = arr.entries();
console.log(eArr.next().value); // [0, 10]
console.log(eArr.next().value); // [1, 20]
console.log(eArr.next().value); // [2, 30]
console.log(eArr.next().value); // [3, 40]
console.log(eArr.next().value); // [4, 50]
```

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}

## 另见

- [JavaScript 类型化数组](/en-US/docs/Web/JavaScript/Typed_arrays)
- {{jsxref("TypedArray")}}
- {{jsxref("TypedArray.prototype.keys()")}}
- {{jsxref("TypedArray.prototype.values()")}}
- {{jsxref("TypedArray.prototype.@@iterator()", "TypedArray.prototype[@@iterator]()")}}
