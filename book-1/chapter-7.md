## BOM(浏览器对象模型)

#### 1、window对象

- 概念：BOM 的核心对象是 window，它表示浏览器的一个实例。在浏览器中，window 对象有双重角色，它既是通过 JavaScript 访问浏览器窗口的一个接口，又是 ECMAScript 规定的 Global 对象。这意味着在网页中定义的任何一个对象、变量和函数，都以 window 作为其 Global 对象，因此有权访问parseInt()等方法

- 全局作用域：

  - 所有在全局作用域中声明的变量、函数都会变成 window 对象的属性和方法

    ```js
    var age = 29;
    function sayAge(){
     	alert(this.age);
    }
    alert(window.age); //29
    sayAge(); //29
    window.sayAge(); //29
    ```

  - 全局变量不能通过 delete 操作符删除，而直接在 window 对象上的定义的属性可以

    ```js
    var age = 29;
    window.color = "red";
    //在 IE < 9 时抛出错误，在其他所有浏览器中都返回 false
    delete window.age;
    //在 IE < 9 时抛出错误，在其他所有浏览器中都返回 true
    delete window.color; //returns true
    alert(window.age); //29
    alert(window.color); //undefined 
    ```

  - 尝试访问未声明的变量会抛出错误，但通过查询 window 对象，可以知道某个可能未声明的变量是否存在

    ```js
    //这里会抛出错误，因为 oldValue 未定义
    var newValue = oldValue;
    //这里不会抛出错误，因为这是一次属性查询
    //newValue 的值是 undefined
    var newValue = window.oldValue;
    ```

- 窗口关系及框架

  - 如果页面中包含框架，则每个框架都拥有自己的 window 对象，并且保存在 frames 集合中
  - top 对象始终指向最高（最外）层的框架，也就是浏览器窗口。使用它可以确保在一个框架中正确地访问另一个框架
  - 与 top 相对的另一个 window 对象是 parent。顾名思义，parent（父）对象始终指向当前框架的直接上层框架
  - 与框架有关的最后一个对象是 self，它始终指向 window；实际上，self 和 window 对象可以互换使用。引入 self 对象的目的只是为了与 top 和 parent 对象对应起来，因此它不格外包含其他值

- 窗口位置

  - 确定和修改 window 对象位置的属性和方法

    - IE、Safari、Opera 和 Chrome 都提供了screenLeft 和 screenTop 属性，分别用于表示窗口相对于屏幕左边和上边的位置
    - Firefox 则在screenX 和 screenY 属性中提供相同的窗口位置信息
    - Opera虽然也支持 screenX 和 screenY 属性，但与 screenLeft 和 screenTop 属性并不对应

    ```js
    var leftPos = (typeof window.screenLeft == "number") ?
     	window.screenLeft : window.screenX;
    var topPos = (typeof window.screenTop == "number") ?
     	window.screenTop : window.screenY;
    ```

  - 将窗口精确地移动到一个新位置:

    - moveTo()：接收两个参数，新位置的 x 和 y 坐标值

      ```js
      //将窗口移动到屏幕左上角
      window.moveTo(0,0);
      //将窗口移动到(200,300)
      window.moveTo(200,300);
      ```

    - moveBy()：在水平和垂直方向上移动的像素数

      ```js
      //将窗向下移动 100 像素
      window.moveBy(0,100);
      //将窗口向左移动 50 像素
      window.moveBy(-50,0); 
      ```

