##四、引用类型

#### 1、Object 类型

- 使用 new 操作符后跟 Object 构造函数创建对象

  ```js
  //正常模式
  var person = new Object();
  person.name = "Nicholas";
  person.age = 29; 
  
  //简写
  var person = {}; //与 new Object()相同
  person.name = "Nicholas";
  person.age = 29; 
  ```

- 使用对象字面量表示法创建对象

  ```js
  //正常模式
  var person = {
   	name : "Nicholas",
   	age : 29
  }; 
  
  //属性名也可以使用字符串
  var person = {
   "name" : "Nicholas",
   "age" : 29,
   5 : true
  }; 
  ```

- 对象字面量也是向函数传递大量可选参数的首选方式

  ```js
  function displayInfo(args) {
      var output = "";
   	if (typeof args.name == "string"){
       	output += "Name: " + args.name + "\n";
   	}
   	if (typeof args.age == "number") {
   	 	output += "Age: " + args.age + "\n";
   	}
   	alert(output);
  }
  displayInfo({
   	name: "Nicholas",
   	age: 29
  });
  displayInfo({
   	name: "Greg"
  }); 
  ```

- 访问对象的方法：点表示法和方括号法

  ```js
  //点表示法
  alert(person.name); //"Nicholas"
  
  //方括号法
  alert(person["name"]); //"Nicholas"---正常
  
  var propertyName = "name";
  alert(person[propertyName]); //"Nicholas" ---使用变量
  
  person["first name"] = "Nicholas"; //--包含会导致语法错误的字符，比如空格
  ```

#### 2、Array 类型

- 概念

  - 数组的每一项可以保存任何类型的数据，数组最多可以包含 4 294 967 295 个项，超过这个上限值，就会发生异常

  - 创建数组的基本方式：Array 构造函数

    ```js
    var colors1 = new Array(); //空数组
    var colors2 = new Array(20); //数组长度为20
    var colors3 = Array(3); //省略new操作符，效果同上
    var colors4 = new Array('Grey'); // 创建一个包含 1 项，即字符串"Greg"的数组
    var colors5 = Array('Grey'); // 省略new操作符，效果同上
    var colors6 = new Array("red", "blue", "green"); //["red", "blue", "green"]
    ```

  - 创建数组的基本方式：数组字面量表示法

    ```js
    var colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
    var names = []; // 创建一个空数组
    var values = [1,2,]; // 不要这样！这样会创建一个包含 2 或 3 项的数组
    var options = [,,,,,]; // 不要这样！这样会创建一个包含 5 或 6 项的数组
    ```

  - 在读取和设置数组的值时，要使用方括号并提供相应值的基于 0 的数字索引

    ```js
    var colors = ["red", "blue", "green"]; // 定义一个字符串数组
    alert(colors[0]); // 显示第一项
    colors[2] = "black"; // 修改第三项
    colors[3] = "brown"; // 新增第四项
    ```

  - 使用length访问数组的长度

    ```js
    var colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
    var names = []; // 创建一个空数组
    alert(colors.length); //3
    alert(names.length); //0 
    ```

  - 使用length可以设置数组的长度

    ```js
    var colors1 = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
    colors1.length = 4;
    alert(colors1[3]); //undefined 
    
    var colors2 = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
    colors[colors2.length] = "black"; //（在位置 3）添加一种颜色
    colors[colors2.length] = "brown"; //（在位置 4）再添加一种颜色
    
    var colors3 = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
    colors3[99] = "black"; // （在位置 99）添加一种颜色
    alert(colors3.length); // 100 
    ```

- 检测数组

  - `instanceof `操作符：假定只有一个全局执行环境

    ```js
    if (value instanceof Array){
     	//对数组执行某些操作
    } 
    ```

  - `Array.isArray()`方法

    ```js
    if (Array.isArray(value)){
     	//对数组执行某些操作
    } 
    ```

- 转换方法

  - 调用数组的 toString()方法会返回由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串，而
    调用 valueOf()返回的还是数组

    ```js
    var colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
    alert(colors.toString()); // red,blue,green
    alert(colors.valueOf()); // red,blue,green
    alert(colors); // red,blue,green ,等同于toString
    ```

  - toLocaleString()方法经常也会返回与 toString()和 valueOf()方法相同的值，但也不总是如此

    ```js
    var person1 = {
     	toLocaleString : function () {
     		return "Nikolaos";
     	},
     	toString : function() {
     		return "Nicholas";
     	}
    };
    var person2 = {
     	toLocaleString : function () {
     		return "Grigorios";
     	},
     	toString : function() {
     		return "Greg";
     	}
    };
    var people = [person1, person2];
    alert(people); //Nicholas,Greg
    alert(people.toString()); //Nicholas,Greg
    alert(people.toLocaleString()); //Nikolaos,Grigorios 
    ```

  - 数组继承的 toLocaleString()、toString()和 valueOf()方法，在默认情况下都会以逗号分隔的字符串的形式返回数组项，而如果使用 join()方法，则可以使用不同的分隔符来构建这个字符串。join()方法只接收一个参数，即用作分隔符的字符串，然后返回包含所有数组项的字符串。

    ```js
    var colors = ["red", "green", "blue"];
    alert(colors.join(",")); //red,green,blue
    alert(colors.join()); //red,green,blue
    alert(colors.join(undefined)); //red,green,blue
    alert(colors.join("||")); //red||green||blue
    ```

  - 如果数组中的某一项的值是 null 或者 undefined，那么该值在 join()、toLocaleString()、toString()和 valueOf()方法返回的结果中以空字符串表示

    ```js
    var arr = [1,'red',undefined,null];
    arr.join();//"1,red,,"
    arr.toLocaleString() ;//"1,red,,"
    arr.toString();////"1,red,,"
    arr.valueOf();//[1, "red", undefined, null]
    ```

- 栈方法

  - push()方法：接收任意数量的参数，把它们逐个添加到数组末尾，并返回修改后数组的长度

  - pop()方法：从数组末尾移除最后一项，减少数组的 length 值，然后返回移除的项

    ```js
    var colors = new Array(); // 创建一个数组
    var count = colors.push("red", "green"); // 推入两项
    alert(count); //2
    
    count = colors.push("black"); // 推入另一项
    alert(count); //3
    
    var item = colors.pop(); // 取得最后一项
    alert(item); //"black"
    alert(colors.length); //2 
    ```

- 队列方法

  - shift()方法：移除数组中的第一个项并返回该项，同时将数组长度减 1

    ```js
    var colors = new Array(); //创建一个数组
    var count = colors.push("red", "green"); //推入两项
    alert(count); //2
    
    count = colors.push("black"); //推入另一项
    alert(count); //3
    
    var item = colors.shift(); //取得第一项
    alert(item); //"red"
    alert(colors.length); //2
    ```

  - unshift()方法：在数组前端添加任意个项并返回新数组的长度

    ```js
    var colors = new Array(); //创建一个数组
    var count = colors.unshift("red", "green"); //推入两项
    alert(count); //2 
    
    count = colors.unshift("black"); //推入另一项
    alert(count); //3 
    
    var item = colors.pop(); //取得最后一项
    alert(item); //"green"
    alert(colors.length); //2 
    ```

