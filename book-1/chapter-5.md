## 面向对象的程序设计

#### 1、理解对象

- 定义对象

  - 创建一个 Object 的实例

    ```js
    var person = new Object();
    person.name = "Nicholas";
    person.age = 29;
    person.job = "Software Engineer";
    person.sayName = function(){
     	alert(this.name);//"Nicholas"
    }; 
    ```

  - 使用对象字面量语法

    ```js
    var person = {
     	name: "Nicholas",
     	age: 29,
     	job: "Software Engineer",
     	sayName: function(){
     		alert(this.name);
     	}
    }; 
    ```

- 属性类型

  - 数据属性：包含一个数据值的位置，在这个位置可以读取和写入值

    - `[[Configurable]]`：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 true

    - `[[Enumerable]]`：表示能否通过 for-in 循环返回属性。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 true。

    - `[[Writable]]`：表示能否修改属性的值。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 true

    - `[[Value]]`：包含这个属性的数据值。读取属性值的时候，从这个位置读；写入属性值的时候，把新值保存在这个位置。这个特性的默认值为 undefined

    - 使用`Object.defineProperty()`方法：接收三个参数：属性所在的对象、属性的名字和一个描述符对象；其中，描述符（descriptor）对象的属性必须是：configurable、enumerable、writable 和 value

      ```js
      var person = {};
      Object.defineProperty(person, "name", {
       	writable: false,
       	value: "Nicholas"
      });
      alert(person.name); //"Nicholas"
      person.name = "Greg";
      alert(person.name); //"Nicholas"
      ```

  - 访问器属性

    - `[[Configurable]]`：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为数据属性。对于直接在对象上定义的属性，这个特性的默认值为true

    - `[[Enumerable]]`：表示能否通过 for-in 循环返回属性。对于直接在对象上定义的属性，这个特性的默认值为 true

    - `[[Get]]`：在读取属性时调用的函数。默认值为 undefined

    - `[[Set]]`：在写入属性时调用的函数。默认值为 undefined

    - 使用`Object.defineProperty()`方法操作

      ```js
      var book = {
       	_year: 2004,
       	edition: 1
      };
      Object.defineProperty(book, "year", {
       	get: function(){
       		return this._year;
       	},
       	set: function(newValue){
       		if (newValue > 2004) {
       			this._year = newValue;
       			this.edition += newValue - 2004;
      		}
       	}
      });
      book.year = 2005;
      alert(book.edition); //2 
      ```

    - 在不支持 `Object.defineProperty()` 方法的浏览器中使用两个非标准的方法`defineGetter__()`和`__defineSetter__()`操作，但是不能修改 [[Configurable]] 和[[Enumerable]]。

      ```js
      var book = {
       	_year: 2004,
       	edition: 1
      };
      //定义访问器的旧有方法
      book.__defineGetter__("year", function(){
       	return this._year;
      });
      book.__defineSetter__("year", function(newValue){
       	if (newValue > 2004) {
       		this._year = newValue;
       		this.edition += newValue - 2004;
       	}
      });
      book.year = 2005;
      alert(book.edition); //2 
      ```

- 定义多个属性

  - `Object.defineProperties()`方法：通过描述符一次定义多个属性

  - 接收两个对象参数：第一个对象是要添加和修改其属性的对象，第二个对象的属性与第一个对象中要添加或修改的属性一一对应

    ```js
    var book = {};
    Object.defineProperties(book, {
     	_year: {
     		value: 2004
    	},
    	edition: {
     		value: 1
    	},
    	year: {
     		get: function(){ 
    			return this._year;
     		},
     		set: function(newValue){
     			if (newValue > 2004) {
     				this._year = newValue;
     				this.edition += newValue - 2004;
     			}
     		}
     	}
    }); 
    ```

- 读取属性的特性

  - `Object.getOwnPropertyDescriptor()`方法；接收两个参数，属性所在的对象和要读取其描述符的属性名称；

  - 返回值是一个对象，如果是访问器属性，这个对象的属性有 configurable、enumerable、get 和 set；如果是数据属性，这个对象的属性有 configurable、enumerable、writable 和 value

    ```js
    var book = {};
    Object.defineProperties(book, {
    	_year: {
     		value: 2004
     	},
     	edition: {
     		value: 1
     	},
     	year: {
     		get: function(){
     		return this._year;
     	},
     	set: function(newValue){
     			if (newValue > 2004) {
     				this._year = newValue;
     				this.edition += newValue - 2004;
     			}
     		}
     	}
    });
    
    var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
    alert(descriptor.value); //2004
    alert(descriptor.configurable); //false 
    alert(typeof descriptor.get); //"undefined"
    
    var descriptor = Object.getOwnPropertyDescriptor(book, "year");
    alert(descriptor.value); //undefined
    alert(descriptor.enumerable); //false
    alert(typeof descriptor.get); //"function" 
    ```

