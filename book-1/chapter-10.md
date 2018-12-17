## DOM扩展

#### 1、选择符API

- querySelector()方法

  - 接收一个 CSS 选择符，返回与该模式匹配的第一个元素，如果没有找到匹配的元素，返回 null

    ```js
    //取得 body 元素
    var body = document.querySelector("body");
    //取得 ID 为"myDiv"的元素
    var myDiv = document.querySelector("#myDiv");
    //取得类为"selected"的第一个元素
    var selected = document.querySelector(".selected");
    //取得类为"button"的第一个图像元素
    var img = document.body.querySelector("img.button");
    ```

  - 通过 Document 类型调用 querySelector()方法时，会在文档元素的范围内查找匹配的元素。而通过 Element 类型调用 querySelector()方法时，只会在该元素后代元素的范围内查找匹配的元素

  - 如果传入了不被支持的选择符，querySelector()会抛出错误

- querySelectorAll()方法

  - 接收一个 CSS 选择符，返回的一个 NodeList 的实例，包含匹配的所有元素，如果没有找到匹配的元素，NodeList 就是空的

  - 能够调用 querySelectorAll()方法的类型包括 Document、DocumentFragment 和 Element

    ```js
    //取得某<div>中的所有<em>元素（类似于 getElementsByTagName("em")）
    var ems = document.getElementById("myDiv").querySelectorAll("em");
    //取得类为"selected"的所有元素
    var selecteds = document.querySelectorAll(".selected");
    //取得所有<p>元素中的所有<strong>元素
    var strongs = document.querySelectorAll("p strong");
    ```

  - 要取得返回的 NodeList 中的每一个元素，可以使用 item()方法，也可以使用方括号语法

    ```js
    var i, len, strong;
    for (i=0, len=strongs.length; i < len; i++){
     	strong = strongs[i]; //或者 strongs.item(i)
     	strong.className = "important";
    }
    ```

  - 如果传入了浏览器不支持的选择符或者选择符中有语法错误，querySelectorAll()会抛出错误

- matchesSelector()方法

  - 接收一个参数，即 CSS 选择符，如果调用元素与该选择符匹配，返回 true；否则，返回 false

    ```js
    if (document.body.matchesSelector("body.page1")){
     	//true
    } 
    ```

  - 兼容性：

    - IE 9+通过 msMatchesSelector()支持该方法
    - Firefox 3.6+通过 mozMatchesSelector()支持该方法
    - Safari 5+和 Chrome 通过 webkitMatchesSelector()支持该方法

    ```js
    function matchesSelector(element, selector){
     if (element.matchesSelector){
     	return element.matchesSelector(selector);
     } else if (element.msMatchesSelector){
     	return element.msMatchesSelector(selector);
     } else if (element.mozMatchesSelector){
     	return element.mozMatchesSelector(selector);
     } else if (element.webkitMatchesSelector){
     	return element.webkitMatchesSelector(selector);
     } else {
     	throw new Error("Not supported.");
     }
    }
    if (matchesSelector(document.body, "body.page1")){
     	//执行操作
    } 
    ```

#### 2、元素遍历

- childElementCount：返回子元素（不包括文本节点和注释）的个数

- firstElementChild：指向第一个子元素；firstChild 的元素版

- lastElementChild：指向最后一个子元素；lastChild 的元素版

- previousElementSibling：指向前一个同辈元素；previousSibling 的元素版

- nextElementSibling：指向后一个同辈元素；nextSibling 的元素版

  ```js
  //跨浏览器遍历某元素的所有子元素的一般方法
  var i,
  len,
  child = element.firstChild;
  while(child != element.lastChild){
   	if (child.nodeType == 1){ //检查是不是元素
   		processChild(child);
   	}
  	child = child.nextSibling;
  } 
  //而使用 Element Traversal 新增的元素
  var i,
     len,
     child = element.firstElementChild;
    while(child != element.lastElementChild){
   		processChild(child); //已知其是元素
   		child = child.nextElementSibling;
  	}
  ```

