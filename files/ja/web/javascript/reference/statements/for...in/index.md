---
title: for...in
slug: Web/JavaScript/Reference/Statements/for...in
---
{{jsSidebar("Statements")}}

**`for...in` 文**は、キーが文字列であるオブジェクトの[列挙可能プロパティ](/ja/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)すべてに対して、継承された列挙可能プロパティも含めて反復処理を行います ([Symbol](/ja/docs/Web/JavaScript/Reference/Global_Objects/Symbol) がキーになったものは無視します)。

{{EmbedInteractiveExample("pages/js/statement-forin.html")}}

## 構文

```
for (variable in object)
  文
```

- `variable`
  - : 反復するごとに、 `variable` に異なるプロパティ名が代入されます。
- `object`
  - : このオブジェクトの列挙可能プロパティに対して反復処理がされます。

## 解説

`for...in` ループは、列挙可能なシンボル以外のプロパティに対してのみ反復処理を行います。 `Array` や `Object` のような組込みコンストラクターから生成したオブジェクトは、列挙可能でないプロパティを `Objet.prototype` や `String.prototype` から、例えば {{jsxref("String")}} の {{jsxref("String.indexOf", "indexOf()")}} メソッドや {{jsxref("Object")}} の {{jsxref("Object.toString", "toString()")}} メソッドを継承しています。このループは、対象オブジェクト自身とそのオブジェクトがプロトタイプから継承しているすべての列挙可能なプロパティを反復します (プロトタイプチェーンで対象オブジェクトに近いプロパティは、親プロトタイプのプロパティを上書きします)。

### プロパティの変更や削除

`for...in` ループは、任意の順序でオブジェクトのプロパティに対して反復します (なぜ繰り返しの見かけの順序に依存できないのかについては、詳細は {{jsxref("Operators/delete", "delete")}} 演算子を見てください)。

もしプロパティがある反復で修正されて、その後に訪問されたなら、ループにより公開される値は後の時点での値となります。訪問される前に削除されたプロパティは、それから後には訪問されません。オブジェクトに対する反復が起きている中でそのオブジェクトに追加されたプロパティは、訪問されるかもしれませんし反復から省略されるかもしれません。

一般的に、現在訪問しているプロパティ以外のものに関しては、反復の間はオブジェクトにプロパティを追加、修正、または削除しないのが一番です。追加したプロパティが訪問されるか、(現在のもの以外の)修正したプロパティが修正される前または後に訪問されるか、または削除したプロパティが削除される前に訪問されるかといったことには、何の保証もありません。

### 配列の繰り返しと for...in

> **Note:** **注:** `for...in` はインデックスの順序が重要となる {{jsxref("Array", "配列")}} の繰り返しには使うべきではありません。

配列のインデックスは単に整数値の名前で列挙できるプロパティであり、そうでないと一般的なオブジェクトのプロパティとして一意になりません。 `for...in` は特定の順序で並べられる保証はありません。 `for...in` ループ文はすべての列挙できるプロパティを返し、その中には非整数型やそれを引き継いだインデックス名があります。

繰り返しの順序が実装依存なため、配列の繰り返しは要素を一貫した順番で参照することになるとは限りません。このため、アクセスの順番が大事となる配列を繰り返す時には、数値のインデックスでの {{jsxref("Statements/for", "for")}} ループ (か {{jsxref("Array.prototype.forEach()")}} か {{jsxref("Statements/for...of", "for...of")}} ループ) を使った方が良いです。

### 独自のプロパティだけで繰り返す

オブジェクトに付属するプロパティだけを考えればよい場合、 {{jsxref("Object.getOwnPropertyNames", "getOwnPropertyNames()")}} を使うか、 {{jsxref("Object.prototype.hasOwnProperty", "hasOwnProperty()")}} を実行してチェックします({{jsxref("Object.prototype.propertyIsEnumerable", "propertyIsEnumerable")}} も使用できます)。または、外部のコードインターフェイスをまったく知らない場合は、チェックメソッドを備えた組み込みの prototypes を継承できます。