#### 2、创建对象

- Object 构造函数或对象字面量可以创建单个对象，但使用同一个接口创建很多对象，会产生大量的重复代码

  ```js
  var person = new Object();
  var person = {};
  ```

- 工厂模式，解决了创建多个相似对象的问题，但却没有解决对象识别的问题（对象的类型）

  ```js
  function createPerson(name, age, job){
   	var o = new Object();
   	o.name = name;
   	o.age = age;
  	o.job = job;
   	o.sayName = function(){
   		alert(this.name);
   	};
   	return o;
  }
  var person1 = createPerson("Nicholas", 29, "Software Engineer");
  var person2 = createPerson("Greg", 27, "Doctor");
  ```

- 构造函数模式：

  - 语法：构造函数始终都应该以一个大写字母开头，创建实例必须使用 new 操作符

    ```js
    function Person(name, age, job){
     	this.name = name;
     	this.age = age;
     	this.job = job;
     	this.sayName = function(){
     		alert(this.name);
     	};
    }
    var person1 = new Person("Nicholas", 29, "Software Engineer");
    var person2 = new Person("Greg", 27, "Doctor"); 
    ```

  - 与工厂模式的不同之处

    - 没有显式地创建对象
    - 直接将属性和方法赋给了 this 对象
    - 没有 return 语句

  - 调用构造函数创建实例后台经历的步骤：

    - 创建一个新对象
    - 将构造函数的作用域赋给新对象（因此 this 就指向了这个新对象）
    - 执行构造函数中的代码（为这个新对象添加属性）
    - 返回新对象

  - 实例具有constructor（构造函数）属性：用来标识对象类型的

    ```js
    
    ```

  - 使用` instanceof`操作符检测对象类型

    ```js
    alert(person1 instanceof Object); //true
    alert(person1 instanceof Person); //true
    alert(person2 instanceof Object); //true
    alert(person2 instanceof Person); //true
    ```

  - 将构造函数当作函数

    ```js
    // 当作构造函数使用
    var person = new Person("Nicholas", 29, "Software Engineer");
    person.sayName(); //"Nicholas"
    // 作为普通函数调用
    Person("Greg", 27, "Doctor"); // 添加到 window
    window.sayName(); //"Greg"
    // 在另一个对象的作用域中调用
    var o = new Object();
    Person.call(o, "Kristen", 25, "Nurse");
    o.sayName(); //"Kristen" 
    ```

  - 构造函数的问题：

    - 每个方法都要在每个实例上重新创建一遍

      ```js
      function Person(name, age, job){
       	this.name = name;
       	this.age = age;
       	this.job = job;
       	this.sayName = new Function("alert(this.name)");//与声明函数在逻辑上是等的
      } 
      alert(person1.sayName == person2.sayName); //false 
      ```

    - 通过把函数定义转移到构造函数外部来解决这个问题

      ```js
      function Person(name, age, job){
       	this.name = name;
       	this.age = age;
       	this.job = job;
       	this.sayName = sayName;
      }
      function sayName(){
       alert(this.name);
      }
      var person1 = new Person("Nicholas", 29, "Software Engineer");
      var person2 = new Person("Greg", 27, "Doctor");
      //新问题1：在全局作用域中定义的函数实际上只能被某个对象调用，这让全局作用域有点名不副实
      //新问题2：如果对象需要定义很多方法，那么就要定义很多个全局函数，没有封装性
      ```