- 窗口大小

  - 确定一个窗口的大小

    - 在 IE9+、Safari 和 Firefox中，outerWidth 和 outerHeight 返回浏览器窗口本身的尺寸（无论是从最外层的 window 对象还是从某个框架访问）
    - 在 IE9+、Safari 和 Firefox中，innerWidth 和 innerHeight则表示该容器中页面视图区的大小（减去边框宽度）
    - 在 Chrome 中，outerWidth、outerHeight 与innerWidth、innerHeight 返回相同的值，即视口（viewport）大小而非浏览器窗口大小

  - 确定页面视口的大小

    - 在 IE、Firefox、Safari、Opera 和 Chrome 中，document.documentElement.clientWidth 和
      document.documentElement.clientHeight 中保存了页面视口的信息

    - 在 IE6 的混杂模式，就必须通过 document.body.clientWidth 和 document.body.
      clientHeight 取得相同信息

    - 混杂模式下的 Chrome，则无论通过 document.documentElement还是 document.body 中的 clientWidth 和 clientHeight 属性，都可以取得视口的大小

    ```js
    var pageWidth = window.innerWidth;
    var pageHeight = window.innerHeight;
    
    if (typeof pageWidth != "number"){
     	if (document.compatMode == "CSS1Compat"){
     		pageWidth = document.documentElement.clientWidth;
     		pageHeight = document.documentElement.clientHeight;
     	} else {
     		pageWidth = document.body.clientWidth;
     		pageHeight = document.body.clientHeight;
     	}
    } 
    ```

    - 对于移动设备，window.innerWidth 和 window.innerHeight 保存着可见视口，也就是屏幕上可见页面区域的大小；移动 IE 浏览器不支持这些属性，但通过 document.documentElement.clientWidth
      和 document.documentElement.clientHeihgt 提供了相同的信息。随着页面的缩放，这些值
      也会相应变化
    - 在其他移动浏览器中，document.documentElement 度量的是布局视口，即渲染后页面的实际大
      小（与可见视口不同，可见视口只是整个页面中的一小部分）；移动 IE 浏览器把布局视口的信息保存在document.body.clientWidth和document.body.clientHeight中。这些值不会随着页面缩放变化

  - 调整浏览器窗口的大小的方法：

    - resizeTo()：接收两个参数，浏览器窗口的新宽度和新高度
    - resizeBy()：接收两个参数，新窗口与原窗口的宽度和高度之差

    ```js
    //调整到 100×100
    window.resizeTo(100, 100);
    //调整到 200×150
    window.resizeBy(100, 50);
    //调整到 300×300
    window.resizeTo(300, 300);
    ```


- 导航和打开窗口

  - window.open()方法

    - 4 个参数：要加载的 URL、窗口目标、一个特性字符串以及一个表示新页面是否取代浏览器历史记录中当前加载页面的布尔值

    - 通常只须传递第一个参数，最后一个参数只在不打开新窗口的情况下使用

    - 如果为 window.open()传递了第二个参数，而且该参数是已有窗口或框架的名称，那么就会在具有该名称的窗口或框架中加载第一个参数指定的 URL，第二个参数也可以是下列任何一个特殊的窗口名
      称：_self、_parent、_top 或_blank；

      ```js
      //等同于< a href="http://www.wrox.com" target="topFrame"></a>
      window.open("http://www.wrox.com/", "topFrame"); 
      ```

  - 弹出窗口

    - 如果给 window.open()传递的第二个参数并不是一个已经存在的窗口或框架，那么该方法就会根
      据在第三个参数位置上传入的字符串创建一个新窗口或新标签页

    - 如果没有传入第三个参数，那么就会打开一个带有全部默认设置（工具栏、地址栏和状态栏等）的新浏览器窗口（或者打开一个新标签页——根据浏览器设置）

    - 在不打开新窗口的情况下，会忽略第三个参数

    - 第三个参数是一个逗号分隔的设置字符串，表示在新窗口中都显示哪些特性

      ```js
      window.open("http://www.wrox.com/","wroxWindow",
       "height=400,width=400,top=10,left=10,resizable=yes"); 
      ```

    - window.open()方法会返回一个指向新窗口的引用

      ```js
      var wroxWin = window.open("http://www.wrox.com/","wroxWindow",
       "height=400,width=400,top=10,left=10,resizable=yes");
      //调整大小
      wroxWin.resizeTo(500,500);
      //移动位置
      wroxWin.moveTo(100,100); 
      ```

    - 调用 close()方法还可以关闭新打开的窗口

      ```js
      wroxWin.close();
      alert(wroxWin.closed); //true 
      ```

    - 新创建的 window 对象有一个 opener 属性，其中保存着打开它的原始窗口对象

      ```js
      var wroxWin = window.open("http://www.wrox.com/","wroxWindow",
       "height=400,width=400,top=10,left=10,resizable=yes");
      alert(wroxWin.opener == window); //true 
      ```

    - 在 Chrome中，将新创建的标签页的 opener 属性设置为 null，即表示在单独的进程中运行新标签页

      ```js
      var wroxWin = window.open("http://www.wrox.com/","wroxWindow",
       "height=400,width=400,top=10,left=10,resizable=yes");
      wroxWin.opener = null; 
      ```

  - 安全限制

    - IE6 对弹出窗口施加了多方面的安全限制，包括不允许在屏幕之外创建弹出窗口、不允许将弹出窗口移动到屏幕以外、不允许关闭状态栏等
    - IE7 则增加了更多的安全限制，如不允许关闭地址栏、默认情况下不允许移动弹出窗口或调整其大小
    - Firefox 1 从一开始就不支持修改状态栏，因此无论给 window.open()传入什么样的特性字符串，弹出窗口中都会无一例外地显示状态栏；Firefox 3 又强制始终在弹出窗口中显示地址栏
    - Opera 只会在主浏览器窗口中打开弹出窗口，但不允许它们出现在可能与系统对话框混淆的地方
    - 此外，有的浏览器只根据用户操作来创建弹出窗口，只能通过单击或者击键来打开弹出窗口
    - Chrome 对于那些不是用户有意打开的弹出窗口，只显示它们的标题栏，并把它们放在浏览器窗口的右下角

  - 弹出窗口屏蔽程序

    - 大多数浏览器都内置有弹出窗口屏蔽程序，如果是浏览器内置的屏蔽程序阻止的弹出窗口，那么 window.open()很可能会返回 null

      ```js
      var wroxWin = window.open("http://www.wrox.com", "_blank");
      if (wroxWin == null){
       	alert("The popup was blocked!");
      } 
      ```

    - 如果是浏览器扩展或其他程序阻止的弹出窗口，那么 window.open()通常会抛出一个错误，综合两种情况，封装函数如下：

      ```js
      var blocked = false;
      try {
       	var wroxWin = window.open("http://www.wrox.com", "_blank");
       	if (wroxWin == null){
       	 	blocked = true;
       	}
      	} catch (ex){
       		blocked = true;
      	}
      if (blocked){
       	alert("The popup was blocked!");
      } 
      ```

