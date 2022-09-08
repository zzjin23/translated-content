---
title: Firefox 26 for developers
slug: Mozilla/Firefox/Releases/26
---
{{FirefoxSidebar}}

## Web 开发者需要注意的变化

### CSS

- `text-decoration-line` 属性仍然带有前缀，不过现在它把 `'blink'` 也看看成了它的合法属性值，虽然并不会真正的让内容产生闪烁效果 ({{bug("812995")}}).
- 非标准的 [`-moz-text-blink`](/zh-CN/docs/Web/CSS/-moz-text-blink) 属性已被删除 ({{bug("812995")}}).
- 实现了 [CSS Image Values Level 4](http://dev.w3.org/csswg/css-images-4/) 中的 [`image-orientation`](/zh-CN/docs/Web/CSS/image-orientation) 属性，比起 Level 3, Level 4 增加了对 `from-image` 关键字和 EXIF 的支持。
- 支持了 `position:sticky`, 需要开启 `layout.css.sticky.enabled` 选项 ({{bug(886646)}}).

### HTML

_No change._

### JavaScript

- 实现了 ES6 中的 [`Math`](/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math): `Math.fround()` 方法 ({{bug("900125")}}).

### Interfaces/APIs/DOM

- {{domxref("DOMImplementation.createDocument")}} 方法的第三个参数 `doctype` 成为可选参数 ({{bug(909859)}}).
- 实现了新的 {{domxref("element.classList")}} 规范，可以同时添加或删除多个类名，比如 `element.classList.add("x", "y", "z")` ({{bug(814014)}}).
- 实现了 {{domxref("URL.URL", "URL()")}} 构造函数 ({{bug("887364")}}).
- The properties {{domxref("URLUtils.origin")}}, {{domxref("URLUtils.password")}}, and {{domxref("URLUtils.username")}} are now available to all interfaces implementing {{domxref("URLUtils")}}: {{domxref("URL")}}, {{domxref("Location")}}, {{domxref("HTMLAnchorElement")}}, and {{domxref("HTMLAreaElement")}} ({{bug("887364")}}).
- The {{domxref("URL")}} interface is now accessible from Web Workers ({{bug("887364")}}).
- IndexedDB can now be used as a "optimistic" storage area so it doesn't require any prompts and data is stored in a pool with LRU eviction policy, in short temporary storage ({{bug("785884")}}).
- Path of the persistent storage has been changed from \<profile>/indexedDB to \<profile>/storage/persistent (on b2g from /data/local/indexedDB to /data/local/storage/persistent).

### MathML

- Inconsistent renderings of {{MathMLElement("mmultiscripts")}}, {{MathMLElement("msub")}}, {{MathMLElement("msup")}} and {{MathMLElement("msubsup")}} have been unified and the error handling of these elements has been improved ({{bug("827713")}}).

### SVG

_No change._

## 相关链接

- [Firefox 26 网站兼容性](/zh-CN/docs/Mozilla/Firefox/Releases/26/Site_Compatibility)

### 更早版本

{{Firefox_for_developers('25')}}
