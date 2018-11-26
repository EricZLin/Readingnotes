## 二、JavaScript基本概念

#### 1、语法

- **区分大小写**： ECMAScript 中的一切（变量、函数名和操作符）都区分大小写

- **标识符**：

  - 标识符就是指变量、函数、属性的名字，或者函数的参数
  - 第一个字符必须是一个字母、下划线（_）或一个美元符号（$）
  - 其他字符可以是字母、下划线、美元符号或数字
  - 标识符中的字母也可以包含扩展的 ASCII 或 Unicode 字母字符（如 À 和 Æ）--不推荐
  - 采用驼峰大小写格式，也就是第一个字母小写，剩下的每个单词的首字母大写
  - 不能把关键字、保留字、true、false和null用作标识符

- **注释**：包括单行注释和块级注释

  ```js
  //单行注释
  
  /* 
  * 这是一个多行（块级）注释,以一个斜杠和一个星号开头，以一个星号和一个斜杠结尾
  * 二三行都以星号开头这种格式在企业级应用中用得比较多，并不是必需的
  */ 
  ```

- **严格模式**

  - 严格模式是为 JavaScript 定义了一种不同的解析与执行模型（ECMAScript 5 引入）

  - 要在整个脚本中启用严格模式，可以在顶部添加如下代码：

    ```js
    "use strict"
    ```

  - 在函数内部的上方包含这条编译指示，也可以指定函数在严格模式下执行：

    ```js
    function doSomething(){
        "use strict";
        //函数体
    }
    ```

  - 支持严格模式的浏览器包括 IE10+、Firefox 4+、Safari 5.1+、Opera 12+和 Chrome

- **语句**

  - 建议以分号结尾，可以避免某些错误(不完整输入)，有利压缩代码，增加性能（解析快）

    ```js
    var sum = a + b      // 即使没有分号也是有效的语句——不推荐
    var diff = a - b;    //   有效的语句——推荐
    ```

  - 建议使用代码块，可以让编码意图更加清晰，而且也能降低修改代码时出错的几率

    ```js
    if (test) 
        alert(test);       // 有效但容易出错，不要使用 
     
    if (test){             // 推荐使用 
        alert(test); 
    } 
    ```

####  2、关键字和保留字

- **关键字**：不能用作标识符

  ```js
  break         do           instanceof        typeof 
  case          else         new               var 
  catch         finally      return            void 
  continue      for          switch            while 
  debugger*
    
  function     this          with 
  default       if           throw 
  delete        in           try 
  ```

- **保留字**：另外一组不能用作标识符的保留字，有可能在将来被用作关键字

  ```js
  abstract      enum            int            short 
  boolean       export          interface      static 
  byte          extends         long           super 
  char          final           native         synchronized 
  class         float           package        throws 
  const         goto            private        transient 
  debugger      implements      protected      volatile 
  double        import          public 
  ```

- **第 5 版**在非严格模式下运行时的保留字缩减为下列这些：

  ```js
  class          enum          extends         super 
  const          export        import
  ```

- 在**严格模式**下，第 5 版还对以下保留字施加了限制：

  ```js
  implements     package       public 
  interface      private       static 
  let  
  ```

- **补充**：

  - let和yield是第 5 版新增的保留字；其他保留字都是第 3 版定义的
  - ECMA-262 第 5 版对eval和arguments还施加了限制。在严格模式下，这两个名字也不能作为标识符或属性名，否则会抛出错误

#### 3、变量

- **松散类型**：可以用来保存任何类型的数据，每个变量仅仅是一个用于保存值的占位符而已

- **定义**：使用var操作符（注意var是一个关键字），后跟变量名（即一个标识符）

  ```js
  var message; //定义变量，未经过初始化的变量，会保存一个特殊的值——undefined
  
  var message = "hi"; //定义变量的同时就可以设置变量的值
  ```

- **作用域**：用var操作符定义的变量将成为定义该变量的作用域中的局部变量，如果在函数中使用var定义一个变量，那么这个变量在函数退出后就会被销毁。

  ```js
  function test(){
      var message = 'hi';//局部变量
  }
  test()；
  alert(message);//错误
  ```

- **定义多个变量**：把每个变量（初始化或不初始化均可）用逗号分隔开

  ```js
  var message = 'hi',
      found = false,
      age = 29;
  ```

- 注意：在严格模式下，不能定义名为eval或arguments的变量，否则会导致语法错误

#### 4、数据类型

> ECMAScript 中有 5 种简单数据类型（也称为基本数据类型）：Undefined、Null、Boolean、Number
> 和String。还有 1 种复杂数据类型——Object，Object本质上是由一组无序的名值对组成的。

- `typeof`操作符：

  - 返回值可以用来检测数据类型

    ```js
    "undefined" --如果这个值未定义
    "boolean"   --如果这个值是布尔值
    "string"    --如果这个值是字符串
    "number"   --如果这个值是数值
    "object"   --如果这个值是对象或者null（特殊值null被认为是一个空的对象引用）
    "function" --如果这个值是函数（特殊：函数在 ECMAScript 中是对象，不是一种数据类型）
    ```

  - 是一个操作符而不是函数(圆括号不是必须的)，操作数可以是变量，也可以是数值字面量

    ```js
    var message = "some string"; 
    alert(typeof message);     // "string" 
    alert(typeof(message));    // "string" 
    alert(typeof 95);          // "number"
    ```

- `Undefined`类型 

  - 定义：Undefined类型只有一个值，即特殊的undefined。在使用var声明变量但未对其加以初始化时，
    这个变量的值就是undefined

    ```js
    var message; 
    alert(message == undefined); //true
    ```

  -  显示地初始化变量。不存在需要显示地把一个变量设置为undfined的情况，字面量undefined主要是用于比较，而ECMA-262第三版引入这个值为了正式区分空对象指针与未经初始化的变量

    ```js
    var message = undefined; 
    alert(message == undefined); //true
    ```

  - 包含undefined值的变量与尚未定义的变量的区别。对于尚未声明过的变量，只能执行一项操作，即使用typeof操作符检测其数据类型（对未经声明的变量调用delete不会导致错误，但这样做没什么实际意义，而且在严格模式下确实会导致错误）。但是对未初始化的变量执行typeof操作符会返回undefined值，而对未声明的变量执行typeof操作符同样也会返回undefined值。

    ```js
    var message; 
    alert(message);     // "undefined" （这个变量声明之后默认取得了 undefined 值）
    alert(age);         // 产生错误 （这个变量并没有声明 ）
    
    alert(typeof message);     // "undefined" 
    alert(typeof age);         // "undefined"
    ```

