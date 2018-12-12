## 客户端检测

#### 1、能力检测

- 基本概念

  - 定义：最常用也最为人们广泛接受的客户端检测形式是能力检测（又称特性检测）

  - 目标：不是识别特定的浏览器，而是识别浏览器的能力,，基本模式如下：

    ```js
    if (object.propertyInQuestion){
     //使用 object.propertyInQuestion
    }
    ```

  - 举例：

    ```js
    function getElement(id){
     	if (document.getElementById){
     		return document.getElementById(id);
     	} else if (document.all){
     		return document.all[id]; 
        } else {
     		throw new Error("No way to retrieve element!");
     	}
    } 
    ```

  - 要理解能力检测，首先必须理解两个重要的概念：

    - 先检测达成目的的最常用的特性
    - 必须测试实际要用到的特性

    ```js
    function getWindowWidth(){
     	if (document.all){ //假设是 IE
     		return document.documentElement.clientWidth; //错误的用法！！！
     	} else {
     		return window.innerWidth;
     	}
    } 
    ```

- 更可靠的能力检测

  - 错误的能力检测：

    ```js
    //不要这样做！这不是能力检测——只检测了是否存在相应的方法
    function isSortable(object){
     	return !!object.sort;//任何包含 sort属性的对象也会返回 true
    } 
    var result = isSortable({ sort: true }); 
    ```

  - 在可能的情况下，要尽量使用 `typeof `进行能力检测

    ```js
    //这样更好：检查 sort 是不是函数
    function isSortable(object){
     	return typeof object.sort == "function";
    } 
    ```

  - 关于`document.createElement()`的检测

    - 大多数浏览器在检测到` document.createElement()`存在时，都会返回 true

    - 在 IE8 及之前版本中，返回 false，因为 `typeof document.createElement` 返回的是"object"，而不是"function"；`document.createElement()`函数确实是一个 COM 对象,不是DOM 对象

      ```js
       //在 IE8 及之前版本中不行
      function hasCreateElement(){
       	return typeof document.createElement == "function";
      } 
      ```

    - `IE9` 纠正了这个问题，对所有 DOM 方法都返回"function"

  - 关于`ActiveX` 对象（只有 IE 支持）的检测

    - 不使用` typeof `测试某个属性会导致错误

      ```js
      //在 IE 中会导致错误
      var xhr = new ActiveXObject("Microsoft.XMLHttp");
      if (xhr.open){ //这里会发生错误
       	//执行操作
      } 
      ```

    - 使用 `typeof `操作符：注意但 IE对 `typeof xhr.open `会返回"unknown"

      ```js
      //作者：Peter Michaux
      function isHostMethod(object, property) {
       	var t = typeof object[property];
       	return t=='function' ||(!!(t=='object' && object[property])) ||
                 t=='unknown';
      } 
      result = isHostMethod(xhr, "open"); //true
      result = isHostMethod(xhr, "foo"); //false 
      ```

- 能力检测和浏览器检测

  - 检测某个或某几个特性并不能够确定浏览器

    ```js
    //错误！还不够具体
    var isFirefox = !!(navigator.vendor && navigator.vendorSub);
    //错误！假设过头了
    var isIE = !!(document.all && document.uniqueID); 
    ```

  - 根据浏览器不同将能力组合起来是更可取的方式

    ```js
    //确定浏览器是否支持 Netscape 风格的插件
    var hasNSPlugins = !!(navigator.plugins && navigator.plugins.length);
    //确定浏览器是否具有 DOM1 级规定的能力
    var hasDOM1 = !!(document.getElementById && document.createElement &&
     document.getElementsByTagName); 
    ```

  - 在实际开发中，应该将能力检测作为确定下一步解决方案的依据，而不是用它来判断用户使用的是什么浏览器

#### 2、怪癖检测

