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

    - “DOM2级样式”规范还为 style 对象定义了一些属性和方法
      - cssText：如前所述，通过它能够访问到 style 特性中的 CSS 代码
      - length：应用给元素的 CSS 属性的数量
      - parentRule：表示 CSS 信息的 CSSRule 对象。本节后面将讨论 CSSRule 类型
      - getPropertyCSSValue(propertyName)：返回包含给定属性值的 CSSValue 对象
      - getPropertyPriority(propertyName)：如果给定的属性使用了!important 设置，则返回
        "important"；否则，返回空字符串。
      - getPropertyValue(propertyName)：返回给定属性的字符串值
      - item(index)：返回给定位置的 CSS 属性的名称
      - removeProperty(propertyName)：从样式中删除给定属性
      - setProperty(propertyName,value,priority)：将给定属性设置为相应的值，并加上优先权标志（"important"或者一个空字符串）

  - 计算的样式

    - getComputedStyle()方法：两个参数，要取得计算样式的元素和一个伪元素字符串（例如":after"）如果不需要伪元素信息，第二个参数可以是 null。getComputedStyle()方法返回一个 CSSStyleDeclaration 对象（与 style 属性的类型相同），其中包含当前元素的所有计算的样式

      ```html
      <!DOCTYPE html>
      <html>
      <head>
      	<title>Computed Styles Example</title>
       	<style type="text/css">
       		#myDiv {
       			background-color: blue;
       			width: 100px;
       			height: 200px;
       		}
       	</style>
      </head>
      <body>
       	<div id="myDiv" style="background-color: red; border: 1px solid black">	   </div>
          <script>
          var myDiv = document.getElementById("myDiv");
      	var computedStyle = document.defaultView.getComputedStyle(myDiv, null);
      	alert(computedStyle.backgroundColor); // "red"
      	alert(computedStyle.width); // "100px"
      	alert(computedStyle.height); // "200px"
      	alert(computedStyle.border); // 在某些浏览器中是"1px solid black"
         	</script>
      </body>
      </html> 
      ```

    - 在 IE 中，每个具有 style 属性的元素还有一个 currentStyle 属性。这个属性是 CSSStyleDeclaration 的实例，包含当前元素全部计算后的样式

      ```js
      var myDiv = document.getElementById("myDiv");
      var computedStyle = myDiv.currentStyle;
      alert(computedStyle.backgroundColor); //"red"
      alert(computedStyle.width); //"100px"
      alert(computedStyle.height); //"200px"
      alert(computedStyle.border); //undefined 
      ```

- 操作样式表

  - 检测浏览器是否支持 DOM2 级样式表

    ```js
    var supportsDOM2StyleSheets =
    document.implementation.hasFeature("StyleSheets", "2.0");
    ```

  - CSSStyleSheet 继承自 StyleSheet，后者可以作为一个基础接口来定义非 CSS 样式表。从StyleSheet 接口继承而来的属性如下：

    - disabled：表示样式表是否被禁用的布尔值。这个属性是可读/写的，将这个值设置为 true 可以禁用样式表
    - href：如果样式表是通过包含的，则是样式表的 URL；否则，是 null
    - media：当前样式表支持的所有媒体类型的集合。与所有 DOM 集合一样，这个集合也有一个length 属性和一个 item()方法。也可以使用方括号语法取得集合中特定的项。如果集合是空列表，表示样式表适用于所有媒体。在 IE 中，media 是一个反映`<link>`和`<style>`元素media特性值的字符串
    - ownerNode：指向拥有当前样式表的节点的指针，样式表可能是在 HTML 中通过`<link>`或`<style/>`引入的（在 XML 中可能是通过处理指令引入的）。如果当前样式表是其他样式表通过
      @import 导入的，则这个属性值为 null。IE 不支持这个属性
    - parentStyleSheet：在当前样式表是通过@import 导入的情况下，这个属性是一个指向导入它的样式表的指针
    - title：ownerNode 中 title 属性的值
    - type：表示样式表类型的字符串。对 CSS 样式表而言，这个字符串是"type/css"
    - cssRules：样式表中包含的样式规则的集合。IE 不支持这个属性，但有一个类似的 rules 属性。
    - ownerRule：如果样式表是通过@import 导入的，这个属性就是一个指针，指向表示导入的规则；否则，值为 null。IE 不支持这个属性
    - deleteRule(index)：删除 cssRules 集合中指定位置的规则。IE 不支持这个方法，但支持一个类似的 removeRule()方法
    - insertRule(rule,index)：向 cssRules 集合中指定的位置插入 rule 字符串。IE 不支持这个方法，但支持一个类似的 addRule()方法