- `Null`类型 

  - Null类型是第二个（第一个是undefined）只有一个值的数据类型，这个特殊的值是null。从逻辑角度来看，null值表示一个空对象指针（所以使用typeof操作符检测null值时会返回"object"）

    ```js
    var car = null; 
    alert(typeof car);     // "object"
    ```

  - 如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为null而不是其他值。这样
    只要直接检查null值就可以知道相应的变量是否已经保存了一个对象的引用

    ```js
    if (car != null){ 
        // 对 car 对象执行某些操作 
    } 
    ```

  - undefined值是派生自null值的，因此 ECMA-262 规定对它们的相等性测试要返回true：位于null和undefined之间的相等操作符（==）总是返回true

    ```js
    alert(null == undefined);//true
    ```

  - null 和 undefined的区别：无论在什么情况下都没有必要把一个变量的值显式地设置为undefined，可是同样的规则对null却不适用。换句话说，只要意在保存对象的变量还没有真正保存对象，就应该明确地让该变量保存null值。这样做不仅可以体现null作为空对象指针的惯例，而且也有助于进一步区分null和undefined

- `Boolean`类型

  - Boolean类型只有两个字面值：true和false。字面值true和false是区分大小写的。也就是说，True和False（以及其他的混合大小写形式）都不是Boolean值，只是标识符。

    ```js
    var found = true; 
    var lost = false;
    ```

  - 要将一个值转换为其对应的Boolean值，可以调用转型函数Boolean()，返回的这个值是true还是false，取决于要转换值的数据类型及其实际值

    | 数据类型  |        转换为true的值        | 转换为false的值 |
    | :-------: | :--------------------------: | :-------------: |
    |  Boolean  |             true             |      false      |
    |  Srting   |        任何非空字符串        | “ ”（空字符串） |
    |  Number   | 任何非零数字值（包括无穷大） |     0和NaN      |
    |  Object   |           任何对象           |      null       |
    | Undefined |              /               |    undefined    |

  - 流程控制语句会自动执行boolean转换，因此确切地知道在流控制语句中使用的是什么变量至关重要。错误地使用一个对象而不是一个Boolean值，就有可能彻底改变应用程序的流程

    ```js
    var message = "Hello world!"; 
    if (message){ 
        alert("Value is true"); 
    }
    ```

- `Number`类型

  - 这种类型使用 IEEE754 格式来表示整数和浮点数值（浮点数值在某些语言中也被称为双精度数值）。为支持各种数值类型，ECMA-262 定义了不同的数值字面量格式。

    - 十进制：最基本的数值字面量格式

      ```js
      var intNum = 55;    // 整数
      ```

    - 八进制：字面值的第一位必须是零（0），然后是八进制数字序列（0～7）。如果字面值中的
      数值超出了范围，那么前导零将被忽略，后面的数值将被当作十进制数值解析。注意八进制字面量在严格模式下是无效的，会导致支持的 JavaScript 引擎抛出错误

      ```js
      var octalNum1 = 070;      // 八进制的 56 
      var octalNum2 = 079;      // 无效的八进制数值——解析为 79 
      var octalNum3 = 08;      // 无效的八进制数值——解析为 8 
      ```

    - 十六进制：字面值的前两位必须是 0x，后跟任何十六进制数字（0～9 及 A～F）。其中，字母 A～F
      可以大写，也可以小写。在进行算术计算时，所有的所有的八进制个十六进制表示的值都会转换为十进制的数值

      ```js
      var hexNum1 = 0xA;       // 十六进制的10
      var hexNum2 = ox1f;      // 十六进制的31
      ```

    - 鉴于 JavaScript 中保存数值的方式，可以保存正零（+0）和负零（0）。正零和负零被认为相等

  - 浮点数值：就是该数值中必须包含一个小数点，并且小数点后面必须至少有一位数字。

    ```js
    //1.小数点前面可以没有整数，但我们不推荐这种写法
    var floatNum1 = 1.1; 
    var floatNum2 = 0.1; 
    var floatNum3 = .1;   // 有效，但不推荐 
    
    //2.保存浮点数值需要的内存空间是保存整数值的两倍
    var floatNum1 = 1.;        // 小数点后面没有数字——解析为 1 
    var floatNum2 = 10.0;      // 整数——解析为 10 
    
    //3.对于那些极大或极小的数值，可以用 e 表示法（即科学计数法）表示的浮点数值表示
    var floatNum1 = 3.125e7;  // 等于 31250000
    var floatNum2= 3-e7;  // 等于 0.0000003
    
    //4.浮点数值的最高精度是17位小数，在进行计算时会产生误差
    if (a + b == 0.3){          // 不要做这样的测试！ 
        alert("You got 0.3."); 
    } 
    ```

  - 数值范围：由于内存的限制，ECMAScript 并不能保存世界上所有的数值。

    ```js
    console.log(Number.MAX_VALUE); //1.7976931348623157e+308--->最大值
    console.log(Number.MIN_VALUE); //5e-324-----最小值
    
    var result = Number.MAX_VALUE + Number.MAX_VALUE;
    console.log(result);//Infinity----->超过就是无穷大（可正可负）
    console.log(isFinite(result));//false--->确定一个数值是不是有穷的使用isFinite()函数
    
    console.log(Number.NEGATIVE_INFINITY);//-Infinity
    console.log(Number.POSITIVE_INFINITY);//Infinity
    ```

  - NaN：即非数值（Not a Number）是一个特殊的数值，这个数值用于表示一个本来要返回数值的操作数未返回数值的情况（这样就不会抛出错误了）

    ```js
    //1.任何涉及NaN的操作都会返回NaN
    console.log(NaN/10);//NaN
    
    //2.NaN与任何值都不相等，包括NaN本身
    console.log(NaN == 1);//false
    console.log(NaN == NaN);//false
    
    //3.isNaN()函数可以判断参数是否为NaN,即是否"不是数值"
    /*在基于对象调用isNaN()函数时，会首先调用对象的valueOf()方法，然后确定该方法返回的值是否可以转换为数值。如果不能，则基于这个返回值再调用toString()方法，再测试返回值。*/
    console.log(isNaN(NaN));//true
    console.log(isNaN(10));//false（（10 是一个数值）
    console.log(isNaN('10'));//false（可以被转换成数值 10）
    console.log(isNaN('blue'));//true（不能转换成数值） 
    console.log(isNaN(true));//false（可以被转换成数值 1） 
    ```

  - 数值转换

    - Number()：可以用于任何数据类型

      ```js
      //如果是 Boolean 值，true 和 false 将分别被转换为 1 和 0
      console.log(Number(true));//1
      console.log(Number(false));//0
      
      //如果是数字值，只是简单的传入和返回
      console.log(Number(100));//100
      
      //如果是 undefined，返回 NaN
      console.log(Number(undefined));//NaN
      
      //如果是 null 值，返回 0
      console.log(Number(null));//0
      
      //如果是字符串，遵循下列规则:
      console.log(Number('111'));//111
      console.log(Number('1.1'));//1.1
      console.log(Number('0xf'));//15
      console.log(Number(''));//0
      console.log(Number('111dasda'));//NaN
      console.log(Number('sqwer'));//NaN
      
      /*如果是对象，则调用对象的 valueOf()方法，然后依照前面的规则转换返回的值。
      如果转换的结果是 NaN，则调用对象的 toString()方法，然后再次依照前面的规则转换返回的字符串值*/
      ```

    - parseInt()：专门用于把字符串转换成数值

      ```js
      //忽略字符串前面的空格，直至找到第一个非空格字符
      var num1 = parseInt("   12"); //12
      
      //如果第一个字符不是数字字符或者负号，parseInt()就会返回 NaN
      var num2 = parseInt(""); // NaN 
      
      //只解析数字字符
      var num3 = parseInt("1234blue"); // 1234
      var num4 = parseInt(22.5); // 22
      
      //parseInt()能够识别出各种整数格式（0x---十六进制， 0---八进制）
      var num5 = parseInt("070"); // 56（八进制数---ES3，ES5解析为70）
      var num6 = parseInt("70"); // 70（十进制数）
      var num7 = parseInt("0xf"); // 15（十六进制数）
      
      //第二个参数是基数，代表进制，建议指定
      var num1 = parseInt("10", 2); //2 （按二进制解析）
      var num2 = parseInt("10", 8); //8 （按八进制解析）
      var num3 = parseInt("10", 10); //10 （按十进制解析）
      var num4 = parseInt("10", 16); //16 （按十六进制解析）
      ```

    - parseFloat()：专门用于把字符串转换成数值

      ```js
      /*
      1.从第一个字符（位置 0）开始解析每个字符,一直解析到字符串末尾或者解析到遇见一个无效的浮点数字字符为止(第一个小数点是有效的，第二个小数点是无效的)
      2.始终都会忽略前导的零，只解析十进制值，因此它没有用第二个参数指定基数的用法
      3.字符串包含的是一个可解析为整数的数（没有小数点，或者小数点后都是零），返回整数
      */
      var num1 = parseFloat("1234blue"); //1234 （整数）
      var num2 = parseFloat("0xA"); //0
      var num3 = parseFloat("22.5"); //22.5
      var num4 = parseFloat("22.34.5"); //22.34
      var num5 = parseFloat("0908.5"); //908.5
      var num6 = parseFloat("3.125e7"); //31250000 
      ```

