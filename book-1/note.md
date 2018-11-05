## 一、在HTML中使用JavaScript



#### 1、`<script>`元素

- 6个属性

  - async

    > 可选，表示立即下载脚本，但不妨碍页面中的其他操作，比如下载其他资源或者等待加载其他脚本。只对外部文件有效。

  - charst

    > 可选，表示通过src属性指定的代码的字符集。由于大部分浏览器会忽略它的值，因此用的比较少。

  - defer

    > 可选，表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本文件有效。IE7以前的的版本的对嵌入脚本也支持这个属性。

  - language

    > 已废弃，原来用于表示编写代码使用的脚本语言（比如JavaScript、JavaScript1.2、VBScript）。大部分浏览器会忽略这个属性。

  - src

    > 可选，表示包含要执行代码的外部文件。

  - type

    > 可选，可以看成是language的替代属性。表示编写代码使用的脚本语言的内容类型（MIME类型）。

    - `text/javascript`和`text/ecmascript`已经不被推荐使用，但一直使用的都是`text/javascript`
    - 实际上服务器在传送JavaScript文件使用的MIME类型是`application/x-javascript`,但是`type`使用这个值可能导致脚本会被忽略。
    - IE浏览器中可以使用`application/application`或者`application/ecmascript`
    - 目前使用的依旧还是`text/javascript`，如果没有这个属性，则其默认值仍是`text/javascript`

- 使用`<script>`元素的方式

  - 直接在页面中嵌入JavaScript代码

    - 在解释器对`script`元素内部的代码求值完毕之前，页面中的其他内容都不会被浏览器加载或者显示

      ```html
      <script type='text/javascript'>
          function sayHi(){
              console.log('hi')
          }
      </script>
      ```

    - 在使用`<script>`嵌入JavaScript代码时，不要在代码中的任何地方出现`</script>`，当浏览器遇到字符串`</script>`会被认为是结束的标签，可以通过转义字符`/`解决这个问题

      ```html
      <script type='text/javascript'>
          function sayscript(){
              console.log('</script>')
          }
      	function sayscript(){
              console.log('<\/script>')
          }
      </script>
      ```

  - 包含外部JavaScript文件

    - 在HTML文档中，使用`<script>`的`src`属性

      ```html
      <script type="text/javascript" src="example.js"></script>
      ```

    - 在XHTML文档中，可以省略代码中结束的`</script>`标签（不能再HTML文档中使用）

      ```html
      <script type="text/javascript" src="example.js" />
      ```

  - 扩展：

    - 一般情况下，外部JavaScript文件带有`.js`扩展名。但是这个扩展名不是必需的，因为浏览器不会检查包含JavaScript的文件的扩展名。这样可以使用JSP、PHP或其他服务端语言动态生成JavaScript代码。但是服务器通常还是需要看扩展名决定为响应应用哪种MIME类型，如果不使用扩展名，请确保服务器能够返回正确的MIME类型。

    - 带有`src`属性的<script>元素不应该再开始标签和结束标签之间包含额外的JavaScript代码。如果包含了嵌入的的代码，只会下载并执行外部脚本文件，嵌入的代码会被忽略。

      ```html
      <script type="text/javascript" src="example.js">alert('some js')</script>
      ```

    - 带有`src`属性的<script>元素还可以包含来自外部域的JavaScript文件（可以指向当前HTML页面所在域之外的某个域中的完整URL），这一点与<img>元素非常相似

      ```html
      <script type="text/javascript" src="http://www.swhere.com/a.js"></script>
      ```

    - 在不存在`defer`和`async`属性的<script>元素中，通过src引入的外部脚本文件，浏览器都会按照<script>出现的先后顺序对它们依次解析。

      ```html
      <script type="text/javascript" src="first.js"></script>
      <script type="text/javascript" src="then.js"></script>
      <script type="text/javascript" src="end.js"></script>
      ```