- 元素大小
  - 偏移量

    - offsetHeight：元素在垂直方向上占用的空间大小，以像素计。包括元素的高度、（可见的）水平滚动条的高度、上边框高度和下边框高度
    - offsetWidth：元素在水平方向上占用的空间大小，以像素计。包括元素的宽度、（可见的）垂直滚动条的宽度、左边框宽度和右边框宽度
    - offsetLeft：元素的左外边框至包含元素的左内边框之间的像素距离
    - offsetTop：元素的上外边框至包含元素的上内边框之间的像素距离

    ```js
    //取得元素的左偏移量
    function getElementLeft(element){
     	var actualLeft = element.offsetLeft;
     	var current = element.offsetParent;
     	while (current !== null){
     		actualLeft += current.offsetLeft;
     		current = current.offsetParent;
     	}
     	return actualLeft;
    }
    //取得元素的上偏移量
    function getElementTop(element){
     	var actualTop = element.offsetTop;
     	var current = element.offsetParent;
     	while (current !== null){
     		actualTop += current. offsetTop;
     		current = current.offsetParent;
     	}
     	return actualTop;
    }
    ```

  - 客户区大小

    - clientWidth 属性是元素内容区宽度加上左右内边距宽度
    - clientHeight 属性是元素内容区高度加上上下内边距高度
    - 客户区大小就是元素内部的空间大小，因此滚动条占用的空间不计算在内

    ```js
    function getViewport(){
     	if (document.compatMode == "BackCompat"){
     		return {
     			width: document.body.clientWidth,
     			height: document.body.clientHeight
     		};
     		} else {
     			return {
     				width: document.documentElement.clientWidth,
     				height: document.documentElement.clientHeight
     			};
     		}
    } 
    ```

  - 滚动大小

    - scrollHeight：在没有滚动条的情况下，元素内容的总高度

    - scrollWidth：在没有滚动条的情况下，元素内容的总宽度

    - scrollLeft：被隐藏在内容区域左侧的像素数。通过设置这个属性可以改变元素的滚动位置

    - scrollTop：被隐藏在内容区域上方的像素数。通过设置这个属性可以改变元素的滚动位置

    - IE（在标准模式）中 scrollWidth 和 scrollHeight 等于文档内容区域的大小，而 clientWidth 和 clientHeight 等于视口大小

    - 在确定文档的总高度时（包括基于视口的最小高度时），必须取得 scrollWidth/clientWidth 和
      scrollHeight/clientHeight 中的最大值，才能保证在跨浏览器的环境下得到精确的结果

      ```js
      var docHeight = Math.max(document.documentElement.scrollHeight,
       document.documentElement.clientHeight);
      var docWidth = Math.max(document.documentElement.scrollWidth,
       document.documentElement.clientWidth);
      ```

    - 对于运行在混杂模式下的 IE，则需要用 document.body 代替 document.documentElement

    - 通过 scrollLeft 和 scrollTop 属性既可以确定元素当前滚动的状态，也可以设置元素的滚动位
      置

      ```js
      function scrollToTop(element){
       	if (element.scrollTop != 0){
       		element.scrollTop = 0;
       	}
      }
      ```

    - 

  - 确定元素大小

    - 每个元素都有一个 getBoundingClientRect()方法，返回会一个矩形对象，包含 4 个属性：left、top、right 和 bottom

      ```js
      function getBoundingClientRect(element){
          var scrollTop = document.documentElement.scrollTop;
       	var scrollLeft = document.documentElement.scrollLeft;
      
          if (element.getBoundingClientRect){
              if (typeof arguments.callee.offset != "number"){
                  var scrollTop = document.documentElement.scrollTop;
                  var temp = document.createElement("div");
                  temp.style.cssText = "position:absolute;left:0;top:0;";
                  document.body.appendChild(temp);
                  arguments.callee.offset = -temp.getBoundingClientRect().top - 				scrollTop;
                  document.body.removeChild(temp);
                  temp = null;
       		}
       		var rect = element.getBoundingClientRect();
       		var offset = arguments.callee.offset;
       		return {
       			left: rect.left + offset,
       			right: rect.right + offset,
       			top: rect.top + offset,
       			bottom: rect.bottom + offset
       		};
          } else {
       		var actualLeft = getElementLeft(element);
       		var actualTop = getElementTop(element);
       		return {
       			left: actualLeft - scrollLeft,
       			right: actualLeft + element.offsetWidth - scrollLeft,
       			top: actualTop - scrollTop,
       			bottom: actualTop + element.offsetHeight - scrollTop
       		}
       	} 
      } 
      ```

