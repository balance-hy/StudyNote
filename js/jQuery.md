# jQuery

首先，在页面顶部添加 `script` 标签， 记得在后面为它添加结束标签。

浏览器会运行 `script` 标签所有的 JavaScript 脚本包括 jQuery。

在 `script` 标签中添加代码 `$(document).ready(function() {`。 然后在后面（仍在该 `script` 标签内）用 `});` 闭合它。

稍后将详细介绍 `functions`， 重要的是要知道，在浏览器加载页面后，你放入此 `function` 的代码将立即运行。

有一点很重要，如果没有 `document ready function`，代码将在 HTML 页面呈现之前运行，这可能会导致错误。

```html
<script type="text/javascript">
    	$(document).ready(function() {});
</script>
```

现在我们有一个 `document ready` 函数。

首先，完成第一个 jQuery 语句。 **所有的 jQuery 函数都以 `$` 开头**，这个符号通常被称为美元符号（dollar sign operator）或 bling。

jQuery 通常选取并操作带有选择器（selector）的 HTML 标签。

比如，想要给 `button` 元素添加跳跃效果。 只需要在 document ready 函数内添加如下代码：

```js
$("button").addClass("animated bounce");
```

请注意，我们已经在后台引入了 jQuery 库和 Animate.css 库，所以你可以在编辑器里直接使用它们。 你将使用 jQuery 将 Animate.css `bounce` class 应用于 `button` 元素。

如何使所有的 `button` 标签都有弹跳的动画效果？ 用 `$("button")` 选取所有的 button 标签，并用 `.addClass("animated bounce");` 给其添加一些 CSS 属性。

## $ 符号

在 jQuery 中，`$` 符号是一个非常重要的标识符，它通常用来代替全名的 `jQuery` 对象。 `$` 符号是一个合法的 JavaScript 变量名，可以用于创建 jQuery 对象、选择元素以及执行各种 DOM 操作。这是为了简化代码和提高可读性而引入的。

下面是一些常见的 `$` 符号在 jQuery 中的用法：

1. **创建 jQuery 对象**：使用 `$` 符号来包装一个或多个 DOM 元素，创建 jQuery 对象，以便后续进行 jQuery 操作。

   ```js
   var $element = $("#myElement"); // 选择元素并创建 jQuery 对象
   ```

2. **选择元素**：使用 `$` 符号作为选择器，可以选择一个或多个 DOM 元素。

   ```js
   var $paragraphs = $("p"); // 选择所有 <p> 元素
   ```

3. **执行 DOM 操作**：使用 `$` 符号来执行各种 DOM 操作，如添加、删除、修改元素的属性或内容。

   ```js
   $element.addClass("highlighted"); // 添加 CSS 类
   $element.text("New text"); // 设置元素文本内容
   ```

4. **事件处理**：使用 `$` 符号来绑定事件处理程序。

   ```js
   $element.click(function() {
     // 处理点击事件
   });
   ```

5. **Ajax 请求**：使用 `$` 符号的 AJAX 方法来执行异步请求。

   ```js
   $.ajax({
     url: "example.com/data",
     success: function(data) {
       // 处理响应数据
     }
   });
   ```

总之，`$` 符号是 jQuery 的核心标识符，用于引用 jQuery 对象、执行 DOM 操作和处理事件等。它有助于简化和提高 JavaScript 代码的可读性。请注意，虽然 `$` 符号通常用于 jQuery，但它本质上只是一个 JavaScript 变量，因此你可以在代码中自定义 `$` 变量，但这可能导致冲突，所以谨慎使用。

## Dom元素

DOM 元素是文档对象模型（Document Object Model）中的基本构建块，代表网页中的各种元素或节点。DOM 元素是网页上可操作的基本单位，它们可以是 HTML 元素（如段落、标题、按钮等）或文档中的其他部分（如文本、属性等）。每个 DOM 元素都是文档树的一部分，可以通过 JavaScript 来访问、操作和修改。

以下是一些关于 DOM 元素的要点：

1. **HTML 元素**：HTML 元素是 DOM 中的一种元素，它们代表网页中的标记元素，如 `<p>`、`<div>`、`<a>`、`<button>` 等。这些元素具有各种属性和方法，可以通过 JavaScript 来操纵和操作。
2. **文本元素**：文本元素是 DOM 元素的一种，它包含纯文本内容，例如段落中的文字。你可以使用 JavaScript 来获取、修改和操作文本元素的内容。
3. **属性**：DOM 元素可以有各种属性，这些属性包括元素的 ID、类名、样式、事件处理程序等。通过 JavaScript，你可以访问和修改这些属性。
4. **节点**：DOM 元素是 DOM 树中的节点，DOM 树是网页的逻辑结构表示。每个 DOM 元素都有父节点、子节点和兄弟节点。你可以使用 JavaScript 遍历 DOM 树，访问不同的元素。
5. **操作元素**：通过 JavaScript，你可以执行各种操作，如创建、删除、插入和移动元素。这使你能够实时更新网页内容。
6. **事件处理**：DOM 元素可以关联事件处理程序，以便在特定事件发生时执行 JavaScript 代码。例如，你可以将点击事件处理程序附加到按钮元素，以便在用户点击按钮时执行特定操作。

总之，DOM 元素是构成网页的基本组成部分，通过 JavaScript 可以实现对这些元素的访问、操作和控制，以实现动态和交互性的网页功能

## 方法

### addClass 添加类

jQuery 的 `.addClass()` 方法用来给标签添加类。