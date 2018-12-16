DOM(文档对象模型)

#### 1、节点层次

- **Node类型**

  - DOM1 级定义了一个 Node 接口，该接口将由 DOM 中的所有节点类型实现，JavaScript 中的所有节点类型都继承自 Node 类型，因此所有节点类型都共享着相同的基本属性和方法

  - 每个节点都有一个 nodeType 属性，用于表明节点的类型。节点类型由在 Node 类型中定义的下列12 个数值常量来表示，任何节点类型必居其一：

    - Node.ELEMENT_NODE(1)；
    - Node.ATTRIBUTE_NODE(2)；
    - Node.TEXT_NODE(3)；
    - Node.CDATA_SECTION_NODE(4)
    - Node.ENTITY_REFERENCE_NODE(5)
    - Node.ENTITY_NODE(6)；
    - Node.PROCESSING_INSTRUCTION_NODE(7)
    - Node.COMMENT_NODE(8)
    - Node.DOCUMENT_NODE(9)；
    - Node.DOCUMENT_TYPE_NODE(10)
    - Node.DOCUMENT_FRAGMENT_NODE(11)
    - Node.NOTATION_NODE(12)

    ```js
    if (someNode.nodeType == 1){ //适用于所有浏览器
     	alert("Node is an element.");
    } 
    ```

  - nodeName 和 nodeValue 属性：这两个属性的值完全取决于节点的类型

    ```js
     if (someNode.nodeType == 1){
       	value = someNode.nodeName; //nodeName 的值是元素的标签名
      } 
    ```

  - 节点关系

    - 每个节点都有一个 childNodes 属性，其中保存着一个 NodeList 对象。NodeList 是一种类数组对象，用于保存一组有序的节点，可以通过位置来访问这些节点

      ```js
      var firstChild = someNode.childNodes[0];//通过方括号访问
      var secondChild = someNode.childNodes.item(1);//使用 item()方法访问
      var count = someNode.childNodes.length; //这个对象也有 length 属性(访问那一刻)
      
      //以将 NodeList 对象转换为数组：兼容IE8及之前浏览器
      function convertToArray(nodes){
       	var array = null;
       	try {
       		array = Array.prototype.slice.call(nodes, 0); //针对非 IE 浏览器
      	 } catch (ex) {
       		array = new Array();
       		for (var i=0, len=nodes.length; i < len; i++){
       			array.push(nodes[i]);
       		}
       	}
       	return array;
      } 
      ```

    - 每个节点都有一个 parentNode 属性，该属性指向文档树中的父节点，包含在 childNodes 列表中
      的所有节点都具有相同的父节点，因此它们的 parentNode 属性都指向同一个节点；包含在childNodes 列表中的每个节点相互之间都是同胞节点。通过使用列表中每个节点的 previousSibling
      和 nextSibling 属性，可以访问同一列表中的其他节点。列表中第一个节点的 previousSibling 属性
      值为 null，而列表中最后一个节点的 nextSibling 属性的值同样也为 null；如果列表中只有一个节点，那么该节点的 nextSibling 和 previousSibling 都为 null

      ```js
      if (someNode.nextSibling === null){
       	alert("Last node in the parent’s childNodes list.");
      } else if (someNode.previousSibling === null){
          alert("First node in the parent’s childNodes list.");
      } 
      ```

    - 父节点与其第一个和最后一个子节点之间也存在特殊关系。父节点的 firstChild 和 lastChild属性分别指向其 childNodes 列表中的第一个和最后一个节点；在只有一个子节点的情况下，firstChild 和
      lastChild 指向同一个节点；如果没有子节点，那么 firstChild 和 lastChild 的值均为 null

      ```js
      someNode.firstChild === someNode.childNodes[0]
      someNode.lastChild === childNodes [someNode.childNodes.length-1]
      ```

    - hasChildNodes()也是在节点包含一或多个子节点的情况下返回 true

    - 所有节点都有的最后一个属性是 ownerDocument，该属性指向表示整个文档的文档节点。这种关系表示的是任何节点都属于它所在的文档，任何节点都不能同时存在于两个或更多个文档中。通过这个属性，我们可以不必在节点层次中通过层层回溯到达顶端，而是可以直接访问文档节点

  - 操作节点：

    - appendChild()方法：用于向 childNodes 列表的末尾添加一个节点，返回新增的节点

      ```js
      //添加节点后，childNodes 的新增节点、父节点及以前的最后一个子节点的关系指针都会相应地得到更新
      var returnedNode = someNode.appendChild(newNode);
      alert(returnedNode == newNode); //true
      alert(someNode.lastChild == newNode); //true
      
      /*如果传入到 appendChild()中的节点已经是文档的一部分了，那结果就是将该节点从原来的位置
      转移到新位置*/
      //someNode 有多个子节点
      var returnedNode = someNode.appendChild(someNode.firstChild);
      alert(returnedNode == someNode.firstChild); //false
      alert(returnedNode == someNode.lastChild); //true 
      ```

    - insertBefore()方法：把节点放在 childNodes 列表中某个特定的位置上。这个方法接受两个参数：要插入的节点和作为参照的节点。插入节点后，被插入的节点会变成参照节点的前一个同胞节点（previousSibling），同时被方法返回。如果参照节点是null，则 insertBefore()与 appendChild()执行相同的操作

      ```js
      //插入后成为最后一个子节点
      returnedNode = someNode.insertBefore(newNode, null);
      alert(newNode == someNode.lastChild); //true
      //插入后成为第一个子节点
      var returnedNode = someNode.insertBefore(newNode, someNode.firstChild);
      alert(returnedNode == newNode); //true
      alert(newNode == someNode.firstChild); //true
      //插入到最后一个子节点前面
      returnedNode = someNode.insertBefore(newNode, someNode.lastChild);
      alert(newNode == someNode.childNodes[someNode.childNodes.length-2]); //true 
      ```

    - replaceChild()方法：接受的两个参数是是要插入的节点和要替换的节点，要替换的节点将由这个
      方法返回并从文档树中被移除，同时由要插入的节点占据其位置

      ```js
      //替换第一个子节点
      var returnedNode = someNode.replaceChild(newNode, someNode.firstChild);
      //替换最后一个子节点
      returnedNode = someNode.replaceChild(newNode, someNode.lastChild); 
      ```

    - removeChild()方法：移除节点，接受一个参数，即要移除的节点。被移除的节点将成为方法的返回值

      ```js
      //移除第一个子节点
      var formerFirstChild = someNode.removeChild(someNode.firstChild);
      //移除最后一个子节点
      var formerLastChild = someNode.removeChild(someNode.lastChild);
      ```

    - 注意：要使用这几个方法必须先取得父节点（使用 parentNode 属性）。另外，并不是所有类型的节点都有子节点，如果在不支持子节点的节点上调用了这些方法，将会导致错误发生

  - 其他方法

    - cloneNode()方法：用于创建调用这个方法的节点的一个完全相同的副本。cloneNode()方法接受一个布尔值参数，表示是否执行深复制；复制后返回的节点副本属于文档所有，但并没有为它指定父节点，所以需要通过 appendChild()、insertBefore()或 replaceChild()将它添加到文档中

      ```html
      <ul>
       <li>item 1</li>
       <li>item 2</li>
       <li>item 3</li>
      </ul> 
      <script>
          //已经将<ul>元素的引用保存在了变量 myList 中
      	var deepList = myList.cloneNode(true);
      	alert(deepList.childNodes.length); //3（IE < 9）或 7（其他浏览器）
      	var shallowList = myList.cloneNode(false);
      	alert(shallowList.childNodes.length); //0 
      </script>
      ```

    - normalize()方法：处理文档树中的文本节点，由于解析器的实现或 DOM 操作等原因，可能会出现文本节点不包含文本，或者接连出现两个文本节点的情况。当在某个节点上调用这个方法时，就会在该节点的后代节点中查找上述两种情况。如果找到了空文本节点，则删除它；如果找到相邻的文本节点，则将它们合并为一个文本节点

