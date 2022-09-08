---
title: TypedArray.prototype.keys()
slug: Web/JavaScript/Reference/Global_Objects/TypedArray/keys
---
{{JSRef}}

**`keys()`**方法返回新的 `Array Iterator` 对象，包含数组中每个下标的键。

## 语法

```plain
arr.keys()
```

### 返回值

新的 **`Array Iterator`** 对象

## 示例

### 使用`for...of`循环的迭代

```js
var arr = new Uint8Array([10, 20, 30, 40, 50]);
var eArray = arr.keys();
// 你的浏览器必须支持 for..of 循环
// 以及 for 循环中的 let 区域变量
for (let n of eArray) {
  console.log(n);
}
```

### 备选的迭代

```js
var arr = new Uint8Array([10, 20, 30, 40, 50]);
var eArr = arr.keys();
console.log(eArr.next().value); // 0
console.log(eArr.next().value); // 1
console.log(eArr.next().value); // 2
console.log(eArr.next().value); // 3
console.log(eArr.next().value); // 4
```

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}

## 另见

- [JavaScript 类型化数组](/en-US/docs/Web/JavaScript/Typed_arrays)
- {{jsxref("TypedArray")}}
- {{jsxref("TypedArray.prototype.entries()")}}
- {{jsxref("TypedArray.prototype.values()")}}
- {{jsxref("TypedArray.prototype.@@iterator()", "TypedArray.prototype[@@iterator]()")}}
- [for...of](/en-US/docs/Web/JavaScript/Reference/Statements/for...of)
- [迭代协议](/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)
