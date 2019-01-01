## DOM2和DOM3

#### 1、DOM变化

- DOM各版本

  - DOM1主要定义的是 HTML 和 XML 文档的底层结构

  - DOM2 和 DOM3 级则在这个结构的基础上引入了更多的交互能力，也支持了更高级的 XML 特性

    - DOM2 级核心（DOM Level 2 Core）：在 1 级核心基础上构建，为节点添加了更多方法和属性
    - DOM2 级视图（DOM Level 2 Views）：为文档定义了基于样式信息的不同视图
    - DOM2 级事件（DOM Level 2 Events）：说明了如何使用事件与 DOM 文档交互
    - DOM2 级样式（DOM Level 2 Style）：定义了如何以编程方式来访问和改变 CSS 样式信息
    - DOM2 级遍历和范围（DOM Level 2 Traversal and Range）：引入了遍历 DOM 文档和选择其特定部分的新接口
    - DOM2 级 HTML（DOM Level 2 HTML）：在 1 级 HTML 基础上构建，添加了更多属性、方法和新接口

  - 可以通过下列代码来确定浏览器是否支持这些 DOM 模块

    ```js
    var supportsDOM2Core = document.implementation.hasFeature("Core", "2.0");
    var supportsDOM3Core = document.implementation.hasFeature("Core", "3.0");
    var supportsDOM2HTML = document.implementation.hasFeature("HTML", "2.0");
    var supportsDOM2Views = document.implementation.hasFeature("Views", "2.0");
    var supportsDOM2XML = document.implementation.hasFeature("XML", "2.0"); 
    ```