- 间歇调用和超时调用

  - 间歇调用：每隔指定的时间就执行一次代码

    - 使用 window 对象的 setTimeout()方法，它接受两个参数：要执行的代码和以毫秒表示的时间（即在执行代码前需要等待多少毫秒）

    - 第一个参数可以是一个包含 JavaScript 代码的字符串（就和在 eval()函数中使用的字符串一样），也可以是一个函数

      ```js
      //不建议传递字符串！
      setTimeout("alert('Hello world!') ", 1000);
      //推荐的调用方式
      setTimeout(function() {
       	alert("Hello world!");
      }, 1000); 
      ```

    - 第二个参数是一个表示等待多长时间的毫秒数，但经过该时间后指定的代码不一定会执行

    - 调用 setTimeout()之后，该方法会返回一个数值 ID，表示超时调用。这个超时调用 ID 是计划执行代码的唯一标识符，可以通过它来取消超时调用。要取消尚未执行的超时调用计划，可以调用clearTimeout()方法并将相应的超时调用 ID 作为参数传递给它

      ```js
      //设置超时调用
      var timeoutId = setTimeout(function() {
       	alert("Hello world!");
      }, 1000);
      //注意：把它取消
      clearTimeout(timeoutId);
      ```

    - 超时调用的代码都是在全局作用域中执行的，因此函数中 this 的值在非严格模式下指向 window 对象，在严格模式下是 undefined

  - 超时调用：在指定的时间过后执行代码

    - 使用 window 对象的 是 setInterval()方法，它接受两个参数：要执行的代码和以毫秒表示的时间（即在执行代码前需要等待多少毫秒）

      ```js
      //不建议传递字符串！
      setInterval ("alert('Hello world!') ", 10000);
      //推荐的调用方式
      setInterval (function() {
       alert("Hello world!");
      }, 10000); 
      ```

    - 调用 setInterval()方法同样也会返回一个间歇调用 ID，该 ID 可用于在将来某个时刻取消间歇调用。要取消尚未执行的间歇调用，可以使用 clearInterval()方法并传入相应的间歇调用 ID。取消间歇调用的重要性要远远高于取消超时调用，因为在不加干涉的情况下，间歇调用将会一直执行到页面卸载

      ```js
      //间歇调用方式
      var num = 0;
      var max = 10;
      var intervalId = null;
      function incrementNumber() {
       	num++;
       	//如果执行次数达到了 max 设定的值，则取消后续尚未执行的调用
       	if (num == max) {
       		clearInterval(intervalId);
       		alert("Done");
       	}
      }
      intervalId = setInterval(incrementNumber, 500); 
      
      //超时调用实现（最佳模式）
      var num = 0;
      var max = 10;
      function incrementNumber() {
       	num++;
       	//如果执行次数未达到 max 设定的值，则设置另一次超时调用
       	if (num < max) {
       		setTimeout(incrementNumber, 500);
       	} else {
       		alert("Done");
       	}
      }
      setTimeout(incrementNumber, 500); 
      ```