- 重排序方法

  - reverse()方法：反转数组项的顺序

    ```js
    var values = [1, 2, 3, 4, 5];
    values.reverse();
    alert(values); //5,4,3,2,1
    ```

  - sort()方法：

    - 默认是按升序排列数组项，会调用每个数组项的 toString()转型方法，然后比较得到的字符串，以确定如何排序。即使数组中的每一项都是数值，sort()方法比较的也是字符串，

      ```js
      var values = [0, 1, 5, 10, 15];
      values.sort();
      alert(values); //0,1,10,15,5 
      ```

    - sort()方法可以接收一个比较函数作为参数，以便我们指定哪个值位于哪个值的前面

      ```js
      function compare(value1, value2) {
       	if (value1 < value2) {
       		return 1;
       	} else if (value1 > value2) {
       		return -1;
       	} else {
       		return 0;
       	}
      }
      var values = [0, 1, 5, 10, 15];
      values.sort(compare);
      alert(values); // 15,10,5,1,0 
      ```

    - 对于数值类型或者其 valueOf()方法会返回数值类型的对象类型，可以使用更简单的比较函数

      ```js
      function compare(value1, value2){
       	return value2 - value1;
      } 
      ```

- 操作方法

  - concat()方法：基于当前数组中的所有项创建一个新数组

    ```js
    //在没有给 concat()方法传递参数的情况下，它只是复制当前数组并返回副本
    //如果传递给 concat()方法的是一或多个数组，则该方法会将这些数组中的每一项都添加到结果数组中
    //如果传递的值不是数组，这些值就会被简单地添加到结果数组的末尾
    var colors = ["red", "green", "blue"];
    var colors2 = colors.concat("yellow", ["black", "brown"]);
    alert(colors); //red,green,blue
    alert(colors2); //red,green,blue,yellow,black,brown 
    ```

  - slice()方法：基于当前数组中的一或多个项创建一个新数组

    ```js
    //在只有一个参数的情况下，slice()方法返回从该参数指定位置开始到当前数组末尾的所有项
    //如果有两个参数，该方法返回起始和结束位置之间的项,但不包括结束位置的项
    //slice()方法不会影响原始数组
    //如果 slice()方法的参数中有一个负数，则用数组长度加上该数来确定相应的位置
    var colors = ["red", "green", "blue", "yellow", "purple"];
    var colors2 = colors.slice(1);
    var colors3 = colors.slice(1,4);
    alert(colors2); //green,blue,yellow,purple
    alert(colors3); //green,blue,yellow
    ```

  - splice()方法：

    ```js
    //删除：指定 2 个参数：要删除的第一项的位置和要删除的项数
    //插入：提供 3 个参数：起始位置、0（要删除的项数）、要插入的项
    //替换：指定3个参数：起始位置、要删除的项数和要插入的任意数量的项
    //始终都会返回一个数组，该数组中包含从原始数组中删除的项（如果没有删除任何项，则返回空数组）
    var colors = ["red", "green", "blue"];
    var removed = colors.splice(0,1); // 删除第一项
    alert(colors); // green,blue
    alert(removed); // red，返回的数组中只包含一项
    
    removed = colors.splice(1, 0, "yellow", "orange"); // 从位置 1 开始插入两项
    alert(colors); // green,yellow,orange,blue
    alert(removed); // 返回的是一个空数组
    
    removed = colors.splice(1, 1, "red", "purple"); // 插入两项，删除一项
    alert(colors); // green,red,purple,orange,blue
    alert(removed); // yellow，返回的数组中只包含一项
    ```

- 位置方法

  - indexOf()方法：接受两个参数是查找项和查找起点位置的索引，从数组的开头（位置 0）开始向后查找

  - lastIndexOf()方法：接受两个参数是查找项和查找起点位置的索引，从数组的末尾开始向前查找

    ```js
    //这两个方法都返回要查找的项在数组中的位置，或者在没找到的情况下返回-1
    var numbers = [1,2,3,4,5,4,3,2,1];
    alert(numbers.indexOf(4)); //3 
    alert(numbers.lastIndexOf(4)); //5 
    
    alert(numbers.indexOf(4, 4)); //5
    alert(numbers.lastIndexOf(4, 4)); //3 
    
    var person = { name: "Nicholas" };
    var people = [{ name: "Nicholas" }]; 
    
    var morePeople = [person]; 
    alert(people.indexOf(person)); //-1
    alert(morePeople.indexOf(person)); //0 
    ```

- 迭代方法

  - every()方法：对数组中的每一项运行给定函数，如果该函数对每一项都返回 true，则返回 true

    ```JS
    //三个参数：数组项的值、该项在数组中的位置和数组对象本身
    var numbers = [1,2,3,4,5,4,3,2,1];
    var everyResult = numbers.every(function(item, index, array){
     return (item > 2);
    });
    alert(everyResult); //false
    ```

  - some()方法：对数组中的每一项运行给定函数，如果该函数对任一项返回 true，则返回 true

    ```JS
    //三个参数：数组项的值、该项在数组中的位置和数组对象本身
    var numbers = [1,2,3,4,5,4,3,2,1];
    var someResult = numbers.some(function(item, index, array){
     return (item > 2);
    });
    alert(someResult); //true 
    ```

  - filter()方法：对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组

    ```JS
    //三个参数：数组项的值、该项在数组中的位置和数组对象本身
    var numbers = [1,2,3,4,5,4,3,2,1];
    var filterResult = numbers.filter(function(item, index, array){
     return (item > 2);
    });
    alert(filterResult); //[3,4,5,4,3] 
    ```

  - map()方法：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组

    ```JS
    var numbers = [1,2,3,4,5,4,3,2,1];
    var mapResult = numbers.map(function(item, index, array){
     return item * 2;
    });
    alert(mapResult); //[2,4,6,8,10,8,6,4,2] 
    ```

  - forEach()方法：对数组中的每一项运行给定函数。这个方法没有返回值

    ```JS
    var numbers = [1,2,3,4,5,4,3,2,1];
    numbers.forEach(function(item, index, array){
     //执行某些操作
    }); 
    ```

- 归并方法

  - reduce()方法：从数组的第一项开始，逐个遍历到最后，然后构建一个最终返回的值

    ```JS
    
    ```

  - reduceRight()方法：从数组的最后一项开始，向前遍历到第一项，然后构建一个最终返回的值

    ```JS
    
    ```

#### 3、Date 类型