- **Document类型**

  - 概念：JavaScript 通过 Document 类型表示文档。在浏览器中，document 对象是 HTMLDocument（继承自 Document 类型）的一个实例，表示整个 HTML 页面。而且，document 对象是 window 对象的一个属性，因此可以将其作为全局对象来访问。

  - 特征：

    - nodeType 的值为 9
    - nodeName 的值为"#document"
    - nodeValue 的值为 null
    - parentNode 的值为 null
    - ownerDocument 的值为 null
    - 其子节点可能是一个 DocumentType（最多一个）、Element（最多一个）、ProcessingInstruction
      或 Comment

  - 文档的子节点：

    - 访问Document 节点的子节点的快捷方式：第一个就是documentElement属性，该属性始终指向 HTML 页面中的`<html>`元素；另一个就是通过 childNodes 列表访问文档元素

      ```html
      <html>
       	<body>
              
       	</body>
      </html> 
      <script>
      	var html = document.documentElement; //取得对<html>的引用
      	alert(html === document.childNodes[0]); //true
      	alert(html === document.firstChild); //true
      </script>
      ```

    - document 对象还有一个 body 属性，直接指向`<body>`元素

      ```js
      var body = document.body; //取得对<body>的引用
      ```

    - Document 另一个可能的子节点是 DocumentType，通常将`<!DOCTYPE>`标签看成一个与文档其他
      部分不同的实体，部分不同的实体，可以通过 doctype 属性来访问它的信息

      ```js
      var doctype = document.doctype; //取得对<!DOCTYPE>的引用
      ```

    - 出现在元素外部的注释应该算是文档的子节点，但是不同的浏览器在是否解析这些注释以及能否正确处理它们等方面，也存在很大差异

  - 文档信息

    - title属性：包含着`<title>`元素中的文本——显示在浏览器窗口的标题栏或标签页上；通过这个属性可以取得当前页面的标题，也可以修改当前页面的标题并反映在浏览器的标题栏中。修改 title 属性的值不会改变

      ```js
      //取得文档标题
      var originalTitle = document.title;
      //设置文档标题
      document.title = "New page title"; 
      ```

    - URL 属性：包含页面完整的 URL（即地址栏中显示的 URL），不可以设置

      ```js
      //取得完整的 URL
      var url = document.URL;
      ```

    - domain 属性：只包含页面的域名。可以设置，但由于安全方面的限制，也并非可以给 domain 设
      置任何值

      ```js
      //取得域名
      var domain = document.domain; 
      
      //假设页面来自 p2p.wrox.com 域
      document.domain = "wrox.com"; // 成功
      document.domain = "nczonline.net"; // 出错！
      
      //当页面中包含来自其他子域的框架或内嵌框架时，能够设置 document.domain 就非常方便了。由于跨域安全限制，来自不同子域的页面无法通过 JavaScript 通信。而通过将每个页面的document.domain 设置为相同的值，这些页面就可以互相访问对方包含的 JavaScript 对象了
      
      //如果域名一开始是“松散的”（loose），那么不能将它再设置为“紧绷的”（tight）
      //假设页面来自于 p2p.wrox.com 域
      document.domain = "wrox.com"; //松散的（成功）
      document.domain = "p2p.wrox.com"; //紧绷的（出错！）
      ```

    - referrer属性：保存着链接到当前页面的那个页面的 URL，不可以设置

      ```js
      //取得来源页面的 URL
      var referrer = document.referrer; 
      ```

  - 查找元素

    - getElementById()方法，接收一个参数：要取得的元素的 ID。如果找到相应的元素则返回该元素，如果不存在带有相应 ID 的元素，则返回 null

      ```html
      <div id="myDiv">Some text</div>
      <script>
      	//可以使用下面的代码取得这个元素：
      	var div = document.getElementById("myDiv"); //取得<div>元素的引用
      </script>
      ```

    - getElementsByTagName()方法，接受一个参数，即要取得元素的标签名，而返回的是包含零或多个元素的 NodeList，这个方法会返回一个 HTMLCollection 对象，作为一个“动态”集合

      ```js
      //与 NodeList 对象类似的方法
      var images = document.getElementsByTagName("img");
      alert(images.length); //输出图像的数量
      alert(images[0].src); //输出第一个图像元素的 src 特性
      alert(images.item(0).src); //输出第一个图像元素的 src 特性
      
      //HTMLCollection 对象的方法
      //<img src="myimage.gif" name="myImage"> 
      var myImage = images.namedItem("myImage"); //namedItem
      var myImage = images["myImage"]; //按名称访问项
      
      //取得文档中的所有元素
      var allElements = document.getElementsByTagName("*");
      ```

    - getElementsByName()方法，只有 HTMLDocument 类型才有的方法，会返回带有给定 name 特性的所有元素

      ```html
      <fieldset>
       	<legend>Which color do you prefer?</legend>
       	<ul>
       		<li><input type="radio" value="red" name="color" id="colorRed">
       		<label for="colorRed">Red</label></li>
       		<li><input type="radio" value="green" name="color"id="colorGreen">
       		<label for="colorGreen">Green</label></li>
       		<li><input type="radio" value="blue" name="color" id="colorBlue">
       		<label for="colorBlue">Blue</label></li>
       	</ul>
      </fieldset>
      <script>
          ///返回一个 HTMLCollectioin
      	var radios = document.getElementsByName("color"); /
      </script>
      ```

  - 特殊集合：这些集合都是 HTMLCollection 对象，为访问文档常用的部分提供了快捷方式

    - document.anchors，包含文档中所有带 name 特性的元素；
    - document.applets，包含文档中所有的`<applet>`元素，不再推荐使用了
    - document.forms，包含文档中所有的`<form>`元素
    - document.images，包含文档中所有的`<img>`元素
    - document.links，包含文档中所有带 href 特性的`<a>`元素

  - DOM 一致性检测

    - 由于 DOM 分为多个级别，也包含多个部分，因此检测浏览器实现了 DOM 的哪些部分就十分必要

    - document.implementation 属性就是为此提供相应信息和功能的对象，与浏览器对 DOM 的实现
      直接对应

    - DOM1 级只为 document.implementation 规定了一个方法，即 hasFeature()方法，接受两个参数：要检测的 DOM 功能的名称及版本号。如果浏览器支持给定名称和版本的功能，则该方法返回 true

      ```js
      var hasXmlDom = document.implementation.hasFeature("XML", "1.0");
      ```

    - 建议多数情况下，在使用 DOM 的某些特殊的功能之前，最好除了检测hasFeature()之外，还同时使用能力检测

  - 文档写入

    - write()方法，接受一个字符串参数，即要写入到输出流中的文本，原样写入

      ```html
      <html>
      <head>
       	<title>document.write() Example</title>
      </head>
      <body>
       	<p>The current date and time is:
       	<script type="text/javascript">
       		document.write("<strong>" + (new Date()).toString() + </strong>");
       	</script>
       	</p>
      </body>
      </html> 
      ```

    - writeln()方法，接受一个字符串参数，即要写入到输出流中的文本，在字符串的末尾添加换行符

    - open()和 close()分别用于打开和关闭网页的输出流，如果是在页面加载期间使用 write()
      或 writeln()方法，则不需要用到这两个方法