- 针对XML命名空间的变化

  - XML命名空间

     - 命名空间要使用 xmlns 特性来指定，XHTML 的命名空间是 http://www.w3.org/1999/xhtm

       ```xml
        <html xmlns="http://www.w3.org/1999/xhtml">
          	<head>
          		<title>Example XHTML page</title>
          	</head>
          	<body>
          		Hello world!
          	</body>
         </html> 
       ```

     - 要想明确地为 XML命名空间创建前缀，可以使用 xmlns 后跟冒号，再后跟前缀

       ```xml
       <xhtml:html xmlns:xhtml="http://www.w3.org/1999/xhtml">
        	<xhtml:head>
        		<xhtml:title>Example XHTML page</xhtml:title>
        	</xhtml:head>
        	<xhtml:body>
        		Hello world!
        	</xhtml:body>
       </xhtml:html> 
       ```

     - 有时候为了避免不同语言间的冲突，也需要使用命名空间来限定特性

       ```xml
       <xhtml:html xmlns:xhtml="http://www.w3.org/1999/xhtml">
        	<xhtml:head>
       	 	<xhtml:title>Example XHTML page</xhtml:title>
        	</xhtml:head>
        	<xhtml:body xhtml:class="home">
        		Hello world!
        	</xhtml:body>
       </xhtml:html>
       ```

     - 在混合使用两种语言的情况下，命名空间的用处就非常大了

       ```xml
       <html xmlns="http://www.w3.org/1999/xhtml">
        	<head>
        		<title>Example XHTML page</title>
        	</head>
        	<body>
        		<svg xmlns="http://www.w3.org/2000/svg" version="1.1"
        			viewBox="0 0 100 100" style="width:100%; height:100%">
        			<rect x="0" y="0" width="100" height="100" style="fill:red"/>
        		</svg>
        	</body>
       </html> 
       ```

  - Node 类型的变化

     -  DOM2 级中，Node 类型包含下列特定于命名空间的属性

       ```xml
       localName：不带命名空间前缀的节点名称。
       namespaceURI：命名空间 URI 或者（在未指定的情况下是）null。
       prefix：命名空间前缀或者（在未指定的情况下是）null
       当节点使用了命名空间前缀时，其 nodeName 等于 prefix+":"+ localName
       <html xmlns="http://www.w3.org/1999/xhtml">
        	<head>
        		<title>Example XHTML page</title>
        	</head>
        	<body>
        		<s:svg xmlns:s="http://www.w3.org/2000/svg" version="1.1"
        			viewBox="0 0 100 100" style="width:100%; height:100%">
        			<s:rect x="0" y="0" width="100" height="100"style="fill:red"/>
        		</s:svg>
        	</body>
       </html> 
       ```

     - DOM3 级在此基础上更进一步，又引入了下列与命名空间有关的方法

       ```js
       isDefaultNamespace(namespaceURI)：在指定的 namespaceURI 是当前节点的默认命名空间的情况下返回 true。
       lookupNamespaceURI(prefix)：返回给定 prefix 的命名空间。
       lookupPrefix(namespaceURI)：返回给定 namespaceURI 的前缀
       //针对前面的例子，可以执行下列代码
       document.body.isDefaultNamespace("http://www.w3.org/1999/xhtml");//true
       //假设 svg 中包含着对<s:svg>的引用
       alert(svg.lookupPrefix("http://www.w3.org/2000/svg")); //"s"
       alert(svg.lookupNamespaceURI("s")); //"http://www.w3.org/2000/svg" 
       ```

  - Document 类型的变化

     - createElementNS(namespaceURI, tagName)：使用给定的 tagName 创建一个属于命名空间 namespaceURI 的新元素

     - createAttributeNS(namespaceURI, attributeName)：使用给定的 attributeName 创建一个属于命名空间 namespaceURI 的新特性

     - getElementsByTagNameNS(namespaceURI, tagName)：返回属于命名空间 namespaceURI的 tagName 元素的 NodeList

       ```js
       //创建一个新的 SVG 元素
       var svg = document.createElementNS("http://www.w3.org/2000/svg","svg");
       //创建一个属于某个命名空间的新特性
       var att = document.createAttributeNS("http://www.somewhere.com", "random");
       //取得所有 XHTML 元素
       var elems = document.getElementsByTagNameNS("http://www.w3.org/1999/xhtml", "*");
       ```

  - Element 类型的变化

     - getAttributeNS(namespaceURI,localName)：取得属于命名空间 namespaceURI 且名为localName 的特性
     - getAttributeNodeNS(namespaceURI,localName)：取得属于命名空间 namespaceURI 且名为 localName 的特性节点
     - getElementsByTagNameNS(namespaceURI, tagName)：返回属于命名空间 namespaceURI的 tagName 元素的 NodeList
     - hasAttributeNS(namespaceURI,localName)：确定当前元素是否有一个名为 localName的特性，而且该特性的命名空间是 namespaceURI。注意，“DOM2 级核心”也增加了一个hasAttribute()方法，用于不考虑命名空间的情况
     - removeAttriubteNS(namespaceURI,localName)：删除属于命名空间 namespaceURI 且名为 localName 的特性
     - setAttributeNS(namespaceURI,qualifiedName,value)：设置属于命名空间 namespaceURI 且名为 qualifiedName 的特性的值为 valuesetAttributeNodeNS(attNode)：设置属于命名空间 namespaceURI 的特性节点

  - NamedNodeMap 类型的变化

     - getNamedItemNS(namespaceURI,localName)：取得属于命名空间 namespaceURI 且名为
       localName 的项
     - removeNamedItemNS(namespaceURI,localName)：移除属于命名空间 namespaceURI 且名
       为 localName 的项
     - setNamedItemNS(node)：添加 node，这个节点已经事先指定了命名空间信息