- `String`类型

  - 定义：用于表示由零或多个 16 位 Unicode 字符组成的字符序列，即字符串。字符串可以由双引号（"）或单引号（'）表示

    ```js
    var firstName = "Nicholas";
    var lastName = 'Zakas'; 
    var firstName = 'Nicholas"; // 语法错误（左右引号必须匹配）
    ```

  - 字符字面量：即转义序列，用于表示非打印字符，或者具有其他用途的字符

    | 字面量 |                            含 义                             |
    | :----: | :----------------------------------------------------------: |
    |   \n   |                             换行                             |
    |   \t   |                             制表                             |
    |   \b   |                             空格                             |
    |   \r   |                             回车                             |
    |   \f   |                             进纸                             |
    |  \\\   |                             斜杠                             |
    |  \\'   |          单引号（'），在用单引号表示的字符串中使用           |
    |  \\"   |          双引号（"），在用双引号表示的字符串中使用           |
    |  \xnn  | 以十六进制代码nn表示的一个字符（其中n为0～F）例如\x41表示"A" |
    | \unnnn | 以十六进制代码nnnn表示的一个Unicode字符（其中n为0～F）,如\u03a3表示字符Σ |

  -  字符串的特点：ECMAScript 中的字符串是不可变的，也就是说，字符串一旦创建，它们的值就不能改变。要改变某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量。

    ```js
    var lang = "Java";
    lang = lang + "Script"; 
    ```

  - 转换为字符串

    - toString()方法

      ```js
      //1.数值、布尔值、对象和字符串值都有toString()方法
      var age = 11;
      var ageAsString = age.toString(); // 字符串"11"
      var found = true;
      var foundAsString = found.toString(); // 字符串"true" 
      
      //2.在调用数值的 toString()方法时，可以传递一个参数：输出数值的基数
      var num = 10;
      alert(num.toString()); // "10"
      alert(num.toString(2)); // "1010"
      alert(num.toString(8)); // "12"
      alert(num.toString(10)); // "10"
      alert(num.toString(16)); // "a" 
      
      //3.如果值是 null，则返回"null"；如果值是 undefined，则返回"undefined"
      var value1 = null;
      var value2;
      alert(String(value3)); // "null"
      alert(String(value4)); // "undefined" 
      ```

    - 加号操作符（见后面操作符部分）

- `Object`类型

  - 定义：一组数据和功能的集合，对象可以通过执行 new 操作符后跟要创建的对象类型的名称来创建。而创建 Object 类型的实例并为其添加属性和（或）方法，就可以创建自定义对象

    ```js
    var o = new Object(); 
    var o = new Object; // 有效，但不推荐省略圆括号
    ```

  - Object 类型所具有的任何属性和方法也同样存在于更具体的对象中，Object 的每个实例都具有下列属性和方法：

    - constructor：保存着用于创建当前对象的函数。对于前面的例子而言，构造函数（constructor）
      就是 Object()
    - hasOwnProperty(propertyName)：用于检查给定的属性在当前对象实例中（而不是在实例的原型中）是否存在。其中，作为参数的属性名（propertyName）必须以字符串形式指定（例如：o.hasOwnProperty("name")）
    - isPrototypeOf(object)：用于检查传入的对象是否是传入对象的原型
    - propertyIsEnumerable(propertyName)：用于检查给定的属性是否能够使用 for-in 语句来枚举。与 hasOwnProperty()方法一样，作为参数的属性名必须以字符串形式指定
    - toLocaleString()：返回对象的字符串表示，该字符串与执行环境的地区对应
    - toString()：返回对象的字符串表示
    - valueOf()：返回对象的字符串、数值或布尔值表示。通常与 toString()方法的返回值相同

  - 扩展：从技术角度讲，ECMA-262 中对象的行为不一定适用于 JavaScript 中的其他对象。浏览器环境中的对象，比如 BOM 和 DOM 中的对象，都属于宿主对象，因为它们是由宿主实现提供和定义的。ECMA-262 不负责定义宿主对象，因此宿主对象可能会也可能不会继承 Object

#### 5、操作符

> ECMA-262 描述了一组用于操作数据值的操作符，包括算术操作符（如加号和减号）、位操作符、关系操作符和相等操作符。ECMAScript 操作符的与众不同之处在于，它们能够适用于很多值，例如字符串、数字值、布尔值，甚至对象。不过，在应用于对象时，相应的操作符通常都会调用对象的 valueOf()和（或）toString()方法，以便取得可以操作的值。