#### 3、遍历

- 检测浏览器对 DOM2 级遍历能力的支持情况

  ```js
  var supportsTraversals = document.implementation.hasFeature("Traversal", "2.0");
  var supportsNodeIterator = (typeof document.createNodeIterator == "function");
  var supportsTreeWalker = (typeof document.createTreeWalker == "function");
  ```

- NodeIterator

  - 可以使用 document.createNodeIterator()方法创建它的新实例，这个方法接受下列 4 个参数

    - root：想要作为搜索起点的树中的节点
    - whatToShow：表示要访问哪些节点的数字代码
    - filter：是一个 NodeFilter 对象，或者一个表示应该接受还是拒绝某种特定节点的函数
    - entityReferenceExpansion：布尔值，表示是否要扩展实体引用。这个参数在 HTML 页面中没有用，因为其中的实体引用不能扩展。

  - 举例：

    ```js
    var iterator = document.createNodeIterator(document, NodeFilter.SHOW_ALL,
     null, false); 
    ```

  - 两个主要方法是 nextNode()和 previousNode()，在深度优先的 DOM 子树遍历中，nextNode()方法用于向前前进一步，而 previousNode()用于向后后退一步。由于 nextNode()和 previousNode()方法都基于 NodeIterator 在 DOM 结构中的内部指针工作，所以 DOM 结构的变化会反映在遍历的结果中。

    ```html
    <div id="div1">
     	<p><b>Hello</b> world!</p>
     	<ul>
     		<li>List item 1</li>
     		<li>List item 2</li>
     		<li>List item 3</li>
     	</ul>
    </div>
    <script>
    	var div = document.getElementById("div1");
    	var iterator = document.createNodeIterator(div, NodeFilter.SHOW_ELEMENT,
     		null, false);
    	var node = iterator.nextNode();
    	while (node !== null) {
     		alert(node.tagName); //输出标签名
     		node = iterator.nextNode();
    	}
    </script>
    ```

- TreeWalker

  - 更高级的版本，除了包括 nextNode()和 previousNode()在内的相同的功能之外，这个类型还提供了下列用于在不同方向上遍历 DOM 结构的方法
    - parentNode()：遍历到当前节点的父节点
    - firstChild()：遍历到当前节点的第一个子节点；
    - lastChild()：遍历到当前节点的最后一个子节点
    - nextSibling()：遍历到当前节点的下一个同辈节点
    - previousSibling()：遍历到当前节点的上一个同辈节点

#### 4、范围（range）