- 其他方面的变化

  - DocumentType 类型的变化

    - DocumentType 类型新增了 3 个属性：publicId、systemId 和 internalSubset

      ```html
      <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
       "http://www.w3.org/TR/html4/strict.dtd"
      [<!ELEMENT name (#PCDATA)>] >
      ```

    - 前两个属性表示的是文档类型声明中的两个信息段，这两个信息段在 DOM1 级中是没有办法访问到

      ```js
      alert(document.doctype.publicId);//-//W3C//DTD HTML 4.01//EN
      alert(document.doctype.systemId); //"http://www.w3.org/TR/html4/strict.dtd
      ```

    - 最后一个属性 internalSubset，用于访问包含在文档类型声明中的额外定义

      ```js
      document.doctype.internalSubset//<!ELEMENT name (#PCDATA)>
      ```

  - Document 类型的变化

    - importNode()：从一个文档中取得一个节点，然后将其导入到另一个文档，使其成为这个文档结构的一部分；接受两个参数，要复制的节点和一个表示是否复制子节点的布尔值；返回的结果是原来节点的副本，但能够在当前文档中使用

      ```js
      var newNode = document.importNode(oldNode, true); //导入节点及其所有子节点
      document.body.appendChild(newNode);
      ```

    - defaultView 的属性：保存着一个指针，指向拥有给定文档的窗口（或框架），在 IE 中有一个等价的属性名叫 parentWindow（Opera 也支持这个属性）

      ```js
      var parentWindow = document.defaultView || document.parentWindow; 
      ```

    - document.implementation 对象的createDocumentType()方法：用于创建一个新的DocumentType节点，接受 3 个参数，文档类型名称、publicId、systemId

      ```js
      var doctype = document.implementation.createDocumentType("html",
       "-//W3C//DTD HTML 4.01//EN",
       "http://www.w3.org/TR/html4/strict.dtd"); 
      ```

    - document.implementation 对象的createDocument()方法：创建新文档，法接受 3 个参数，针对文档中元素的 namespaceURI、文档元素的标签名、新文档的文档类型

      ```js
      //创建一个空的新 XML 文档
      var doc = document.implementation.createDocument("", "root", null); 
      //创建一个 XHTML 文档
      var doctype = document.implementation.createDocumentType("html",
       " -//W3C//DTD XHTML 1.0 Strict//EN",
       "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd");
      var doc = document.implementation.createDocument("http://www.w3.org/1999/xhtml",
      "html", doctype); 
      ```

    - document.implementation对象的createHTMLDocument()方法：创建一个完整的 HTML 文档，包括`<html>、<head>、<title>、<body>`元素；这个方法只接受一个参数，即新创建文档的标题题（放在`<title>`里的字符串），返回新的 HTML 文档

      ```js
      var htmldoc = document.implementation.createHTMLDocument("New Doc");
      alert(htmldoc.title); //"New Doc"
      alert(typeof htmldoc.body); //"object" 
      ```

  - Node 类型的变化

    - isSupported()方法：法用于确定当前节点具有什么能力，这个方法也接受相同的两个参数：特性名和特性版本号，如果浏览器实现了相应特性，而且能够基于给定节点执行该特性，就返回 true

      ```js
      if (document.body.isSupported("HTML", "2.0")){
       //执行只有"DOM2 级 HTML"才支持的操作
      } 
      ```

    - isSameNode()和 isEqualNode()方法：接受一个节点参数，并在传入节点与引用的节点相同或相等时返回 true；所谓相同，指的是两个节点引用的是同一个对象。所谓相等，指的是两个节点是相同的类型，具有相等的属性（nodeName、nodeValue，等等），而且它们的 attributes 和 childNodes 属性也相等（相同位置包含相同的值）

      ```js
      var div1 = document.createElement("div");
      div1.setAttribute("class", "box");
      var div2 = document.createElement("div");
      div2.setAttribute("class", "box");
      alert(div1.isSameNode(div1)); //true
      alert(div1.isEqualNode(div2)); //true
      alert(div1.isSameNode(div2)); //false
      ```

    - setUserData()方法：会将数据指定给节点，它接受 3 个参数：要设置的键、实际的数据（可以是任何数据类型）和处理函数；处理函数会在带有数据的节点被复制、删除、重命名或引入一个文档时
      调用，因而你可以事先决定在上述操作发生时如何处理用户数据。处理函数接受 5 个参数：表示操作类型的数值（1 表示复制，2 表示导入，3 表示删除，4 表示重命名）、数据键、数据值、源节点和目标节点。在删除节点时，源节点是 null；除在复制节点时，目标节点均为 null

      ```js
      //以下代码可以将数据指定给一个节点
      document.body.setUserData("name", "Nicholas", function(){}); 
      //然后，使用 getUserData()并传入相同的键，就可以取得该数据
      var value = document.body.getUserData("name"); 
      //在函数内部，你可以决定如何存储数据
      var div = document.createElement("div");
      div.setUserData("name", "Nicholas", function(operation, key, value, src, dest){
       if (operation == 1){
       dest.setUserData(key, value, function(){}); } 
      });
      var newDiv = div.cloneNode(true);
      alert(newDiv.getUserData("name")); //"Nicholas" 
      ```

  - 框架的变化

    - 框架和内嵌框架分别用 HTMLFrameElement 和 HTMLIFrameElement 表示，它们在 DOM2级中都有了一个新属性，名叫 contentDocument：包含一个指针，指向表示框架内容的文档对象

      ```js
      var iframe = document.getElementById("myIframe");
      var iframeDoc = iframe.contentDocument; //在 IE8 以前的版本中无效
      ```

    - IE8 之前不支持框架中的 contentDocument 属性，但支持一个名叫 contentWindow 的属性，该属性返回框架的 window 对象，而这个 window 对象又有一个 document 属性

      ```js
      var iframe = document.getElementById("myIframe");
      var iframeDoc = iframe.contentDocument || iframe.contentWindow.document;
      ```

