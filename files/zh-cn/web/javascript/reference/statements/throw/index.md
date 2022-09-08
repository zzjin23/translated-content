---
title: throw
slug: Web/JavaScript/Reference/Statements/throw
---
{{jsSidebar("Statements")}}

**`throw`** **语句**用来抛出一个用户自定义的异常。当前函数的执行将被停止（`throw` 之后的语句将不会执行），并且控制将被传递到调用堆栈中的第一个 [`catch`](/zh-CN/docs/Web/JavaScript/Reference/Statements/try...catch) 块。如果调用者函数中没有 `catch` 块，程序将会终止。

{{EmbedInteractiveExample("pages/js/statement-throw.html")}}

## 语法

```plain
throw expression;
```

- `expression`
  - : 要抛出的表达式。

## 描述

使用`throw`语句来抛出一个异常。当你抛出异常时，`expression` 指定了异常的内容。下面的每行都抛出了一个异常：

```js
throw "Error2"; // 抛出了一个值为字符串的异常
throw 42;       // 抛出了一个值为整数 42 的异常
throw true;     // 抛出了一个值为 true 的异常
```

注意`throw`语句同样受到[自动分号插入（ASI](/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Automatic_semicolon_insertion)）机制的控制，在`throw`关键字和值之间不允许有行终止符。

## 示例

### 抛出一个对象

你可以在抛出异常时指定一个对象。然后可以在`catch`块中引用对象的属性。以下示例创建一个类型为`UserException`的对象，并在`throw`语句中使用它。

```js
function UserException(message) {
   this.message = message;
   this.name = "UserException";
}
function getMonthName(mo) {
   mo = mo-1; // 调整月份数字到数组索引 (1=Jan, 12=Dec)
   var months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul",
      "Aug", "Sep", "Oct", "Nov", "Dec"];
   if (months[mo] !== undefined) {
      return months[mo];
   } else {
      throw new UserException("InvalidMonthNo");
   }
}

try {
   // statements to try
   var myMonth = 15; // 15 超出边界并引发异常
   var monthName = getMonthName(myMonth);
} catch (e) {
   var monthName = "unknown";
   console.log(e.message, e.name); // 传递异常对象到错误处理
}
```

### 另一个抛出异常对象的示例

下面的示例中测试一个字符串是否是美国邮政编码。如果邮政编码是无效的，那么`throw`语句将会抛出一个类型为 `ZipCodeFormatException`的异常对象实例。

```js
/*
 * 创建 ZipCode 示例。
 *
 * 可被接受的邮政编码格式：
 *    12345
 *    12345-6789
 *    123456789
 *    12345 6789
 *
 * 如果构造函数参数传入的格式不符合以上任何一个格式，将会抛出异常。
 */

function ZipCode(zip) {
   zip = new String(zip);
   pattern = /[0-9]{5}([- ]?[0-9]{4})?/;
   if (pattern.test(zip)) {
      // zip code value will be the first match in the string
      this.value = zip.match(pattern)[0];
      this.valueOf = function() {
         return this.value
      };
      this.toString = function() {
         return String(this.value)
      };
   } else {
      throw new ZipCodeFormatException(zip);
   }
}

function ZipCodeFormatException(value) {
   this.value = value;
   this.message = "不是正确的邮政编码";
   this.toString = function() {
      return this.value + this.message
   };
}

/*
 * 这可能是一个验证美国地区中的脚本
 */

const ZIPCODE_INVALID = -1;
const ZIPCODE_UNKNOWN_ERROR = -2;

function verifyZipCode(z) {
   try {
      z = new ZipCode(z);
   } catch (e) {
      if (e instanceof ZipCodeFormatException) {
         return ZIPCODE_INVALID;
      } else {
         return ZIPCODE_UNKNOWN_ERROR;
      }
   }
   return z;
}

a = verifyZipCode(95060);         // 返回 95060
b = verifyZipCode(9560);          // 返回 -1
c = verifyZipCode("a");           // 返回 -1
d = verifyZipCode("95060");       // 返回 95060
e = verifyZipCode("95060 1234");  // 返回 95060 1234
```

### 重新抛出异常

你可以使用`throw`来抛出异常。下面的例子捕捉了一个异常值为数字的异常，并在其值大于 50 后重新抛出异常。重新抛出的异常传播到闭包函数或顶层，以便用户看到它。

```js
try {
   throw n; // 抛出一个数值异常
} catch (e) {
   if (e <= 50) {
      // 异常在 1-50 之间时，直接处理
   } else {
      // 异常无法处理，重新抛出
      throw e;
   }
}
```

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}

## 相关链接

- [`try...catch`](/en-US/docs/Web/JavaScript/Reference/Statements/try...catch)