- DOM中的范围

  - 创建范围：使用createRange()方法

    ```js
    //检测浏览器是否支持范围
    var supportsRange = document.implementation.hasFeature("Range", "2.0");
    var alsoSupportsRange = (typeof document.createRange == "function");
    //创建
    var range = document.createRange(); 
    ```

  - Range 类型的实例的属性，在把范围放到文档中特定的位置时，这些属性都会被赋值
    - startContainer：包含范围起点的节点（即选区中第一个节点的父节点）
    - startOffset：范围在 startContainer 中起点的偏移量。如果 startContainer 是文本节点、注释节点或 CDATA 节点，那么 startOffset 就是范围起点之前跳过的字符数量。否则，startOffset 就是范围中第一个子节点的索引
    - endContainer：包含范围终点的节点（即选区中最后一个节点的父节点）
    - endOffset：范围在 endContainer 中终点的偏移量（与 startOffset 遵循相同的取值规则）
    - commonAncestorContainer：startContainer 和 endContainer 共同的祖先节点在文档树中位置最深的那个

  - Range类型的实例的方法：
    - 用 DOM 范围实现简单选择：使用 selectNode()或 selectNodeContents()
    - 用 DOM 范围实现复杂选择：使用 setStart()和 setEnd()方法
    - 操作 DOM 范围中的内容：够从文档中删除范围所包含的内容使用deleteContents()；从文档中移除范围选区使用extractContents()方法；使用 cloneContents()创建范围对象的一个副本，然后在文档的其他地方插入该副本
    - 插入 DOM 范围中的内容：使用 insertNode()方法
    - 折叠 DOM 范围：使用 collapse()方法
    - 比较 DOM 范围：使用 compareBoundaryPoints()方法
    - 复制 DOM 范围：使用 cloneRange()方法
    - 清理 DOM 范围：调用 detach()方法

- IE8 及更早版本中的范围
  - 用 IE 范围实现简单的选择：使用范围的 findText()方法找到第一次出现的给定文本，并将范围移过来以环绕该文本；IE 中与 DOM 中的 selectNode()方法最接近的方法是 moveToElementText()，这个方法接受一个 DOM 元素，并选择该元素的所有文本，包括 HTML 标签
  - 使用 IE 范围实现复杂的选择：以特定的增量向四周移动范围。为此，IE 提供了 4 个方法，move()、moveStart()、moveEnd()和 expand()
  - 操作 IE 范围中的内容：使用 text 属性或 pasteHTML()方法
  - 折叠 IE 范围：collapse()方法
  - 比较 IE 范围：compareEndPoints()方法
  - 复制 IE 范围：duplicate()方法

#### 5、总结

​	DOM2 级规范定义了一些模块，用于增强 DOM1 级。“DOM2 级核心”为不同的 DOM 类型引入了一些与 XML 命名空间有关的方法。这些变化只在使用 XML 或 XHTML 文档时才有用；对于 HTML 文档没有实际意义。除了与 XML 命名空间有关的方法外，“DOM2 级核心”还定义了以编程方式创建Document 实例的方法，也支持了创建 DocumentType 对象。

​	“DOM2 级样式”模块主要针对操作元素的样式信息而开发，其特性简要总结如下：

- 每个元素都有一个关联的 style 对象，可以用来确定和修改行内的样式。

-  要确定某个元素的计算样式（包括应用给它的所有 CSS 规则），可以使用 getComputedStyle()方法

- IE不支持 getComputedStyle()方法，但为所有元素都提供了能够返回相同信息 currentStyle属性

- 可以通过 document.styleSheets 集合访问样式表

- 除 IE 之外的所有浏览器都支持针对样式表的这个接口，IE 也为几乎所有相应的 DOM 功能提供了自己的一套属性和方法。

  “DOM2 级遍历和范围”模块提供了与 DOM 结构交互的不同方式，简要总结如下：

- 遍历即使用 NodeIterator 或 TreeWalker 对 DOM 执行深度优先的遍历

- NodeIterator 是一个简单的接口，只允许以一个节点的步幅前后移动。而 TreeWalker 在提供相同功能的同时，还支持在 DOM 结构的各个方向上移动，包括父节点、同辈节点和子节点等方向

- 范围是选择 DOM 结构中特定部分，然后再执行相应操作的一种手段

- 使用范围选区可以在删除文档中某些部分的同时，保持文档结构的格式良好，或者复制文档中的相应部分

- IE8 及更早版本不支持“DOM2 级遍历和范围”模块，但它提供了一个专有的文本范围对象，可以用来完成简单的基于文本的范围操作。IE9 完全支持 DOM 遍历