- **一元操作符**：只能操作一个值的操作符

  - 递增和递减操作符

    - 前置型

      ```js
      //写法
      var age = 29;
      ++age; //age = age + 1
      --age; //age = age - 1
      
      //副效应：变量的值都是在语句被求值以前改变的
      var age = 29;
      var anotherAge = --age + 2;
      alert(age); // 输出 28
      alert(anotherAge); // 输出 30 
      
      var num1 = 2;
      var num2 = 20;
      var num3 = --num1 + num2; // 等于 21
      var num4 = num1 + num2; // 等于 21
      ```

    - 后置型

      ```js
      //写法
      var age = 29;
      age++; 
      
      //副效应：递增和递减操作是在包含它们的语句被求值之后才执行的
      var num1 = 2;
      var num2 = 20;
      var num3 = num1-- + num2; // 等于 22
      var num4 = num1 + num2; // 等于 21 
      ```

    - 规则：

      ```js
      var s1 = "2"; //包含有效数字字符的字符串时，先将其转换为数字值,再执行加减 1 的操作
      var s2 = "z"; //不包含有效数字字符的字符串时，将变量的值设置为 NaN
      var b = false;//布尔值，先将其转换为 0或者1 再执行加减 1 的操作
      var f = 1.1;  //浮点数值，执行加减 1 的操作
      var o = {//对象，先调用对象的 valueOf()方法，如果结果是NaN，在调用toString()方法
       valueOf: function() {
       return -1;
       }
      };  
      s1++; // 值变成数值 3
      s2++; // 值变成 NaN
      b++; // 值变成数值 1
      f--; // 值变成 0.10000000000000009（由于浮点舍入错误所致）
      o--; // 值变成数值-2 
      ```

  - 一元加和减操作符

    - 加：对数值不会产生影响；对非数值，类似Number()转型函数

      ```js
      var s1 = "01";
      var s2 = "1.1";
      var s3 = "z";
      var b = false;
      var f = 1.1;
      var o = {
       valueOf: function() {
       return -1;
       }
      };
      s1 = +s1; // 值变成数值 1
      s2 = +s2; // 值变成数值 1.1
      s3 = +s3; // 值变成 NaN
      b = +b; // 值变成数值 0
      f = +f; // 值未变，仍然是 1.1
      o = +o; // 值变成数值-1 
      
      ```

    - 减：把数值变成负数；对非数值，先符遵循与一元加操作符相同的规则，再将得到的数值转换为负数

      ```js
      var s1 = "01";
      var s2 = "1.1";
      var s3 = "z";
      var b = false;
      var f = 1.1;
      var o = {
       valueOf: function() {
       return -1;
       }
      };
      s1 = -s1; // 值变成了数值-1
      s2 = -s2; // 值变成了数值-1.1
      s3 = -s3; // 值变成了 NaN
      b = -b; // 值变成了数值 0
      f = -f; // 变成了-1.1
      o = -o; // 值变成了数值 1 
      ```

- **位操作符**

  - 概念：

    - ECMAScript 中的所有数值都以 IEEE-754 64 位格式存储

    - 对于有符号的整数，32 位中的前 31 位用于表示整数的值，第 32 位（符号位）用于表示数值的符号：0 表示正数，1 表示负数

    - 负数以二进制码存储，使用的格式是二进制补码

      ```js
      //求-18的的二进制码
      0000 0000 0000 0000 0000 0000 0001 0010 //第一步：绝对值18的二进制码
      1111 1111 1111 1111 1111 1111 1110 1101 //第二步：求二进制反码
                                            1 //第三步：得到的二进制反码加 1
      ---------------------------------------
      1111 1111 1111 1111 1111 1111 1110 1110 //
      ```

    - 在以二进制字符串形式输出一个负数时，看到的只是这个负数绝对值的二进制码前面加上了一个负号。

      ```js
      var num = -18;
      alert(num.toString(2)); // "-10010" 
      ```

    - 在对特殊的 NaN 和 Infinity 值应用位操作时，这两个值都会被当成 0 来处理

    - 对非数值应用位操作符，会先使用 Number()函数将该值转换为一个数值（自动完成），然后再应用位操作。得到的结果将是一个数值

  - 按位非（NOT）：由一个波浪线（~）表示，执行按位非的结果就是返回数值的反码

    ```js
    var num1 = 25;    // 二进制 00000000000000000000000000011001
    var num2 = ~num1; // 二进制 11111111111111111111111111100110
    alert(num2); // -26 
    ```

  - 按位与（AND）：由一个和号字符（&）表示，它有两个操作符数

    ```js
    //只在两个数值的对应位都是 1 时才返回 1，任何一位是 0，结果都是 0(全真则真，有假则假)
    var result = 25 & 3;// 0000 0000 0000 0000 0000 0000 0001 1001 
                        // 0000 0000 0000 0000 0000 0000 0000 0011
                        // 0000 0000 0000 0000 0000 0000 0000 0001 
    alert(result); //1 
    ```

  - 按位或（OR）：由一个竖线符号（|）表示，同样也有两个操作数

    ```js
    //在有一个位是 1 的情况下就返回 1，而只有在两个位都是 0 的情况下才返回 0(有真则真，全假则假)
    var result = 25 | 3;// 0000 0000 0000 0000 0000 0000 0001 1001
                        // 0000 0000 0000 0000 0000 0000 0000 0011 
                        // 0000 0000 0000 0000 0000 0000 0001 1011 
    alert(result); //27 
    ```

  - 按位异或（XOR）：由一个插入符号（^）表示，也有两个操作数

    ```js
    //对应位上只有一个 1 时才返回 1，如果对应的两位都是 1 或都是 0，则返回0
    var result = 25 ^ 3;// 0000 0000 0000 0000 0000 0000 0001 1001
                        // 0000 0000 0000 0000 0000 0000 0000 0011 
                        // 0000 0000 0000 0000 0000 0000 0001 1010
    alert(result); //26 
    ```

  - 左移：由两个小于号（<<）表示，这个操作符会将数值的所有位向左移动指定的位数

    ```js
    var oldValue = 2;             // 0000 0000 0000 0000 0000 0000 0000 0010
    var newValue = oldValue << 5; // 0000 0000 0000 0000 0000 0000 0010 0000
    alert(newValue);              // 64
    ```

  - 有符号的右移：由两个大于号（>>）表示，这个操作符会将数值向右移动，但保留符号位

    ```js
    var oldValue = 64;            // 0000 0000 0000 0000 0000 0000 0010 0000
    var newValue = oldValue  >> 5;// 0000 0000 0000 0000 0000 0000 0000 0010
    alert(newValue);              // 2
    ```

  - 无符号右移操作符由 3 个大于号（>>>）表示，这个操作符会将数值的所有 32 位都向右移动

    ```js
    var oldValue = -64;           // 1111 1111 1111 1111 1111 1111 1100 0000
    var newValue = oldValue >>> 5;// 0000 0111 1111 1111 1111 1111 1111 1110
    alert(newValue);              // 134217726 
    ```