- **Element类型**

  - 概念：用于表现 XML 或 HTML元素，提供了对元素标签名、子节点及特性的访问

  - 特征：

    - nodeType 的值为 1；
    - nodeName 的值为元素的标签名；
    - nodeValue 的值为 null；
    - parentNode 可能是 Document 或 Element；
    - 其子节点可能是 Element、Text、Comment、ProcessingInstruction、CDATASection 或EntityReference

    ```js
    //访问元素的标签名，可以使用 nodeName 属性，也可以使用 tagName 属性
    var div = document.getElementById("myDiv");
    alert(div.tagName); //"DIV"
    alert(div.tagName == div.nodeName); //true 
    
    //这种做法适用于 HTML 文档，也适用于 XML 文档
    if (element.tagName.toLowerCase() == "div"){ //这样最好（适用于任何文档）
     	//在此执行某些操作
    }
    ```

  - HTML 元素：所有 HTML 元素都由 HTMLElement 类型表示，具有下列标准特性

    - id，元素在文档中的唯一标识符
    - title，有关元素的附加说明信息，一般通过工具提示条显示出来
    - lang，元素内容的语言代码，很少使用
    - dir，语言的方向，值为"ltr"（left-to-right，从左至右）或"rtl"（right-to-left，从右至左）
    - className，与元素的class 特性对应，即为元素指定的CSS类

    ```html
    <div id="myDiv" class="bd" title="Body text" lang="en" dir="ltr"></div> 
    <script>
    	var div = document.getElementById("myDiv");
        //获取
    	alert(div.id); //"myDiv""
    	alert(div.className); //"bd"
    	alert(div.title); //"Body text"
    	alert(div.lang); //"en"
    	alert(div.dir); //"ltr"
        //设置
        div.id = "someOtherId";
    	div.className = "ft";
    	div.title = "Some other text";
    	div.lang = "fr";
    	div.dir ="rtl"; 
    </script>
    ```

  - 取得特性：getAttribute()方法

    - 传递给 getAttribute()的特性名与实际的特性名相同

      ```js
      var div = document.getElementById("myDiv");
      alert(div.getAttribute("id")); //"myDiv"
      alert(div.getAttribute("class")); //"bd"
      alert(div.getAttribute("title")); //"Body text"
      alert(div.getAttribute("lang")); //"en"
      alert(div.getAttribute("dir")); //"ltr" 
      ```

    - 通过 getAttribute()方法也可以取得自定义特性

      ```js
      //<div id="myDiv" my_special_attribute="hello!"></div> 
      var value = div.getAttribute("my_special_attribute");
      //根据 HTML5 规范，自定义特性应该加上 data-前缀以便验证
      ```

    - 只有公认的（非自定义的）特性才会以属性的形式添加到 DOM对象中

      ```js
      //<div id="myDiv" align="left" my_special_attribute="hello!"></div> 
      alert(div.id); //"myDiv"
      alert(div.my_special_attribute); //undefined（IE 除外）
      alert(div.align); //"left" 
      ```

    - style属性，用于通过 CSS 为元素指定样式。在通过 getAttribute()访问时，返回的 style 特性值中包含的是 CSS 文本，而通过属性来访问它则会返回一个对象

    - onclick 事件处理程序，当在元素上使用时，onclick 特性中包含的是 JavaScript 代码，如果通过 getAttribute()访问，则会返回相应代码的字符串。而在访问onclick 属性时，则会返回一个 JavaScript 函数（如果未在元素中指定相应特性，则返回 null）。这是因为 onclick 及其他事件处理程序属性本身就应该被赋予函数值

    - 由于存在这些差别，在通过 JavaScript 以编程方式操作 DOM 时，开发人员经常不使用getAttribute()，而是只使用对象的属性。只有在取得自定义特性值的情况下，才会使用getAttribute()方法

  - 设置特性：setAttribute()方法

    - 接受两个参数：要设置的特性名和值。如果特性已经存在，setAttribute()会以指定的值替换现有的值；如果特性不存在，setAttribute()则创建该属性并设置相应的值

      ```js
      div.setAttribute("id", "someOtherId");
      div.setAttribute("class", "ft");
      div.setAttribute("title", "Some other text");
      div.setAttribute("lang","fr");
      div.setAttribute("dir", "rtl"); 
      ```

    - 通过 setAttribute()方法既可以操作 HTML 特性也可以操作自定义特性

    - 因为所有特性都是属性，所以直接给属性赋值可以设置特性的值

      ```js
      //公认的（非自定义的）特性
      div.id = "someOtherId";
      div.align = "left"; 
      //自定义的属性，该属性不会自动成为元素的特性
      div.mycolor = "red";
      alert(div.getAttribute("mycolor")); //null（IE 除外）
      ```

  - 移除属性：是 removeAttribute()方法

    - 用于彻底删除元素的特性，不仅会清除特性的值，而且也会从元素中完全删除特性

      ```js
      div.removeAttribute("class"); 
      ```

    - 这个方法并不常用，但在序列化 DOM 元素时，可以通过它来确切地指定要包含哪些特性

  -  attributes 属性：attributes 属性中包含一个NamedNodeMap，与 NodeList 类似

    - getNamedItem(name)方法：返回 nodeName 属性等于 name 的节点。attributes 属性中包含一系列节点，每个节点的 nodeName 就是特性的名称，而节点的 nodeValue就是特性的值

      ```js
      var id = element.attributes.getNamedItem("id").nodeValue; 
      var id = element.attributes["id"].nodeValue; //简写
      element.attributes["id"].nodeValue = "someOtherId"; //设置特性的值
      ```

    - removeNamedItem(name)方法：从列表中移除 nodeName 属性等于 name 的节点。直接删除具有给定名称的特性,返回表示被删除特性的 Attr 节点

      ```js
      var oldAttr = element.attributes.removeNamedItem("id"); 
      ```

    - setNamedItem(node)方法：向列表中添加节点，以节点的 nodeName 属性为索引

      ```js
      element.attributes.setNamedItem(newAttr); 
      ```

    - item(pos)：返回位于数字 pos 位置处的节点

    - 使用attributes 属性遍历元素的特性，在需要将 DOM 结构序列化为 XML 或 HTML 字符串时，多数都会涉及遍历元素特性

      ```js
      function outputAttributes(element){
       	var pairs = new Array(),
       	attrName,
       	attrValue,
       	i,
       	len;
       	for (i=0, len=element.attributes.length; i < len; i++){
       		attrName = element.attributes[i].nodeName;
       		attrValue = element.attributes[i].nodeValue;
       		if (element.attributes[i].specified) {
       			pairs.push(attrName + "=\"" + attrValue + "\"");
       		}
       	}
       	return pairs.join(" ");
      } 
      ```

  - 创建元素

    - 使用 document.createElement()方法可以创建新元素，只接受一个参数，即要创建元素的标签名

      ```js
      var div = document.createElement("div"); //创建
      div.id = "myNewDiv";                    
      div.className = "box"; 
      document.body.appendChild(div); //添加到文档
      ```

    -  IE 中可以以另一种方式使用 createElement()，即为这个方法传入完整的元素标签，也可以包含属性

      ```js
      var div = document.createElement("<div id=\"myNewDiv\" class=\"box\"></div >"); 
      ```

    - 存在的问题：

      - 不能设置动态创建的`<iframe>`元素的 name 特性
      - 不能通过表单的 reset()方法重设动态创建的`<input>`元素
      - 动态创建的 type 特性值为"reset"的`<buttou>`元素重设不了表单
      - 动态创建的一批 name 相同的单选按钮彼此毫无关系。name 值相同的一组单选按钮本来应该用
        于表示同一选项的不同值，但动态创建的一批这种单选按钮之间却没有这种关系

      ```js
      if (client.browser.ie && client.browser.ie <=7){
       	//创建一个带 name 特性的 iframe 元素
       var iframe = document.createElement("<iframe name=\"myframe\"</iframe>");
       //创建 input 元素
       var input = document.createElement("<input type=\"checkbox\">");
       //创建 button 元素
       var button = document.createElement("<button type=\"reset\"></button>");
       //创建单选按钮
       var radio1 = document.createElement("<input type=\"radio\"name=\"choice\" "＋"value=\"1\">");
       var radio2 = document.createElement("<input type=\"radio\"name=\"choice\" "＋"value=\"2\">");
      } 
      ```

  - 元素的子节点

    - 元素可以有任意数目的子节点和后代节点，因为元素可以是其他元素的子节点

    - 元素的childNodes 属性中包含了它的所有子节点，这些子节点有可能是元素、文本节点、注释或处理指令

    - 元素也支持getElementsByTagName()方法。在通过元素调用这个方法时，除了搜索起点是当前元素之外，其他方面都跟通过 document 调用这个方法相同，因此结果只会返回当前元素的后代

      ```js
      var ul = document.getElementById("myList");
      var items = ul.getElementsByTagName("li"); 
      ```