- 系统对话框

  - alert()方法：接受一个字符串并将其显示给用户。具体来说，调用alert()方法的结果就是向用户显示一个系统对话框，其中包含指定的文本和一个 OK（“确定”）按钮

  - confirm()方法：向用户显示一个系统对话框，除了显示 OK 按钮外，还会显示一个 Cancel（“取消”）按钮，两个按钮可以让用户决定是否执行给定的操作；true 表示单击了 OK，false 表示单击了 Cancel 或单击了右上角的 X 按钮

    ```js
    if (confirm("Are you sure?")) {
     	alert("I'm so glad you're sure! ");
    } else {
     	alert("I'm sorry to hear you're not sure. ");
    } 
    ```

  - prompt()方法：这是一个“提示”框，用于提示用户输入一些文本。提示框中除了显示 OK 和 Cancel 按钮之外，还会显示一个文本输入域，以供用户在其中输入内容；接受两个参数：要显示给用户的文本提示和文本输入域的默认值（可以是一个空字符串）；如果用户单击了 OK 按钮，则 prompt()返回文本输入域的值；如果用户单击了 Cancel 或没有单击OK 而是通过其他方式关闭了对话框，则该方法返回 null

    ```js
    var result = prompt("What is your name? ", "");
    if (result !== null) {
     	alert("Welcome, " + result);
    }
    ```

  - 除了上述三种对话框之外，Google Chrome 浏览器还引入了一种新特性。如果当前脚本在执行过程中会打开两个或多个对话框，那么从第二个对话框开始，每个对话框中都会显示一个复选框，以便用户阻止后续的对话框显示，除非用户刷新页面

  - 还有两个可以通过 JavaScript 打开的对话框，即“查找”和“打印”。这两个对话框都是异步显示的，能够将控制权立即交还给脚本。这两个对话框与用户通过浏览器菜单的“查找”和“打印”命令打开的对话框相同。而在 JavaScript 中则可以像下面这样通过 window 对象的 find()和 print()方法打开它们

    ```js
    //显示“打印”对话框
    window.print();
    //显示“查找”对话框
    window.find(); 
    ```

#### 2、location对象

- 属性：

  | 属 性 名 | 例 子                                                 | 说 明                                                        |
  | :------- | :---------------------------------------------------- | :----------------------------------------------------------- |
  | hash     | "#contents"                                           | 返回URL中的hash（#号后跟零或多个字符），如果URL中不包含散列，则返回空字符串 |
  | host     | "www.wrox.com:80"                                     | 返回服务器名称和端口号（如果有）                             |
  | hostname | "www.wrox.com"                                        | 返回不带端口号的服务器名称                                   |
  | href     | "http://www.wrox.com"                                 | 返回当前加载页面的完整URL。而location对象的<brtoString()方法也返回这个值 |
  | pathname | "/WileyCDA/" 返回URL中的目录和（或）文件名port "8080" | 返回URL中的目录和（或）文件名                                |
  | port     | "8080"                                                | 返回URL中指定的端口号。如果URL中不包含端口号，则这个属性返回空字符串 |
  | protocol | "http:"                                               | 返回页面使用的协议。通常是http:或https:                      |
  | search   | "?q=javascript"                                       | 返回URL的查询字符串。这个字符串以问号开头                    |

- 查询字符串参数

  ```js
  function getQueryStringArgs(){
   	//取得查询字符串并去掉开头的问号
   	var qs = (location.search.length > 0 ? location.search.substring(1) : ""),
   	//保存数据的对象
   	args = {},
  
   	//取得每一项
   	items = qs.length ? qs.split("&") : [],
   	item = null,
   	name = null, value = null,
   	//在 for 循环中使用
   	i = 0,
   	len = items.length;
   	//逐个将每一项添加到 args 对象中
   	for (i=0; i < len; i++){
  		 item = items[i].split("=");
   		name = decodeURIComponent(item[0]);
   		value = decodeURIComponent(item[1]);
   		if (name.length) {
   			args[name] = value;
   		}
   	}
   	return args;
  }
  //假设查询字符串是?q=javascript&num=10
  var args = getQueryStringArgs();
  alert(args["q"]); //"javascript"
  alert(args["num"]); //"10" 
  ```

