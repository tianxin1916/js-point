### es核心知识梳理
#### 标识符-变量和函数，属性的名字以及函数参数
  - 定义名称注意不要使用关键字以及保留字，比如class,true,Object,undefined等等
  - 标识符必须以字母或下划线（——）或者美元符号（$）开始，后续字符可以是数字，字母，下划线，美元符号（数字是不允许出现在首个字符上），比如：my_ddd,_ll,$fff
  - 建议采用驼峰大小写形式，首字母小写，后面有意义字母大写，比如：doSomething
#### 变量
   - 针对es5 定义变量var 首先要分清楚全局变量以及局部变量
   ***
   ```
    function aa(){
        a=100
    }
    aa()
    console.log(a)//100,指向全局变量
   ```
   ```
   function aa(){
       var a=100
   }
   aa()
   console.log(a)//报错，函数aa中指向全局变量
   ```
   - es6 let和var 区别
   1. 不允许重复声明
      - let 不允许在相同作用域内，重复声明一个变量
      ```
       function (){
           let a=900;
           var a=90;//报错
       }
      ```
      ```
      function(a){
          let a=9000//报错
      }
      ```
   2. 不存在变量提升
      - var 定义变量会发生“变量提升”，即变量可以在声明之前使用，值为undefined,这种用法或多有点奇怪，按照一般逻辑变量应该在声明语句之后可以使用，let命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错
      ```
      //var 的情况
      console.log(foo);
      var foo=2
    
      //let的情况
       console.log(bar);//报错ReferenceError
      let bar =2;
      ```
   3. 暂时性死区
      - 只要块级作用域内存在let命令，它所声明的变量就“绑定”这个区域，不再受外部影响
        ```
        var t=12;
        if(true){
         t=90;//报错ReferenceError 
         let t;
        }
       //分析上面代码中虽然存在全局变量t，但是块级作用域内let又声明了一个局部变量，导致后者绑定这个块级作用域，所以let声明变量前，对t赋值报错

       ```
     
   4. 块级作用域
      - es5只有全局作用域和函数作用域，没有块级作用域
      ```
       var t='2222';
       function f(){
                            // var t;
           console.log(t); //undefined
           if(false){
               var t="hhhhkkkkkk" //t="hhhhkkkkkk"
           }
       }
       f()//undefined
       //分析原因在于变量提升,导致内层t变量覆盖外层t变量
      ```
      ***
      ```
      var s='hhh';
      for(var i=0;i<s.length;i++){
          console.log(s[i])
      }
      console.log(i)//5
      //分析变量i只用来控制循环，但循环结束后，并没有消失，泄露成了全局变量
      ```
      - es6 let为js新增了块级作用域
      ```
        function f1(){
            let n=90;
            if(true){let n=10}
            console.log(n);//5
        }
        //分析，函数有两个代码块，都声明了变量n，这表示外层代码块不受内层代码块的影响，如果使用var，最后输出结果就是10
      ```
   5. es6 常量const
      - const声明一个只读的常量，一旦声明，常量的值就不能改变
      - 声明的变量不得改变值，意味着const一旦声明变量就必须立即初始化，不能留到以后赋值
      - const的作用域与let命令相同，在声明的块级作用域内有效
      - 不可重复声明，也不存在变量提升，存在暂时性死区
#### 数据类型
   -  基本类型（原始数据类型）
      - Number类型
      - String类型
      - Boolean类型
      - Undefined类型
      - Null
   - 三大引用类型，其实本质都是对象
      - Object
      - Array
      - Function
      - 注意引用类型的赋值其实就是对象保存在栈区地址指针的赋值，因此两个变量指向同一个对象，任何操作都会相互影响
    - typeof 运算符
      - typeof(undefined)="undefined"
      - typeof(Boolean)="boolean"
      - typeof(String)="string"
      - typeof(Number)="number"
      - typeof(Object/null)="object"
      - typeof(Function)="function"
