## 函数表达式

#### 1、定义函数

- 函数声明定义函数

  ```js
  function functionName(arg0, arg1, arg2) {
   	//函数体
  } 
  //只在 Firefox、Safari、Chrome 和 Opera 有效
  alert(functionName.name); //"functionName" 
  ```

- 函数声明提升

  ```js
  //在执行代码之前会先读取函数声明
  sayHi();
  function sayHi(){
   alert("Hi!");
  } 
  ```

- 函数表达式定义函数

  ```js
  var functionName = function(arg0, arg1, arg2){
   	//函数体
  };
  ```

- 匿名函数（拉姆达函数）

  ```js
  //没有函数提升
  sayHi(); //错误：函数还不存在
  var sayHi = function(){
   	alert("Hi!");
  };
  
  //不要在if语句中使用函数声明
  if(condition){
   	function sayHi(){
   		alert("Hi!");
   	}
  } else {
   	function sayHi(){
   		alert("Yo!");
   }
  }
  //使用函数表达式定义函数
  var sayHi;
  if(condition){
   	sayHi = function(){
   		alert("Hi!");
   	};
  } else {
   	sayHi = function(){
   		alert("Yo!");
   	};
  } 
  ```

- 函数作为其他函数的值返回

  ```js
  function createComparisonFunction(propertyName) {
   	return function(object1, object2){
   		var value1 = object1[propertyName];
   		var value2 = object2[propertyName];
          if (value1 < value2){
   			return -1;
   		} else if (value1 > value2){
   			return 1;
   		} else {
  			return 0;
   		}
   	};
  } 
  ```

#### 2、递归

- 递归函数是在一个函数通过名字调用自身的情况下构成的

  ```js
  function factorial(num){
   	if (num <= 1){
   		return 1;
   	} else {
   		return num * factorial(num-1);
   	}
  }
  var anotherFactorial = factorial;
  factorial = null;
  alert(anotherFactorial(4)); //出错！
  ```

- arguments.callee 是一个指向正在执行的函数的指针，因此可以用它来实现对函数的递归调用

  ```js
  function factorial(num){
   	if (num <= 1){
   		return 1;
   	} else {
   		return num * arguments.callee(num-1);
   	}
  } 
  ```

#### 3、闭包

- 概念：指有权访问另一个函数作用域中的变量的函数

  - 创建闭包的常见方式，就是在一个函数内部创建另一个函数

    ```js
    function createComparisonFunction(propertyName) {
     	return function(object1, object2){
     		var value1 = object1[propertyName];
     		var value2 = object2[propertyName];
     		if (value1 < value2){
     			return -1;
     		} else if (value1 > value2){
     			return 1;
     		} else {
     			return 0;
     		}
     	};
    } 
    ```

  - 原理：

    - 当某个函数被调用时，会创建一个执行环境（execution context）及相应的作用域链，然后，使用 arguments 和其他命名参数的值来初始化函数的活动对象（activation object），但在作用域链中，外部函数的活动对象始终处于第二位，外部函数的外部函数的活动对象处于第三位，直至作为作用域链终点的全局执行环境。
    - 在函数执行过程中，为读取和写入变量的值，就需要在作用域链中查找变量，无论什么时候在函数中访问一个变量时，就会从作用域链中搜索具有相应名字的变量。一般来讲，当函数执行完毕后，局部活动对象就会被销毁，内存中仅保存全局作用域（全局执行环境的变量对象）
    - 但是，闭包的情况又有所不同，在另一个函数内部定义的函数会将包含函数（即外部函数）的活动对象添加到它的作用域链中，函数在执行完毕后，其活动对象也不会被销毁，因为匿名函数的作用域链仍然在引用这个活动对象
    - 由于闭包会携带包含它的函数的作用域，因此会比其他函数占用更多的内存。过度使用闭包可能会导致内存占用过多，建议只在绝对必要时再考虑使用闭包

- 闭包与变量

  - 闭包只能取得包含函数中任何变量的最后一个值

    ```js
    //在每个函数内部 i 的值都是 10
    function createFunctions(){
     	var result = new Array();
     	for (var i=0; i < 10; i++){
     		result[i] = function(){
     			return i;
     		};
     	}
     	return result;
    }
    createFunctions();
    ```

  - 创建另一个匿名函数强制让闭包的行为符合预期:

    ```js
    //每个函数都应该返自己的索引值
    function createFunctions(){
     	var result = new Array();
     	for (var i=0; i < 10; i++){
     		result[i] = function(num){
     			return function(){
     				return num;
     			};
     		}(i);
     	}
     	return result;
    } 
    ```