- 位置操作

  - assign()方法

    ```js
    location.assign("http://www.wrox.com"); 
    ```

  - location.href属性

    ```js
    location.href = "http://www.wrox.com"; 
    ```

  - 每次修改 location 的属性（hash 除外），页面都会以新 URL 重新加载

    ```js
    //假设初始 URL 为 http://www.wrox.com/WileyCDA/
    //将 URL 修改为"http://www.wrox.com/WileyCDA/#section1"
    location.hash = "#section1";
    //将 URL 修改为"http://www.wrox.com/WileyCDA/?q=javascript"
    location.search = "?q=javascript";
    //将 URL 修改为"http://www.yahoo.com/WileyCDA/"
    location.hostname = "www.yahoo.com";
    //将 URL 修改为"http://www.yahoo.com/mydir/"
    location.pathname = "mydir";
    //将 URL 修改为"http://www.yahoo.com:8080/WileyCDA/"
    location.port = 8080; 
    ```

  - replace()方法：不会在历史记录中生成新记录

    ```html
    <!DOCTYPE html>
    <html>
    <head>
     <title>You won't be able to get back here</title>
    </head>
     <body>
     <p>Enjoy this page for a second, because you won't be coming back here.</p>
     <script type="text/javascript">
     	setTimeout(function () {
     		location.replace("http://www.wrox.com/");
     	}, 1000);
     </script>
    </body>
    </html> 
    ```

  - reload()方法：重新加载当前显示的页面

    ```js
    location.reload(); //重新加载（有可能从缓存中加载）
    location.reload(true); //重新加载（从服务器重新加载）
    ```

#### 3、navigation对象

- 属性

  ```js
  window.navigator 
  ```

- 检测插件

  - 对于非 IE 浏览器，可以使用plugins 数组来达到这个目的。该数组中的每一项都包含下列属性

    - name：插件的名字
    - description：插件的描述
    - filename：插件的文件名
    - length：插件所处理的 MIME 类型数量

    ```js
    //检测插件（在 IE 中无效）
    function hasPlugin(name){
     	name = name.toLowerCase();
     	for (var i=0; i < navigator.plugins.length; i++){
     		if (navigator. plugins [i].name.toLowerCase().indexOf(name) > -1){
     			return true;
     		}
     	}
     	return false;
    }
    //检测 Flash
    alert(hasPlugin("Flash"));
    //检测 QuickTime
    alert(hasPlugin("QuickTime"));
    ```

  - 在 IE 中检测插件的唯一方式就是使用专有的 ActiveXObject 类型，并尝试创建一个特定插件的实例

    ```js
    //检测 IE 中的插件
    function hasIEPlugin(name){
     	try {
     		new ActiveXObject(name);
     		return true;
     	} catch (ex){
    		 return false;
     	}
    }
    //检测 Flash
    alert(hasIEPlugin("ShockwaveFlash.ShockwaveFlash"));
    //检测 QuickTime
    alert(hasIEPlugin("QuickTime.QuickTime")); 
    ```

  - 典型的做法是针对每个插件分别创建检测函数

    ```js
    //检测所有浏览器中的 Flash
    function hasFlash(){
     	var result = hasPlugin("Flash");
     	if (!result){
     		result = hasIEPlugin("ShockwaveFlash.ShockwaveFlash");
     	} 
        return result;
    }
    //检测所有浏览器中的 QuickTime
    function hasQuickTime(){
     	var result = hasPlugin("QuickTime");
     	if (!result){
     		result = hasIEPlugin("QuickTime.QuickTime");
     	}
     	return result;
    }
    //检测 Flash
    alert(hasFlash());
    //检测 QuickTime
    alert(hasQuickTime()); 
    ```

  - plugins 集合有一个名叫 refresh()的方法，用于刷新 plugins 以反映最新安装的插件。这个方法接收一个参数：表示是否应该重新加载页面的一个布尔值。如果将这个值设置为 true，则会重新加载包含插件的所有页面；否则，只更新 plugins集合，不重新加载页面

- 注册处理程序

  - registerContentHandler()方法接收三个参数：要处理的 MIME 类型、可以处理该 MIME类型的页面的 URL 以及应用程序的名称

    ```js
    navigator.registerContentHandler("application/rss+xml",
     "http://www.somereader.com?feed=%s", "Some Reader"); 
    ```

  - registerProtocolHandler()方法接收三个参数：要处理的协议（例如，mailto 或 ftp）、处理该协议的页面的 URL 和应用程序的名称

    ```js
    navigator.registerProtocolHandler("mailto",
     "http://www.somemailclient.com?cmd=%s", "Some Mail Client"); 
    ```