#### 运算符
   - 显隐式转换
     ```
     5-true //4
     NaN -2 //NaN
     9-'' //9
     4-null //4
     ```
     ```
     console.log(!NaN) //true
     console.log(!"blue") //false
     console.log(||'')//false
     console.log(||0)//false
     console.log(2,3,8,9,10,222) //222
    ```
   - 或与非（|| && ）运算
     ```
      console.log(1 && 3) //3
      console.log(1 && 'foo' || 0) //'foo'
      console.log(1 || 'foo' && 0) // 1
     ```
#### 语句
   - break 和 continue 语句用于在循环中精确地控制代码的执行。其中，break 语句会立即退出循环，强制继续执行循环后面的语句。而 continue 语句虽然也是立即退出循环，但退出循环后会从循环的顶部继续执行。请看下面的例子：
     ```
      var num = 0;

      for (var i=1; i < 10; i++) {
       if (i % 5 == 0) {
        break;
       }
       num++;
     }
     console.log(num);   // 4
     //退出全部循环
     ```
     ```
        var num = 0;

        for (var i=1; i < 10; i++) {
          if (i % 5 == 0) {
            continue;
          }
           num++;
         }
        console.log(num);   // 8
        //退出当前循环
     ```
  - try语句
    try-catch-finally 语句是js中异常处理机制，语法如下
    ```
     try {
     // 通常来讲，这里的代码会从头执行到尾而不会产生任何问题，
     // 但有时会抛出一个异常，要么是由 throw 语句直接抛出异常，
     // 要么是通过调用一个方法间接抛出异常
     }
     catch(e) {
     // 当且仅当 try 语句块抛出了异常，才会执行这里的代码
     // 这里可以通过局部变量 e 来获得对 Error 对象或者抛出的其他值的引用
     // 这里的代码块可以基于某种原因处理这个异常，也可以忽略这个异常，
     // 还可以通过 throw 语句重新抛出异常
     }
     finally {
     // 不管 try 语句块是否抛出了异常，这里的逻辑总是会执行，终止 try 语句块的方式有：
     // 1）正常终止，执行完语句块的最后一条语句
     // 2）通过 break、continue 或 return 语句终止
     // 3）抛出一个异常，异常被 catch 从句捕获
     // 4）抛出一个异常，异常未被捕获，继续向上传播
     }
    ```
    ```
#### 对象
 - 创建方式
  - 使用对象字面量创建
     ```
     var person={
         name:'zzz',
         age:10
     }
     ```
  - 使用new关键字创建
    ```
     var person=new Object()
     person.name="zzz"
    ```
  - 使用Object.create() 函数创建对象
    ```
     var person=Object.create(object.prototype)
     person.name='zzz'
     ```
  - 属性的访问
    - person.name='zzz' \\ person['name']='zzz'
  - 检测属性
    - console.log('name' in person)
    - person.name!='undefined'
    - person.hasOwnProperty('name')
    - person.propertyIsEnumerable('name')
   - 序列化对象(JSON)
     - s=JSON.stringfy(person) // JSON.parse(s)
#### 数组
 - 创建数组
   - 使用字面量创建
    ```
    var arr=[]
    arr=[2,3,4,6,5,44,55]
    ```
   - 使用new关键字创建
   ```
    var arr=new Array()
    arr=[22,33,444,55]
   ```
  - 数组遍历
    - for循环
     ```
     for(var i=0;i<arr.length;i++){

     }
     ```
   - forEach循环
     ```
      arr.forEach(val=>{

      })
     ```
  - 数组检测
   ```
   Array.isArray([]) //true (ES5)
   [] instanceof Array //true
   ```
  - 插入，栈和队列
    ```
    var arr=[2,5,6,8,7]
    arr.push(11)
    arr.pop() // 11 取最后一项
    arr.shift() // 2 取第一项
    ```
  - 排序方法
    - reverse() --倒序
    - sort(compare())
  - 操作方法
    - concat() 合并
    - slice() 返回从该参数指定位置开始到当前位置数组末尾的所有项,创建新数组,(含头不含尾),若参数中有一个复数，则用数组长度加上该数来确定相应位置
     ```
     var arr=[2,5,6,8,7]
     arr.slice(1)//[5,6,8,7]
     arr.slice(-2,-1) === arr.slice(3,4)

     ``` 
    - splice() 主要用途是向数组的中部插入元素
      - 删除: 可以删除任意数量的项，只需指定2个参数：起始位置和要删除元素的数量，例如，splice(0,2)会删除数组中的前两项
      - 插入: 可以向指定位置插入任意数量的项，只需提供3个参数：起始位置，0（要删除元素的数量）和要插入的元素// splice(2,0,88)
      - 替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定3个参数：起始位置，要删除元素的数量和要插入的元素
        ```
          var colors=['ff','gg','kkk']
          var removed=colors.splice(0,1) // ['ff']
          colors // ['gg','kkk']
          colors.splice(1,0,'ccc','ddd') //[ ]
          colors // ['gg','ccc','ddd','kkk']
          colors.splice(1,1,'www','xxx') // ['ccc']
          colors // ['gg','www','xxx','ddd','kkk']
        ```
  - 位置方法 (以下两个方法都接收两个参数:要查找的项和表示查找起点位置的索引)
   - indexOf():从数组的开头（位置0）开始向后查找
   - lastIndexOf():则从数组末尾开始向前查找
   - 以上两个方法返回要查找的项在数组的位置，或者在没找到情况下返回-1，其中使用全等操作符
   ```
    var numbers=[1,2,3,4,5,3,2,55,44]
    numbers.indexOf(3) // 2
    numbers.lastIndexOf(3) // 5
   ```
 - 迭代方法
   - every() :对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true
   - filter()：对数组中的每一项运行给定的函数，返回该函数会返回true的项组成的数组
   - forEach()：对数组中每一项运行给定函数，这个方法没有返回值
   - map()：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组
   - some()：对数组中的每一项运行给定的函数，如果该函数对任一项返回true,则返回true
   - reduce() / reduceRight():缩小数组的方法,接受的参数（前一个值，当前值，项的索引和数组对象），可用于数组求和方式
   ```
    var numbers=[2,4,5,6,3,7,8,9,11,22]
    var everyRes=numbers.every((item,index,arr)=> item>3) // false

    var someRes=numbers.some((item,index,arr)=> item>3) //true

    var filterRes=numbers.filter((item,index,arr)=>item>3) // [4,5,6,7,8,9,11,22]

    numbers.map((item,index,arr)=>item*2) // [4,8,10,12,6,14,16,18,22,44]

    numbers.reduce((pre,cur,index,arr)=>prev+cur) //77
   ```
#### 函数
 - 函数定义（在js中，函数实际上是对象）
   ```
    function sum(n1,n2){
    }

    var sum=function(n1,n2){

    }
    var sum=new Function('n1','n2','return n1+n2')
   ```
 - 函数的形参和实参
   - 在函数内部，有两个特殊的对象：arguments和this，其中arguments是一个类数组对象，包含着传入函数中的所有参数，这个对象还有一个名叫calle的属性，该属性是一个 指针，指向拥有这个arguments对象的函数
   ```
    function fun(num){
        if(num=1){
            return 1
        }else{
            return num*fun(num-1)
        }
    }
    //////////
    function fun(num){
        if(num=1){
            return 1
        }else{
            return num*fun(num-1)
        }
    }
   ```
   - 函数内部的另一个特殊对象是this,this引用的是函数据以执行的环境对象 （当在网页的全局作用域中调用函数时，this对象引用的是window）
   ```
     window.color = "red";
     var o = { color: "blue" };

     function sayColor(){
     console.log(this.color);
     }
     sayColor();     // "red" ，this指向window

     o.sayColor = sayColor;
     o.sayColor();   // "blue"，this引用的是对象o
    // 函数的名字仅仅是一个包含指针变量而已，因此，即使是在不同的环境中执行，
    全局的sayColor()函数与o.sayColor()指向的仍然是同一个函数，使用call()和
    apply()来扩充作用域的最大好处就是对象不需要与方法有任何耦合关系
   ```