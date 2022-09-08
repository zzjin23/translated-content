---
title: 'ARIA: list role'
slug: Web/Accessibility/ARIA/Roles/List_role
---
ARIA `list` 角色可用于标识项目列表。它通常与 `listitem` 角色结合使用，该角色用于标识包含在列表中的列表项。

```html
<section role="list">
  <div role="listitem">List item 1</div>
  <div role="listitem">List item 2</div>
  <div role="listitem">List item 3</div>
</section>
```

## 描述

任何由外部容器和内部元素列表组成的内容都可以分别使用 `list` 和 `listitem` 容器识别为辅助技术。一个 `list` 必须有一个或多个 `listitem` 子级，或者，有一个或多个 `group` 作为子级，每个 `group`有一个或多个 `listitem` 作为子项。

关于应该使用哪些元素来标记 `list` 和 `listitem` 没有硬性规定，但是您应该确保 `listitem` 在列表的上下文中有意义，例如购物清单、食谱步骤、行车路线。

> **警告：** 如果可能，您应该使用适当的语义化 HTML 元素来标记 `list` 及其 `listitem`，如 {{HTMLElement("ul")}}、{{HTMLElement("ol")}} 和 {{HTMLElement("li")}}。有关完整示例，请参阅 [最佳实践](#best_practices)。

### 关联的 WAI-ARIA 角色、状态和属性

- [listitem](/zh-CN/docs/Web/Accessibility/ARIA/Roles/Listitem_role)
  - : 列表或目录中的单个项目。角色为 listitem 的元素只能在角色为 `list` 或 `group` 的元素中找到。
- [group](/zh-CN/docs/Web/Accessibility/ARIA/Roles/group_role)
  - : 相关对象的集合，在嵌套在列表中时仅限于列表项，其重要性不足以在页面目录中拥有自己的位置。

### 键盘交互

无

### 所需的 JavaScript 功能

无

## 示例

[ARIA Lists](https://www.scottohara.me/blog/2018/05/26/aria-lists.html) — Scott O'Hara 的一些有用的例子和想法

## 最佳实践

仅在必要时使用 `role="list"` 和 `role="listitem"`。例如，无法控制 HTML 但能够在之后动态使用 JavaScript 提升无障碍的时候。

与 HTML {{HTMLElement("ol")}} 和 {{HTMLElement("ul")}} 不同，ARIA `list` 角色不区分有序列表和无序列表。如果可能，您应该使用适当的语义 HTML 元素来标记列表（{{HTMLElement("ol")}} 和 {{HTMLElement("ul")}}）和列表项（{{HTMLElement("li")}})。例如，我们上面的例子应该改写如下：

```html
<ul>
  <li>List item 1</li>
  <li>List item 2</li>
  <li>List item 3</li>
</ul>
```

或者如果列表项的顺序很重要，则使用有序列表：

```html
<ol>
  <li>List item 1</li>
  <li>List item 2</li>
  <li>List item 3</li>
</ol>
```

> **备注：** ARIA `list` / `listitem` 角色不区分有序列表和无序列表。

顺便说一句，请注意，如果您使用 {{HTMLElement("ol")}} 或 {{HTMLElement("ul")}} 的语义 HTML 元素并应用 [`presentation`](/zh-CN/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_presentation_role) 角色，则每个子 `li` 元素都会继承 `presentation` 角色，因为 ARIA 需要 `listitem` 元素具有父 `list` 元素。因此，`li` 元素不会暴露给辅助技术，但是包含在这些 `li` 元素中的元素，包括嵌套列表，对辅助技术是可见的。

> **备注：** 如果要标记将用作选项卡式界面的项目列表，则应改为使用 `tab`、`tabpanel` 和 `tablist` 角色。

## 规范

{{Specifications}}

## 屏幕阅读器支持

TBD

## 参见

- [Accessibility Object Model](https://wicg.github.io/aom/spec/)
- [ARIA in HTML](https://w3c.github.io/html-aria/)
- {{HTMLElement("ul")}}
- {{HTMLElement("ol")}}
- {{HTMLElement("li")}}
- [ARIA: listitem role](/zh-CN/docs/Web/Accessibility/ARIA/Roles/Listitem_role)
- [ARIA: group role](/zh-CN/docs/Web/Accessibility/ARIA/Roles/group_role)

<section id="Quick_links"><ol><li><a href="/en-US/docs/Web/Accessibility/ARIA/Roles"><strong>WAI-ARIA roles</strong></a>{{ListSubpagesForSidebar("/en-US/docs/Web/Accessibility/ARIA/Roles")}}</li></ol></section>
