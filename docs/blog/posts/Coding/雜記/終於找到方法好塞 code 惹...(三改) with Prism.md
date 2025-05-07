---
tags:
  - Codinggg/Tools
categories:
  - 雜記
date: 2025-01-14 11:01:37
updated: 2025-03-31 17:05:30
---

哎呀真好看 (捧臉

<!-- more -->

使用方法：[Prism](https://prismjs.com/)

1. [Theme] > [Edit HTML]    
2. 找到 `</head>`，在前一行貼上 `<script src='http://ajax.googleapis.com/ajax/libs/jquery/1.8.2/jquery.min.js'></script>`，並且在兩者之間加上 `<style>`(貼上從 Prism 那裏拿到的 CSS 全部內容)`</style>`。    
3. 找到 `</body>`，在前一行貼上 `<script>`(貼上從 Prism 那裏拿到的 JS 全部內容)`</script>`    
4. 嘗試使用，觀看效果
  

ref: 
* [Prism.js 網頁程式碼高亮顯示工具](https://www.blogger.com/blog/post/edit/6558487227761678795/3425602686891804413#)
* [Prism.js 推薦套件和使用方式教學](https://www.blogger.com/blog/post/edit/6558487227761678795/3425602686891804413#)
* [[技術分享] 寫給會在部落格中撰寫程式碼的你 ─ 在網頁中嵌入高亮程式碼上色 (syntax highlighting)](https://www.blogger.com/blog/post/edit/6558487227761678795/3425602686891804413#)
* [使用 Prism.js 在 Blogger 加入程式語言高亮（Highlight）](https://www.blogger.com/blog/post/edit/6558487227761678795/3425602686891804413#)

  

```csharp
[TypeFilter(typeof(xxx))]
[HttpPost]
[Route("ooo")]
public async Task<aaa> BBB(User user)
{
    try
    {
        var result = ...;
        return result;
    }
    catch (Exception e)
    {
        _logger.LogError($"{e.Message}");
        return ...;
    }
}
```

```html
<pre class="line-numbers">
	<code class="language-markup">(Enter code here)
	</code>
</pre>
```


> Written with [StackEdit](https://stackedit.io/).