- 创建对象

  - 根据当前日期和时间创建对象：使用 new 操作符和 Date 构造函数

    ```js
    var now = new Date(); //Tue Dec 04 2018 22:39:16 GMT+0800 (中国标准时间)
    ```

  - 根据特定的日期和时间创建日期对象：

    - Date.parse()方法接收一个表示日期的字符串参数，然后尝试根据这个字符串返回相应日期的毫秒数

      ```js
      var someDate1 = new Date(Date.parse("May 25, 2004")); 
      var someDate2 = new Date(Date.parse("6/13/2004")); 
      var someDate3 = new Date(Date.parse("Tue May 25 2004 00:00:00 GMT-0700")); 
      var someDate4 = new Date(Date.parse("2004-05-25T00:00:00")); 
      var someDate5 = new Date("May 25, 2004"); //可以省略，后台调用Date.parse
      var someDate6 = new Date("NotNumber"); //NaN
      ```

    - Date.UTC()方法同样也返回表示日期的毫秒数，但它与 Date.parse()在构建值时使用不同的信息

      ```js
      //GMT 时间
      var y21k=new Date(Date.UTC(2000, 0));// GMT 时间 2000 年 1 月 1 日午夜零时
      var allFives1=new Date(Date.UTC(2005,4,5,17,55,55)); //2005,5,5,17:55:55
      //本地时间
      var y2k2 = new Date(2000, 0); // 本地时间 2000 年 1 月 1 日午夜零时
      var allFives2 = new Date(2005, 4, 5, 17, 55, 55);//2005年5月5日下午 5:55:55
      ```

  - ECMAScript 5 添加了 Data.now()方法：表示调用这个方法时的日期和时间的毫秒数

    ```js
    //支持 Data.now()方法
    var start = Date.now();//取得开始时间
    doSomething();//调用函数
    var stop = Date.now(),//取得停止时间
    result = stop – start; 
    
    //在不支持它的浏览器中,使用+操作符把 Data 对象转换成字符串
    var start = +new Date();//取得开始时间
    doSomething();//调用函数
    var stop = +new Date(),//取得停止时间
    result = stop - start; 
    ```

- 继承的方法

  - toLocaleString()和 toString()方法：按照与浏览器设置的地区相适应的格式返回日期和时间

    ```js
    new Date().toLocaleString()；//2018/12/4 下午10:48:27
    new Date().toString()；//Tue Dec 04 2018 22:48:54 GMT+0800 (中国标准时间)
    ```

  - Date 类型对象调用valueOf()方法

    ```js
    new Date().valueOf();//1543934978417
    typeof new Date().valueOf();//number
    ```

- 日期格式化方法

  - toDateString()——以特定于实现的格式显示星期几、月、日和年

    ```JS
    new Date().toDateString();//Tue Dec 04 2018
    ```

  - toTimeString()——以特定于实现的格式显示时、分、秒和时区

    ```JS
    new Date().toTimeString();//22:53:27 GMT+0800 (中国标准时间)
    ```

  - toLocaleDateString()——以特定于地区的格式显示星期几、月、日和年

    ```JS
    new Date().toLocaleDateString()//2018/12/4
    ```

  - toLocaleTimeString()——以特定于实现的格式显示时、分、秒

    ```JS
    new Date().toLocaleTimeString()//下午10:54:36
    ```

  - toUTCString()——以特定于实现的格式完整的 UTC 日期

    ```JS
    new Date().toUTCString()//Tue, 04 Dec 2018 14:55:13 GMT
    ```

- 日期/时间组件方法

  | 方 法                    | 说 明                                                        |
  | :----------------------- | :----------------------------------------------------------- |
  | getTime()                | 返回表示日期的毫秒数；与valueOf()方法返回的值相同            |
  | setTime(毫秒)            | 以毫秒数设置日期，会改变整个日期                             |
  | getFullYear()            | 取得4位数的年份（如2007而非仅07）                            |
  | getUTCFullYear()         | 返回UTC日期的4位数年份                                       |
  | setFullYear(年)          | 设置日期的年份。传入的年份值必须是4位数字（如2007而非仅07）  |
  | setUTCFullYear(年)       | 设置UTC日期的年份。传入的年份值必须是4位数字（如2007而非仅07） |
  | getMonth()               | 返回日期中的月份，其中0表示一月，11表示十二月                |
  | getUTCMonth()            | 返回UTC日期中的月份，其中0表示一月，11表示十二月             |
  | setMonth(月)             | 设置日期的月份。传入的月份值必须大于0，超过11则增加年份      |
  | setUTCMonth(月)          | 设置UTC日期的月份。传入的月份值必须大于0，超过11则增加年份   |
  | getDate()                | 返回日期月份中的天数（1到31）                                |
  | getUTCDate()             | 返回UTC日期月份中的天数（1到31）                             |
  | setDate(日)              | 设置日期月份中的天数。如果传入的值超过了该月中应有的天数，则增加月份 |
  | setUTCDate(日)           | 设置UTC日期月份中的天数。如果传入的值超过了该月中应有的天数，则增加月份 |
  | getDay()                 | 返回日期中星期的星期几（其中0表示星期日，6表示星期六）       |
  | getUTCDay()              | 返回UTC日期中星期的星期几（其中0表示星期日，6表示星期六）    |
  | getHours()               | 返回日期中的小时数（0到23）                                  |
  | getUTCHours()            | 返回UTC日期中的小时数（0到23）                               |
  | setHours(时)             | 设置日期中的小时数。传入的值超过了23则增加月份中的天数       |
  | setUTCHours(时)          | 设置UTC日期中的小时数。传入的值超过了23则增加月份中的天数    |
  | getMinutes()             | 返回日期中的分钟数（0到59）                                  |
  | getUTCMinutes()          | 返回UTC日期中的分钟数（0到59）                               |
  | setMinutes(分)           | 设置日期中的分钟数。传入的值超过59则增加小时数               |
  | setUTCMinutes(分)        | 设置UTC日期中的分钟数。传入的值超过59则增加小时数            |
  | getSeconds()             | 返回日期中的秒数（0到59）                                    |
  | getUTCSeconds()          | 返回UTC日期中的秒数（0到59）                                 |
  | setUTCSeconds(秒)        | 设置UTC日期中的秒数。传入的值超过了59会增加分钟数            |
  | getMilliseconds()        | 返回日期中的毫秒数                                           |
  | getUTCMilliseconds()     | 返回UTC日期中的毫秒数                                        |
  | setMilliseconds(豪秒)    | 设置日期中的毫                                               |
  | setUTCMilliseconds(毫秒) | 设置UTC日期中的毫秒数                                        |
  | getTimezoneOffset()      | 返回本地时间与UTC时间相差的分钟数。例如，美国东部标准时间返回300。在某地进入夏令时的情况下，这个值会有所变化 |

#### 4、RegExp 类型

- RegExp基本概念

  - 语法：包括模式（字符类、限定符、分组、向前查找以及反向引用）和标志（g、i、m）

    ```js
    //表达式
    var expression = / pattern / flags ; 
    //例子
    var pattern1 = /at/g; // 匹配字符串中所有"at"的实例
    var pattern2 = /[bc]at/i; // 匹配第一个"bat"或"cat"，不区分大小写
    var pattern3 = /.at/gi; // 匹配所有以"at"结尾的 3 个字符的组合，不区分大小写
    ```

  - 转义：模式中使用的所有元字符都必须转义`( [ { \ ^ $ | ) ? * + .]} `

    ```js
    var pattern1 = /[bc]at/i;// 匹配第一个"bat"或"cat"，不区分大小写
    var pattern2 = /\[bc\]at/i; // 匹配第一个" [bc]at"，不区分大小写
    var pattern3 = /.at/gi;  // 匹配所有以"at"结尾的 3 个字符的组合，不区分大小写
    var pattern4 = /\.at/gi;  //匹配所有".at"，不区分大小写
    ```

  - 创建方式

    - 字面量形式（前面的方式）

      ```js
      var pattern1 = /[bc]at/i; 
      ```

    - RegExp 构造函数：

      ```js
      //两个参数:一个是要匹配的字符串模式，另一个是可选的标志字符串
      var pattern1 = new RegExp("[bc]at", "i");
      //由于 RegExp 构造函数的模式参数是字符串，所以在某些情况下要对字符进行双重转义
      var pattern2 = new RegExp("\\[bc\\]at") //==/\[bc\]at/ 
      var pattern3 = new RegExp("\\.at) //==/\.at/ 
      var pattern4 = new RegExp("name\\/age") //==/name\/age/
      var pattern5 = new RegExp("\\d.\\d{1,2}" ) //==/\d.\d{1,2}//
      ```

    - 实例是否共享

      ```js
      //在ES3中，正则表达式字面量始终会共享同一个RegExp实例，
      //而使用构造函数创建的每一个新 RegExp 实例都是一个新实例
      //ES5明确规定，使用正则表达式字面量必须像直接调用 RegExp 构造函数一样，每次都创新实例
      var re = null;
      for (var i=0; i < 10; i++){
       	re = /cat/g;
       	re.test("catastrophe");
      }
      for (var i=0; i < 10; i++){
       re = new RegExp("cat", "g");
       re.test("catastrophe");
      } 
      ```

