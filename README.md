# 个人学习JavaScript项目
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