#### 3、HTML5

- 与类相关的扩充

  - getElementsByClassName()方法：接收一个参数，即一个包含一或多个类名的字符串，返回带有指定类的所有元素的 NodeList

    ```js
    //取得所有类中包含"username"和"current"的元素，类名的先后顺序无所谓
    var allCurrentUsernames = document.getElementsByClassName("username current");
    //取得 ID 为"myDiv"的元素中带有类名"selected"的所有元素
    var selected = document.getElementById("myDiv").getElementsByClassName("selected");
    ```

  - classList 属性

    - 概念：一种操作类名的方式，可以让操作更简单也更安全，那就是为所有元素添加classList 属性。这个 classList 属性是新集合类型 DOMTokenList 的实例。与其他 DOM 集合类似，DOMTokenList 有一个表示自己包含多少元素的 length 属性，而要取得每个元素可以使用 item()方法，也可以使用方括号语法

    - 方法：

      ```js
      add(value)：将给定的字符串值添加到列表中。如果值已经存在，就不添加了
      contains(value)：表示列表中是否存在给定的值，如果存在则返回 true，否则返回 false
      remove(value)：从列表中删除给定的字符串
      toggle(value)：如果列表中已经存在给定的值，删除它；如果列表中没有给定的值，添加它
      ```

    - 有了 classList 属性，除非你需要全部删除所有类名，或者完全重写元素的 class 属性，否则也
      就用不到 className 属性了

      ```js
      //<div class="bd user disabled">...</div> 
      //删除"disabled"类
      div.classList.remove("disabled");
      //添加"current"类
      div.classList.add("current");
      //切换"user"类
      div.classList.toggle("user");
      //确定元素中是否包含既定的类名
      if (div.classList.contains("bd") && !div.classList.contains("disabled")){
       	//执行操作
      )
      //迭代类名
      for (var i=0, len=div.classList.length; i < len; i++){
       	doSomething(div.classList[i]);
      }
      ```

- 焦点管理

  - document.activeElement 属性：始终会引用 DOM 中当前获得了焦点的元素。元素获得焦点的方式有页面加载、用户输入（通常是通过按 Tab 键）和在代码中调用 focus()方法

    ```js
    var button = document.getElementById("myButton");
    button.focus();
    alert(document.activeElement === button); //true 
    ```

  - document.hasFocus()方法：用于确定文档是否获得了焦点

    ```js
    var button = document.getElementById("myButton"); 
    button.focus();
    alert(document.hasFocus()); //true 
    ```

- HTMLDocument的变化

  - Document 的readyState 属性：

    - loading，正在加载文档；
    - complete，已经加载完文档

    ```js
    if (document.readyState == "complete"){
      //执行操作
    }
    ```

  - 兼容模式：document 添加了一个名为 compatMode 的属性，为了告诉开发人员浏览器采用了哪种渲染模式

    ```js
    if (document.compatMode == "CSS1Compat"){
     	alert("Standards mode");//标准模式下，值等于"CSS1Compat"
    } else {
     	alert("Quirks mode");//混杂模式下，值等于"BackCompat"。
    } 
    ```

  -  head 属性：作为对 document.body 引用文档的元素的补充，引用文档的元素

    ```JS
    var head = document.head || document.getElementsByTagName("head")[0]; 
    ```

- 字符集属性

  - charset 属性：表示文档中实际使用的字符集，也可以用来指定新字符集。默认情况下，这个属性的值为"UTF-16"，但可以通过元素、响应头部或直接设置 charset 属性修改这个值

    ```js
    alert(document.charset); //"UTF-16"
    document.charset = "UTF-8"; 
    ```

  - defaultCharset属性：表示根据默认浏览器及操作系统的设置，当前文档默认的字符集应该是什么。如果文档没有使用默认的字符集，那 charset 和 defaultCharset 属性的值可能会不一样

    ```js
    if (document.charset != document.defaultCharset){
     	alert("Custom character set being used.");
    }
    ```