- RegExp实例属性

  - global：布尔值，表示是否设置了 g 标志

    ```js
    (/\[bc\]at/i).global//false
    (new RegExp("\\[bc\\]at", "i")).global//false
    ```

  - ignoreCase：布尔值，表示是否设置了 i 标志

    ```js
    (/\[bc\]at/i).ignoreCase//true
    (new RegExp("\\[bc\\]at", "i")).ignoreCase//true
    ```

  - lastIndex：整数，表示开始搜索下一个匹配项的字符位置，从 0 算起

    ```js
    (/\[bc\]at/i).lastIndex //0
    (new RegExp("\\[bc\\]at", "i")).lastIndex //0
    ```

  - multiline：布尔值，表示是否设置了 m 标志

    ```js
    (/\[bc\]at/i).multiline //false
    (new RegExp("\\[bc\\]at", "i")).multiline //false
    ```

  - source：正则表达式的字符串表示，按照字面量形式而非传入构造函数中的字符串模式返回

    ```js
    (/\[bc\]at/i).source // "\[bc\]at" 
    (new RegExp("\\[bc\\]at", "i")).source // "\[bc\]at" 
    ```

- RegExp实例方法

  - exec()方法

    ```js
    //接受一个参数，即要应用模式的字符串，然后返回包含第一个匹配项信息的数组
    //在没有匹配项的情况下返回 null
    //返回的数组虽然是 Array 的实例，但包含两个额外的属性：index 和 input
    //index 表示匹配项在字符串中的位置，而 input 表示应用正则表达式的字符串
    //在数组中，第一项是与整个模式匹配的字符串，其他项是与模式中的捕获组匹配的字符串
    //如果模式中没有捕获组，则该数组只包含一项
    var text = "mom and dad and baby";
    var pattern = /mom( and dad( and baby)?)?/gi; 
    var matches = pattern.exec(text);
    alert(matches.index); // 0
    alert(matches.input); // "mom and dad and baby"
    alert(matches[0]); // "mom and dad and baby"
    alert(matches[1]); // " and dad and baby"
    alert(matches[2]); // " and baby" 
    
    //在模式中设置了全局标志（g），它每次也只会返回一个匹配项
    //在不设置全局标志的情况下，在同一个字符串上多次调用 exec()将始终返回第一个匹配项的信息
    //在设置全局标志的情况下，每次调用 exec()则都会在字符串中继续查找新匹配项
    var text = "cat, bat, sat, fat";
    
    var pattern1 = /.at/; 
    var matches = pattern1.exec(text);
    alert(matches.index); //0
    alert(matches[0]); //cat
    alert(pattern1.lastIndex); //0 
    
    matches = pattern1.exec(text);
    alert(matches.index); //0
    alert(matches[0]); //cat
    alert(pattern1.lastIndex); //0 
    
    var pattern2 = /.at/g; 
    var matches = pattern2.exec(text);
    alert(matches.index); //0
    alert(matches[0]); //cat
    alert(pattern2.lastIndex); //3
    
    matches = pattern2.exec(text);
    alert(matches.index); //5
    alert(matches[0]); //bat
    alert(pattern2.lastIndex); //8 
    ```

  - test()方法

    ```js
    //接受一个字符串参数。在模式与该参数匹配的情况下返回true；否则，返回 false
    var text = "000-00-0000";
    var pattern = /\d{3}-\d{2}-\d{4}/;
    if (pattern.test(text)){
     	alert("The pattern was matched.");
    } 
    ```

  - RegExp 实例继承的 toLocaleString()和 toString()方法都会返回正则表达式的字面量

    ```js
    var pattern = new RegExp("\\[bc\\]at", "gi");
    alert(pattern.toString()); // /\[bc\]at/gi
    alert(pattern.toLocaleString()); // /\[bc\]at/gi 
    ```

  - 正则表达式的 valueOf()方法返回正则表达式本身

    ```js
    (new RegExp("\\[bc\\]at", "gi")).valueOf();// /\[bc\]at/gi
    (/\[bc\]at/gi).valueOf();// /\[bc\]at/gi
    ```