- **Text类型**

  - 概念：文本节点由 Text 类型表示，包含的是可以照字面解释的纯文本内容。纯文本中可以包含转义后的HTML 字符，但不能包含 HTML 代码

  - 特征：

    - nodeType 的值为 3
    - nodeName 的值为"#text"
    - nodeValue 的值为节点所包含的文本
    - parentNode 是一个 Element
    - 不支持（没有）子节点

  - 操作：可以通过 nodeValue 属性或 data 属性访问 Text 节点中包含的文本，这两个属性中包含的值相同；对 nodeValue 的修改也会通过 data 反映出来，反之亦然。使用下列方法可以操作节点中的文本：

    - appendData(text)：将 text 添加到节点的末尾

    - deleteData(offset, count)：从 offset 指定的位置开始删除 count 个字符

    - insertData(offset, text)：在 offset 指定的位置插入 text

    - replaceData(offset, count, text)：用text替换从offset指定的位置开始到offset+count为止处的文本

    - splitText(offset)：从 offset 指定的位置将当前文本节点分成两个文本节点

    - substringData(offset, count)：提取从 offset 指定的位置开始到 offset+count 为止处的字符串

    - length 属性，保存着节点中字符的数目，nodeValue.length 和 data.length 中也保存着同样的值

    - 在默认情况下，每个可以包含内容的元素最多只能有一个文本节点，而且必须确实有内容存在

      ```html
      <!-- 没有内容，也就没有文本节点 -->
      <div></div>
      <!-- 有空格，因而有一个文本节点 -->
      <div> </div>
      <!-- 有内容，因而有一个文本节点 -->
      <div>Hello World!</div> 
      <script>
      	var textNode = div.firstChild; //或者 div.childNodes[0] 
          div.firstChild.nodeValue = "Some other message";
          //输出结果是"Some &lt;strong&gt;other&lt;/strong&gt; message"
      	div.firstChild.nodeValue = "Some <strong>other</strong> message";
      </script>
      ```

  -  创建文本节点

    - 使用document.createTextNode()创建新文本节点，这个方法接受一个参数：要插入节点中的文本；在创建新文本节点的同时，也会为其设置 ownerDocument 属性

      ```js
      //
      var element = document.createElement("div");
      element.className = "message";
      var textNode = document.createTextNode("Hello world!");
      element.appendChild(textNode);
      document.body.appendChild(element); 
      
      //一般情况下，每个元素只有一个文本子节点，在某些情况下也可能包含多个文本子节点
      //如果两个文本节点是相邻的同胞节点，那么这两个节点中的文本就会连起来显示，中间不会有空格
      var element = document.createElement("div");
      element.className = "message";
      var textNode = document.createTextNode("Hello world!");
      element.appendChild(textNode);
      var anotherTextNode = document.createTextNode("Yippee!");
      element.appendChild(anotherTextNode);
      document.body.appendChild(element); 
      ```

    - 一般情况下，每个元素只有一个文本子节点，在某些情况下也可能包含多个文本子节点，如果两个文本节点是相邻的同胞节点，那么这两个节点中的文本就会连起来显示，中间不会有空格

      ```js
      var element = document.createElement("div");
      element.className = "message";
      var textNode = document.createTextNode("Hello world!");
      element.appendChild(textNode);
      var anotherTextNode = document.createTextNode("Yippee!");
      element.appendChild(anotherTextNode);
      document.body.appendChild(element); 
      ```

  - 规范化文本节点

    - 在一个包含两个或多个文本节点的父元素上调用 normalize()方法，则会将所有文本节点合并成一个
      节点，结果节点的 nodeValue 等于将合并前每个文本节点的 nodeValue 值拼接起来的值

      ```js
      var element = document.createElement("div");
      element.className = "message";
      var textNode = document.createTextNode("Hello world!");
      element.appendChild(textNode);
      var anotherTextNode = document.createTextNode("Yippee!");
      element.appendChild(anotherTextNode);
      document.body.appendChild(element);
      alert(element.childNodes.length); //2
      element.normalize();
      alert(element.childNodes.length); //1
      alert(element.firstChild.nodeValue); // "Hello world!Yippee!" 
      ```

    - 浏览器在解析文档时永远不会创建相邻的文本节点，这种情况只会作为执行 DOM 操作的结果出现

  -  分割文本节点

    - Text 类型提供了一个作用与 normalize()相反的方法：splitText()，这个方法会将一个文本节点分成两个文本节点，即按照指定的位置分割 nodeValue 值

    - 原来的文本节点将包含从开始到指定位置之前的内容，新文本节点将包含剩下的文本

    - 返回一个新文本节点，该节点与原节点的parentNode 相同

      ```js
      var element = document.createElement("div");
      element.className = "message";
      var textNode = document.createTextNode("Hello world!");
      element.appendChild(textNode);
      document.body.appendChild(element);
      var newNode = element.firstChild.splitText(5);
      alert(element.firstChild.nodeValue); //"Hello"
      alert(newNode.nodeValue); //" world!"
      alert(element.childNodes.length); //2 
      ```