- **布尔操作符**

  -  逻辑非：由一个叹号（！）表示，返回一个布尔值；两个逻辑非操作符，会模拟 Boolean()转型函数

  |             操作数             | 返回布尔值 |
  | :----------------------------: | :--------: |
  |              对象              |   false    |
  |            空字符串            |    true    |
  |           非空字符串           |   false    |
  |             数值 0             |    true    |
  | 任意非 0 数值（包括 Infinity） |   false    |
  |              NaN               |    true    |
  |              null              |    true    |
  |           undefined            |    true    |

  -  逻辑与：符由两个和号（&&）表示，有两个操作数

    - 规则：全真则真，有假则假

      ```js
      var result = true && true;//true
      var result = true && false;//false
      var result = false && true;//false
      var result = false && false;//false
      ```

    - 有一个操作数不是布尔值：

      ```js
      //如果第一个操作数是对象，则返回第二个操作数
      var result1 = new Object() && true;//true
      var result2 = new Object() && false;//false
      //如果第二个操作数是对象，则只有在第一个操作数的求值结果为 true 的情况下才会返回该对象
      var result3 = true && new Object();//{}
      var result4 = false && new Object();//false
      //如果两个操作数都是对象，则返回第二个操作数
      var result5 = new Object() && {name:'eric'};//{ name: 'eric' }
      //如果有一个操作数是 null，则返回 null
      var result6 = true && null;//null
      //如果有一个操作数是 NaN，则返回 NaN
      var result7 = true && NaN;//NaN
      //如果有一个操作数是 undefined，则返回 undefined
      var result8 = true && undefined;//undefined
      ```

    - 短路操作：如果第一个操作数是 false，则无论第二个操作数是什么值，结果都不再可能是true 了

      ```js
      var found = false;
      var result = (found && someUndefinedVariable); 
      alert(result); //  会执行（"false"）
      ```

  -  逻辑或：符由两个竖线符号（||）表示，有两个操作数

    - 规则：有真则真，全假则假

      ```js
      var result = true || true;//true
      var result = true || false;//true
      var result = false || true;//true
      var result = false || false;//false
      ```

    - 有一个操作数不是布尔值：

      ```js
      //如果第一个操作数是对象，则返回第一个操作数
      var result1 = new Object() || true;//{}
      var result2 = new Object() || false;//{}
      //如果第一个操作数的求值结果为 false，则返回第二个操作数
      var result3 = false || new Object();//{}
      //如果两个操作数都是对象，则返回第一个操作数
      var result4 = new Object() || {name:'eric'};//{}
      //如果有两个操作数是 null，则返回 null
      var result6 = null || null;//null
      //如果有两个操作数是 NaN，则返回 NaN
      var result7 = null || null;//NaN
      //如果有两个操作数是 undefined，则返回 undefined
      var result8 = undefined || undefined;//undefined
      ```

    - 短路操作：如果第一个操作数的求值结果为true，就不会对第二个操作数求值了

      ```js
      var found = true;
      var result = (found || someUndefinedVariable); // 不会发生错误
      alert(result); // 会执行（"true"）
      ```

    - 运用：避免为变量赋 null 或 undefined 值

      ```js
      var myObject = preferredObject || backupObject; 
      //如果 preferredObject 的值不是 null，那么它的值将被赋给 myObject
      //如果是 null，则将 backupObject 的值赋给 myObject
      ```

- **乘性操作符**

  - 乘法：由一个星号（*）表示，用于计算两个数值的乘积

    ```js
    //操作数都是数值，执行乘法运算
    var result1 = 34 * 56;//1221
    //如果有一个操作数是 NaN，则结果是 NaN
    var result2 = 34 * NaN;//NaN
    //如果是 Infinity 与 0 相乘，则结果是 NaN
    var result3 = Infinity * 0;//NaN
    //如果是 Infinity 与非 0 数值相乘，则结果是 Infinity 或-Infinity
    var result4 = Infinity * 8;//Infinity
    var result5 = Infinity * -8;//-Infinity
    //如果是 Infinity 与 Infinity 相乘，则结果是 Infinity
    var result6 = Infinity * Infinity;//Infinity
    //如果有一个操作数不是数值，则调用 Number()将其转换为数值，然后再应用上面的规则
    var result7 = true * 9;//9
    var result8 = null * 9;//0
    var result9 = '111' * 11;//1221
    ```

  - 除法：由一个斜线符号（/）表示，执行第二个操作数除第一个操作数的计算

    ```js
    //操作数都是数值，执行除法运算
    var result1 =  66 / 11;//6
    //如果有一个操作数是 NaN，则结果是 NaN
    var result2 = 34 / NaN;//NaN
    var result3 = NaN / 55;//NaN
    ////如果是 Infinity 被 Infinity 除，则结果是 NaN
    var result4 = Infinity / Infinity;//NaN
    //如果是零被零除，则结果是 NaN；
    var result5 = 0 / 0;//NaN
    //如果是非零的有限数被零除，则结果是 Infinity 或-Infinity
    var result6 = 77 / 0;//Infinity
    var result7 = -12 / 0;//-Infinity
    //如果是 Infinity 被任何非零数值除，则结果是 Infinity 或-Infinity
    var result8 = Infinity / 21;//Infinity
    var result9 = Infinity / -21;//-Infinity
    //如果有一个操作数不是数值，则在后台调用 Number()将其转换为数值，然后再应用上面的规则
    var result10 = true / 1;//1
    var result11 = null / 9;//0
    var result12 = '111' / 10;//11.1
    ```

  - 求模（余数）：由一个百分号（%）表示

    ```js
    //如果操作数都是数值，执行常规的除法计算，返回除得的余数
    var result1 = 26 % 5; //1
    //如果被除数是无穷大值而除数是有限大的数值，则结果是 NaN
    var result2 = Infinity % 28; //NaN
    //如果被除数是有限大的数值而除数是零，则结果是 NaN
    var result3 = 10 % 0; //NaN
    //如果是 Infinity 被 Infinity 除，则结果是 NaN
    var result4 = Infinity % Infinity;//NaN
    //如果被除数是有限大的数值而除数是无穷大的数值，则结果是被除数
    var result5 = 100 % Infinity;//100
    //如果被除数是零，则结果是零
    var result6 = 0 % Infinity;//0
    //如果有一个操作数不是数值，则在后台调用 Number()将其转换为数值，然后再应用上面的规则
    var result7 = true % 1;//0
    var result8 = null % 9;//0
    var result9 = '111' % 10;//1
    ```

