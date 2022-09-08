---
title: width
slug: Web/CSS/@media/width
---
{{cssref}}

`可以使用width` [CSS](/en-US/docs/CSS) {{cssxref("@media")}} 基于{{glossary("viewport")}}宽度 (或页面框，用于[paged media](/en-US/docs/Web/CSS/Paged_media)) 应用样式。

## 语法

该`width`特性被指定为{{cssxref("&lt;length&gt;")}}，表示 viewport 宽度的值。这是一个范围特性，也就是说，您也可以使用前缀 **`min-width`** 和 **`max-width`** 分别查询最小值和最大值。

## 示例

### HTML

```html
<div>改变 viewport 的宽度的同时，观察这个元素的变化。</div>
```

### CSS

```css
/* 精确宽度 */
@media (width: 360px) {
  div {
    color: red;
  }
}

/* 最小宽度 */
@media (min-width: 35rem) {
  div {
    background: yellow;
  }
}

/* 最大宽度 */
@media (max-width: 50rem) {
  div {
    border: 2px solid blue;
  }
}
```

### 结果

{{EmbedLiveSample('示例','90%')}}

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat("css.at-rules.media.width")}}
