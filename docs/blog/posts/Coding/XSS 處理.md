---
tags:
  - IS/XSS
categories:
  - IS
date: 2025-01-08 03:15:43
updated: 2025-02-21 17:09:46
---
## `C#`

- 預設：`HttpUtility.HtmlEncode()`
- 可考慮：[mganss/HtmlSanitizer: Cleans HTML to avoid XSS attacks](https://github.com/mganss/HtmlSanitizer)
	- [浮雲雅築: [研究][ASP.NET] 防 XSS 的 HtmlSanitizer ( HTML消毒劑)](https://shaurong.blogspot.com/2021/09/aspnet-xss-htmlsanitizer-html.html)


## `JS`

- 好東西：[cure53/DOMPurify: DOMPurify - a DOM-only, super-fast, uber-tolerant XSS sanitizer for HTML, MathML and SVG. DOMPurify works with a secure default, but offers a lot of configurability and hooks. Demo:](https://github.com/cure53/DOMPurify?tab=readme-ov-file)