- 原型模式

  - prototype（原型）属性：每个函数都有这个属性，是一个指针，指向一个包含可以由特定类型的所有实例共享的属性和方法的对象

    ```js
    function Person(){};
    Person.prototype.name = "Nicholas";
    Person.prototype.age = 29;
    Person.prototype.job = "Software Engineer";
    Person.prototype.sayName = function(){
     	alert(this.name);
    };
    var person1 = new Person();
    person1.sayName(); //"Nicholas"
    var person2 = new Person(); 
    person2.sayName(); //"Nicholas"
    alert(person1.sayName == person2.sayName); //true 
    ```

  - 理解原型对象

    - 只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个 prototype（`__proto__`）属性，这个属性指向函数的原型对象，原型对象都会自动获得一个 constructor（构造函数）属性

    - `isPrototypeOf()`方法：来确定对象之间是否存在[[Prototype]]原型关系

      ```js
      alert(Person.prototype.isPrototypeOf(person1)); //true
      alert(Person.prototype.isPrototypeOf(person2)); //true
      ```

    - `Object.getPrototypeOf()`方法：返回[[Prototype]]的值

      ```js
      alert(Object.getPrototypeOf(person1) == Person.prototype); //true
      alert(Object.getPrototypeOf(person1).name); //"Nicholas" 
      ```

    - 多个对象实例共享原型所保存的属性和方法的基本原理：通过层层搜索查找对象属性，首先从对象实例本身开始，找不到则继续搜索指针指向的原型对象，在原型对象中查找具有给定名字的属性

    - 可以通过对象实例访问保存在原型中的值，但不能通过对象实例重写原型中的值，当为对象实例添加一个属性时，这个属性就会屏蔽原型对象中保存的同名属性

      ```js
      
      ```

    - 使用 delete 操作符则可以完全删除实例属性，从而能够重新访问原型中的属性

      ```js
      function Person(){}
      Person.prototype.name = "Nicholas";
      Person.prototype.age = 29;
      Person.prototype.job = "Software Engineer";
      Person.prototype.sayName = function(){
       	alert(this.name);
      };
      var person1 = new Person();
      var person2 = new Person();
      person1.name = "Greg";
      alert(person1.name); //"Greg"——来自实例
      alert(person2.name); //"Nicholas"——来自原型
      delete person1.name;
      alert(person1.name); //"Nicholas"——来自原型
      ```

    - `hasOwnProperty()`方法：检测一个属性是存在于实例中，还是存在于原型中

      ```js
      function Person(){}
      Person.prototype.name = "Nicholas";
      Person.prototype.age = 29;
      Person.prototype.job = "Software Engineer";
      Person.prototype.sayName = function(){ 
          alert(this.name);
      };
      var person1 = new Person();
      var person2 = new Person();
      alert(person1.hasOwnProperty("name")); //false
      person1.name = "Greg";
      alert(person1.name); //"Greg"——来自实例
      alert(person1.hasOwnProperty("name")); //true
      alert(person2.name); //"Nicholas"——来自原型
      alert(person2.hasOwnProperty("name")); //false
      delete person1.name;
      alert(person1.name); //"Nicholas"——来自原型
      alert(person1.hasOwnProperty("name")); //false 
      ```

  -  原型与 in 操作符

    - 单独使用in 操作符：在通过对象能够访问给定属性时返回 true，无论该属性存在于实例中还是原型中

      ```js
      function Person(){}
      Person.prototype.name = "Nicholas";
      Person.prototype.age = 29;
      Person.prototype.job = "Software Engineer";
      Person.prototype.sayName = function(){
       	alert(this.name);
      };
      
      var person1 = new Person();
      var person2 = new Person();
      
      alert(person1.hasOwnProperty("name")); //false
      alert("name" in person1); //true
      
      person1.name = "Greg";
      alert(person1.name); //"Greg" ——来自实例
      alert(person1.hasOwnProperty("name")); //true
      alert("name" in person1); //true
      
      alert(person2.name); //"Nicholas" ——来自原型
      alert(person2.hasOwnProperty("name")); //false
      alert("name" in person2); //true
      
      delete person1.name;
      alert(person1.name); //"Nicholas" ——来自原型
      alert(person1.hasOwnProperty("name")); //false
      alert("name" in person1); //true 
      ```

    - `hasPrototypeProperty()`方法：同时使用 `hasOwnProperty()`方法和 `in `操作符，就可以确定该属性到底是存在于对象中，还是存在于原型中

      ```js
      function hasPrototypeProperty(object, name){
       	return !object.hasOwnProperty(name) && (name in object);
      } 
      
      function Person(){}
      Person.prototype.name = "Nicholas";
      Person.prototype.age = 29;
      Person.prototype.job = "Software Engineer";
      Person.prototype.sayName = function(){
       	alert(this.name);
      };
      var person = new Person();
      alert(hasPrototypeProperty(person, "name")); //true
      person.name = "Greg";
      alert(hasPrototypeProperty(person, "name")); //false 
      ```

    - 在for-in 循环里使用：返回的是所有能够通过对象访问的、可枚举的（enumerated）属性，其中
      既包括存在于实例中的属性，也包括存在于原型中的属性

      ```js
      var o = {
       	toString : function(){
       		return "My Object";
       	}
      };
      for (var prop in o){
       	if (prop == "toString"){
       	alert("Found toString"); //在 IE 中不会显示
       }
      } 
      ```

    - `Object.keys()`方法：接收一个对象作为参数，返回一个包含所有可枚举属性的字符串数组

      ```js
      function Person(){}
      Person.prototype.name = "Nicholas";
      Person.prototype.age = 29;
      Person.prototype.job = "Software Engineer";
      Person.prototype.sayName = function(){
       	alert(this.name);
      };
      var keys = Object.keys(Person.prototype);
      alert(keys); //"name,age,job,sayName"
      var p1 = new Person();
      p1.name = "Rob";
      p1.age = 31;
      var p1keys = Object.keys(p1);
      alert(p1keys); //"name,age" 
      ```

    - `Object.getOwnPropertyNames()`方法：返回所有实例属性，无论它是否可枚举

      ```js
      var keys = Object.getOwnPropertyNames(Person.prototype);
      alert(keys); //"constructor,name,age,job,sayName" 
      ```

  -  更简单的原型语法

    - 用一个包含所有属性和方法的对象字面量来重写整个原型对象，但是导致constructor 属性不再指向 Person 

      ```js
      function Person(){}
      Person.prototype = {
       	name : "Nicholas",
       	age : 29,
       	job: "Software Engineer",
       	sayName : function () {
       		alert(this.name);
       	}
      }; 
      
      var friend = new Person();
      alert(friend instanceof Object); //true
      alert(friend instanceof Person); //true
      alert(friend.constructor == Person); //false
      alert(friend.constructor == Object); //true 
      ```

    - 解决办法：重新设置个constructor 属性的指向，但是导致它的`[[Enumerable]]`特性被设置为 true,默认情况下，原生的 constructor 属性是不可枚举的

      ```js
      function Person(){}
      Person.prototype = {
       	constructor : Person,
       	name : "Nicholas",
       	age : 29,
       	job: "Software Engineer",
       	sayName : function () {
       		alert(this.name);
       	}
      };
      ```

    - 解决办法：使用`Object.defineProperty()`方法重设构造函数

      ```js
      function Person(){}
      Person.prototype = {
       	name : "Nicholas",
      	age : 29,
       	job : "Software Engineer",
       	sayName : function () {
       		alert(this.name);
       	}
      }; 
      //重设构造函数，只适用于 ECMAScript 5 兼容的浏览器
      Object.defineProperty(Person.prototype, "constructor", {
       	enumerable: false,
       	value: Person
      }); 
      ```

  - 原型的动态性

    - 对原型对象所做的任何修改都能够立即从实例上反映出来

      ```js
      var friend = new Person();
      Person.prototype.sayHi = function(){
       	alert("hi");
      };
      friend.sayHi(); //"hi"（没有问题！）
      ```

    - 实例中的指针仅指向原型，而不指向构造函数

      ```js
      function Person(){}
      var friend = new Person();
      Person.prototype = {
       	constructor: Person,
       	name : "Nicholas",
       	age : 29,
       	job : "Software Engineer",
       	sayName : function () {
       		alert(this.name);
       	}
      };
      friend.sayName(); //error 
      //调用构造函数时会为实例添加一个指向最初原型的[[Prototype]]指针，
      //而把原型修改为另外一个对象就等于切断了构造函数与最初原型之间的联系
      ```

  - 原生对象的原型

    - 所有原生引用类型（Object、Array、String，等等）都在其构造函数的原型上定义了方法

      ```js
      alert(typeof Array.prototype.sort); //"function"
      alert(typeof String.prototype.substring); //"function" 
      ```

    - 通过原生对象的原型，不仅可以取得所有默认方法的引用，而且也可以定义新方法

      ```js
      String.prototype.startsWith = function (text) {
       	return this.indexOf(text) == 0;
      };
      var msg = "Hello world!";
      alert(msg.startsWith("Hello")); //true 
      ```

  - 原型对象的问题

    - 所有实例在默认情况下都将取得相同的属性值

    - 原型中所有属性是被很多实例共享的，这种共享对于函数非常合适，对于包含引用类型值的属性来说会出现问题

      ```js
      function Person(){}
      Person.prototype = {
       	constructor: Person,
       	name : "Nicholas",
       	age : 29,
       	job : "Software Engineer",
       	friends : ["Shelby", "Court"],
       	sayName : function () {
       		alert(this.name);
       	}
      };
      var person1 = new Person();
      var person2 = new Person();
      person1.friends.push("Van");
      alert(person1.friends); //"Shelby,Court,Van"
      alert(person2.friends); //"Shelby,Court,Van"
      alert(person1.friends === person2.friends); //true 
      ```