- 概念：与能力检测类似，目标是识别浏览器的特殊行为；但与能力检测确认浏览器支持什么能力不同，怪癖检测是想要知道浏览器存在什么缺陷（“怪癖”也就是 bug）这通常需要运行一小段代码，以确定某一特性不能正常工作

- 举例：

  - IE8 及更早版本中存在一个 bug，即如果某个实例属性与[[Enumerable]]标记为 false 的某个原型属性同名，那么该实例属性将不会出现在fon-in 循环当中

    ```js
    var hasDontEnumQuirk = function(){
    	var o = { toString : function(){} };
    	for (var prop in o){ 
         	if (prop == "toString"){return false;}
    	}
     	return true;
    }(); 
    ```

  - Safari 3 以前版本会枚举被隐藏的属性

    ```js
    var hasEnumShadowsQuirk = function(){
     	var o = { toString : function(){} };
     	var count = 0;
     	for (var prop in o){
     		if (prop == "toString"){
     			count++;
     		}
    	}
     	return (count > 1);
    }(); 
    ```

#### 3、用户代理检测

- 概念：
  - 用户代理检测：通过检测用户代理字符串来确定实际使用的浏览器；在每一次 HTTP 请求过程中，用户代理字符串是作为响应首部发送的，而且该字符串可以通过 JavaScript 的 `navigator.userAgent `属性访问；在服务器端，通过检测用户代理字符串来确定用户使用的浏览器是一种常用而且广为接受的做法，而在客户端，用户代理检测一般被当作一种万不得已才用的做法，其优先级排在能力检测和（或）怪癖检测之后
  - 电子欺骗：就是指浏览器通过在自己的用户代理字符串加入一些错误或误导性信息，来达到欺骗服务器的目的

- 用户代理字符串的历史

  - 早期的浏览器：`Mozilla/版本号 [语言] (平台; 加密类型) `
  - Netscape Navigator 3 ：`Mozilla/版本号 (平台; 加密类型 [; 操作系统或 CPU 说明])`
  - Internet Explorer 3 ：`Mozilla/2.0 (compatible; MSIE 版本号; 操作系统) `
  -  Netscape Communicator 4：`Mozilla/版本号 (平台; 加密类型 [; 操作系统或 CPU 说明]) `
  - IE4～IE8：`Mozilla/4.0 (compatible; MSIE 版本号; 操作系统; Trident/Trident 版本号) `
  - Gecko：`Mozilla/Mozilla 版本号 (平台; 加密类型; 操作系统或 CPU; 语言; 预先发行版本)
    Gecko/Gecko 版本号 应用程序或产品/应用程序或产品版本号`
  - WebKit :`Mozilla/5.0 (平台; 加密类型; 操作系统或 CPU; 语言) AppleWebKit/AppleWebKit 版本号
    (KHTML, like Gecko) Safari/Safari 版本号`
  -  Konqueror:`Mozilla/5.0 (compatible; Konqueror/ 版本号; 操作系统或 CPU) KHTML/ KHTML 版本号 (like Gecko) `
  -  Chrome :`Mozilla/5.0 ( 平台; 加密类型; 操作系统或 CPU; 语言) AppleWebKit/AppleWebKit 版本号 (KHTML,like Gecko) Chrome/ Chrome 版本号 Safari/ Safari 版本`
  - Opera :`Opera/ 版本号 (操作系统或 CPU; 加密类型; 语言) `
  -  iOS 和 Android :`Mozilla/5.0 (平台; 加密类型; 操作系统或 CPU like Mac OS X; 语言)
    AppleWebKit/AppleWebKit 版本号 (KHTML, like Gecko) Version/浏览器版本号
    Mobile/移动版本号 Safari/Safari 版本号`