- **Comment类型**

  - 特征：注释在 DOM 中是通过 Comment 类型来表示的

    - nodeType 的值为 8
    - nodeName 的值为"#comment"
    - nodeValue 的值是注释的内容
    - parentNode 可能是 Document 或 Element
    - 不支持（没有）子节点

  - Comment 类型与 Text 类型继承自相同的基类，因此它拥有除 splitText()之外的所有字符串操作方法；与 Text 类型相似，也可以通过 nodeValue 或 data 属性来取得注释的内容

    ```html
    <div id="myDiv"><!--A comment --></div>
    <script>
    	//注释节点可以通过其父节点来访问
        var div = document.getElementById("myDiv");
    	var comment = div.firstChild;
    	alert(comment.data); //"A comment" 
        //使用 document.createComment()并为其传递注释文本也可以创建注释节点
        var comment = document.createComment("A comment "); 
    </script>
    
    ```

- **CDATASection类型**

  - 概念：CDATASection 类型只针对基于 XML 的文档，表示的是 CDATA 区域。与 Comment 类似，
    CDATASection 类型继承自 Text 类型，因此拥有除 splitText()之外的所有字符串操作方法

  - 特征：

    - nodeType 的值为 4；
    - nodeName 的值为"#cdata-section"；
    - nodeValue 的值是 CDATA 区域中的内容；
    - parentNode 可能是 Document 或 Element；
    - 不支持（没有）子节点

  - CDATA 区域只会出现在 XML 文档中，因此多数浏览器都会把 CDATA 区域错误地解析为 Comment
    或 Element

    ```html
    <div id="myDiv"><![CDATA[This is some content.]]></div> 
    ```

  - 在真正的 XML 文档中，可以使用 document.createCDataSection()来创建 CDATA 区域，只需为其传入节点的内容即可