- 组合使用构造函数模式和原型模式

  - 构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性
  - 每个实例都会有自己的一份实例属性的副本，但同时又共享着对方法的引用，最大限度地节省了内存
  - 这种混成模式还支持向构造函数传递参数

  ```js
  function Person(name, age, job){
   	this.name = name;
   	this.age = age;
   	this.job = job;
   	this.friends = ["Shelby", "Court"];
  }
  Person.prototype = {
   	constructor : Person,
   	sayName : function(){
   		alert(this.name);
   	}
  }
  var person1 = new Person("Nicholas", 29, "Software Engineer");
  var person2 = new Person("Greg", 27, "Doctor");
  person1.friends.push("Van");
  alert(person1.friends); //"Shelby,Count,Van"
  alert(person2.friends); //"Shelby,Count"
  alert(person1.friends === person2.friends); //false
  alert(person1.sayName === person2.sayName); //true 
  ```

- 动态原型模式

  - 通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型

    ```js
    function Person(name, age, job){
     //属性
     	this.name = name;
     	this.age = age;
     	this.job = job; //方法
     	if (typeof this.sayName != "function"){
    		Person.prototype.sayName = function(){
     			alert(this.name);
     		};
    	}
    } 
    var friend = new Person("Nicholas", 29, "Software Engineer");
    friend.sayName(); 
    ```

  - 使用动态原型模式时，不能使用对象字面量重写原型，如果在已经创建了实例的情况下重写原型，那么就会切断现有实例与新原型之间的联系

