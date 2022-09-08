---
title: padding-block-end
slug: Web/CSS/padding-block-end
---
{{CSSRef}}

**`padding-block-end`** は [CSS](/ja/docs/Web/CSS) のプロパティで、要素のブロック方向の論理的な末尾側のパディングを定義し、それが要素の書字方向やテキストの方向に応じて物理的なパディングに対応づけられます。

{{EmbedInteractiveExample("pages/css/padding-block-end.html")}}

## 構文

```css
/* <length> 値 */
padding-block-end: 10px;   /* 絶対的な長さ */
padding-block-end: 1em;    /* テキストの大きさに対する相対値 */

/* <percentage> 値 */
padding-block-end: 5%;     /* ブロックコンテナーの幅に対する割合のパディング */

/* グローバル値 */
padding-block-end: inherit;
padding-block-end: initial;
padding-block-end: revert;
padding-block-end: unset;
```

### 値

- {{cssxref("&lt;length&gt;")}}
  - : パディングの寸法を固定値で指定します。負の値は指定できません。
- {{cssxref("&lt;percentage&gt;")}}
  - : パディングの寸法をパーセント値で表したもので、含まれるブロックの _inline-size_ に対する相対値です。負の値は指定できません。

## 解説

`padding-block-end` プロパティは、{{cssxref("padding-top")}} プロパティと同じ値を取ると仕様で定義されています。しかし、このプロパティが対応づけられる物理的なプロパティは、 {{cssxref("writing-mode")}}, {{cssxref("direction")}}, {{cssxref("text-orientation")}} に設定された値に依存します。したがって、{{cssxref("padding-bottom")}}, {{cssxref("padding-right")}}, {{cssxref("padding-left")}} の何れかに対応づけられる可能性があります。

関連プロパティとして、要素の他のマージンを定義する {{cssxref("padding-block-start")}}, {{cssxref("padding-inline-start")}}, {{cssxref("padding-inline-end")}} があります。

## 公式定義

{{cssinfo}}

## 形式文法

{{csssyntax}}

## 例

<h3 id="Setting_block_end_padding_for_vertical_text">縦書きテキストにおけるブロック方向の末尾のパディングの設定</h3>

#### HTML

```html
<div>
  <p class="exampleText">Example text</p>
</div>
```

#### CSS

```css
div {
  background-color: yellow;
  width: 120px;
  height: 120px;
}

.exampleText {
  writing-mode: vertical-lr;
  padding-block-end: 20px;
  background-color: #C8C800;
}
```

#### 結果

{{EmbedLiveSample("Setting_block_end_padding_for_vertical_text", 140, 140)}}

## 仕様書

{{Specifications}}

## ブラウザーの互換性

{{Compat}}

## 関連情報

- 対応づけ先の物理的プロパティ: {{cssxref("padding-top")}}, {{cssxref("padding-right")}}, {{cssxref("padding-bottom")}}, {{cssxref("padding-left")}}
- {{cssxref("writing-mode")}}, {{cssxref("direction")}}, {{cssxref("text-orientation")}}