- 自定义数据属性

  - HTML5 规定可以为元素添加非标准的属性，但要添加前缀 data-，这些属性可以任意添加、随便命名，只要以 data-开头即可

    ```html
    <div id="myDiv" data-appId="12345" data-myname="Nicholas"></div> 
    ```

  - 添加了自定义属性之后，可以通过元素的 dataset 属性来访问自定义属性的值。dataset 属性的值是 DOMStringMap 的一个实例，也就是一个名值对儿的映射。在这个映射中，每个 data-name 形式
    的属性都会有一个对应的属性，只不过属性名没有 data-前缀（比如，自定义属性是 data-myname，
    那映射中对应的属性就是 myname）

    ```js
    //本例中使用的方法仅用于演示
    var div = document.getElementById("myDiv");
    //取得自定义属性的值
    var appId = div.dataset.appId;
    var myName = div.dataset.myname;
    //设置值
    div.dataset.appId = 23456;
    div.dataset.myname = "Michael";
    //有没有"myname"值呢？
    if (div.dataset.myname){ 
        alert("Hello, " + div.dataset.myname);
    } 
    ```

- 插入标记

  - innerHTML 属性

    - 在读模式下，innerHTML 属性返回与调用元素的所有子节点（包括元素、注释和文本节点）对应的 HTML 标记；

      ```html
      <div id="content">
       	<p>This is a <strong>paragraph</strong> with a list following it.</p>
       	<ul>
       		<li>Item 1</li>
       		<li>Item 2</li>
       		<li>Item 3</li>
       	</ul>
      </div>
      对于上面的<div>元素来说，它的 innerHTML 属性会返回如下字符串
      <p>This is a <strong>paragraph</strong> with a list following it.</p>
      <ul>
       	<li>Item 1</li>
       	<li>Item 2</li>
       	<li>Item 3</li>
      </ul> 
      ```

    - 在写模式下，innerHTML 会根据指定的值创建新的 DOM 树，然后用这个 DOM 树完全替换调用元素原先的所有子节点

      ```js
      div.innerHTML = "Hello world!"; 
      div.innerHTML = "Hello & welcome, <b>\"reader\"!</b>"; 
      //结果：
      //<div id="content">Hello &amp; welcome, <b>&quot;reader&quot;!</b></div> 
      ```

    - 不同浏览器返回的文本格式会有所不同。IE 和 Opera 会将所有标签转换为大写形式，而 Safari、Chrome 和 Firefox 则会原原本本地按照原先文档中（或指定这些标签时）的格式返回 HTML，包空格和缩进

    - 在大多数浏览器中，通过 innerHTML 插入`<script>`元素并不会执行其中的脚本

      ```js
      div.innerHTML = "<script defer>alert('hi');<\/script>"; //无效
      //下面这几行代码都可以正常执行
      div.innerHTML="_<script defer>alert('hi');<\/script>";
      div.innerHTML="<div>&nbsp;</div><script defer>alert('hi');<\/script>";
      div.innerHTML="<input type=\"hidden\"><script defer>alert('hi')<\/script>"; 
      ```

    - 大多数浏览器都支持以直观的方式通过 innerHTML 插入`<style>`元素

      ```js
      div.innerHTML="<style type=\"text/css\">body{background-color:red;</style>"
      //IE8 及更早版本中
      div.innerHTML="_<style type=\"text/css\">body{background-color:red;</style>"
      div.removeChild(div.firstChild);
      ```

    - 并不是所有元素都支持 innerHTML 属性。不支持 innerHTML 的元素有：

      ```html
      <col>、<colgroup>、<frameset>、<head>、<html>、<style>、<table>、<tbody>、<thead>、<tfoot>和<tr>
      ```

    - 无论什么时候，只要使用 innerHTML 从外部插入 HTML，都应该首先以可靠的方式处理 HTML。IE8 为此提供了 window.toStaticHTML()方法，这个方法接收一个参数，即一个 HTML 字符串；返回一个经过无害处理后的版本——从源 HTML 中删除所有脚本节点和事件处理程序属性

      ```js
      var text = "<a href=\"#\" onclick=\"alert('hi')\">Click Me</a>";
      var sanitized = window.toStaticHTML(text); //Internet Explorer 8 only
      alert(sanitized); //"<a href=\"#\">Click Me</a>"
      ```

  - outerHTML 属性

    - 在读模式下，outerHTML 返回调用它的元素及所有子节点的 HTML 标签

      ```html
      <div id="content">
       	<p>This is a <strong>paragraph</strong> with a list following it.</p>
       	<ul>
       		<li>Item 1</li>
       		<li>Item 2</li>
       		<li>Item 3</li>
       		</ul>
      </div> 
      对于上面的<div>元素来说，它的 outerHTML  属性会返回如下字符串
      <div id="content">
       	<p>This is a <strong>paragraph</strong> with a list following it.</p>
       	<ul>
       		<li>Item 1</li>
       		<li>Item 2</li>
       		<li>Item 3</li>
       		</ul>
      </div>
      ```

    - 在写模式下，outerHTML会根据指定的 HTML 字符串创建新的 DOM 子树，然后用这个 DOM 子树完全替换调用元素。

      ```js
      div.outerHTML = "<p>This is a paragraph.</p>"; 
      //这行代码完成的操作与下面这些 DOM 脚本代码一样
      var p = document.createElement("p");
      p.appendChild(document.createTextNode("This is a paragraph."));
      div.parentNode.replaceChild(p, div); 
      ```

  - insertAdjacentHTML()方法：它接收两个参数，插入位置和要插入的 HTML 文本

    - 第一个参数必须是下列值之一(这些值都必须是小写形式)：

      ```js
      "beforebegin"，在当前元素之前插入一个紧邻的同辈元素；
      "afterbegin"，在当前元素之下插入一个新的子元素或在第一个子元素之前再插入新的子元素；
      "beforeend"，在当前元素之下插入一个新的子元素或在最后一个子元素之后再插入新的子元素；
      "afterend"，在当前元素之后插入一个紧邻的同辈元素。
      ```

    - 第二个参数是一个 HTML 字符串（与 innerHTML 和 outerHTML的值相同），如果浏览器无法解析该字符串，就会抛出错误

      ```js
      //作为前一个同辈元素插入
      element.insertAdjacentHTML("beforebegin", "<p>Hello world!</p>");
      //作为第一个子元素插入
      element.insertAdjacentHTML("afterbegin", "<p>Hello world!</p>");
      //作为最后一个子元素插入
      element.insertAdjacentHTML("beforeend", "<p>Hello world!</p>");
      //作为后一个同辈元素插入
      element.insertAdjacentHTML("afterend", "<p>Hello world!</p>"); 
      ```

  - 内存与性能问题

    - 在使用 innerHTML、outerHTML 属性和 insertAdjacentHTML()方法时，最好先手工删除要被替换的元素的所有事件处理程序和 JavaScript 对象属性

    - 创建和销毁 HTML 解析器也会带来性能损失，所以最好能够将设置 innerHTML或 outerHTML 的次数控制在合理的范围内

      ```js
      var itemsHtml = "";
      for (var i=0, len=values.length; i < len; i++){
       	itemsHtml += "<li>" + values[i] + "</li>";
      }
      ul.innerHTML = itemsHtml; 
      ```