- 寄生构造函数模式

  - 基本思想：创建一个函数，该函数的作用仅仅是封装创建对象的代码，然后再返回新创建的对象；但
    从表面上看，这个函数又很像是典型的构造函数。构造函数在不返回值的情况下，默认会返回新对象实例。而通过在构造函数的末尾添加一个 return 语句，可以重写调用构造函数时返回的值

    ```js
    function Person(name, age, job){
     var o = new Object();
     o.name = name;
     o.age = age;
     o.job = job;
     o.sayName = function(){
     	alert(this.name);
     };
     return o;
    }
    var friend = new Person("Nicholas", 29, "Software Engineer");
    friend.sayName(); //"Nicholas"
    ```

  - 这个模式可以在特殊的情况下用来为对象创建构造函数。假设我们想创建一个具有额外方法的特殊数组。由于不能直接修改 Array 构造函数，因此可以使用这个模式。

    ```js
    function SpecialArray(){
     	//创建数组
     	var values = new Array();
     	//添加值
     	values.push.apply(values, arguments);
     	//添加方法
    	 values.toPipedString = function(){
    		 return this.join("|");
     	};
    
     	//返回数组
     	return values;
    }
    var colors = new SpecialArray("red", "blue", "green");
    alert(colors.toPipedString()); //"red|blue|green"
    ```

  - 返回的对象与构造函数或者与构造函数的原型属性之间没有关系，不能依赖` instanceof `操作符来确定对象类型

- 稳妥构造函数模式

  - 稳妥对象，指的是没有公共属性，而且其方法也不引用 this 的对象；适合在一些安全的环境中（这些环境中会禁止使用 this 和 new），或者在防止数据被其他应用程序（如 Mashup程序）改动时使用。

  - 稳妥构造函数遵循与寄生构造函数类似的模式，但有两点不同：

    - 一是新创建对象的实例方法不引用 this
    - 二是不使用 new 操作符调用构造函数

    ```js
    function Person(name, age, job){
     	//创建要返回的对象
     	var o = new Object();
        //可以在这里定义私有变量和函数
     	//添加方法
     	o.sayName = function(){
     		alert(name);
     	};
     	//返回对象
     	return o;
    } 
    var friend = Person("Nicholas", 29, "Software Engineer");
    friend.sayName(); //"Nicholas
    ```

  - 稳妥构造函数模式提供的这种安全性，使得它非常适合在某些安全执行环境——例如，ADsafe（www.adsafe.org）和 Caja（http://code.google.com/p/google-caja/）提供的环境——下使用

  - 使用稳妥构造函数模式创建的对象与构造函数之间也没有什么关系，因此 instanceof 操作符对这种对象也没有意义