- 关于this对象

  - 一般情况：this 对象是在运行时基于函数的执行环境绑定的

    - 在全局函数中，this 等于 window
    - 当函数被作为某个对象的方法调用时，this 等于那个对象

  - 匿名函数的this对象：

    - 匿名函数的执行环境具有全局性，其 this 对象通常指向 window

      ```js
      var name = "The Window";
      var object = {
       	name : "My Object",
       	getNameFunc : function(){
       		return function(){
       			return this.name;
       		};
       	}
      };
      alert(object.getNameFunc()()); //"The Window"（在非严格模式下）
      ```

    - 解决办法：把外部作用域中的 this 对象保存在一个闭包能够访问到的变量里

      ```js
      var name = "The Window";
      var object = {
       	name : "My Object",
       	getNameFunc : function(){
              var that = this;
       		return function(){
       			return that.name;
       		};
       	}
      };
      alert(object.getNameFunc()()); //"My Object" 
      ```

  - this 的值可能会意外地改变的特殊情况：

    ```js
    var name = "The Window";
    var object = {
     	name : "My Object",
     	getName: function(){
     		return this.name;
     	}
    }; 
    object.getName(); //"My Object"
    (object.getName)(); //"My Object"
    (object.getName = object.getName)(); //"The Window"，在非严格模式下
    ```

- 内存泄漏

  - 闭包的作用域链中保存着一个HTML 元素，那么就意味着该元素将无法被销毁

    ```js
    function assignHandler(){
     	var element = document.getElementById("someElement");
     	element.onclick = function(){
     		alert(element.id);
     	};
    }
    ```

  - 解决办法：

    ```js
    function assignHandler(){
     	var element = document.getElementById("someElement");
     	var id = element.id;
     	element.onclick = function(){
     		alert(id);
     	};
     	element = null;
    }
    ```

#### 4、模仿块级作用域

- JavaScript 没有块级作用域：在块语句中定义的变量，实际上是在包含函数中而非语句中创建的

  ```js
  function outputNumbers(count){
   	for (var i=0; i < count; i++){
   		alert(i);
   	}
      var i; //重新声明变量,也不会改变它的值
   	alert(i); //计数
  }
  ```

- 使用自调用函数产生块级作用域（通常称为私有作用域）

  ```js
  function outputNumbers(count){
   	(function () {
   		for (var i=0; i < count; i++){
   			alert(i);
   		}
   	})();
   	alert(i); //导致一个错误！
  } 
  ```

- 这种技术经常在全局作用域中被用在函数外部，从而限制向全局作用域中添加过多的变量和函数。这种做法可以减少闭包占用的内存问题，因为没有指向匿名函数的引用。只要函数执行完毕，就可以立即销毁其作用域链

  ```js
  (function(){
   	var now = new Date();
   	if (now.getMonth() == 0 && now.getDate() == 1){
   		alert("Happy new year!");
   	}
  })(); 
  ```

#### 5、私有变量

- 基本概念：

  - 严格来讲，JavaScript 中没有私有成员的概念；所有对象属性都是公有的，不过，倒是有一个私有变量的概念。任何在函数中定义的变量，都可以认为是私有变量，因为不能在函数的外部访问这些变量。私有变量包括函数的参数、局部变量和在函数内部定义的其他函数。

    ```js
    function add(num1, num2){
     	var sum = num1 + num2;
     	return sum;
    }
    ```

  - 特权方法：有权访问私有变量和私有函数的公有方法

  - 创建特权方法的方式之一：在构造函数中定义特权方法

    - 基本模式：

      ```js
      function MyObject(){
       	//私有变量和私有函数
       	var privateVariable = 10;
       	function privateFunction(){
       		return false;
       	}
       	//特权方法
       	this.publicMethod = function (){
       	privateVariable++;
       		return privateFunction();
       	};
      } 
      ```

    - 利用私有和特权成员，可以隐藏那些不应该被直接修改的数据

      ```js
      function Person(name){
       	this.getName = function(){
       		return name;
       	};
       	this.setName = function (value) {
       		name = value;
       	};
      }
      var person = new Person("Nicholas");
      alert(person.getName()); //"Nicholas"
      person.setName("Greg");
      alert(person.getName()); //"Greg" 
      ```

    - 缺点：必须使用构造函数模式来达到这个目的，构造函数模式的缺点是针对每个实例都会创建同样一组新方法，而使用静态私有变量来实现特权方法就可以避免这个问题

  - 创建特权方法的方式之二：静态私有变量（见下）

- 静态私有变量

  - 基本模式

    ```js
    (function(){
     	//私有变量和私有函数
     	var privateVariable = 10;
     	function privateFunction(){
     		return false;
     	}
     	//构造函数
     	MyObject = function(){};
     	//公有/特权方法
    	 MyObject.prototype.publicMethod = function(){
     		privateVariable++;
     		return privateFunction();
     	};
    })(); 
    ```

  - 与在构造函数中定义特权方法的主要区别：私有变量和函数是由实例共享的。由于特权方法是在原型上定义的，因此所有实例都使用同一个函数。而这个特权方法，作为一个闭包，总是保存着对包含作用域的引用。

    ```js
    (function(){
     	var name = "";
    	Person = function(value){
     		name = value;
    	};
    	Person.prototype.getName = function(){
     		return name;
    	};
    	Person.prototype.setName = function (value){
        	name = value;
    	};
    })();
    var person1 = new Person("Nicholas");
    alert(person1.getName()); //"Nicholas"
    person1.setName("Greg");
    alert(person1.getName()); //"Greg"
    var person2 = new Person("Michael");
    alert(person1.getName()); //"Michael"
    alert(person2.getName()); //"Michael" 
    ```

  - 优点：以这种方式创建静态私有变量会因为使用原型而增进代码复用，但每个实例都没有自己的私有变
    量。多查找作用域链中的一个层次，就会在一定程度上影响查找速度。而这正是使用闭包和私有变量的一个显明的不足之处