- scrollIntoView()方法

  - scrollIntoView()可以在所有 HTML 元素上调用，通过滚动浏览器窗口或某个容器元素，调用元素就可以出现在视口中
  - 如果给这个方法传入 true 作为参数，或者不传入任何参数，那么窗口滚动之后会让调用元素的顶部与视口顶部尽可能平齐
  - 如果传入 false 作为参数，调用元素会尽可能全部出现在视口中，（可能的话，调用元素的底部会与视口顶部平齐。）不过顶部不一定平齐

  ```js
  //让元素可见
  document.forms[0].scrollIntoView(); 
  ```

#### 4、专有扩展

- 文档模式

  - 文档模式决定了你可以使用哪个级别的 CSS，可以在 JavaScript 中使用哪些 API，以及如何对待文档类型（doctype）

  - 要强制浏览器以某种模式渲染页面，可以使用 HTTP 头部信息X-UA-Compatible，或通过等价的标签来设置

    ```html
    <meta http-equiv="X-UA-Compatible" content="IE=IEVersion">
    ```

  - 默认情况下，浏览器会通过文档类型声明来确定是使用最佳的可用文档模式，还是使用混杂模式，通过 document.documentMode 属性可以知道给定页面使用的是什么文档模式

    ```js
    var mode = document.documentMode; 
    ```