#### 3、继承

- 原型链

  - 基本概念

    - 原型链是实现继承的主要方法：利用原型让一个引用类型继承另一个引用类型的属性和方法
    - 构造函数、原型和实例的关系：每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针
    - 让原型对象等于另一个类型的实例，此时的原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着一个指向另一个构造函数的指针。假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实例与原型的链条

  - 基本模式

    ```js
    function SuperType(){
     	this.property = true;
    }
    SuperType.prototype.getSuperValue = function(){
     	return this.property;
    };
    function SubType(){
     	this.subproperty = false;
    }
    //继承了 SuperType
    SubType.prototype = new SuperType();
    SubType.prototype.getSubValue = function (){
     	return this.subproperty;
    };
    var instance = new SubType();
    alert(instance.getSuperValue()); //true 
    ```

  - 注意的地方：

    - 别忘记默认的原型：所有函数的默认原型都是 Object 的实例，因此默认原型都会包含一个内部指针，指向` Object.prototype`。这也正是所有自定义类型都会继承 `toString()`、`valueOf()`等默认方法的根本原因

    - 确定原型和实例的关系：`instanceof`操作符和`isPrototypeOf()`方法

      ```js
      //使用instanceof操作符，测试实例与原型链中出现过的构造函数，结果就会返回 true
      alert(instance instanceof Object); //true
      alert(instance instanceof SuperType); //true
      alert(instance instanceof SubType); //true 
      
      //isPrototypeOf()方法,原型链中出现过的原型，都是该原型链所派生的实例的原型，返回 true
      alert(Object.prototype.isPrototypeOf(instance)); //true
      alert(SuperType.prototype.isPrototypeOf(instance)); //true
      alert(SubType.prototype.isPrototypeOf(instance)); //true 
      ```

    - 谨慎地定义方法：

      ```js
      //1.给原型添加方法的代码一定要放在替换原型的语句之后
      function SuperType(){
       	this.property = true;
      }
      SuperType.prototype.getSuperValue = function(){
       	return this.property;
      };
      function SubType(){
      	 this.subproperty = false;
      }
      //继承了 SuperType
      SubType.prototype = new SuperType();
      //添加新方法
      SubType.prototype.getSubValue = function (){
       	return this.subproperty;
      };
      //重写超类型中的方法
      SubType.prototype.getSuperValue = function (){
       	return false;
      };
      var instance = new SubType();
      alert(instance.getSuperValue()); //false 
      
      //2.在通过原型链实现继承时，不能使用对象字面量创建原型方法,这样做就会重写原型链
      function SuperType(){
       	this.property = true;
      }
      SuperType.prototype.getSuperValue = function(){
       	return this.property;
      };
      function SubType(){
       	this.subproperty = false;
      }
      //继承了 SuperType
      SubType.prototype = new SuperType();
      //使用字面量添加新方法，会导致上一行代码无效
      SubType.prototype = {
       	getSubValue : function (){
       		return this.subproperty;
       	},
       	someOtherMethod : function (){
      		 return false;
       	}
      };
      var instance = new SubType();
      alert(instance.getSuperValue()); //error!
      ```

  -  原型链的问题

    - 最主要的问题来自包含引用类型值的原型，在通过原型来实现继承时，原型实际上会变成另一个类型的实例，于是，原先的实例属性也就顺理成章地变成了现在的原型属性了

      ```js
      function SuperType(){
       	this.colors = ["red", "blue", "green"];
      }
      function SubType(){}
      //继承了 SuperType
      SubType.prototype = new SuperType();
      var instance1 = new SubType();
      instance1.colors.push("black");
      alert(instance1.colors); //"red,blue,green,black"
      var instance2 = new SubType();
      alert(instance2.colors); //"red,blue,green,black" 
      ```

    - 在创建子类型的实例时，不能向超类型的构造函数中传递参数