#### 4、screen对象

- screen 对象基本上只用来表明客户端的能力，其中包括浏览器窗口外部的显示器的信息，如像素宽度和高度等；这些信息经常集中出现在测定客户端能力的站点跟踪工具中，但通常不会用于影响功能。不过，有时候也可能会用到其中的信息来调整浏览器窗口大小，使其占据屏幕的可用空间

  ```js
  window.resizeTo(screen.availWidth, screen.availHeight); 
  ```

- 涉及移动设备的屏幕大小时，情况有点不一样。运行 iOS 的设备始终会像是把设备竖着拿在手里一
  样，因此返回的值是 768×1024。而 Android 设备则会相应调用 screen.width 和 screen.height 的值

#### 5、history对象

- history 对象保存着用户上网的历史记录，从窗口被打开的那一刻算起。因为 history 是 window对象的属性，因此每个浏览器窗口、每个标签页乃至每个框架，都有自己的 history 对象与特定的window 对象关联

- 使用 go()方法可以在用户的历史记录中任意跳转，可以向后也可以向前。这个方法接受一个参数，表示向后或向前跳转的页面数的一个整数值。负数表示向后跳转（类似于单击浏览器的“后退”按钮），正数表示向前跳转（类似于单击浏览器的“前进”按钮）

  ```js
  //后退一页
  history.go(-1);
  //前进一页
  history.go(1);
  //前进两页
  history.go(2); 
  ```

- 也可以给 go()方法传递一个字符串参数，此时浏览器会跳转到历史记录中包含该字符串的第一个位置——可能后退，也可能前进，具体要看哪个位置最近。如果历史记录中不包含该字符串，那么这个方法什么也不做

  ```js
  //跳转到最近的 wrox.com 页面
  history.go("wrox.com");
  //跳转到最近的 nczonline.net 页面
  history.go("nczonline.net"); 
  ```

- 另外，还可以使用两个简写方法 back()和 forward()来代替 go()。顾名思义，这两个方法可以模仿浏览器的“后退”和“前进”按钮

  ```js
  //后退一页
  history.back();
  //前进一页
  history.forward();
  ```

- history 对象还有一个 length 属性，保存着历史记录的数量。这个数量包括所有历史记录，即所有向后和向前的记录。对于加载到窗口、标签页或框架中的第一个页面而言，history.length 等于 0。通过像下面这样测试该属性的值，可以确定用户是否一开始就打开了你的页面

  ```js
  if (history.length == 0){
   //这应该是用户打开窗口后的第一个页面
  }
  ```

#### 6、总结

1、浏览器对象模型（BOM）以 window 对象为依托，表示浏览器窗口以及页面可见区域。同时，window
对象还是 ECMAScript 中的 Global 对象，因而所有全局变量和函数都是它的属性，且所有原生的构造函数及其他函数也都存在于它的命名空间下。本章讨论了下列 BOM 的组成部分：

- 在使用框架时，每个框架都有自己的 window 对象以及所有原生构造函数及其他函数的副本。每个框架都保存在 frames 集合中，可以通过位置或通过名称来访问。
- 有一些窗口指针，可以用来引用其他框架，包括父框架。
-  top 对象始终指向最外围的框架，也就是整个浏览器窗口。
- parent 对象表示包含当前框架的框架，而 self 对象则回指 window。
- 使用 location 对象可以通过编程方式来访问浏览器的导航系统。设置相应的属性，可以逐段或整体性地修改浏览器的 URL。
- 调用 replace()方法可以导航到一个新 URL，同时该 URL 会替换浏览器历史记录中当前显示的页面。
- navigator 对象提供了与浏览器有关的信息。到底提供哪些信息，很大程度上取决于用户的浏览器；不过，也有一些公共的属性（如 userAgent）存在于所有浏览器中

2、BOM 中还有两个对象：screen 和 history，但它们的功能有限。screen 对象中保存着与客户端显示器有关的信息，这些信息一般只用于站点分析。history 对象为访问浏览器的历史记录开了一个小缝隙，开发人员可以据此判断历史记录的数量，也可以在历史记录中向后或向前导航到任意页面