- RegExp构造函数属性（静态属性）

  - 各个属性及其返回值

    ```js
    var text = "this has been a short summer";
    var pattern = /(.)hort/g;
    /*
     * 注意：Opera 不支持 input、lastMatch、lastParen 和 multiline 属性
     * Internet Explorer 不支持 multiline 属性
     */
    if (pattern.test(text)){//写法一
     	alert(RegExp.input); // this has been a short summer
     	alert(RegExp.leftContext); // this has been a
     	alert(RegExp.rightContext); // summer
     	alert(RegExp.lastMatch); // short
     	alert(RegExp.lastParen); // s
     	alert(RegExp.multiline); // false
    }
    if (pattern.test(text)){//写法二
     	alert(RegExp.$_); // this has been a short summer
     	alert(RegExp["$`"]); // this has been a
     	alert(RegExp["$'"]); // summer
     	alert(RegExp["$&"]); // short
     	alert(RegExp["$+"]); // s
     	alert(RegExp["$*"]); // false
    } 
    ```

  -  9 个用于存储捕获组的构造函数属性

    ```js
    var text = "this has been a short summer";
    var pattern = /(..)or(.)/g;
    
    if (pattern.test(text)){
     	alert(RegExp.$1); //sh
     	alert(RegExp.$2); //t
    } 
    ```

- 模式的局限性

  - 匹配字符串开始和结尾的\A 和\Z 锚，但支持以插入符号（^）和美元符号（$）
  - 向后查找，但完全支持向前查找
  - 并集和交集类
  - 原子组（atomic grouping）
  - Unicode 支持（单个字符除外，如\uFFFF）
  - 命名的捕获组，但支持编号的捕获组
  - s（single，单行）和 x（free-spacing，无间隔）匹配模式
  - 条件匹配
  - 正则表达式注释

#### 5、Function 类型

- 基本概念

  - 函数实际上是对象。每个函数都是 Function 类型的实例，而且都与其他引用类型一样具有属性和方法

  - 函数定义的方式：

    - 函数声明语法

      ```js
      function sum (num1, num2) {
       	return num1 + num2;
      } 
      ```

    - 函数表达式

      ```js
      var sum = function(num1, num2){
      	 return num1 + num2;
      };
      ```

    - Function 构造函数

      ```js
      var sum = new Function("num1", "num2", "return num1 + num2"); // 不推荐
      ```

  - 函数名仅仅是指向函数的指针，不会与某个函数绑定

    ```js
    function sum(num1, num2){
     	return num1 + num2;
    }
    alert(sum(10,10)); //20
    var anotherSum = sum;
    alert(anotherSum(10,10)); //20
    sum = null;
    alert(anotherSum(10,10)); //20 
    ```

- 没有重载

  ```js
  function addSomeNumber(num){
   	return num + 100;
  }
  function addSomeNumber(num) {
   	return num + 200;
  }
  var result = addSomeNumber(100); //300 
  ```

- 函数声明与函数表达式

  - 解析器会率先读取函数声明，并使其在执行任何代码之前可用（可以访问）

    ```js
    alert(sum(10,10));//20
    function sum(num1, num2){
     	return num1 + num2;
    } 
    ```

  - 函数表达式必须等到解析器执行到它所在的代码行，才会真正被解释执行

    ```js
    alert(sum(10,10));//报错
    var sum = function(num1, num2){
     	return num1 + num2;
    }; 
    ```

  - 可以同时使用函数声明和函数表达式，但是这种语法在 Safari 中会导致错误

    ```js
    var sum = function sum(){}
    ```

- 作为值的函数

  - 可以像传递参数一样把一个函数传递给另一个函数

    ```js
    function callSomeFunction(someFunction, someArgument){
     	return someFunction(someArgument);
    }
    function add10(num){
     	return num + 10;
    }
    var result1 = callSomeFunction(add10, 10);
    alert(result1); //20
    function getGreeting(name){
     	return "Hello, " + name;
    }
    var result2 = callSomeFunction(getGreeting, "Nicholas");
    alert(result2); //"Hello, Nicholas" 
    ```

  - 可以将一个函数作为另一个函数的结果返回

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
    var data = [{name: "Zachary", age: 28}, {name: "Nicholas", age: 29}];
    data.sort(createComparisonFunction("name"));
    alert(data[0].name); //Nicholas
    data.sort(createComparisonFunction("age"));
    alert(data[0].name); //Zachary 
    ```

- 函数内部属性

  - arguments：类数组对象，包含着传入函数中的所有参数；有一个callee属性，是一个指针，指向拥有这个 arguments 对象的函数

    ```js
    function factorial(num){
     if (num <=1) {
     	return 1;
     } else {
     	return num * arguments.callee(num-1)
     }
    } 
    ```

  - this：引用的是函数据以执行的环境对象

    ```js
    window.color = "red";
    var o = { color: "blue" };
    function sayColor(){
     	alert(this.color);
    }
    sayColor(); //"red" //
    o.sayColor = sayColor;
    o.sayColor(); //"blue" 
    ```

  - caller：保存着调用当前函数的函数的引用，如果是在全局作用域中调用当前函数，它的值为 null

    ```js
    function outer(){
     	inner();
    }
    function inner(){
     	alert(inner.caller);//也可以写成
    }
    outer();//显示 outer()函数的源代码。
    ```

- 函数属性和方法

  - length属性：表示函数希望接收的命名参数的个数

    ```js
    function sayName(name){
     	alert(name);
    }
    function sum(num1, num2){
     	return num1 + num2;
    }
    function sayHi(){
     	alert("hi");
    }
    alert(sayName.length); //1
    alert(sum.length); //2
    alert(sayHi.length); //0 
    ```

  - prototype属性：保存所有实例方法的原型对象（后面介绍）

  - apply()方法：在特定的作用域中调用函数，实际上等于设置函数体内 this 对象的值

    ```js
    //接收两个参数：一个是在其中运行函数的作用域，另一个是参数数组
    //第二个参数可以是 Array 的实例，也可以是arguments 对象
    function sum(num1, num2){
     	return num1 + num2;
    }
    function callSum1(num1, num2){
     	return sum.apply(this, arguments); // 传入 arguments 对象
    }
    function callSum2(num1, num2){
     	return sum.apply(this, [num1, num2]); // 传入数组
    }
    alert(callSum1(10,10)); //20
    alert(callSum2(10,10)); //20 
    ```

  - call()方法：与 apply()方法的作用相同，它们的区别仅在于接收参数的方式不同

    ```js
    //第一个参数是 this 值没有变化，变化的是其余参数都直接传递给函数
    function sum(num1, num2){
     	return num1 + num2;
    }
    function callSum(num1, num2){
     	return sum.call(this, num1, num2);
    }
    alert(callSum(10,10)); //20
    
    //apply()和 call()真正的用武之地:扩充函数赖以运行的作用域
    window.color = "red";
    var o = { color: "blue" };
    function sayColor(){
     	alert(this.color);
    }
    sayColor(); //red
    sayColor.call(this); //red
    sayColor.call(window); //red
    sayColor.call(o); //blue 
    ```

  - bind()方法：创建一个函数的实例，其 this 值会被绑定到传给 bind()函数的值

    ```js
    window.color = "red";
    var o = { color: "blue" };
    function sayColor(){
     	alert(this.color);
    }
    var objectSayColor = sayColor.bind(o);
    objectSayColor(); //blue 
    ```

  - toLocaleString()和 toString()方法：始终都返回函数的代码

    ```js
    function sayHello(){console.log('hello');}
    sayHello.toString();//"function sayHello(){console.log('hello');}"
    sayHello.toLocaleString();//"function sayHello(){console.log('hello');}"
    ```

  - valueOf()方法：也只返回函数代码

    ```js
    function sayHello(){console.log('hello');}
    sayHello.valueOf()//sayHello(){console.log('hello');}
    ```

#### 6、基本包装类型

- 基本概念
  - 为了便于操作基本类型值，提供了 3 个特殊的引用类型：Boolean、Number 和String

  - 当读取一个基本类型值的时候，后台就会创建一个对应的基本包装类型的对象

    ```js
    var s1 = "some text";
    var s2 = s1.substring(2); 
    
    //后台处理：
    var s1 = new String("some text");//创建 String 类型的一个实例；
    var s2 = s1.substring(2);        //在实例上调用指定的方法
    s1 = null;                       //销毁这个实例
    ```

  - 引用类型与基本包装类型的主要区别就是对象的生存期

    ```js
    var s1 = "some text";
    s1.color = "red";
    alert(s1.color); //undefined 
    ```

  - Object 构造函数也会像工厂方法一样，根据传入值的类型返回相应基本包装类型的实例

    ```js
    var obj = new Object("some text");
    alert(obj instanceof String); //true 
    ```

  - 使用 new 调用基本包装类型的构造函数，与直接调用同名的转型函数是不一样的

    ```js
    var value = "25";
    var number = Number(value); //转型函数
    alert(typeof number); //"number"
    var obj = new Number(value); //构造函数
    alert(typeof obj); //"object"
    ```