- 借用构造函数

  - 基本思想：在子类型构造函数的内部调用超类型构造函数

    ```js
    function SuperType(){
     	this.colors = ["red", "blue", "green"];
    }
    function SubType(){
    	//继承了 SuperType
     	SuperType.call(this);
    }
    var instance1 = new SubType();
    instance1.colors.push("black");
    alert(instance1.colors); //"red,blue,green,black"
    var instance2 = new SubType();
    alert(instance2.colors); //"red,blue,green" 
    ```

  - 传递参数：可以在子类型构造函数中向超类型构造函数传递参数

    ```js
    function SuperType(name){
     	this.name = name;
    }
    function SubType(){
     	//继承了 SuperType，同时还传递了参数
     	SuperType.call(this, "Nicholas");
    
     	//实例属性
     	this.age = 29;
    }
    var instance = new SubType();
    alert(instance.name); //"Nicholas";
    alert(instance.age); //29
    ```

  - 借用构造函数的问题：方法都在构造函数中定义，因此函数复用就无从谈起了。而且，在超类型的原型中定义的方法，对子类型而言也是不可见的，结果所有类型都只能使用构造函数模式

- 组合继承：

  - 基本思想：使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承

    ```js
    function SuperType(name){
     	this.name = name;
     	this.colors = ["red", "blue", "green"];
    }
    SuperType.prototype.sayName = function(){
     	alert(this.name); 
    };
    function SubType(name, age){
     	//继承属性
     	SuperType.call(this, name);
     	this.age = age;
    }
    //继承方法
    SubType.prototype = new SuperType();
    SubType.prototype.constructor = SubType;
    SubType.prototype.sayAge = function(){
     	alert(this.age);
    };
    var instance1 = new SubType("Nicholas", 29);
    instance1.colors.push("black");
    alert(instance1.colors); //"red,blue,green,black"
    instance1.sayName(); //"Nicholas";
    instance1.sayAge(); //29
    var instance2 = new SubType("Greg", 27);
    alert(instance2.colors); //"red,blue,green"
    instance2.sayName(); //"Greg";
    instance2.sayAge(); //27 
    ```

  - 组合继承避免了原型链和借用构造函数的缺陷，融合了它们的优点，成为 JavaScript 中最常用的继承模式。而且，`instanceof` 和` isPrototypeOf()`也能够用于识别基于组合继承创建的对象

- 原型式继承

  - 思路：借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义类型

    ```js
    function object(o){
     	function F(){}
     	F.prototype = o;
     	return new F();
    } 
    var person = {
     	name: "Nicholas",
     	friends: ["Shelby", "Court", "Van"]
    };
    var anotherPerson = object(person);
    anotherPerson.name = "Greg";
    anotherPerson.friends.push("Rob");
    var yetAnotherPerson = object(person);
    yetAnotherPerson.name = "Linda";
    yetAnotherPerson.friends.push("Barbie");
    alert(person.friends); //"Shelby,Court,Van,Rob,Barbie"
    ```

  - Object.create()方法：规范化了原型式继承，这个方法接收两个参数，一个用作新对象原型的对象和（可选的）一个为新对象定义额外属性的对象

    ```js
    //在传入一个参数的情况下，Object.create()与 object()方法的行为相同
    var person = {
     	name: "Nicholas",
     	friends: ["Shelby", "Court", "Van"]
    };
    var anotherPerson = Object.create(person);
    anotherPerson.name = "Greg";
    anotherPerson.friends.push("Rob");
    
    var yetAnotherPerson = Object.create(person);
    yetAnotherPerson.name = "Linda";
    yetAnotherPerson.friends.push("Barbie");
    alert(person.friends); //"Shelby,Court,Van,Rob,Barbie"
    
    //第二个参数与Object.defineProperties()方法的第二个参数格式相同：
    //每个属性都是通过自己的描述符定义的。以这种方式指定的任何属性都会覆盖原型对象上的同名属性
    var person = {
         name: "Nicholas",
         friends: ["Shelby", "Court", "Van"]
    }; 
    var anotherPerson = Object.create(person, {
     	name: {
     		value: "Greg"
     	}
    });
    
    alert(anotherPerson.name); //"Greg" 
    ```

- 寄生式继承

  - 思路：与寄生构造函数和工厂模式类似，即创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，最后再像真地是它做了所有工作一样返回对象

    ```js
    function createAnother(original){
     	var clone = object(original); //通过调用函数创建一个新对象
     	clone.sayHi = function(){ //以某种方式来增强这个对象
     		alert("hi");
     	};
     	return clone; //返回这个对象
    }
    var person = {
     	name: "Nicholas",
     	friends: ["Shelby", "Court", "Van"]
    };
    var anotherPerson = createAnother(person);
    anotherPerson.sayHi(); //"hi" 
    ```

  - 在主要考虑对象而不是自定义类型和构造函数的情况下，寄生式继承也是一种有用的模式。使用寄生式继承来为对象添加函数，会由于不能做到函数复用而降低效率；这一点与构造函数模式类似