#### 2、外部脚本的位置

- 正常模式

  - 放在页面的<head>元素中，页面会在等到所有的JavaScript文件被下载、解析和执行完成以后，才会开始呈现页面的内容。如果JavaScript文件过大或过多，就会导致浏览器在呈现页面时出现明显的延迟，而延迟期间，浏览器窗口会一片空白。

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Example HTML Page</title>
        <script type="text/javascript" src="example1.js"></script>
        <script type="text/javascript" src="example2.js"></script>
        <script type="text/javascript" src="example3.js"></script>
    </head>
    <body>
        <!-- 内容 -->
    </body>
    </html>
    ```

  - 放在<body>元素后面，再解析包含的JavaScript代码之前，页面的内容将会完全呈现在浏览器窗口中，就可以避免出现这样的问题

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Example HTML Page</title>
    </head>
    <body>
        <!-- 内容 -->
        <script type="text/javascript" src="example1.js"></script>
        <script type="text/javascript" src="example2.js"></script>
        <script type="text/javascript" src="example3.js"></script>
    </body>
    </html>
    ```

- 延迟脚本：HTML4.01定义了为<script>标签定义了`defer`属性。这个属性的使用后，脚本会被延迟到整个页面都被解析完毕后再运行，即告诉浏览器立即下载，但延迟执行

  - 延迟脚本并不一定会按照顺序执行，也不一定会在`DOMContentLoaded`事件触发前执行，但是HTML5规范要求是脚本要按照它们出现的先后顺序执行，并且先于`DOMContentLoaded`事件执行，因此，最好只包含一个延迟脚本
  - HTML5明确规定`defer`属性只适用于外部脚本文件，如果支持HTML5实现会忽略给嵌入脚本设置的`defer`属性。IE4—IE7还支持对嵌入脚本的`defer`属性，IE8以后完全支持HTML5规定的行为
  - 在XMHTML中，要把`defer`属性设置为`defer="defer"`，而不能省略

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Example HTML Page</title>
      <script type="text/javascript" src="example1.js" defer='defer'></script>
      <script type="text/javascript" src="example2.js" defer='defer'></script>
      <script type="text/javascript" src="example3.js" defer='defer'></script>
  </head>
  <body>
      <!-- 内容 -->
  </body>
  </html>
  ```

- 异步脚本：HTML5为<script>标签定义了`async`属性。

  - 同样与defer类似，async只适用于外部脚本文件，并告诉浏览器立即下载文件。但与defer不同的是，标记为async的脚本并不保证按照指定它们的先后顺序执行。指定async属性的目的是不让页面等待两个脚本下载和执行，从而异步加载页面其他内容，为此，建议异步脚本不要在加载期间修改 DOM。
  - 异步脚本一定会在页面的load事件前执行，但可能会在DOMContentLoaded事件触发之前或之后执行。支持异步脚本的浏览器有 Firefox 3.6、Safari 5 和 Chrome
  - 在 XHTML 文档中，要把async属性设置为async="async"，不可以省略。

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Example HTML Page</title>
      <script type="text/javascript" src="example1.js" defer='async'></script>
      <script type="text/javascript" src="example2.js" defer='defer'></script>
      <script type="text/javascript" src="example3.js" defer='defer'></script>
  </head>
  <body>
      <!-- 内容 -->
  </body>
  </html>
  ```

#### 3、在XHTML中的用法

> 可扩展超文本标记语言，即 XHTML（Extensible HyperText Markup Language），是将 HTML 作为XML 的应用而重新定义的一个标准。编写 XHTML 代码的规则要比编写 HTML 严格得多，而且直接影响能否在嵌入 JavaScript 代码时使用<script/>标签。