- 用户代理字符串检测技术

  ```js
  //用户代理字符串检测脚本，包括检测呈现引擎、平台、Windows 操作系统、移动设备和游戏系统
  var client = function(){
   	//呈现引擎
   	var engine = {
  		ie: 0,
   		gecko: 0,
   		webkit: 0,
  		khtml: 0,
   		opera: 0,
   		//完整的版本号
   		ver: null
   	};
   	//浏览器
   	var browser = {
   		//主要浏览器
   		ie: 0,
   		firefox: 0,
   		safari: 0,
   		konq: 0,
   		opera: 0,
          chrome: 0,
   		//具体的版本号
   		ver: null
      };
      //平台、设备和操作系统
   	var system = {
   		win: false,
   		mac: false,
   		x11: false,
   		//移动设备
   		iphone: false,
   		ipod: false,
   		ipad: false,
   		ios: false,
   		android: false,
   		nokiaN: false,
   		winMobile: false,
   		//游戏系统
   		wii: false,
   		ps: false
   	}; 
      //检测呈现引擎和浏览器
   	var ua = navigator.userAgent;
   	if (window.opera){
   		engine.ver = browser.ver = window.opera.version();
   		engine.opera = browser.opera = parseFloat(engine.ver);
   	} else if (/AppleWebKit\/(\S+)/.test(ua)){
   		engine.ver = RegExp["$1"];
   		engine.webkit = parseFloat(engine.ver);
   		//确定是 Chrome 还是 Safari
   		if (/Chrome\/(\S+)/.test(ua)){
   			browser.ver = RegExp["$1"];
   			browser.chrome = parseFloat(browser.ver);
   		} else if (/Version\/(\S+)/.test(ua)){
  		 	browser.ver = RegExp["$1"];
   			browser.safari = parseFloat(browser.ver);
   		} else {
   			//近似地确定版本号
   			var safariVersion = 1;
   			if (engine.webkit < 100){
   				safariVersion = 1;
   			} else if (engine.webkit < 312){
   				safariVersion = 1.2;
          	} else if (engine.webkit < 412){
   				safariVersion = 1.3;
   			} else {
   				safariVersion = 2;
   			}
   			browser.safari = browser.ver = safariVersion;
          }
   	} else if (/KHTML\/(\S+)/.test(ua) || /Konqueror\/([^;]+)/.test(ua)){
   		engine.ver = browser.ver = RegExp["$1"];
   		engine.khtml = browser.konq = parseFloat(engine.ver);
   	} else if (/rv:([^\)]+)\) Gecko\/\d{8}/.test(ua)){
   		engine.ver = RegExp["$1"];
   		engine.gecko = parseFloat(engine.ver);
   		//确定是不是 Firefox
   		if (/Firefox\/(\S+)/.test(ua)){
  			 browser.ver = RegExp["$1"];
   			 browser.firefox = parseFloat(browser.ver);
   		}
   	} else if (/MSIE ([^;]+)/.test(ua)){
   		engine.ver = browser.ver = RegExp["$1"];
   		engine.ie = browser.ie = parseFloat(engine.ver);
   	} 
      //检测浏览器
  	browser.ie = engine.ie;
   	browser.opera = engine.opera; 
  	//检测平台
   	var p = navigator.platform;
   	system.win = p.indexOf("Win") == 0;
   	system.mac = p.indexOf("Mac") == 0;
   	system.x11 = (p == "X11") || (p.indexOf("Linux") == 0); 
      //检测 Windows 操作系统
   	if (system.win){
   		if (/Win(?:dows )?([^do]{2})\s?(\d+\.\d+)?/.test(ua)){
   			if (RegExp["$1"] == "NT"){
   				switch(RegExp["$2"]){
   					case "5.0":
   						system.win = "2000";
   						break;
   					case "5.1":
   						system.win = "XP";
   						break;
   					case "6.0":
   						system.win = "Vista";
   						break;
   					case "6.1":
   						system.win = "7";
   						break;
   					default:
   						system.win = "NT";
  						break;
   				}
   			} else if (RegExp["$1"] == "9x"){
   				system.win = "ME";
   			} else {
   				system.win = RegExp["$1"];
   			}
  	 	}
   	}
      //移动设备
      system.iphone = ua.indexOf("iPhone") > -1;
   	system.ipod = ua.indexOf("iPod") > -1;
   	system.ipad = ua.indexOf("iPad") > -1;
   	system.nokiaN = ua.indexOf("NokiaN") > -1;
  	//windows mobile
   	if (system.win == "CE"){
   		system.winMobile = system.win;
   	} else if (system.win == "Ph"){
   		if(/Windows Phone OS (\d+.\d+)/.test(ua)){;
   			system.win = "Phone";
   			system.winMobile = parseFloat(RegExp["$1"]);
   		}
   	}
  
   	//检测 iOS 版本
   	if (system.mac && ua.indexOf("Mobile") > -1){
   		if (/CPU (?:iPhone )?OS (\d+_\d+)/.test(ua)){
   			system.ios = parseFloat(RegExp.$1.replace("_", "."));
   		} else {
   			system.ios = 2; //不能真正检测出来，所以只能猜测
   		}
   	}
   	//检测 Android 版本
   	if (/Android (\d+\.\d+)/.test(ua)){
   		system.android = parseFloat(RegExp.$1);
   	}
   	//游戏系统
   	system.wii = ua.indexOf("Wii") > -1;
  	 system.ps = /playstation/i.test(ua);
   	//返回这些对象
   	return {
   		engine: engine,
   		browser: browser,
   		system: system
   	};
  }(); 
  ```