- children属性

  - 由于 IE9 之前的版本与其他浏览器在处理文本节点中的空白符时有差异，因此就出现了 children属性

  - 这个属性是 HTMLCollection 的实例，只包含元素中同样还是元素的子节点

  - 除此之外，children 属性与 childNodes 没有什么区别，即在元素只包含元素子节点时，这两个属性的值相同

    ```js
    var childCount = element.children.length;
    var firstChild = element.children[0];
    ```

- contains()方法

  - 调用 contains()方法的应该是祖先节点，也就是搜索开始的节点，这个方法接收一个参数，即要检测的后代节点。如果被检测的节点是后代节点，该方法返回 true；否则，返回 false

    ```js
    alert(document.documentElement.contains(document.body)); //true 
    ```

  -  compareDocumentPosition()也能够确定节点间的关系，这个方法用于确定两个节点间的关系，返回一个表示该关系的位掩码（ bitmask）

    ```js
    var result = document.documentElement.compareDocumentPosition(document.body);
    alert(!!(result & 16)); //16代表节点被包含（给定的节点是参考节点的后代）
    ```

  - 兼容性写法

    ```js
    function contains(refNode, otherNode){
     	if (typeof refNode.contains == "function" &&
     				(!client.engine.webkit || client.engine.webkit >= 522)){
     		return refNode.contains(otherNode);
     	} else if (typeof refNode.compareDocumentPosition == "function"){
     		return !!(refNode.compareDocumentPosition(otherNode) & 16);
     	} else {
     		var node = otherNode.parentNode;
     		do {
     			if (node === refNode){
     					return true;
     			} else {
     					node = node.parentNode;
     			}
     		} while (node !== null); 
            	return false;
     	}
    } 
    ```

- 插入文本
  - innerText 属性：可以操作元素中包含的所有文本内容，包括子文档树中的文本

    - 在通过innerText 读取值时，它会按照由浅入深的顺序，将子文档树中的所有文本拼接起来

      ```html
      <div id="content">
       	<p>This is a <strong>paragraph</strong> with a list following it.</p>
       	<ul>
      		<li>Item 1</li>
       		<li>Item 2</li>
       		<li>Item 3</li>
       	</ul>
      </div>
      对于这个例子中的<div>元素而言，其 innerText 属性会返回下列字符串：
      This is a paragraph with a list following it.
      Item 1
      Item 2
      Item 3 
      ```

    - 在通过innerText 写入值时，结果会删除元素的所有子节点，插入包含相应文本值的文本节点；也对文本中存在的 HTML 语法字符（小于号、大于号、引号及和号）进行了编码

      ```html
      div.innerText = "Hello & welcome, <b>\"reader\"!</b>"; 
      运行以上代码之后，会得到如下所示的结果
      <div id="content">
          Hello &amp;welcome,&lt;b&gt;&quot;reader&quot;!&lt;/b&gt;
      </div> 
      ```

    - 设置 innerText 永远只会生成当前节点的一个子文本节点，而为了确保只生成一个子文本节点，就必须要对文本进行 HTML 编码。利用这一点，可以通过 innerText 属性过滤掉 HTML 标签。方法是将 innerText 设置为等于 innerText，这样就可以去掉所有 HTML 标签

      ```JS
      div.innerText = div.innerText; 
      ```

    - 由于不同浏览器处理空白符的方式不同，因此输出的文本可能会也可能不会包含原始 HTML 代码中的缩进

    - 兼容性：

      ```js
      function getInnerText(element){
       	return (typeof element.textContent == "string") ?
       	element.textContent : element.innerText;
      }
      function setInnerText(element, text){
       	if (typeof element.textContent == "string"){
       		element.textContent = text;
       	} else {
       		element.innerText = text;
       	}
      } 
      setInnerText(div, "Hello world!");
      alert(getInnerText(div)); //"Hello world!" 
      ```

  - outerText 属性

    - 在读取文本值时，outerText 与 innerText 的结果完全一样。但在写模式下，outerText 就完全不
      同了：outerText 不只是替换调用它的元素的子节点，而是会替换整个元素（包括子节点）

      ```js
      div.outerText = "Hello world!"; 
      //这行代码实际上相当于如下两行代码：
      var text = document.createTextNode("Hello world!");
      div.parentNode.replaceChild(text, div); 
      ```

    - 本质上，新的文本节点会完全取代调用 outerText 的元素。此后，该元素就从文档中被删除，无法访问