- 以下面的代码块为例，虽然它们在 HTML 中是有效的，但在 XHTML 中则是无效的。 在 HTML 中，有特殊的规则用以确定<script>元素中的哪些内容可以被解析，但这些特殊的规则在 XHTML 中不适用。这里比较语句a < b中的小于号（<）在 XHTML 中将被当作开始一个新标签来解析。但是作为标签来讲，小于号后面不能跟空格，因此就会导致语法错误。

  ```html
  <script type="text/javascript">
      function compare (a,b) {
          if (a < b) {
              alert('A is less than B')
          } else if (a > b) { 
              alert('A is greater than B')
          } else { 
              alert('A is equal to B')
          }
      }
  </script>
  ```

- 避免在 XHTML 中出现类似语法错误的方法有两个。

  - 一是用相应的 HTML 实体（&lt;）替换代码中所有的小于号（<），替换后的代码类似如下所示： 

    ```html
    <script type="text/javascript">
        function compare (a,b) {
            if (a &lt; b) {
                alert('A is less than B')
            } else if (a > b) { 
                alert('A is greater than B')
            } else { 
                alert('A is equal to B')
            }
        }
    </script>
    ```

  - 二是用一个 CData 片段来包含 JavaScript 代码。在 XHTML（XML）中，CData 片段是文档中的一个特殊区域，这个区域中可以包含不需要解析的任意格式的文本内容。因此，在 CData 片段中就可以使用任意字符，而且不会导致语法错误。引入 CData 片段后的 JavaScript 代码块如下所示：

    ```html
    <script type="text/javascript"><![CDATA[
            function compare (a,b) {
            if (a < b) {
                alert('A is less than B')
            } else if (a > b) { 
                alert('A is greater than B')
            } else { 
                alert('A is equal to B')
            }
        }
    ]]></script>
    ```

  - 在兼容 XHTML 的浏览器中，这个方法可以解决问题。但实际上，还有不少浏览器不兼容 XHTML，因而不支持 CData 片段。再使用 JavaScript 注释将 CData 标记注释掉就可以了。这种格式在所有现代浏览器中都可以正常使用。虽然有几分 hack 的味道，但它能通过 XHTML 验证，而且对 XHTML 之前的浏览器也会平稳退化。

    ```html
    <script type="text/javascript">
    //<![CDATA[
        function compare (a,b) {
        if (a < b) {
            alert('A is less than B')
        } else if (a > b) { 
            alert('A is greater than B')
        } else { 
            alert('A is equal to B')
        }
      }
    //]]>
    </script>
    ```

#### 4、嵌入代码与外部文件（使用外部文件的优点）

- 可维护性：遍及不同 HTML 页面的 JavaScript 会造成维护问题。但把所有 JavaScript 文件都放在一个文件夹中，维护起来就轻松多了。而且开发人员因此也能够在不触及 HTML 标记的情况下，集中精力编辑 JavaScript 代码
- 可缓存：浏览器能够根据具体的设置缓存链接的所有外部 JavaScript 文件。也就是说，如果有两个页面都使用同一个文件，那么这个文件只需下载一次。因此，最终结果就是能够加快页面加载的速度。
- 适应未来：通过外部文件来包含 JavaScript 无须使用前面提到 XHTML 或注释 hack。HTML 和XHTML 包含外部文件的语法是相同的。

#### 5、文档模式

- 文档类型通过`doctype`来切换。一般分为：混杂模式（quirks mode）和标准模式（standards mode）。如果在文档开始处没有发现文档类型声明，则所有浏览器都会默认开启混杂模式。但采用混杂模式不是什么值得推荐的做法，需要使用某些 hack 技术，使跨浏览器的行为一致。

- 对于标准模式，可以通过使用下面任何一种文档类型来开启：

  ```html
  <!-- HTML 4.01 严格型 --> 
  <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" 
  "http://www.w3.org/TR/html4/strict.dtd"> 
   
  <!-- XHTML 1.0 严格型 --> 
  <!DOCTYPE html PUBLIC 
  "-//W3C//DTD XHTML 1.0 Strict//EN" 
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd"> 
   
  <!-- HTML 5 --> 
  <!DOCTYPE html> 
  ```