- 模块模式

  - 单例：指的就是只有一个实例的对象

    ```js
    var singleton = {
     	name : value,
     	method : function () {
     	//这里是方法的代码
    	}	
    }; 
    ```

  - 模块模式：为单例创建私有变量和特权方法

    ```js
    var singleton = function(){
     	//私有变量和私有函数
     	var privateVariable = 10;
     	function privateFunction(){
     		return false;
     	} //特权/公有方法和属性
     	return {
     		publicProperty: true,
     		publicMethod : function(){
     			privateVariable++;
     			return privateFunction();
     		}
     	};
    }(); 
    ```

  - 从本质上来讲，这个对象字面量定义的是单例的公共接口。这种模式在需要对单例进行某些初始化，同时又需要维护其私有变量时是非常有用的

    ```js
    var application = function () {
        //私有变量和函数
        var components = new Array();
        //初始化
        components.push(new BaseComponent());
        //公共
        return {
            getComponentCount: function () {
                return components.length;
            },
            registerComponent: function (component) {
                if (typeof component == "object") {
                    components.push(component);
                }
            }
        };
    }();
    ```

  - 如果必须创建一个对象并以某些数据对其进行初始化，同时还要公开一些能够访问这些私有数据的方法，那么就可以使用模块模式。以这种模式创建的每个单例都是 Object 的实例，因为最终要通过一个对象字面量来表示它。事实上，这也没有什么；毕竟，单例通常都是作为全局对象存在的，我们不会将它传递给一个函数。因此，也就没有什么必要使用` instanceof `操作符来检查其对象类型了

- 增强的模块模式

  - 增强：在返回对象之前加入对其增强的代码的，这种增强的模块模式适合那些单例必须是某种类型的实例，同时还必须添加某些属性和（或）方法对其加以增强的情况

    ```js
    var singleton = function(){
     	//私有变量和私有函数
     	var privateVariable = 10;
     	function privateFunction(){
     		return false;
    	}
     	//创建对象
     	var object = new CustomType();
     	//添加特权/公有属性和方法
     	object.publicProperty = true;
     	object.publicMethod = function(){
     		privateVariable++;
     		return privateFunction();
     	};
     	//返回这个对象
     	return object;
    }(); 
    ```

  - 或者这样写：

    ```js
    var application = function(){
     	//私有变量和函数
     	var components = new Array();
     	//初始化
     	components.push(new BaseComponent());
     	//创建 application 的一个局部副本
     	var app = new BaseComponent();
     	//公共接口
     	app.getComponentCount = function(){
     		return components.length;
     	};
     	app.registerComponent = function(component){
     		if (typeof component == "object"){
     			components.push(component);
     		}
     	};
     	//返回这个副本
        return app;
    }(); 
    ```


#### 6、总结

1、在 JavaScript 编程中，函数表达式是一种非常有用的技术。使用函数表达式可以无须对函数命名，从而实现动态编程。匿名函数，也称为拉姆达函数，是一种使用 JavaScript 函数的强大方式。以下总结了函数表达式的特点：

- 函数表达式不同于函数声明。函数声明要求有名字，但函数表达式不需要。没有名字的函数表达式也叫做匿名函数;
- 在无法确定如何引用函数的情况下，递归函数就会变得比较复杂；
- 递归函数应该始终使用 arguments.callee 来递归地调用自身，不要使用函数名——函数名可能会发生变化

2、当在函数内部定义了其他函数时，就创建了闭包。闭包有权访问包含函数内部的所有变量，原理如下：

- 在后台执行环境中，闭包的作用域链包含着它自己的作用域、包含函数的作用域和全局作用域；
- 通常，函数的作用域及其所有变量都会在函数执行结束后被销毁；
- 但是，当函数返回了一个闭包时，这个函数的作用域将会一直在内存中保存到闭包不存在为止；

3、使用闭包可以在 JavaScript 中模仿块级作用域（JavaScript 本身没有块级作用域的概念），要点如下：

- 创建并立即调用一个函数，这样既可以执行其中的代码，又不会在内存中留下对该函数的引用；
- 结果就是函数内部的所有变量都会被立即销毁——除非将某些变量赋值给了包含作用域（即外部作用域）中的变量；

4、闭包还可以用于在对象中创建私有变量，相关概念和要点如下：

- 即使 JavaScript 中没有正式的私有对象属性的概念，但可以使用闭包来实现公有方法，而通过公有方法可以访问在包含作用域中定义的变量；
- 有权访问私有变量的公有方法叫做特权方法；
- 可以使用构造函数模式、原型模式来实现自定义类型的特权方法，也可以使用模块模式、增强的模块模式来实现单例的特权方法；

5、JavaScript 中的函数表达式和闭包都是极其有用的特性，利用它们可以实现很多功能。不过，因为创建闭包必须维护额外的作用域，所以过度使用它们可能会占用大量内存