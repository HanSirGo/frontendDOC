# Reset CSS与Normalize CSS的区别

### **Reset CSS**

Reset.css主要目的是重置浏览器的默认样式。在HTML标签中，浏览器通常会有一些默认的样式，例如`<p>`标签有上下边距，`<strong>`标签有字体加粗样式等。这些默认样式在不同的浏览器中可能会有所不同，给网页开发带来一定的麻烦。Reset.css通过重新定义标签样式，“覆盖”浏览器的CSS默认属性，将浏览器的默认样式全部去掉，以便开发者可以更容易地从头开始定义样式，确保在不同浏览器中保持一致性。

使用Reset.css时，只需将其引入到HTML文件中即可。例如，可以通过`<link>`标签在HTML文件的`<head>`部分引入Reset.css文件。

配置Reset.css主要取决于具体需求。最简单的Reset.css可能只包含几行代码，将所有元素的`padding`、`margin`和`border`都设置为0。而更复杂的Reset.css可能会包含更多的重置样式。

### **Normalize CSS**

Normalize.css的目标是提供一个更为温和的解决方案，通过修复浏览器间的差异，而不是完全消除它们。它尽量保留浏览器的默认样式，不进行过多的重置，同时修复一些浏览器自身的bug，并确保各浏览器的一致性。相比于Reset.css，Normalize.css是一种现代的、为HTML5准备的优质替代方案。

使用Normalize.css同样需要将其引入到HTML文件中。可以从Normalize.css的官方网站下载其源代码，并将其保存为CSS文件，然后通过`<link>`标签在HTML文件中引入。

Normalize.css的配置也取决于具体需求。

#### **区别**

Reset.css和Normalize.css的主要区别在于它们对浏览器默认样式的处理方式。Reset.css选择完全重置浏览器的默认样式，而Normalize.css则选择尽量保留浏览器的默认样式，并修复其中的不一致性和错误。

### Reset.css 使用例子

Reset.css 的使用相对简单，只需将其引入到 HTML 文件的 `<head>` 部分即可。以下是一个简单的例子：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reset.css 示例</title>
    <link rel="stylesheet" href="path/to/reset.css"> <!-- 引入 Reset.css -->
    <style>
    </style>
</head>
<body>
    <p>这是一个段落。</p>
</body>
</html>
```

在这个例子中，`reset.css` 文件被放置在 `path/to/` 目录下，需要将其替换为实际的文件路径。然后，可以在 `<style>` 标签中定义样式，而不用担心浏览器默认样式的影响。

### Normalize.css 使用例子

Normalize.css 的使用与 Reset.css 类似，也是将其引入到HTML 文件的 `<head>` 部分。以下是一个例子：

```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Normalize.css 示例</title>
    <link rel="stylesheet" href="path/to/normalize.css"> <!-- 引入 Normalize.css -->
    <style>
    </style>
</head>
<body>
    <p>这是一个段落。</p>
</body>
</html>
```

在这个例子中，`normalize.css` 文件被放置在 `path/to/` 目录下，需要将其替换为实际的文件路径。然后，可以在 `<style>` 标签中定义样式，而 Normalize.css 会确保不同浏览器之间的一致性，并修复一些常见的浏览器问题。

#### **各自的优缺点**

Reset.css的优点是它可以确保跨浏览器的一致性，因为它无视了浏览器默认的样式。但这也意味着开发者需要从头开始设置所有样式，增加了工作量。同时，由于完全去除了浏览器默认的样式，有时可能会导致一些不必要的样式问题。

Normalize.css的优点是它能够保留浏览器默认的有用样式，减少开发者的工作量。同时，由于它修复了浏览器自身的bug，也提高了网站在各种浏览器中的兼容性。但相应地，由于它保留了更多的浏览器默认样式，可能会在处理某些样式问题时需要花更多的时间。