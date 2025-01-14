---
date: 2025-01-14 11:01:37
updated: 2025-01-14 11:01:47
---
哎呀真好看(捧臉

使用方法：(Prism)

1. [Theme] > [Customize]

2. [Advanced] > [Add CSS]

3. 貼上從 Prism 那裏拿到的 CSS 全部內容

4. 回到設定，[Layout]

5. [Sidebar (Bottom)] > [+ Add a Gadget] > [HTML/JavaScript]

6. [Title] 保持空白、[Content] 貼上從 Prism 那裏拿到的 JS 全部內容

7. 嘗試使用，觀看效果

  

ref: [Prism.js 網頁程式碼高亮顯示工具](https://www.blogger.com/blog/post/edit/6558487227761678795/3425602686891804413#)、[Prism.js 推薦套件和使用方式教學](https://www.blogger.com/blog/post/edit/6558487227761678795/3425602686891804413#)、[[技術分享] 寫給會在部落格中撰寫程式碼的你 ─ 在網頁中嵌入高亮程式碼上色 (syntax highlighting)](https://www.blogger.com/blog/post/edit/6558487227761678795/3425602686891804413#)、[使用 Prism.js 在 Blogger 加入程式語言高亮（Highlight）](https://www.blogger.com/blog/post/edit/6558487227761678795/3425602686891804413#)

  

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