- 滚动

  - 是对 HTMLElement 类型的扩展，因此在所有元素中都可以调用：
    - scrollIntoViewIfNeeded(alignCenter)：只在当前元素在视口中不可见的情况下，才滚动浏览器窗口或容器元素，最终让它可见。如果当前元素在视口中可见，这个方法什么也不做。如果将可选的 alignCenter 参数设置为 true，则表示尽量将元素显示在视口中部（垂直方向）。Safari 和 Chrome 实现了这个方法
    - scrollByLines(lineCount)：将元素的内容滚动指定的行高，lineCount 值可以是正值，也可以是负值。Safari 和 Chrome 实现了这个方法
    - scrollByPages(pageCount)：将元素的内容滚动指定的页面高度，具体高度由元素的高度决定。Safari 和 Chrome 实现了这个方法

  - scrollIntoView()和 scrollIntoViewIfNeeded()的作用对象是元素的容器，而 scrollByLines()和scrollByPages()影响的则是元素自身

    ```js
    //将页面主体滚动 5 行
    document.body.scrollByLines(5); 
    //在当前元素不可见的时候，让它进入浏览器的视口
    document.images[0].scrollIntoViewIfNeeded();
    //将页面主体往回滚动 1 页
    document.body.scrollByPages(-1);
    由于 scrollIntoView()是唯一一个所有浏览器都支持的方法，因此还是这个方法最常用。
    ```

#### 5、总结

- 虽然 DOM 为与 XML 及 HTML 文档交互制定了一系列核心 API，但仍然有几个规范对标准的 DOM进行了扩展。这些扩展中有很多原来是浏览器专有的，但后来成为了事实标准，于是其他浏览器也都提供了相同的实现。本章介绍的三个这方面的规范如下：
  - Selectors API，定义了两个方法，让开发人员能够基于 CSS 选择符从 DOM 中取得元素，这两个方法是 querySelector()和 querySelectorAll()
  - Element Traversal，为 DOM 元素定义了额外的属性，让开发人员能够更方便地从一个元素跳到另一个元素。之所以会出现这个扩展，是因为浏览器处理 DOM 元素间空白符的方式不一样
  - HTML5，为标准的 DOM 定义了很多扩展功能。其中包括在 innerHTML 属性这样的事实标准基础上提供的标准定义，以及为管理焦点、设置字符集、滚动页面而规定的扩展 API
- 虽然目前 DOM 扩展的数量还不多，但随着 Web 技术的发展，相信一定还会涌现出更多扩展来。很多浏览器都在试验专有的扩展，而这些扩展一旦获得认可，就能成为“伪”标准，甚至会被收录到规范的更新版本中