- **DocumentType类型**

  - 特征：包含着与文档的 doctype 有关的所有信息

    - nodeType 的值为 10；
    - nodeName 的值为 doctype 的名称；
    - nodeValue 的值为 null；
    - parentNode 是 Document；
    - 不支持（没有）子节点

  - 在 DOM1 级中，DocumentType 对象不能动态创建，而只能通过解析文档代码的方式来创建。持它的浏览器会把 DocumentType 对象保存在 document.doctype 中 。 DOM1 级描述了DocumentType 对象的 3 个属性：

    - name 表示文档类型的名称
    - entities 是由文档类型描述的实体的 NamedNodeMap 对象
    - notations 是由文档类型描述的符号的NamedNodeMap 对象

    ```html
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
    "http://www.w3.org/TR/html4/strict.dtd"> 
    <script>
    	alert(document.doctype.name); //"HTML"
    </script>
    ```

- **DocumentFragment类型**

  - 概念：在所有节点类型中，只有 DocumentFragment 在文档中没有对应的标记。DOM 规定文档片段
    （document fragment）是一种“轻量级”的文档，可以包含和控制节点，但不会像完整的文档那样占用
    额外的资源。

  - 特征：

    - nodeType 的值为 11；
    - nodeName 的值为"#document-fragment"；
    - nodeValue 的值为 null；
    - parentNode 的值为 null；
    - 子节点可以是 Element、ProcessingInstruction、Comment、Text、CDATASection 或
      EntityReference

  - 使用：不能把文档片段直接添加到文档中，但可以将它作为一个“仓库”来使用，即可以在里面保存将来可能会添加到文档中的节点。要创建文档片段，可以使用document.createDocumentFragment()方法

    ```js
    var fragment = document.createDocumentFragment(); 
    ```

  - 文档片段继承了 Node 的所有方法，通常用于执行那些针对文档的 DOM 操作

    ```html
    <ul id="myList"></ul> 
    <script>
    	var fragment = document.createDocumentFragment();
    	var ul = document.getElementById("myList");
    	var li = null;
    	for (var i=0; i < 3; i++){
     		li = document.createElement("li");
     		li.appendChild(document.createTextNode("Item " + (i+1)));
     		fragment.appendChild(li);
    	}
    	ul.appendChild(fragment); 
    </script>
    ```