- Boolean类型

  - 创建：调用 Boolean构造函数并传入 true 或 false 值

    ```js
    
    ```

  - 方法：

    - 重写了valueOf()方法，返回基本类型值true 或false
    - 重写了toString()和toLocaleString()方法，返回字符串"true"和"false"

  - 布尔表达式中的所有对象都会被转换为 true

    ```js
    var falseObject = new Boolean(false);
    var result = falseObject && true;
    alert(result); //true
    var falseValue = false;
    result = falseValue && true;
    alert(result); //false
    ```

  - typeof 操作符对基本类型返回"boolean"，而对引用类型返回"object"

    ```js
    var falseObject = new Boolean(false);
    var falseValue = false;
    alert(typeof falseObject); //object
    alert(typeof falseValue); //boolean 
    ```

  - 使用 instanceof操作符测试 Boolean 对象会返回 true，而测试基本类型的布尔值则返回 false

    ```js
    var falseObject = new Boolean(false);
    var falseValue = false;
    alert(falseObject instanceof Boolean); //true
    alert(falseValue instanceof Boolean); //false 
    ```

- Number类型

  - 创建：调用 Number 构造函数时向其中传递相应的数值

    ```js
    var numberObject = new Number(10); 
    ```

  - 方法：

    - 重写了valueOf()方法，返回对象表示的基本类型的数值

    - 重写了toString()和toLocaleString()方法，返回字符串形式的数值

      ```js
      //toString()方法传递一个表示基数的参数，返回几进制数值的字符串形式
      var num = 10;
      alert(num.toString()); //"10"
      alert(num.toString(2)); //"1010"
      alert(num.toString(8)); //"12"
      alert(num.toString(10)); //"10"
      alert(num.toString(16)); //"a" 
      ```

    - toFixed()方法会按照指定的小数位返回数值的字符串表示

      ```js
      var num = 10;
      alert(num.toFixed(2)); //"10.00" 
      
      var num = 10.005;
      alert(num.toFixed(2)); //"10.01" 
      ```

    - toExponential()方法：该方法返回以指数表示法（也称 e 表示法）表示的数值的字符串形式

      ```js
      var num = 10;
      alert(num.toExponential(1)); //"1.0e+1" 
      ```

    - toPrecision()方法可能会返回固定大小（fixed）格式，也可能返回指数（exponential）格式

      ```js
      //接收一个参数，即表示数值的所有数字的位数（不包括指数部分）
      var num = 99;
      alert(num.toPrecision(1)); //"1e+2"
      alert(num.toPrecision(2)); //"99"
      alert(num.toPrecision(3)); //"99.0" 
      ```

  - typeof 操作符对基本类型返回"number"，而对引用类型返回"object"

    ```js
    var numberObject = new Number(10);
    var numberValue = 10;
    alert(typeof numberObject); //"object"
    alert(typeof numberValue); //"number" 
    ```

  - 使用 instanceof操作符测试 Number对象会返回 true，而测试基本类型的布尔值则返回 false

    ```js
    var numberObject = new Number(10);
    var numberValue = 10;
    alert(numberObject instanceof Number); //true
    alert(numberValue instanceof Number); //false 
    ```

