---
title: permissions.contains()
slug: Mozilla/Add-ons/WebExtensions/API/permissions/contains
---
{{AddonSidebar()}}

检查扩展名是否具有给定 {{WebExtAPIRef("permissions.Permissions")}} 对象中列出的权限。

The `Permissions` argument may contain either an origins property, which is an array of [host permissions](/zh-CN/Add-ons/WebExtensions/manifest.json/permissions#Host_permissions), or a `permissions` property, which is an array of [API permissions](/zh-CN/Add-ons/WebExtensions/manifest.json/permissions#API_permissions), or both.

This is an asynchronous function that returns a [`Promise`](/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise). The promise is fulfilled with true only if all the extension currently has all the given permissions. For host permissions, if the extension's permissions [pattern-match](/zh-CN/docs/Mozilla/Add-ons/WebExtensions/Match_patterns) the permissions listed in `origins`, then they are considered to match.

## Syntax

```js
var getContains = browser.permissions.contains(
  permissions                // Permissions object
)
```

### Parameters

- `permissions`
  - : A {{WebExtAPIRef("permissions.Permissions")}} object.

### Return value

A [`Promise`](/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise) that will be fulfilled with `true` if the extension already has all the permissions listed in the `permissions` argument, or `false` otherwise.

## Browser compatibility

{{Compat("webextensions.api.permissions.contains")}}

## Examples

```js
// Extension permissions are:
// "webRequest", "tabs", "*://*.mozilla.org/*"

var testPermissions1 = {
  origins: ["*://mozilla.org/"],
  permissions: ["tabs"]
};

browser.permissions.contains(testPermissions1).then((result) => {
  console.log(result);    // true
});

var testPermissions2 = {
  origins: ["*://mozilla.org/"],
  permissions: ["tabs", "alarms"]
};

browser.permissions.contains(testPermissions2).then((result) => {
  console.log(result);   // false, "alarms" doesn't match
});

var testPermissions3 = {
  origins: ["https://developer.mozilla.org/"],
  permissions: ["tabs", "webRequest"]
};

browser.permissions.contains(testPermissions3).then((result) => {
  console.log(result);   // true: "https://developer.mozilla.org/"
});                      // matches: "*://*.mozilla.org/*"

var testPermissions4 = {
  origins: ["https://example.org/"]
};

browser.permissions.contains(testPermissions4).then((result) => {
  console.log(result);   // false, "https://example.org/"
});                      // does not match
```

{{WebExtExamples}}

> **备注：** This API is based on Chromium's [`chrome.permissions`](https://developer.chrome.com/extensions/permissions) API.
>
> Microsoft Edge compatibility data is supplied by Microsoft Corporation and is included here under the Creative Commons Attribution 3.0 United States License.