- **Attr类型**

  - 概念：元素的特性在 DOM 中以 Attr 类型来表示。在所有浏览器中（包括 IE8），都可以访问 Attr 类型
    的构造函数和原型。从技术角度讲，特性就是存在于元素的 attributes 属性中的节点

  - 特征：

    - nodeType 的值为 2；
    - nodeName 的值是特性的名称；
    - nodeValue 的值是特性的值；
    - parentNode 的值为 null；
    - 在 HTML 中不支持（没有）子节点；
    - 在 XML 中子节点可以是 Text 或 EntityReference

  - 方法：尽管它们也是节点，但特性却不被认为是 DOM 文档树的一部分。开发人员最常使用的是 getAttribute()、setAttribute()和 remveAttribute()方法，很少直接引用特性节点

  - 属性：Attr 对象有 3 个属性：name、value 和 specified。其中，name 是特性名称（与 nodeName 的
    值相同），value 是特性的值（与 nodeValue 的值相同），而 specified 是一个布尔值，用以区别特性是在代码中指定的，还是默认的

  - 使用 document.createAttribute()并传入特性的名称可以创建新的特性节点

    ```js
    var attr = document.createAttribute("align");
    attr.value = "left";
    element.setAttributeNode(attr);
    alert(element.attributes["align"].value); //"left"
    alert(element.getAttributeNode("align").value); //"left"
    alert(element.getAttribute("align")); //"left" 
    ```

#### 2、DOM操作技术

- 动态脚本

  - 动态加载的外部 JavaScript 文件能够立即运行

    ```js
    //<script type="text/javascript" src="client.js"></script> 
    function loadScript(url){
     var script = document.createElement("script");
     script.type = "text/javascript";
     script.src = url;
     document.body.appendChild(script);
    } 
    loadScript("client.js"); 
    ```

  - 指定 JavaScript 代码的方式是行内方式

    ```js
    //<script type="text/javascript">function sayHi(){alert("hi");}</script>
    function loadScriptString(code){
     	var script = document.createElement("script");
     	script.type = "text/javascript";
     	try {
     		script.appendChild(document.createTextNode(code));
     	} catch (ex){
    		 script.text = code;
     	}
     	document.body.appendChild(script);
    }
    loadScriptString("function sayHi(){alert('hi');}");
    ```

- 动态样式

  - 使用`<link>`元素用于包含来自外部的文件

    ```js
    //<link rel="stylesheet" type="text/css" href="styles.css">
    function loadStyles(url){
     	var link = document.createElement("link");
        link.rel = "stylesheet";
     	link.type = "text/css";
     	link.href = url;
     	var head = document.getElementsByTagName("head")[0];
     	head.appendChild(link);
    } 
    loadStyles("styles.css");
    ```

  - 使用`<style>`元素用于包含嵌入式 CSS

    ```js
    function loadStyleString(css){
     	var style = document.createElement("style"); 
    	style.type = "text/css";
     	try{
     		style.appendChild(document.createTextNode(css));
     	} catch (ex){
     		style.styleSheet.cssText = css;
     	}
     	var head = document.getElementsByTagName("head")[0];
     	head.appendChild(style);
    }
    loadStyleString("body{background-color:red}"); 
    ```

  - 如果专门针对 IE 编写代码，务必小心使用 styleSheet.cssText 属性。在重用同一个`<style>`元素并再次设置这个属性时，有可能会导致浏览器崩溃。同样，将cssText 属性设置为空字符串也可能导致浏览器崩溃。我们希望 IE 中的这个 bug 能够在将来被修复