## for...in を使用する理由

`for...in` はオブジェクトのプロパティを反復するために作られたものであり、配列での使用は推奨されず、 `Array.prototype.forEach()` や `for...of` などの選択肢があるわけですが、それでは `for...in` を使用する場面は何なのでしょうか？

最も具体的な使い方はデバッグ目的であるかもしれません。これは、オブジェクトのプロパティを (コンソールに出力するなどして) 簡単にチェックする方法になります。データを格納するには配列の方が実用的な場合が多いですが、データを扱うにはキーと値のペアが好まれる状況では (プロパティが "キー" として機能します)、それらのキーが特定の値を保持しているかどうかをチェックしたい場合があるかもしれません。

## 例

### for...in の使用

以下の `for...in` ループは、オブジェクトの列挙可能なシンボルではないプロパティをすべて反復し、そのプロパティ名と値を文字列で記録します。

```js
var obj = {a: 1, b: 2, c: 3};

for (const prop in obj) {
  console.log(`obj.${prop} = ${obj[prop]}`);
}

// Output:
// "obj.a = 1"
// "obj.b = 2"
// "obj.c = 3"
```

### 自身のプロパティの反復処理

次の関数では {{jsxref("Object.prototype.hasOwnProperty", "hasOwnProperty()")}}: の使い方を例示しています。継承されたプロパティは表示されません。

```js
var triangle = {a: 1, b: 2, c: 3};

function ColoredTriangle() {
  this.color = 'red';
}

ColoredTriangle.prototype = triangle;

var obj = new ColoredTriangle();

for (const prop in obj) {
  if (obj.hasOwnProperty(prop)) {
    console.log(`obj.${prop} = ${obj[prop]}`);
  }
}

// Output:
// "obj.color = red"
```

## 仕様書

| 仕様書                                                                                                       |
| ------------------------------------------------------------------------------------------------------------ |
| {{SpecName('ESDraft', '#sec-for-in-and-for-of-statements', 'for...in statement')}} |

## ブラウザーの互換性

{{Compat("javascript.statements.for_in")}}

### 厳格モードにおける初期化式の互換性について

Firefox 40 より前では、`for...in` ループ内で初期化式 (`i=0`) が使用可能でした。

```js example-bad
var obj = {a: 1, b: 2, c: 3};
for (var i = 0 in obj) {
  console.log(obj[i]);
}
// 1
// 2
// 3
```

この標準外の動作はバージョン 40 以降では無視され、[厳格モード](/ja/docs/Web/JavaScript/Reference/Strict_mode)での {{jsxref("SyntaxError")}} ("[for-in loop head declarations may not have initializers](/ja/docs/Web/JavaScript/Reference/Errors/Invalid_for-in_initializer)") エラーが表示されます ([bug 748550](https://bugzilla.mozilla.org/show_bug.cgi?id=748550) および [bug 1164741](https://bugzilla.mozilla.org/show_bug.cgi?id=1164741))。

v8 (Chrome), Chakra (IE/Edge), JSC (WebKit/Safari) といった他のエンジンも同様に非標準のふるまいを削除するよう開発しています。

## 関連情報

- {{jsxref("Statements/for...of", "for...of")}} - プロパティ値に対して繰り返す同様の文
- {{jsxref("Statements/for_each...in", "for each...in")}} - `for...in` に似ていますが、プロパティ名そのものではなく、オブジェクトのプロパティの値に対して反復します。(廃止されました)
- {{jsxref("Statements/for", "for")}}
- [ジェネレーター式](/ja/docs/Web/JavaScript/Guide/Iterators_and_Generators) (`for...of` とあわせて利用できます)
- [プロパティの列挙可能性と所有権](/ja/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)
- {{jsxref("Object.getOwnPropertyNames()")}}
- {{jsxref("Object.prototype.hasOwnProperty()")}}
- {{jsxref("Array.prototype.forEach()")}}
