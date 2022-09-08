---
title: aria-activedescendant 属性の使用
slug: web/Accessibility/ARIA/Attributes/aria-activedescendant
original_slug: Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-activedescendant_attribute
---
この記事では、[`aria-activedescendant`](https://www.w3.org/TR/wai-aria/#aria-activedescendant) プロパティについて説明します。

### 説明

`aria-activedescendant` 属性には、ドキュメントオブジェクトモデル内の複合ウィジェットの一部である、現在アクティブな子オブジェクトの ID が含まれます。 これは、1 つ以上の子にフォーカスを持たせるというオーバーヘッドを伴います。 名前が指定するように、複合ウィジェットの現在アクティブな子を管理するのに役立ちます。

### ユーザーエージェントと支援技術への影響

ユーザーエージェントは、検索、レンダリング、およびエンドユーザーとウェブコンテンツとのやりとりを容易にするソフトウェアで、`aria-activedescendant` プロパティを使用して、フォーカスを持っているアクティブな子について支援技術に通知します。 `aria-activedescendant` プロパティを使用するこのアクティブな子は、常に画面上に表示され、ドキュメントオブジェクトモデルのコンテナの子孫でなければなりません。

> **Note:** 支援技術がどのようにこの技術を扱うべきかについての意見は異なる場合があります。 以下に示す情報は、これらの意見の 1 つで、したがって規範的ではありません。

### 例

#### Example 1:

```html
Code
```

#### 動作する例

### 注

### 使用された ARIA 属性

### 関連する ARIA 技術

### 互換性

TBD: 一般的な UA と AT 製品の組み合わせに関するサポート情報を追加する

### その他のリソース
