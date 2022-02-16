# 如何更改DOM

在“了解DOM”系列的前两期中，我们学习了[如何在DOM中访问元素](https://www.taniarascia.com/how-to-access-elements-in-the-dom)[以及如何遍历DOM](https://www.taniarascia.com/how-to-traverse-the-dom)。使用此知识，开发人员可以使用类、标签、id和选择器查找DOM中的任何节点，并使用父属性、子属性和兄弟姐妹属性查找相关节点。

了解DOM的下一步是学习如何添加、更改、替换和删除节点。待办事项列表应用程序是JavaScript程序的一个实用示例，您需要能够在DOM中创建、修改和删除元素。

在本文中，我们将学习如何创建新节点并将其插入DOM，替换现有节点并删除节点。

## 创建新节点

在静态网站上，元素通过直接在`.html`文件中编写HTML添加到页面中。在动态Web应用程序中，元素和文本通常使用JavaScript添加。`createElement()`和`createTextNode()`方法用于在DOM中创建新节点。

| 财产/方法                                                    | 描述                         |
| :----------------------------------------------------------- | :--------------------------- |
| [`createElement()`](https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement) | 创建新的元素节点             |
| [`createTextNode()`](https://developer.mozilla.org/en-US/docs/Web/API/Document/createTextNode) | 创建新的文本节点             |
| [`node.textContent`](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent) | 获取或设置元素节点的文本内容 |
| [`node.innerHTML`](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML) | 获取或设置元素的HTML内容     |

打开一个新的基本`index.html`文件进行处理。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Learning the DOM</title>
  </head>

  <body>
    <h1>Document Object Model</h1>
  </body>
</html>
```

我们将使用`document`对象上的`createElement()`创建一个新的`p`元素。

```js
const paragraph = document.createElement('p')
```

我们创建了一个新的`p`元素，我们可以在*控制台*中测试。

```
console.log(paragraph);
```

控制台

```html
<p></p>
```

`paragraph`变量输出一个空的`p`元素，如果没有任何文本，这个元素就不是很有用。为了向元素添加文本，我们将设置`textContent`属性。

```js
paragraph.textContent = "I'm a brand new paragraph."
console.log(paragraph);
```

控制台

```html
<p>I'm a brand new paragraph.</p>
```

`createElement()`和`textContent`的组合创建一个完整的元素节点。

设置元素内容的另一种方法是使用`innerHTML`属性，该属性允许您向元素添加HTML和文本。

```js
paragraph.innerHTML = "I'm a paragraph with <strong>bold</strong> text."
```

虽然这将奏效，并且是向元素添加内容的常见方法，但使用`innerHTML`方法可能存在[跨站点脚本（XSS）](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML#Security_considerations)风险，因为内联JavaScript可以添加到元素中。因此，建议改用`textContent`，这将删除HTML标签。

也可以使用`createTextNode()`方法创建一个文本节点。

```js
const text = document.createTextNode("I'm a new text node.")
console.log(text);
```

控制台

```js
"I'm a new text node."
```

使用这些方法，我们创建了新的元素和文本节点，但在它们插入文档之前，它们不会在网站的前端显示。

## 将节点插入DOM

为了查看我们在前端创建的新文本节点和元素，我们需要将它们插入`document`中。`appendChild()`和`insertBefore()`用于向父元素的开头、中间或结尾添加项目，并`replaceChild()`用于将旧节点替换为新节点。

| 财产/方法                                                    | 描述                                     |
| :----------------------------------------------------------- | :--------------------------------------- |
| [`node.appendChild()`](https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild) | 将节点添加为父元素的最后一个子元素       |
| [`node.insertBefore()`](https://developer.mozilla.org/en-US/docs/Web/API/Node/insertBefore) | 在指定的兄弟姐妹节点之前将节点插入父元素 |
| [`node.replaceChild()`](https://developer.mozilla.org/en-US/docs/Web/API/Node/replaceChild) | 将现有节点替换为新节点                   |

让我们用HTML创建一个简单的待办事项列表。

```html
<ul>
  <li>Buy groceries</li>
  <li>Feed the cat</li>
  <li>Do laundry</li>
</ul>
```

当您在浏览器中加载页面时，它看起来如下：

[![做1](https://www.taniarascia.com/static/f20445b8c8da9bd0501445f44022f115/5a190/to-do-1.png)](https://www.taniarascia.com/static/f20445b8c8da9bd0501445f44022f115/ae77d/to-do-1.png)

为了在待办事项列表的末尾添加新项目，我们必须先创建元素并向它添加文本，正如我们在“创建新节点”中所了解到的那样。

```js
// To-do list ul element
const todoList = document.querySelector('ul')

// Create new to-do
const newTodo = document.createElement('li')
newTodo.textContent = 'Do homework'
```

既然我们有了新的待办事项的完整元素，我们可以用`appendChild()`将其添加到列表末尾。

```js
// Add new todo to the end of the list
todoList.appendChild(newTodo)
```

你可以看到新的`li`元素已附加到`ul`的末尾。

```html
<ul>
  <li>Buy groceries</li>
  <li>Feed the cat</li>
  <li>Do laundry</li>
  <li>Do homework</li>
</ul>
```

[![做2](https://www.taniarascia.com/static/42b327d58d5aafc924910134e440d27e/5a190/to-do-2.png)](https://www.taniarascia.com/static/42b327d58d5aafc924910134e440d27e/ae77d/to-do-2.png)

也许我们有更高的优先级待办事项，我们希望将其添加到列表的开头。我们必须创建另一个元素，因为`createElement()`只创建一个元素，不能重用。

```js
// Create new to-do
const anotherTodo = document.createElement('li')
anotherTodo.textContent = 'Pay bills'
```

我们可以使用`insertBefore()`将其添加到列表的开头。此方法需要两个参数——第一个是要添加的新子节点，第二个参数是将立即跟随新节点的兄弟节点。换句话说，您将在下一个兄弟姐妹节点之前插入新节点。

```js
parentNode.insertBefore(newNode, nextSibling)
```

对于我们的待办事项列表示例，我们将在列表的第一个元素子元素之前添加新的`anotherTodo`元素，该子元素目前是`Buy groceries`列表项。

```js
// Add new to-do to the beginning of the list
todoList.insertBefore(anotherTodo, todoList.firstElementChild)
<ul>
  <li>Pay bills</li>
  <li>Buy groceries</li>
  <li>Feed the cat</li>
  <li>Do laundry</li>
  <li>Do homework</li>
</ul>
```

[![做3](https://www.taniarascia.com/static/3c754db322c358f17b502dd1845041b3/5a190/to-do-3.png)](https://www.taniarascia.com/static/3c754db322c358f17b502dd1845041b3/8ae78/to-do-3.png)

新节点已在列表开头成功添加。现在我们知道了如何将节点添加到父元素中。接下来我们可能想做的是用新节点替换现有节点。

我们将修改现有的待办事项，以演示如何替换节点。创建新元素的第一步保持不变。

```js
const modifiedTodo = document.createElement('li')
modifiedTodo.textContent = 'Feed the dog'
```

与`insertBefore()`，`replaceChild()`需要两个参数——新节点和要替换的节点。

```js
parentNode.replaceChild(newNode, oldNode)
```

我们将把列表的第三个元素子元素替换为修改后的待办事项。

```js
// Replace existing to-do with modified to-do
todoList.replaceChild(modifiedTodo, todoList.children[2])
<ul>
  <li>Pay bills</li>
  <li>Buy groceries</li>
  <li>Feed the dog</li>
  <li>Do laundry</li>
  <li>Do homework</li>
</ul>
```

[![做4](https://www.taniarascia.com/static/b644f6766567be62a999e45adc8f2cb5/5a190/to-do-4.png)](https://www.taniarascia.com/static/b644f6766567be62a999e45adc8f2cb5/6937a/to-do-4.png)

通过`appendChild()`、`insertBefore()`和`replaceChild()`的组合，您可以在DOM的任何地方插入节点和元素。

## 从DOM中删除节点

现在我们知道如何创建元素，将它们添加到DOM中，并修改现有元素。最后一步是学习从DOM中删除现有节点。可以使用`removeChild()`从父节点中删除子节点，也可以使用`remove()`删除节点本身。

| 方法                                                         | 描述       |
| :----------------------------------------------------------- | :--------- |
| [`node.removeChild()`](https://developer.mozilla.org/en-US/docs/Web/API/Node/removeChild) | 删除子节点 |
| [`node.remove()`](https://developer.mozilla.org/en-US/docs/Web/API/ChildNode/remove) | 删除节点   |

使用上面的待办事项示例，我们希望在项目完成后删除它们。如果您完成了家庭作业，您可以使用`removeChild()`删除`Do homework`项目，该项目恰好是列表中的最后一个子项。

```js
todoList.removeChild(todoList.lastElementChild)
<ul>
  <li>Pay bills</li>
  <li>Buy groceries</li>
  <li>Feed the dog</li>
  <li>Do laundry</li>
</ul>
```

[![做5](https://www.taniarascia.com/static/bc0d7ec2a5448d29f8abaff540715b5a/5a190/to-do-5.png)](https://www.taniarascia.com/static/bc0d7ec2a5448d29f8abaff540715b5a/6937a/to-do-5.png)

一个更简单的方法是直接在节点上使用`remove()`方法删除节点本身。

```js
// Remove second element child from todoList
todoList.children[1].remove()
<ul>
  <li>Pay bills</li>
  <li>Feed the dog</li>
  <li>Do laundry</li>
</ul>
```

[![做6](https://www.taniarascia.com/static/bd347019b5bb847e8a160f263b78ed55/5a190/to-do-6.png)](https://www.taniarascia.com/static/bd347019b5bb847e8a160f263b78ed55/8ae78/to-do-6.png)

在`removeChild()`和`remove()`，您可以从DOM中删除任何节点。从DOM中删除子元素的另一个方法可能是将父元素的`innerHTML`属性设置为空字符串（“`""`。这不是首选方法，但你可能会看到它。

## 结论

在本教程中，我们学习了如何使用JavaScript创建新节点和元素并将其插入DOM，以及替换和删除现有节点和元素。

在“了解DOM”系列的这个时候，您知道如何访问DOM中的任何元素，浏览DOM中的任何节点，并修改DOM本身。您现在可以自信地使用JavaScript创建基本的前端Web应用程序。



