#个人学习JavaScript项目
## 1.JavaScript的编译与执行
一，编译：

以var a = 2;为例，说明javascript的内部编译过程，主要包括以下三步

1） 分词：

　　1.1  词法单元(token)：把由字符组成的字符串分解成有意义的代码块，这些代码块称为词法单元。

　　1.2  下面是一个栗子：

　　　　var a = 2;被分解成为下面这些词法单元：var、a、=、2、;。这些词法单元组成了一个词法单元流数组

　　　　// 词法分析后的结果
       [  
　　　　　　"var" : "keyword",  
　　　　　　"a" : "identifier",  
　　　　　　"=" : "assignment",  
　　　　　　"2" : "integer",  
　　　　　　";" : "eos" (end of statement)   
　　　　]

2)  解析

　　2.1 抽象语法树” (Abstract Syntax Tree, AST)把词法单元流数组转换成一个由元素逐级嵌套所组成的代表程序语法结构的树

　　2.2 下面是一个栗子：

　　var a = 2;的抽象语法树中有一个叫VariableDeclaration的顶级节点，接下来是一个叫Identifier(它的值是a)的子节点，以及一个叫AssignmentExpression的子节点，且该节点有一个叫Numericliteral(它的值是2)的子节点

　　{  
　　　　operation: "=",  
　　　　left: {  
　　　　　　keyword: "var",  
　　　　　　right: "a"  
　　　　}  
　　　　right: "2"  
　　}

3) 代码生成

　　将AST转换为可执行代码的过程被称为代码生成

　　var a=2;的抽象语法树转为一组机器指令，用来创建一个叫作a的变量(包括分配内存等)，并将值2储存在a中

　　实际上，javascript引擎的编译过程要复杂得多，包括大量优化操作，上面的三个步骤是编译过程的基本概述

　　编译总结：任何代码片段在执行前都要进行编译，大部分情况下编译发生在代码执行前的几微秒。javascript编译器首先会对var a=2;这段程序进行编译，然后做好执行它的准备，并且通常马上就会执行它

编译原理：编译器把程序分解成词法单元(token)，然后把词法单元解析成语法树(AST)，再把语法树变成机器指令等待执行的过程

图解如下：  
![alt 图解](https://github.com/Voryla/LearnJavaScript/blob/master/images/JavaScript%E7%BC%96%E8%AF%91%E5%9B%BE.png)
 

二，执行

　　继续var a = 2;这个栗子

　　1. 执行原理：

　　　　1.1、引擎运行时会首先查询作用域，在当前的作用域集合中是否存在一个叫作a的变量。如果是，引擎就会使用这个变量；如果否，引擎会继续查找该变量

　　　　1.2、如果引擎最终找到了变量a，就会将2赋值给它。否则引擎会抛出一个异常。

　　2. 名词释义：

　　查询：会用到LHS和RHS两种查询方式。

　　LHS是赋值操作时查询；

　　RHS是取值操作时查询。

　　为什么区分LHS和RHS是一件重要的事情？因为在变量还没有声明（在任何作用域中都无法找到变量）的情况下，这两种查询的行为不一样

　　RHS查询失败，引擎会抛出ReferenceError(引用错误)异常

　　function foo(){  
   　　 var b = 0;  
    　　b();  
　　}  
　　foo();//TypeError: b is not a function  
　　LHS  

　　当引擎执行LHS查询时，如果无法找到变量，全局作用域会创建一个具有该名称的变量，并将其返还给引擎

　　function foo(){   
    　　a = 1;      
　　}  
　　foo();  
　　console.log(a);//1  

[原文地址](https://www.cnblogs.com/hyns/p/12383601.html)
****
## 1.JavaScript的小技巧
* 把脚本置于`<body>`元素的底部，可改善显示速度，因为脚本编译会拖慢显示。
* JavaScript 对大小写敏感  
* 如果把要给数值放入引号中，其余数值会被视作字符串并被级联。
* JavaScript 六种数据类型
> 1.object
> 2.number
> 3.string
> 4.boolean
> 5.null
> 6.undefined
* 您可以在字符串内使用引号，只要这些引号与包围字符串的引号不匹配：  
    > var answer = "It's alright";             // 双引号内的单引号  
    > var answer = "He is called 'Bill'";    // 双引号内的单引号  
    > var answer = 'He is called "Bill"';    // 单引号内的双引号     
* 字符串与数值相加时，不同的顺序会产生不同的结果
    > var x = 911 + "Porsche";      // 911Porsche  
    > var x = "911" + "Porsche";    // 911Porsche  
    > var x = 911 + 7 + "Porsche";  // 918Porsche  数值在前，先进行数值运算  
    > var x = "Porsche"+911 + 7 ;   // Porsche9117 数值在后 进行字符串运算
    > var x = 911  + "Porsche + 7"; // 911Porsche7      
* 请注意 (x==y) 与 (x===y) 的区别。
   > ==意为值相等  
   > === 意为值和对象都相等   
   > JavaScript 对象无法进行对比，比较两个 JavaScript 将始终返回 false。   
   > == 比较运算符总是在比较之前进行类型转换（以匹配类型）。  
   >=== 运算符会强制对值和类型进行比较：  
   实例  
   0 == "";        // true  
   1 == "1";       // true  
   1 == true;      // true  
   0 === "";       // false  
   1 === "1";      // false  
   1 === true;     // false  
   
* 关键字const有一定的误导性
    > 实质上，它并没有定义常量"值",而是定义了对值的常量”引用“  
    > 因此，虽然不能更改常量引用的原始值，但是可以更改常量对象中的属性值

    ````javascript
      // 您可以创建 const 对象：
      const car = {type:"porsche", model:"911", color:"Black"};
      
      // 您可以更改属性：
      car.color = "White";
      
      // 您可以添加属性：
      car.owner = "Bill";
      // 但是无法重新给const对象赋值其他的对象
  
*  请勿使用 new Object()
     > 请使用 {} 来代替 new Object()  
      请使用 "" 来代替 new String()  
      请使用 0 来代替 new Number()  
      请使用 false 来代替 new Boolean()  
      请使用 [] 来代替 new Array()  
      请使用 /()/ 来代替 new RegExp()  
      请使用 function (){}来代替 new Function()   
      实例  
      var x1 = {};           // 新对象   
      var x2 = "";           // 新的原始字符串值  
      var x3 = 0;            // 新的原始数值  
      var x4 = false;        // 新的原始布尔值  
      var x5 = [];           // 新的数组对象  
      var x6 = /()/;         // 新的正则表达式  
      var x7 = function(){}; // 新的函数对象    
* 在测试对象非 null 之前，必须先测试未定义：
  >if (typeof myObj !== "undefined" && myObj !== null)  
* 延迟 JavaScript 加载
    请把脚本放在页面底部，使浏览器首先加载页面。
    
    脚本在下载时，浏览器不会启动任何其他的下载。此外所有解析和渲染活动都可能会被阻塞。
    
    HTTP 规范定义浏览器不应该并行下载超过两种要素。
    
    一个选项是在 script 标签中使用 defer="true"。defer 属性规定了脚本应该在页面完成解析后执行，但它只适用于外部脚本。
    
    如果可能，您可以在页面完成加载后，通过代码向页面添加脚本：
    
    实例
    ```javascript 
  <script>
        window.onload = downScripts;
        
        function downScripts() {
            var element = document.createElement("script");
            element.src = "myScript.js";
            document.body.appendChild(element);
        }
  </script>