---
title: TypedArray
slug: Web/JavaScript/Reference/Global_Objects/TypedArray
---
{{JSRef}}

**_TypedArray_** 物件表示了一個底層 [`ArrayBuffer`](/zh-TW/docs/Web/JavaScript/JavaScript_typed_arrays/ArrayBuffer) 的類陣列（array-like）視圖，它能以限定的型別解讀、修改 `ArrayBuffer`。但並沒有名為 `TypedArray` 的內建物件，`TypedArray` 也不存在可直接呼叫的建構式。真正能夠使用的是幾個原型繼承自 `TypedArray` 的內建物件，它們可以建立限定成員型別的物件實體來操作 `ArrayBuffer`。這些 `TypedArray` 型別的物件僅為視圖，並不會存放資料，所有的資料皆實際儲存於 `ArrayBuffer` 物件當中。以下將說明每種限定成員型別之 `TypedArray` 的共同屬性與方法。

{{EmbedInteractiveExample("pages/js/typedarray-constructor.html")}}

## 語法

```plain
new TypedArray(length);
new TypedArray(typedArray);
new TypedArray(object);
new TypedArray(buffer [, byteOffset [, length]]);

where TypedArray() is one of:

Int8Array();
Uint8Array();
Uint8ClampedArray();
Int16Array();
Uint16Array();
Int32Array();
Uint32Array();
Float32Array();
Float64Array();
```

### 參數

- length
  - : When called with a `length` argument, a typed array containing `length` zeroes is created.
- typedArray
  - : When called with a `typedArray` argument, which can be an object of any of the typed array types (such as `Int32Array`), the `typedArray` gets copied into a new typed array. Each value in `typedArray` is converted to the corresponding type of the constructor before being copied into the new array.
- object
  - : When called with an `object` argument, a new typed array is created as if by the `TypedArray.from()` method.
- buffer, byteOffset, length
  - : When called with a `buffer`, and optionally a `byteOffset` and a `length` argument, a new typed array view is created that views the specified {{jsxref("ArrayBuffer")}}. The `byteOffset` and `length` parameters specify the memory range that will be exposed by the typed array view. If both are omitted, all of `buffer` is viewed; if only `length` is omitted, the remainder of `buffer` is viewed.

## 說明

ECMAScript 2015 defines a _TypedArray_ constructor that serves as the `[[Prototype]]` of all _TypedArray_ constructors. This constructor is not directly exposed: there is no global `%TypedArray%` or `TypedArray` property. It is only directly accessible through `Object.getPrototypeOf(Int8Array)` and similar. All *TypedArray*s constructors inherit common properties from the `%TypedArray%` constructor function. Additionally, all typed array prototypes (_TypedArray_`.prototype`) have `%TypedArray%.prototype` as their `[[Prototype]]`.

The `%TypedArray%` constructor on its own is not particularly useful. Calling it or using it in a `new` expression will throw a `TypeError`, except when used during object creation in JS engines that support subclassing. There are at present no such engines, so `%TypedArray%` is only useful to polyfill functions or properties onto all _TypedArray_ constructors.

When creating an instance of a _TypedArray_ (e.g. `Int8Array`), an array buffer is created internally in memory or, if an `ArrayBuffer` object is given as constructor argument, then this is used instead. The buffer address is saved as an internal property of the instance and all the methods of %`TypedArray`%.`prototype`, i.e. set value and get value etc., operate on that array buffer address.

### Property access

You can reference elements in the array using standard array index syntax (that is, using bracket notation). However, getting or setting indexed properties on typed arrays will not search in the prototype chain for this property, even when the indices are out of bound. Indexed properties will consult the {{jsxref("ArrayBuffer")}} and will never look at object properties. You can still use named properties, just like with all objects.

```js
// Setting and getting using standard array syntax
var int16 = new Int16Array(2);
int16[0] = 42;
console.log(int16[0]); // 42

// Indexed properties on prototypes are not consulted (Fx 25)
Int8Array.prototype[20] = 'foo';
(new Int8Array(32))[20]; // 0
// even when out of bound
Int8Array.prototype[20] = 'foo';
(new Int8Array(8))[20]; // undefined
// or with negative integers
Int8Array.prototype[-1] = 'foo';
(new Int8Array(8))[-1]; // undefined

// Named properties are allowed, though (Fx 30)
Int8Array.prototype.foo = 'bar';
(new Int8Array(32)).foo; // "bar"
```

## TypedArray 物件