#### 2、样式

- 概念：

  - 在 HTML 中定义样式的方式有 3 种：通过`<link/>`元素包含外部样式表文件、使用`<style/>`元素
    定义嵌入式样式，以及使用 style 特性定义针对特定元素的样式

  - 确定浏览器是否支持 DOM2 级定义的 CSS 能力：

    ```js
    var supportsDOM2CSS = document.implementation.hasFeature("CSS", "2.0");
    var supportsDOM2CSS2 = document.implementation.hasFeature("CSS2", "2.0");
    ```

- 访问元素的样式

  - 概念

    - 任何支持 style 特性的 HTML 元素在 JavaScript 中都有一个对应的 style 属性；

    - 对于使用短划线（分隔不同的词汇，例如 background-image）的 CSS 属性名，必须将其转换成驼峰大小写形式，才能通过 JavaScript 来访问；

    - float 是 JavaScript 中的保留字，因此不能用作属性名，应该转为cssFloat，IE支持的则是 styleFloat

    - 只要取得一个有效的 DOM 元素的引用，就可以随时使用 JavaScript 为其设置样式

      ```js
      var myDiv = document.getElementById("myDiv");
      //设置背景颜色
      myDiv.style.backgroundColor = "red";
      //改变大小
      myDiv.style.width = "100px";
      myDiv.style.height = "200px";
      //指定边框
      myDiv.style.border = "1px solid black"; 
      ```

    - 通过 style 对象同样可以取得在 style 特性中指定的样式

      ```js
      //<div id="myDiv" style="background-color:blue;width:10px;height:25px"></div>
      alert(myDiv.style.backgroundColor); //"blue"
      alert(myDiv.style.width); //"10px"
      alert(myDiv.style.height); //"25px"
      ```

    - 如果没有为元素设置 style 特性，那么 style 对象中可能会包含一些默认的值，但这些值并不能准确地反映该元素的样式信息

  - DOM 样式属性和方法
  - 计算的样式

- 操作样式表
  - CSS 规则
  - 创建规则
  - 删除规则

- 元素大小
  - 偏移量
  - 客户区大小
  -  滚动大小
  -  确定元素大小

#### 3、遍历

- NodeIterator
- TreeWalker

#### 4、范围

- DOM中的范围
  - 用 DOM 范围实现简单选择
  - 用 DOM 范围实现复杂选择
  - 操作 DOM 范围中的内容
  - 插入 DOM 范围中的内容
  - 折叠 DOM 范围
  - 比较 DOM 范围
  - 复制 DOM 范围
  - 清理 DOM 范围
- IE8 及更早版本中的范围
  - 用 IE 范围实现简单的选择
  - 使用 IE 范围实现复杂的选择
  - 操作 IE 范围中的内容
  - 折叠 IE 范围
  - 比较 IE 范围
  - 复制 IE 范围

#### 5、总结