- 操作表格

  - 传统方法：使用 DOM 来创建下面的 HTML 表格

    ```js
    //创建 table
    var table = document.createElement("table");
    table.border = 1;
    table.width = "100%";
    //创建 tbody
    var tbody = document.createElement("tbody"); 
    table.appendChild(tbody);
    //创建第一行
    var row1 = document.createElement("tr");
    tbody.appendChild(row1);
    var cell1_1 = document.createElement("td");
    cell1_1.appendChild(document.createTextNode("Cell 1,1"));
    row1.appendChild(cell1_1);
    var cell2_1 = document.createElement("td");
    cell2_1.appendChild(document.createTextNode("Cell 2,1"));
    row1.appendChild(cell2_1);
    //创建第二行
    var row2 = document.createElement("tr");
    tbody.appendChild(row2);
    var cell1_2 = document.createElement("td");
    cell1_2.appendChild(document.createTextNode("Cell 1,2"));
    row2.appendChild(cell1_2);
    var cell2_2= document.createElement("td");
    cell2_2.appendChild(document.createTextNode("Cell 2,2"));
    row2.appendChild(cell2_2);
    //将表格添加到文档主体中
    document.body.appendChild(table); 
    ```

  - HTML DOM 还为`<table>`添加了一些属性和方法

    - caption：保存着对元素（如果有）的指针
    - tBodies：是一个元素的 HTMLCollection
    - tFoot：保存着对元素（如果有）的指针
    - tHead：保存着对元素（如果有）的指针
    - rows：是一个表格中所有行的 HTMLCollection
    - createTHead()：创建元素，将其放到表格中，返回引用
    - createTFoot()：创建元素，将其放到表格中，返回引用
    - createCaption()：创建元素，将其放到表格中，返回引用
    - deleteTHead()：删除元素
    - deleteTFoot()：删除元素
    - deleteCaption()：删除元素
    - deleteRow(pos)：删除指定位置的行
    - insertRow(pos)：向 rows 集合中的指定位置插入一行

  - HTML DOM 还为`<tbody>`添加了一些属性和方法

    - rows：保存着元素中行的 HTMLCollection
    - deleteRow(pos)：删除指定位置的行
    - insertRow(pos)：向 rows 集合中的指定位置插入一行，返回对新插入行的引用

  - HTML DOM 还为`<tr>`添加了一些属性和方法

    - cells：保存着元素中单元格的 HTMLCollection
    - deleteCell(pos)：删除指定位置的单元格
    - insertCell(pos)：向 cells 集合中的指定位置插入一个单元格，返回对新插入单元格的引用

  - 使用新方法创建表格

    ```js
    //创建 table
    var table = document.createElement("table");
    table.border = 1;
    table.width = "100%";
    //创建 tbody
    var tbody = document.createElement("tbody");
    table.appendChild(tbody);
    //创建第一行
    tbody.insertRow(0);
    tbody.rows[0].insertCell(0);
    tbody.rows[0].cells[0].appendChild(document.createTextNode("Cell 1,1"));
    tbody.rows[0].insertCell(1);
    tbody.rows[0].cells[1].appendChild(document.createTextNode("Cell 2,1"));
    //创建第二行
    tbody.insertRow(1);
    tbody.rows[1].insertCell(0);
    tbody.rows[1].cells[0].appendChild(document.createTextNode("Cell 1,2"));
    tbody.rows[1].insertCell(1);
    tbody.rows[1].cells[1].appendChild(document.createTextNode("Cell 2,2"));
    //将表格添加到文档主体中
    document.body.appendChild(table); 
    ```

-  使用NodeList

  - 所有 NodeList 对象都是在访问 DOM 文档时实时运行的查询

    ```js
    var divs = document.getElementsByTagName("div"),
     i,
     div;
    for (i=0; i < divs.length; i++){
     	div = document.createElement("div");
     	document.body.appendChild(div);
    } 
    ```

  - 如果想要迭代一个NodeList，最好是使用length属性初始化第二个变量，然后将迭代器与该变量进行比较

    ```js
    var divs = document.getElementsByTagName("div"),
     i,
     len,
     div;
    for (i=0, len=divs.length; i < len; i++){
     	div = document.createElement("div");
     	document.body.appendChild(div);
    } 
    ```

  - 一般来说，应该尽量减少访问 NodeList 的次数。因为每次访问 NodeList，都会运行一次基于文档的查询。所以，可以考虑将从 NodeList 中取得的值缓存起来

#### 3、总结

- DOM 是语言中立的 API，用于访问和操作 HTML 和 XML 文档。DOM1 级将 HTML 和 XML 文档形象地看作一个层次化的节点树，可以使用 JavaScript 来操作这个节点树，进而改变底层文档的外观和结构；DOM 由各种节点构成，简要总结如下：
  - 最基本的节点类型是 Node，用于抽象地表示文档中一个独立的部分；所有其他类型都继承自Node
  - Document 类型表示整个文档，是一组分层节点的根节点。在 JavaScript 中，document 对象是
    Document 的一个实例。使用 document 对象，有很多种方式可以查询和取得节点
  - Element 节点表示文档中的所有 HTML 或 XML 元素，可以用来操作这些元素的内容和特性。
  - 另外还有一些节点类型，分别表示文本内容、注释、文档类型、CDATA 区域和文档片段
- 访问 DOM 的操作在多数情况下都很直观，不过在处理`<script>`和`<style>`元素时还是存在一些复杂性。由于这两个元素分别包含脚本和样式信息，因此浏览器通常会将它们与其他元素区别对待。这些区别导致了在针对这些元素使用 innerHTML 时，以及在创建新元素时的一些问题
- 理解 DOM 的关键，就是理解 DOM 对性能的影响。DOM 操作往往是 JavaScript 程序中开销最大的部分，而因访问 NodeList 导致的问题为最多。NodeList 对象都是“动态的”，这就意味着每次访问NodeList 对象，都会运行一次查询。有鉴于此，最好的办法就是尽量减少 DOM 操作