| Type                                     | Value Range               | Size in bytes | Description                                                               | Web IDL type          | Equivalent C type |
| ---------------------------------------- | ------------------------- | ------------- | ------------------------------------------------------------------------- | --------------------- | ----------------- |
| {{jsxref("Int8Array")}}         | -128 to 127               | 1             | 8-bit two's complement signed integer                                     | `byte`                | `int8_t`          |
| {{jsxref("Uint8Array")}}         | 0 to 255                  | 1             | 8-bit unsigned integer                                                    | `octet`               | `uint8_t`         |
| {{jsxref("Uint8ClampedArray")}} | 0 to 255                  | 1             | 8-bit unsigned integer (clamped)                                          | `octet`               | `uint8_t`         |
| {{jsxref("Int16Array")}}         | -32768 to 32767           | 2             | 16-bit two's complement signed integer                                    | `short`               | `int16_t`         |
| {{jsxref("Uint16Array")}}         | 0 to 65535                | 2             | 16-bit unsigned integer                                                   | `unsigned short`      | `uint16_t`        |
| {{jsxref("Int32Array")}}         | -2147483648 to 2147483647 | 4             | 32-bit two's complement signed integer                                    | `long`                | `int32_t`         |
| {{jsxref("Uint32Array")}}         | 0 to 4294967295           | 4             | 32-bit unsigned integer                                                   | `unsigned long`       | `uint32_t`        |
| {{jsxref("Float32Array")}}     | 1.2x10^-38 to 3.4x10^38   | 4             | 32-bit IEEE floating point number ( 7 significant digits e.g. 1.1234567)  | `unrestricted float`  | `float`           |
| {{jsxref("Float64Array")}}     | 5.0x10^-324 to 1.8x10^308 | 8             | 64-bit IEEE floating point number (16 significant digits e.g. 1.123...15) | `unrestricted double` | `double`          |

## 屬性

- {{jsxref("TypedArray.BYTES_PER_ELEMENT")}}
  - : Returns a number value of the element size for the different typed array objects.
- _TypedArray_.length
  - : Length property whose value is 0.
- {{jsxref("TypedArray.name")}}
  - : Returns the string value of the constructor name. E.g "Int8Array".
- {{jsxref("TypedArray.@@species", "get TypedArray[@@species]")}}
  - : The constructor function that is used to create derived objects.
- {{jsxref("TypedArray.prototype")}}
  - : Prototype for the _TypedArray_ objects.

## 方法

- {{jsxref("TypedArray.from()")}}
  - : Creates a new typed array from an array-like or iterable object. See also {{jsxref("Array.from()")}}.
- {{jsxref("TypedArray.of()")}}
  - : Creates a new typed array with a variable number of arguments. See also {{jsxref("Array.of()")}}.

## TypedArray 原型

All *TypedArray*s inherit from {{jsxref("TypedArray.prototype")}}.

### 屬性

{{page('en-US/Web/JavaScript/Reference/Global_Objects/TypedArray/prototype','Properties')}}

### 方法

{{page('en-US/Web/JavaScript/Reference/Global_Objects/TypedArray/prototype','Methods')}}

### Methods Polyfill

Many of the methods used in Typed Arrays can be polyfilled using the methods present in regular Javascript Arrays. The following snippet of JavaScript demonstrates how you might polyfill any missing Typed Array methods.

```js example-bad
var typedArrayTypes = [Int8Array, Uint8Array, Uint8ClampedArray, Int16Array,
          Uint16Array, ​​​Int32Array, Uint32Array, ​​​Float32Array, Float64Array];

for (var k in typedArrayTypes)
    for (var v in Array.prototype)
        if (Array.prototype.hasOwnProperty(v) &&
          !typedArrayTypes[k].prototype.hasOwnProperty(v))
            typedArrayTypes[k].prototype[v] = Array.prototype[v];
```

## 規範

{{Specifications}}

## 瀏覽器相容性

{{Compat("javascript.builtins.TypedArray")}}

## 相容性備註

Starting with ECMAScript 2015, `TypedArray` constructors require to be constructed with a {{jsxref("Operators/new", "new")}} operator. Calling a `TypedArray` constructor as a function without `new`, will throw a {{jsxref("TypeError")}} from now on.

```js example-bad
var dv = Int8Array([1, 2, 3]);
// TypeError: calling a builtin Int8Array constructor
// without new is forbidden
```

```js example-good
var dv = new Int8Array([1, 2, 3]);
```

## 參見

- [JavaScript typed arrays](/zh-TW/docs/Web/JavaScript/Typed_arrays)
- {{jsxref("ArrayBuffer")}}
- {{jsxref("DataView")}}