- String类型

  - 创建：调用 String构造函数时向其中传递相应的字符串

    ```js
    var stringObject = new String("hello world"); 
    ```

  - 基本方法：

    - valueOf()、toLocaleString()和toString()方法，都返回对象所表示的基本字符串值

    - length属性：表示字符串中包含多个字符

      ```js
      var stringValue = "hello world";
      alert(stringValue.length); //"11" 
      ```

  - 字符方法：

    - charAt()方法：以单字符字符串的形式返回给定位置的那个字符（ECMAScript 中没有字符类型）

      ```js
      var stringValue = "hello world";
      alert(stringValue.charAt(1)); //"e" 
      ```

    - charCodeAt()方法：以字符编码的形式返回给定位置的那个字符

      ```js
      var stringValue = "hello world";
      alert(stringValue.charCodeAt(1)); //输出"101"
      ```

    - 方括号加数字索引来访问字符串中的特定字符

      ```js
      var stringValue = "hello world";
      alert(stringValue[1]); //"e" 
      ```

  - 字符串操作方法

    - concat()方法：用于将一或多个字符串拼接起来，返回拼接得到的新字符串

      ```js
      //一个参数
      var stringValue = "hello ";
      var result = stringValue.concat("world");
      alert(result); //"hello world"
      alert(stringValue); //"hello" 
      //多个参数
      var stringValue = "hello ";
      var result = stringValue.concat("world", "!");
      alert(result); //"hello world!"
      alert(stringValue); //"hello" 
      ```

    - slice()、substr()和 substring()：返回被操作字符串的一个子字符串，都接受一或两个参数

      ```js
      //第一个参数指定子字符串的开始位置
      //slice()和substring()的第二个参数（在指定的情况下）表示子字符串到哪里结束
      //substr()的第二个参数指定的则是返回的字符个数
      //如果没有给这些方法传递第二个参数，则将字符串的长度作为结束位置
      var stringValue = "hello world";
      alert(stringValue.slice(3)); //"lo world"
      alert(stringValue.substring(3)); //"lo world"
      alert(stringValue.substr(3)); //"lo world"
      alert(stringValue.slice(3, 7)); //"lo w"
      alert(stringValue.substring(3,7)); //"lo w"
      alert(stringValue.substr(3, 7)); //"lo worl" 
      
      //在传递给这些方法的参数是负值的情况下，它们的行为就不尽相同
      //slice()方法会将传入的负值与字符串的长度相加，
      //substr()方法将负的第一个参数加上字符串的长度，而将负的第二个参数转换为 0
      //substring()方法会把所有负值参数都转换为 0
      var stringValue = "hello world";
      alert(stringValue.slice(-3)); //"rld"
      alert(stringValue.substring(-3)); //"hello world"
      alert(stringValue.substr(-3)); //"rld"
      alert(stringValue.slice(3, -4)); //"lo w"
      alert(stringValue.substring(3, -4)); //"hel"
      alert(stringValue.substr(3, -4)); //""（空字符串）
      
      ```

  - 字符串位置方法

    - indexOf()方法：：从字符串的开头向后搜索子字符串，返子字符串的位置（没有找到返回-1）

      ```js
      //一个参数
      var stringValue = "hello jsdada";
      alert(stringValue.indexOf("o")); //4
      //两个参数：第二个参数表示从字符串中的哪个位置开始搜索
      var stringValue = "hello world";
      alert(stringValue.indexOf("o", 6)); //7
      ```

    - lastIndexOf()方法：从字符串的末尾向前搜索子字符串，返子字符串的位置（没有找到返回-1）

      ```js
      //一个参数
      var stringValue = "hello jsdasda";
      alert(stringValue.lastIndexOf("o")); //4
      //两个参数：第二个参数表示从字符串中的哪个位置开始搜索
      var stringValue = "hello world";
      alert(stringValue.lastIndexOf("o", 6)); //4 
      ```

    - 扩展：循环调用 indexOf()或 lastIndexOf()来找到所有匹配的子字符串

      ```js
      
      ```

  - trim()方法：

    - 会创建一个字符串的副本，删除前置及后缀的所有空格，然后返回结果

    - 由于 trim()返回的是字符串的副本，所以原始字符串中的前置及后缀空格会保持不变

      ```js
      var stringValue = " hello world ";
      var trimmedStringValue = stringValue.trim();
      alert(stringValue); //" hello world "
      alert(trimmedStringValue); //"hello world" 
      ```

  - 字符串大小写转换方法

    - toLowerCase()和 toUpperCase()是两个经典的方法，借鉴自 java.lang.String 中的同名方法

    - toLocaleLowerCase()和 toLocaleUpperCase()方法则是针对特定地区的实现

      ```js
      var stringValue = "hello world";
      alert(stringValue.toLocaleUpperCase()); //"HELLO WORLD"
      alert(stringValue.toUpperCase()); //"HELLO WORLD"
      alert(stringValue.toLocaleLowerCase()); //"hello world"
      alert(stringValue.toLowerCase()); //"hello world"
      ```

  - 字符串的模式匹配方法

    - match()方法：在字符串上调用这个方法，本质上与调用 RegExp 的 exec()方法相同

      ```js
      //match()方法只接受一个参数，要么是一个正则表达式，要么是一个 RegExp 对象
      var text = "cat, bat, sat, fat";
      var pattern = /.at/;
      //与 pattern.exec(text)相同
      var matches = text.match(pattern);
      alert(matches.index); //0
      alert(matches[0]); //"cat"
      alert(pattern.lastIndex); //0 
      ```

    - search()方法：返回字符串中第一个匹配项的索引；如果没有找到匹配项，则返回-1。

      ```js
      //search()方法始终是从字符串开头向后查找模式
      var text = "cat, bat, sat, fat";
      var pos = text.search(/at/);
      alert(pos); //1 
      ```

    - replace()方法：替换子字符串的操作

      ```js
      //第一个参数可以是一个 RegExp 对象或者一个字符串（这个字符串不会被转换成正则表达式）
      //第二个参数可以是一个字符串或者一个函数
      //如果第一个参数是字符串，那么只会替换第一个子字符串
      //要想替换所有子字符串，唯一的办法就是提供一个正则表达式，而且要指定全局（g）标志
      var text = "cat, bat, sat, fat";
      var result = text.replace("at", "ond");
      alert(result); //"cond, bat, sat, fat"
      result = text.replace(/at/g, "ond");
      alert(result); //"cond, bond, sond, fond" 
      
      //如果第二个参数是字符串，那么还可以使用一些特殊的字符序列
      var text = "cat, bat, sat, fat";
      result = text.replace(/(.at)/g, "word ($1)");
      alert(result); //word (cat), word (bat), word (sat), word (fat)
      
      //第二个参数也可以是一个函数
      function htmlEscape(text){
       	return text.replace(/[<>"&]/g, function(match, pos, originalText){
       		switch(match){
       			case "<":return "&lt;";
       			case ">":return "&gt;";
       			case "&":return "&amp;";
       			case "\"":return "&quot;";
       		}
       	});
      }
      alert(htmlEscape("<p class=\"greeting\">Hello world!</p>"));
      //&lt;p class=&quot;greeting&quot;&gt;Hello world!&lt;/p&gt; 
      ```

    - split()方法：基于指定的分隔符将一个字符串分割成多个子字符串，并将结果放在一个数组中

      ```js
      var colorText = "red,blue,green,yellow";
      var colors1 = colorText.split(","); //["red", "blue", "green", "yellow"]
      var colors2 = colorText.split(",", 2); //["red", "blue"]
      var colors3 = colorText.split(/[^\,]+/); //["", ",", ",", ",", ""] 
      ```

  - localeCompare()方法

    - 在字母表比较两个字符串，并返回下列值中的一个：

      ```js\
      var stringValue = "yellow";
      alert(stringValue.localeCompare("brick")); //1
      alert(stringValue.localeCompare("yellow")); //0
      alert(stringValue.localeCompare("zoo")); //-1 
      ```

    - localeCompare()返回的数值取决于实现

      ```js
      function determineOrder(value) {
       	var result = stringValue.localeCompare(value);
       	if (result < 0){
       		alert(" 'yellow' comes before '" + value + "'.");
       	} else if (result > 0) {
       		alert("'yellow' comes after '" + value + "'.");
       	} else { alert("'yellow' is equal to'" + value + "'.");}
      }
      determineOrder("brick");
      determineOrder("yellow");
      determineOrder("zoo"); 
      ```

    - 

  -  fromCharCode()方法

    - 静态方法：接收一或多个字符编码，然后将它们转换成一个字符串。

    - 从本质上来看，这个方法与实例方法 charCodeAt()执行的是相反的操作

      ```js
      alert(String.fromCharCode(104, 101, 108, 108, 111)); //"hello"
      ```

  - HTML 方法

    |      方 法       | 输出结果                               |
    | :--------------: | -------------------------------------- |
    |   anchor(name)   | \<a name = 'name'>string\</a>          |
    |      big()       | \<big>string\</big>                    |
    |      bold()      | \<b>string\</b>                        |
    |     fixed()      | \<tt>string\</tt>                      |
    | fontcolor(color) | \<font color = 'color''>string\</font> |
    |  fontsize(size)  | \<font color = 'size''>string\</font>  |
    |    italics()     | \<i>string\</i>                        |
    |    link(url)     | \<a href = 'url'>string\</a>           |
    |     small()      | \<small>string\</small>                |
    |     strike()     | \<strike>string\</strike>              |
    |      sub()       | \<sub>string\</sub>                    |
    |      sup()       | \<sup>string\</sup>                    |

#### 7、单体内置对象

- 基本概念

  - 定义：由 ECMAScript 实现提供的、不依赖于宿主环境的对象，这些对象在 ECMAScript 程序执行之前就已经存在了
  - 包括：Object、Array 和 String。ECMA-262 还定义了两个单体内置对象：Global 和 Math