- **加性操作符**

  - 加法

    ```js
    //如果两个操作符都是数值，执行常规的加法计算
    var result1 = 1 + 2;//3
    //如果有一个操作数是 NaN，则结果是 NaN
    var result2 = 1 + NaN;//NaN
    //如果是 Infinity 加 Infinity，则结果是 Infinity
    var result3 = Infinity + Infinity;//Infinity
    //如果是-Infinity 加-Infinity，则结果是-Infinity
    var result4 = -Infinity + -Infinity;//-Infinity
    //如果是 Infinity 加-Infinity，则结果是 NaN
    var result5 = -Infinity + Infinity;//NaN
    //如果是+0 加+0，则结果是+0
    var result6 = +0 + +0;//+0
    //如果是-0 加-0，则结果是-0
    var result7 = -0 + -0;//-0
    //如果是+0 加-0，则结果是+0
    var result8 = +0 + -0;//+0
    //如果两个操作数都是字符串，则将第二个操作数与第一个操作数拼接起来
    var result9 = 'hello' + 'world';//helloworld
    //如果只有一个操作数是字符串，则将另一个操作数转换为字符串，然后再将两个字符串拼接起来
    var result10 = '11' + 1;//111
    //如果有一个操作数是对象、数值或布尔值，则调用它们的 toString()方法取得相应的字符串值
    var result11 = {name:'jack'} + '1';//[object Object]1
    var result12 = 11 + '1';//111
    var result13 = '11' + true;//11true
    //对于 undefined 和 null，则分别调用 String()函数并取得字符串"undefined"和"null"
    var result14 = '11' + undefined;//11undefined
    var result15 = '11' + null;//11null
    ```

  - 减法

    ```js
    //如果两个操作符都是数值，则执行常规的算术减法操作并返回结果
    var result1 = 2 - 1;//1
    //如果有一个操作数是 NaN，则结果是 NaN
    var result2 = 1 - NaN;//NaN
    //如果是 Infinity 减 Infinity，则结果是 NaN
    //如果是-Infinity 减-Infinity，则结果是 NaN
    var result3 = Infinity - Infinity;
    var result4 = -Infinity - -Infinity;
    //如果是 Infinity 减-Infinity，则结果是 Infinity
    var result5 = Infinity - -Infinity;//Infinity
    //如果是-Infinity 减 Infinity，则结果是-Infinity；
    var result6 = -Infinity - Infinity;//-Infinity
    //如果是+0 减+0，则结果是+0
    //如果是+0 减-0，则结果是-0
    //如果是-0 减-0，则结果是+0
    var result7 = +0 - +0;//+0
    var result8 = +0 - -0;//-0
    var result9 = -0 - -0;//+0
    //如果有一个操作数是字符串、布尔值、null 或 undefined，则先用 Number()函数将其转换为数值
    //如果转换的结果是 NaN，则减法的结果就是 NaN
    //如果有一个操作数是对象，则调用对象的 valueOf()方法以取得表示该对象的数值
    //如果得到的值是 NaN，则减法的结果就是 NaN
    //如果对象没有 valueOf()方法，则调用其 toString()方法并将得到的字符串转换为数值
    var result10 = 5 - true; // 4，因为 true 被转换成了 1
    var result11 = NaN - 1; // NaN
    var result12 = 5 - 3; // 2
    var result13 = 5 - ""; // 5，因为"" 被转换成了 0
    var result14 = 5 - "2"; // 3，因为"2"被转换成了 2
    var result15 = 5 - null; // 5，因为 null 被转换成了 0 
    ```

- **关系操作符**

  - 小于（<）、大于（>）、小于等于（<=）和大于等于（>=）这几个关系操作符用于对两个值进行比较，这几个操作符都返回一个布尔值

    ```js
    var result1 = 5 > 3; //true
    var result2 = 5 < 3; //false 
    ```

  - 规则：

    - 如果两个操作数都是数值，则执行数值比较

    - 如果两个操作数都是字符串，则比较两个字符串对应的字符编码值

      ```js
      var result1 = "Brick" < "alphabet"; //true 
      var result2 = "Brick".toLowerCase() < "alphabet".toLowerCase(); //false 
      var result3 = "23" < "3"; //true 
      ```

    - 如果一个操作数是数值，则将另一个操作数转换为一个数值，然后执行数值比较

      ```js
      var result1 = "23" < 3; //false 
      var result2 = "a" < 3; // false，因为"a"被转换成了 NaN
      var result3 = NaN < 3; //false 
      var result4 = NaN >= 3; //false
      ```

    - 如果一个操作数是对象，则调用这个对象的 valueOf()方法，用得到的结果按照前面的规则执行比较。如果对象没有 valueOf()方法，则调用 toString()方法，并用得到的结果根据前面的规则执行比较

    - 如果一个操作数是布尔值，则先将其转换为数值，然后再执行比较

- **相等操作符**

  - 相等和不相等（ == 和 != ）—先转换再比较
    - 如果有一个操作数是布尔值，则在比较相等性之前先将其转换为数值——false 转换为 0，而true 转换为 1

      ```js
      false == 0  //true
      true == 1  //true 
      true == 2  //false 
      
      
      ```

    - 如果一个操作数是字符串，另一个操作数是数值，在比较相等性之前先将字符串转换为数值

      ```js
      "5"==5 //true
      ```

    - 如果一个操作数是对象，另一个操作数不是，则调用对象的 valueOf()方法，用得到的基本类型值按照前面的规则进行比较

    - null 和 undefined 是相等的

      ```js
      null == undefined //true 
      ```

    - 要比较相等性之前，不能将 null 和 undefined 转换成其他任何值

      ```js
      undefined == 0  //false
      null == 0  //false
      ```

    - 如果有一个操作数是 NaN，则相等操作符返回 false，而不相等操作符返回 true。重要提示：即使两个操作数都是 NaN，相等操作符也返回 false；因为按照规则，NaN 不等于 NaN

      ```js
      "NaN" == NaN //false 
      5 == NaN //false
      NaN == NaN //false
      NaN != NaN //true
      ```

    - 如果两个操作数都是对象，则比较它们是不是同一个对象。如果两个操作数都指向同一个对象，则相等操作符返回 true；否则，返回 false

  - 全等和不全等（ === 和 !== ）—仅比较而不转换

    - 全等在两个操作数未经转换就相等的情况下返回 true

      ```js
      var result1 = ("55" == 55); //true，因为转换后相等
      var result2 = ("55" === 55); //false，因为不同的数据类型不相等
      ```

    - 不全等在两个操作数未经转换就不相等的情况下返回 true

      ```js
      var result1 = ("55" != 55); //false，因为转换后相等
      var result2 = ("55" !== 55); //true，因为不同的数据类型不相等
      ```

    - null 和 undefined 是不全等的

      ```js
      null == undefined  //true 
      null === undefined  //false
      ```