- 使用方法：用户代理检测是客户端检测的最后一个选择。只要可能，都应该优先采用能力检测和怪癖检测。用户代理检测一般适用于下列情形：

  - 不能直接准确地使用能力检测或怪癖检测。例如，某些浏览器实现了为将来功能预留的存根（stub）函数。在这种情况下，仅测试相应的函数是否存在还得不到足够的信息

  - 同一款浏览器在不同平台下具备不同的能力。这时候，可能就有必要确定浏览器位于哪个平台下
  - 为了跟踪分析等目的需要知道确切的浏览器

#### 4、总结

- 客户端检测是 JavaScript 开发中最具争议的一个话题。由于浏览器间存在差别，通常需要根据不同浏览器的能力分别编写不同的代码。有不少客户端检测方法，但下列是最经常使用的：
  - 能力检测：在编写代码之前先检测特定浏览器的能力。例如，脚本在调用某个函数之前，可能要先检测该函数是否存在。这种检测方法将开发人员从考虑具体的浏览器类型和版本中解放出来，让他们把注意力集中到相应的能力是否存在上。能力检测无法精确地检测特定的浏览器和版本
  - 怪癖检测：怪癖实际上是浏览器实现中存在的 bug，例如早期的 WebKit 中就存在一个怪癖，即它会在 for-in 循环中返回被隐藏的属性。怪癖检测通常涉及到运行一小段代码，然后确定浏览器是否存在某个怪癖。由于怪癖检测与能力检测相比效率更低，因此应该只在某个怪癖会干扰脚本运行的情况下使用。怪癖检测无法精确地检测特定的浏览器和版本
  - 用户代理检测：通过检测用户代理字符串来识别浏览器。用户代理字符串中包含大量与浏览器有关的信息，包括浏览器、平台、操作系统及浏览器版本。用户代理字符串有过一段相当长的发展历史，在此期间，浏览器提供商试图通过在用户代理字符串中添加一些欺骗性信息，欺骗网站相信自己的浏览器是另外一种浏览器。用户代理检测需要特殊的技巧，特别是要注意 Opera会隐瞒其用户代理字符串的情况。即便如此，通过用户代理字符串仍然能够检测出浏览器所用的呈现引擎以及所在的平台，包括移动设备和游戏系统
- 在决定使用哪种客户端检测方法时，一般应优先考虑使用能力检测。怪癖检测是确定应该如何处理代码的第二选择。而用户代理检测则是客户端检测的最后一种方案，因为这种方法对用户代理字符串具有很强的依赖性