- Global对象

  - 不属于任何其他对象的属性和方法，最终都是它的属性和方法；事实上，没有全局变量或全局函数；所有在全局作用域中定义的属性和函数，都是 Global 对象的属性，诸如 isNaN()、isFinite()、parseInt()以及 parseFloat()等等

  - URI 编码方法

    - encodeURI()方法：主要用于整个 URI（例如，http://www.wrox.com/illegal value.htm）进行编码

      ```js
      var uri = "http://www.wrox.com/illegal value.htm#start";
      //"http://www.wrox.com/illegal%20value.htm#start"
      alert(encodeURI(uri));
      ```

    - encodeURIComponent()方法：主要用于对 URI 中的某一段（例如前面 URI 中的 illegal value.htm）进行编码

      ```js
      var uri = "http://www.wrox.com/illegal value.htm#start";
      //"http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23start"
      alert(encodeURIComponent(uri)); 
      ```

    - decodeURI()方法：只能对使用 encodeURI()替换的字符进行解码

      ```js
      var uri = "http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23start";
      //http%3A%2F%2Fwww.wrox.com%2Fillegal value.htm%23start
      alert(decodeURI(uri));
      ```

    - encodeURIComponent()方法:解码使用 encodeURIComponent()编码的所有字符，即它可以解码任何特殊字符的编码

      ```js
      var uri = "http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23start";
      //http://www.wrox.com/illegal value.htm#start
      alert(decodeURIComponent(uri)); 
      ```

  - eval()方法

    - 像是一个完整的 ECMAScript 解析器，它只接受一个参数，即要执行的 JavaScript）字符串

      ```js
      eval("alert('hi')");//等价于 alert("hi"); 
      ```

    - 通过 eval()执行的代码可以引用在包含环境中定义的变量

      ```js
      var msg = "hello world";
      eval("alert(msg)"); //"hello world" 
      ```

    - 可以在 eval()调用中定义一个函数，然后再在该调用的外部代码中引用这个函数

      ```js
      eval("function sayHi() { alert('hi'); }");
      sayHi(); 
      ```

    - 在 eval()中创建的任何变量或函数都不会被提升

      ```js
      eval("var msg = 'hello world'; ");
      alert(msg); //"hello world"
      ```

    - 严格模式下，在外部访问不到 eval()中创建的任何变量或函数

      ```js
      "use strict";
      eval = "hi"; //causes error 
      ```

  -  Global 对象的属性

    - 特殊的值undefined、NaN 以及 Infinity 都是 Global 对象的属性
    - 所有原生引用类型的构造函数，像Object 和 Function，也都是 Global 对象的属性

  |   属 性   | 说 明            |     属 性      | 说 明                  |
  | :-------: | ---------------- | :------------: | ---------------------- |
  | undefined | 特殊值undefined  |      Date      | 构造函数Date           |
  |    NaN    | 特殊值NaN        |     RegExp     | 构造函数RegExp         |
  | Infinity  | 特殊值Infinity   |     Error      | 构造函数Error          |
  |  Object   | 构造函数Object   |   EvalError    | 构造函数EvalError      |
  |   Array   | 构造函数Array    |   RangeError   | 构造函数RangeError     |
  | Function  | 构造函数Function | ReferenceError | 构造函数ReferenceError |
  |  Boolean  | 构造函数Boolean  |  SyntaxError   | 构造函数SyntaxError    |
  |  String   | 构造函数String   |   TypeError    | 构造函数TypeError      |
  |  Number   | 构造函数Number   |    URIError    | 构造函数URIError       |

  -  window 对象

    - 在全局作用域中声明的所有变量和函数，就都成为了 window对象的属性

      ```js
      var color = "red";
      function sayColor(){
       	alert(window.color);
      }
      window.sayColor(); //"red" 
      ```

    - 另一种取得 Global 对象的方法

      ```js
      var global = function(){
       return this;
      }(); 
      ```

- Math对象

  - Math 对象的属性

    |    属 性     | 说 明                            |
    | :----------: | -------------------------------- |
    |    Math.E    | 自然对数的底数，即常量e的值      |
    |  Math.LN10   | 10的自然对数                     |
    |   Math.LN2   | 2的自然对数                      |
    |  Math.LOG2E  | 以2为底e的对数                   |
    | Math.LOG10E  | 以10为底e的对数                  |
    |   Math.PI    | π的值                            |
    | Math.SQRT1_2 | 1/2的平方根（即2的平方根的倒数） |
    |  Math.SQRT2  | 2的平方根                        |

  - min()和 max()方法

    - 用于确定一组数值中的最小值和最大值

      ```js
      var max = Math.max(3, 54, 32, 16);
      alert(max); //54
      var min = Math.min(3, 54, 32, 16);
      alert(min); //3 
      ```

    - 要找到数组中的最大或最小值，可以像下面这样使用 apply()方法

      ```js
      var values = [1, 2, 3, 4, 5, 6, 7, 8];
      var max = Math.max.apply(Math, values);
      ```

  -  舍入方法

    - Math.ceil()执行向上舍入，即它总是将数值向上舍入为最接近的整数；

      ```js
      alert(Math.ceil(25.9)); //26
      alert(Math.ceil(25.5)); //26
      alert(Math.ceil(25.1)); //26 
      ```

    - Math.floor()执行向下舍入，即它总是将数值向下舍入为最接近的整数;

      ```js
      alert(Math.floor(25.9)); //25
      alert(Math.floor(25.5)); //25
      alert(Math.floor(25.1)); //25 
      ```

    - Math.round()执行标准舍入，即它总是将数值四舍五入为最接近的整数

      ```js
      alert(Math.round(25.9)); //26
      alert(Math.round(25.5)); //26
      alert(Math.round(25.1)); //25 
      ```

  -  random()方法

    - 返回大于等于 0 小于 1 的一个随机数

      ```js
      //值 = Math.floor(Math.random() * 可能值的总数 + 第一个可能的值) 
      var num = Math.floor(Math.random() * 9 + 2); 
      ```

    - 通过一个函数来计算可能值的总数和第一个可能的值

      ```js
      function selectFrom(lowerValue, upperValue) {
       	var choices = upperValue - lowerValue + 1;
       	return Math.floor(Math.random() * choices + lowerValue);
      }
      var num = selectFrom(2, 10);
      alert(num); // 介于 2 和 10 之间（包括 2 和 10）的一个数值
      
      var colors = ["red", "green", "blue", "yellow", "black", "purple", "brown"];
      var color = colors[selectFrom(0, colors.length-1)];
      alert(color); // 可能是数组中包含的任何一个字符串
      ```

  - 其他方法

    |        方 法        | 说 明                 |      方 法      | 说 明              |
    | :-----------------: | --------------------- | :-------------: | ------------------ |
    |    Math.abs(num     | 返回num 的绝对值      |  Math.asin(x)   | 返回x 的反正弦值   |
    |    Math.exp(num)    | 返回Math.E 的num 次幂 |  Math.atan(x)   | 返回x 的反正切值   |
    |    Math.log(num)    | 返回num 的自然对数    | Math.atan2(y,x) | 返回y/x 的反正切值 |
    | Math.pow(num,power) | 返回num 的power 次幂  |   Math.cos(x)   | 返回x 的余弦值     |
    |   Math.sqrt(num)    | 返回num 的平方根      |   Math.sin(x)   | 返回x 的正弦值     |
    |    Math.acos(x)     | 返回x 的反余弦值      |   Math.tan(x)   | 返回x 的正切值     |

#### 8、总结

对象在 JavaScript 中被称为引用类型的值，而且有一些内置的引用类型可以用来创建特定的对象，现简要总结如下：

- 引用类型与传统面向对象程序设计中的类相似，但实现不同；
- Object 是一个基础类型，其他所有类型都从 Object 继承了基本的行为；
- Array 类型是一组值的有序列表，同时还提供了操作和转换这些值的功能；
- Date 类型提供了有关日期和时间的信息，包括当前日期和时间以及相关的计算功能；
- RegExp 类型是 ECMAScript 支持正则表达式的一个接口，提供了最基本的和一些高级的正则表达式功能。

函数实际上是 Function 类型的实例，因此函数也是对象；而这一点正是 JavaScript 最有特色的地方。由于函数是对象，所以函数也拥有方法，可以用来增强其行为。因为有了基本包装类型，所以 JavaScript 中的基本类型值可以被当作对象来访问。三种基本包装类型分别是：Boolean、Number 和 String。以下是它们共同的特征：

- 每个包装类型都映射到同名的基本类型；

- 在读取模式下访问基本类型值时，就会创建对应的基本包装类型的一个对象，从而方便了数据操作；

- 操作基本类型值的语句一经执行完毕，就会立即销毁新创建的包装对象。

在所有代码执行之前，作用域中就已经存在两个内置对象：Global 和 Math。在大多数 ECMAScript实现中都不能直接访问 Global 对象；不过，Web 浏览器实现了承担该角色的 window 对象。全局变量和函数都是 Global 对象的属性。Math 对象提供了很多属性和方法，用于辅助完成复杂的数学计算任务。






​     