- **条件操作符(三元表达式)**

  ```js
  var variable = boolean_expression ? true_value : false_value; 
  var max = (num1 > num2) ? num1 : num2;
  ```

- **赋值操作符**

  - 简单的赋值操作符由等于号（=）表示，其作用就是把右侧的值赋给左侧的变量

    ```js
    var num = 10; 
    ```

  - 如果在等于号（=）前面再添加乘性操作符、加性操作符或位操作符，就可以完成复合赋值操作

    ```js
    var num = 10;
    num *= 10; // 乘/赋值  num = num * 10;
    num /= 10; // 除/赋值  num = num / 10;
    num %= 10; // 模/赋值  num = num % 10;
    num += 10; // 加/赋值  num = num + 10;
    num -= 10; // 减/赋值  num = num - 10;
    num <<= 10; // 左移/赋值 num = num << 10;
    num >>= 10; // 有符号右移/赋值 num = num >> 10;
    num >>>= 10; // 无符号右移/赋值 num = num >>> 10;
    ```

- **逗号操作符**

  - 逗号操作符多用于声明多个变量

    ```js
    var num1=1, num2=2, num3=3; 
    ```

  - 逗号操作符还可以用于赋值

    ```js
    var num = (5, 1, 4, 8, 0); // num 的值为 0
    ```

#### 6、流程控制语句

- **if语句**

  ```js
  // 单行语句
  if (i > 25)
   alert("Greater than 25."); 
  else 
   alert("Less than or equal to 25."); 
  
  //写在一行
  if (condition1) statement1 else if (condition2) statement2 else statement3
  
  //推荐写法：使用代码块
  if (i > 25) {
   alert("Greater than 25.");
  } else if (i < 0) {
   alert("Less than 0.");
  } else {
   alert("Between 0 and 25, inclusive.");
  }
  ```

- **do-while语句**

  ```js
  //循环体内的代码至少会被执行一次
  do {
   statement
  } while (expression);
  //示例：
  var i = 0;
  do {
   i += 2;
  } while (i < 10);
  alert(i); 
  ```

- **while语句**

  ```js
  //循环体内的代码有可能永远不会被执行
  while(expression) statement
  //示例：
  var i = 0;
  while (i < 10) { 
       i += 2;
  } 
  ```

- **for语句**

  - 语法

    ```js
    for (initialization; expression; post-loop-expression) statement
    ```

  - 和while语句的转换

    ```js
    //for 循环语句
    var count = 10;
    for (var i = 0; i < count; i++){
     alert(i);
    } 
    // while 语句
    var count = 10;
    var i = 0;
    while (i < count){
     alert(i);
     i++;
    } 
    ```

  - 在 for 循环的变量初始化表达式中，也可以不使用 var 关键字

    ```js
    var count = 10;
    var i;
    for (i = 0; i < count; i++){
     alert(i);
    } 
    ```

  - 由于 ECMAScript 中不存在块级作用域，因此在循环内部定义的变量也可以在外部访问到

    ```js
    var count = 10;
    for (var i = 0; i < count; i++){ 
         alert(i);
    }
    alert(i); //10 
    ```

  - for 语句中的初始化表达式、控制表达式和循环后表达式都是可选的

    ```js
    for (;;) { // 无限循环
     doSomething();
    } 
    ```

  - 只给出控制表达式实际上就把 for 循环转换成了 while 循环

    ```js
    var count = 10;
    var i = 0;
    for (; i < count; ){
     alert(i);
     i++;
    } 
    ```

- **for-in语句**

  - for-in 语句是一种精准的迭代语句，可以用来枚举对象的属性

    ```js
    //语法
    for (property in expression) statement
    //示例：
    for (var propName in window) {
     document.write(propName);
    } 
    ```

  - 通过 for-in 循环输出的属性名的顺序是不可预测的

    ```js
    var obj={name:'eric',age:25,gender:0}
    for(var proName in obj){
        console.log(proName)
    }
    //name age gender
    ```

  - 如果表示要迭代的对象的变量值为 null 或 undefined，for-in 语句会抛出错误。ECMAScript 5 更正了这一行为；对这种情况不再抛出错误，而只是不执行循环体。建议在使用 for-in 循环之前，先检测确认该对象的值不是 null 或 undefined。

    ```js
    var obj=null;
    for(var proName in obj){
        console.log(proName)
    }
    //undefined
    ```

- **label语句**

  - 使用 label 语句可以在代码中添加标签，以便将来使用

    ```js
    label: statement 
    ```

  - 加标签的语句一般都要与 for 语句等循环语句配合使用

    ```js
    start: for (var i=0; i < count; i++) {
     alert(i);
    } 
    ```

  - 签可以在将来由 break 或 continue 语句引用

- **break和continue语句**

  - break 语句会立即退出循环，强制继续执行循环后面的语句

    ```js
    var num = 0;
    for (var i=1; i < 10; i++) {
     	if (i % 5 == 0) {
     		break;
     	}
     	num++;
    }
    alert(num); //4 
    ```

  - continue 语句虽然也是立即退出循环，但退出循环后会从循环的顶部继续执行

    ```js
    var num = 0;
    for (var i=1; i < 10; i++) {
     	if (i % 5 == 0) {
     		continue;
     	}
         num++;
    }
    alert(num); //8 
    ```

  - break 与 label 语句联合使用

    ```js
    var num = 0;
    outermost:
    for (var i=0; i < 10; i++) {
     	for (var j=0; j < 10; j++) {
     		if (i == 5 && j == 5) {
     		break outermost;
     		}
     		num++;
     	}
    }
    alert(num); //55 
    ```

  - continue 与 label 语句联合使用

    ```js
    var num = 0;
    outermost:
    for (var i=0; i < 10; i++) {
     	for (var j=0; j < 10; j++) {
     		if (i == 5 && j == 5) {
     			continue outermost;
     		}
     		num++;
     	}
    }
    alert(num); //95 
    ```

- **with语句**

  - with 语句的作用是将代码的作用域设置到一个特定的对象中

    ```js
    with (expression) statement; 
    ```

  - 定义 with 语句的目的主要是为了简化多次编写同一个对象的工作

    ```js
    var qs = location.search.substring(1);
    var hostName = location.hostname;
    var url = location.href; 
    ```

  - 包含同一个对象，可以简写

    ```js
    with(location){
     	var qs = search.substring(1);
     	var hostName = hostname;
     	var url = href;
    }
    ```

  - 严格模式下不允许使用 with 语句，否则将视为语法错误。由于大量使用 with 语句会导致性能下降，同时也会给调试代码造成困难，因此在开发大型应用程序时，不建议使用 with 语句