- 对于准标准模式，则可以通过使用过渡型（transitional）或框架集型（frameset）文档类型来触发，如下所示：

  ```html
  <!-- HTML 4.01 过渡型 --> 
  <!DOCTYPE HTML PUBLIC 
  "-//W3C//DTD HTML 4.01 Transitional//EN" 
  "http://www.w3.org/TR/html4/loose.dtd"> 
   
  <!-- HTML 4.01 框架集型 --> 
  <!DOCTYPE HTML PUBLIC 
  "-//W3C//DTD HTML 4.01 Frameset//EN" 
  "http://www.w3.org/TR/html4/frameset.dtd"> 
   
  <!-- XHTML 1.0 过渡型 --> 
  <!DOCTYPE html PUBLIC 
  "-//W3C//DTD XHTML 1.0 Transitional//EN" 
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 
   
  <!-- XHTML 1.0 框架集型 --> 
  <!DOCTYPE html PUBLIC 
  "-//W3C//DTD XHTML 1.0 Frameset//EN" 
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd"> 
  ```

#### 6、`<noscript>`元素

> 早期浏览器都面临一个特殊的问题，即当浏览器不支持 JavaScript 时如何让页面平稳地退化。对这个问题的最终解决方案就是创造一个<noscript>元素，用以在不支持 JavaScript 的浏览器中显示替代的内容

- 这个元素可以包含能够出现在文档<body>中的任何 HTML 元素—<script>元素除外。包含在<noscript>元素中的内容只有在下列情况下（符合上述任何一个条件，浏览器都会显示<noscript>中的内容。而在除此之外的其他情况下，浏览器不会呈现<noscript>中的内容）才会显示出来：
  - 浏览器不支持脚本； 
  - 浏览器支持脚本，但脚本被禁用。

- 比如下面这个页面会在脚本无效的情况下向用户显示一条消息。而在启用了脚本的浏览器中，用户永远也不
  会看到它—尽管它是页面的一部分

  ```html
  <html>  
    <head> 
      <title>Example HTML Page</title> 
      <script type="text/javascript" defer="defer" src="example1.js"></script> 
      <script type="text/javascript" defer="defer" src="example2.js"></script> 
    </head> 
    <body> 
      <noscript> 
        <p>本页面需要浏览器支持（启用）JavaScript。 
      </noscript> 
    </body> 
  </html>
  ```

#### 7、总结

- 把 JavaScript 插入到 HTML 页面中要使用<script>元素。使用这个元素可以把 JavaScript 嵌入到HTML 页面中，让脚本与标记混合在一起；也可以包含外部的 JavaScript 文件。而我们需要注意的地方有： 
  - 在包含外部 JavaScript 文件时，必须将src属性设置为指向相应文件的 URL。而这个文件既可以是与包含它的页面位于同一个服务器上的文件，也可以是其他任何域中的文件。
  - 所有<script>元素都会按照它们在页面中出现的先后顺序依次被解析。在不使用defer和async属性的情况下，只有在解析完前面<script>元素中的代码之后，才会开始解析后面<script>元素中的代码。
  - 由于浏览器会先解析完不使用defer属性的<script>元素中的代码，然后再解析后面的内容，所以一般应该把<script>元素放在页面最后，即主要内容后面，</body>标签前面。 
  - 使用defer属性可以让脚本在文档完全呈现之后再执行。延迟脚本总是按照指定它们的顺序执行。 使用async属性可以表示当前脚本不必等待其他脚本，也不必阻塞文档呈现。不能保证异步脚本按照它们在页面中出现的顺序执行。
- 使用<noscript>元素可以指定在不支持脚本的浏览器中显示的替代内容。但在启用了脚本
  的情况下，浏览器不会显示<noscript>元素中的任何内容。



## JavaScript基本概念