- 寄生组合继承

  - 组合继承最大的问题：就是无论什么情况下，都会调用两次超类型构造函数，一次是在创建子类型原型的时候，另一次是在子类型构造函数内部

    ```js
    //子类型最终会包含超类型对象的全部实例属性，但我们不得不在调用子类型构造函数时重写这些属性
    function SuperType(name){ 
        this.name = name;
     	this.colors = ["red", "blue", "green"];
    }
    SuperType.prototype.sayName = function(){
     	alert(this.name);
    };
    function SubType(name, age){
     	SuperType.call(this, name); //第二次调用 SuperType()
     	this.age = age;
    }
    SubType.prototype = new SuperType(); //第一次调用 SuperType()
    SubType.prototype.constructor = SubType;
    SubType.prototype.sayAge = function(){
     	alert(this.age);
    }; 
    ```

  - 寄生组合式继承，即通过借用构造函数来继承属性，通过原型链的混成形式来继承方法；其背后的基本思路是：不必为了指定子类型的原型而调用超类型的构造函数，我们所需要的无非就是超类型原型的一个副本而已。本质上，就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型

    ```js
    function inheritPrototype(subType, superType){
     	var prototype = object(superType.prototype); //创建对象
     	prototype.constructor = subType; //增强对象
     	subType.prototype = prototype; //指定对象
    }
    function SuperType(name){
     	this.name = name;
     	this.colors = ["red", "blue", "green"];
    }
    SuperType.prototype.sayName = function(){
     	alert(this.name);
    };
    function SubType(name, age){
     	SuperType.call(this, name);
     	this.age = age;
    }
    inheritPrototype(SubType, SuperType);
    SubType.prototype.sayAge = function(){
     	alert(this.age);
    };
    ```

  - YUI 的 `YAHOO.lang.extend()`方法采用了寄生组合继承，从而让这种模式首次出现在了一个应用非常广泛的 JavaScript 库中，要了解有关 YUI 的更多信息，请访问http://developer. yahoo.com/yui/

#### 4、总结

`ECMAScript `支持面向对象（OO）编程，但不使用类或者接口。对象可以在代码执行过程中创建和增强，因此具有动态性而非严格定义的实体。在没有类的情况下，可以采用下列模式创建对象

- 工厂模式，使用简单的函数创建对象，为对象添加属性和方法，然后返回对象。这个模式后来被构造函数模式所取代
- 构造函数模式，可以创建自定义引用类型，可以像创建内置对象实例一样使用 new 操作符。不过，构造函数模式也有缺点，即它的每个成员都无法得到复用，包括函数。由于函数可以不局限于任何对象（即与对象具有松散耦合的特点），因此没有理由不在多个对象间共享函数
- 原型模式，使用构造函数的 prototype 属性来指定那些应该共享的属性和方法。组合使用构造函数模式和原型模式时，使用构造函数定义实例属性，而使用原型定义共享的属性和方法

JavaScript 主要通过原型链实现继承。原型链的构建是通过将一个类型的实例赋值给另一个构造函数的原型实现的。这样，子类型就能够访问超类型的所有属性和方法，这一点与基于类的继承很相似。原型链的问题是对象实例共享所有继承的属性和方法，因此不适宜单独使用。解决这个问题的技术是借用构造函数，即在子类型构造函数的内部调用超类型构造函数。这样就可以做到每个实例都具有自己的属性，同时还能保证只使用构造函数模式来定义类型。使用最多的继承模式是组合继承，这种模式使用原型链继承共享的属性和方法，而通过借用构造函数继承实例属性。此外，还存在下列可供选择的继承模式:

- 原型式继承，可以在不必预先定义构造函数的情况下实现继承，其本质是执行对给定对象的浅复制。而复制得到的副本还可以得到进一步改造
- 寄生式继承，与原型式继承非常相似，也是基于某个对象或某些信息创建一个对象，然后增强对象，最后返回对象。为了解决组合继承模式由于多次调用超类型构造函数而导致的低效率问题，可以将这个模式与组合继承一起使用
- 寄生组合式继承，集寄生式继承和组合继承的优点与一身，是实现基于类型继承的最有效方式