- **switch语句**

  - 语法

    ```js
    switch (expression) {
     case value: statement
     break;
     case value: statement
     break;
     case value: statement
     break;
     case value: statement
     break; 
     default: statement
    } 
    ```

  - 和if-else语句的转换

    ```js
    //if-else语句
    if (i == 25){
     	alert("25");
    } else if (i == 35) {
     	alert("35");
    } else if (i == 45) {
     	alert("45");
    } else {
     	alert("Other");
    } 
    //switch 语句
    switch (i) {
     	case 25: alert("25");
     	break;
     	case 35: alert("35");
     	break;
     	case 45: alert("45");
     	break;
     	default:alert("Other");
    } 
    ```

  - 混合几种情形，在代码中添加注释

    ```js
    switch (i) {
     	case 25:
     	/* 合并两种情形 */
     	case 35: alert("25 or 35");
     	break;
     	case 45: alert("45");
     	break;
     	default: alert("Other");
    } 
    ```

  - switch 语句中可以使用任何数据类型

    ```js
    switch ("hello world") {
     	case "hello" + " world":alert("Greeting was found.");
     	break;
     	case "goodbye":alert("Closing was found.");
     	break;
     	default:alert("Unexpected message was found.");
    }
    ```

  - switch 语句在比较值时使用的是全等操作符，因此不会发生类型转换

    ```js
    var num = 25;
    switch (true) {
     	case num < 0:alert("Less than 0.");
     	break;
     	case num >= 0 && num <= 10:alert("Between 0 and 10.");
     	break;
     	case num > 10 && num <= 20:alert("Between 10 and 20.");
     	break;
     	default:alert("More than 20.");
    } 
    ```

#### 7、函数

- **理解函数**

  - 语法：

    ```js
    function functionName(arg0, arg1,...,argN) {
     	statements
    }
    ```

  - ECMAScript 中的函数在定义时不必指定是否返回值，实际上，任何函数在任何时候都可以通过return 语句后跟要返回的值来实现返回值

    ```js
    function sum(num1, num2) {
     	return num1 + num2;
        alert("Hello world"); // 永远不会执行
    };
    var result = sum(5, 10);
    
    ```

  - return 语句也可以不带有任何返回值。在这种情况下，函数在停止执行后将返回 undefined值

    ```js
    function sayHi(name, message) {
     return;
     alert("Hello " + name + "," + message); //永远不会调用
    } 
    ```

  - 严格模式对函数有一些限制（如果发生以下情况，就会导致语法错误，代码无法执行）

    - 不能把函数命名为 eval 或 arguments；
    - 不能把参数命名为 eval 或 arguments；
    - 不能出现两个命名参数同名的情况

- **理解函数参数**

  - ECMAScript 函数不介意传递进来多少个参数，也不在乎传进来参数是什么数据类型

  - 在函数体内可以通过 arguments 对象来访问这个参数数组，从而获取传递给函数的每一个参数。通过访问 arguments 对象的 length 属性可以获知有多少个参数传递给了函数

    ```js
    function sayHi() {
     	alert("Hello " + arguments[0] + "," + arguments[1]);
    }  
    ```

  - 命名的参数只提供便利，但不是必需的，在命名参数方面，其他语言可能需要事先创建一个函数签名，而将来的调用必须与该签名一致

    ```js
    function doAdd() {
     	if(arguments.length == 1) {
     		alert(arguments[0] + 10);
     	} else if (arguments.length == 2) {
     		alert(arguments[0] + arguments[1]);
     	}
    }
    doAdd(10); //20
    doAdd(30, 20); //50 
    ```

  - 是 arguments 对象可以与命名参数一起使用

    ```js
    function doAdd(num1, num2) {
     	if(arguments.length == 1) {
     		alert(num1 + 10);
     	} else if (arguments.length == 2) {
     		alert(arguments[0] + num2);
     	}
    } 
    ```

  - 没有传递值的命名参数将自动被赋予 undefined 值。ECMAScript 中的所有参数传递的都是值，不可能通过引用传递参数。

- **没有重载**

  - 如果在 ECMAScript 中定义了两个名字相同的函数，则该名字只属于后定义的函数

    ```js
    function addSomeNumber(num){
     	return num + 100;
    }
    function addSomeNumber(num) {
     	return num + 200;
    }
    var result = addSomeNumber(100); //300 
    ```

  - 通过检查传入函数中参数的类型和数量并作出不同的反应，可以模仿方法的重载

    ```js
    function overLoading() {
    　　// 根据arguments.length，对不同的值进行不同的操作
    　　switch(arguments.length) {
    　　　　case 0:
    　　　　　　/*操作1的代码写在这里*/
    　　　　　　break;
    　　　　case 1:
    　　　　　　/*操作2的代码写在这里*/
    　　　　　　break;
    　　　　case 2:
    　　　　　　/*操作3的代码写在这里*/
    　　	//后面还有很多的case......
    	}
     
    }
    ```

#### 8、总结

​	JavaScript 的核心语言特性在 ECMA-262 中是以名为 ECMAScript 的伪语言的形式来定义的。
ECMAScript 中包含了所有基本的语法、操作符、数据类型以及完成基本的计算任务所必需的对象，但
没有对取得输入和产生输出的机制作出规定。理解 ECMAScript 及其纷繁复杂的各种细节，是理解其在
Web 浏览器中的实现——JavaScript 的关键。目前大多数实现所遵循的都是 ECMA-262 第 3 版，但很多
也已经着手开始实现第 5 版了。以下简要总结了 ECMAScript 中基本的要素。

- ECMAScript 中的基本数据类型包括 Undefined、Null、Boolean、Number 和 String
- 与其他语言不同，ECMScript 没有为整数和浮点数值分别定义不同的数据类型，Number 类型可用于表示所有数值
- ECMAScript 中也有一种复杂的数据类型，即 Object 类型，该类型是这门语言中所有对象的基础类型
- 严格模式为这门语言中容易出错的地方施加了限制
- ECMAScript 提供了很多与 C 及其他类 C 语言中相同的基本操作符，包括算术操作符、布尔操作符、关系操作符、相等操作符及赋值操作符等
- ECMAScript 从其他语言中借鉴了很多流控制语句，例如 if 语句、for 语句和 switch 语句等。ECMAScript 中的函数与其他语言中的函数有诸多不同之处。
- 无须指定函数的返回值，因为任何 ECMAScript 函数都可以在任何时候返回任何值
- 实际上，未指定返回值的函数返回的是一个特殊的 undefined 值
- ECMAScript 中也没有函数签名的概念，因为其函数参数是以一个包含零或多个值的数组的形式传递的
- 可以向 ECMAScript 函数传递任意数量的参数，并且可以通过 arguments 对象来访问这些参数
- 由于不存在函数签名的特性，ECMAScript 函数不能重载

