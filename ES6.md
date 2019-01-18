## ES6新特性
#### const与let变量
 ```
  function getClothing(isCold) {
  if (isCold) {
    var freezing = 'Grab a jacket!';
  } else {
    var hot = 'It's a shorts kind of day.';
    console.log(freezing);
  }
  }
 // 运行getClothing(false)后输出的是undefinded,这是因为执行function函数之前，所有的变量都会被提升，提升到函数作用域顶部
 ```
 ----
 - let 与const声明的变量解决了这种问题，因为他们是块级作用域，在代码块（用{}表示）中使用let或const声明变量，该变量会陷入暂时性死区直到该变量声明被处理
 ```
    function getClothing(isCold) {
     if (isCold) {
      const freezing = 'Grab a jacket!';
     } else {
      const hot = 'It's a shorts kind of day.';
      console.log(freezing);
    }
  }
  // 运行getClothing(false)后输出ReferenceError: freezing is not defined，因为freezing没有在else语句，函数作用域或全局作用域内声明，所以抛出ReferenceError
 ```
  ##### 关于使用let与const规则
   - 使用let声明变量可以重新赋值，但不能在同一作用域内重新声明
   - 使用const声明的变量必须赋值初始化，但是不能在同一作用域类重新声明也无法重新赋值
#### 模板字面量
   - 在ES6之前，将字符串连接到一起的方法是+或者concat()方法，如
   ```
    var a='11jjjjj',b='33jjjjooo'
    var c= a+b
   ```
  - 模板字面量本质上是包含嵌入式表达式的字符串字面量
  - 模板字面量用倒引号（``）表示，可以包含用${experssion}表示的占位符
  ```
    let msg=`${a} and ${b}`
  ```

#### 解构
  - 在ES6中，可以使用解构从数组和对象提取值并赋值给独特的变量
    - 数组的解构赋值
        ```
         let a=1;
         let b=2;
         let c=3;
        ```
        Es6允许以下这种做法
        ```
         let [a,b,c]=[1,2,3];
        ```
        ---
        ```
         上面代码表示可以从数组中抽取值赋给对应位置的变量
         当然如果解构不成功，变量的值就为undefined
         let [foo]=[];
         let [bar, line]=[1];
         //foo是undefined bar是1 line是undefined
        ```
        ---
        ```
        如果等号右边的不是数组（或者说不可遍历的解构），就会报错
         let [foo]=null;
         let [foo]=undefined;
         let [foo]=1;
         let [foo]=NaN
         let [foo]=false
         let [foo]={}
        ```
        ---
        ```
         默认值
         数组解构可以赋默认值
         当时数组解构进行赋值的时候会先判断对应位置上是否有新值(判断规则是严格等于===),如果不存在的话默认值才会生效
         let [foo]=[true]; //foo=true
         let [x,y='b']=['a']; // x='a'; y='b'
         let [x,y='b']=['a',undefined];// x='a'; y='b'
         let [x,y='b']=['a',null]; //x='a'; y=null
        ```
        ---
        ```
         如果默认值是一个表达式，那么这个表达式是惰性的，只有在用到的时候才会求值
         function getFoo(){
           return 'foo'
         }
         let [x=getFoo()] = [1];// getFoo并未执行，因为解构的时候发现有对应的值 x=1
        ```
        ---
        ```
         当然默认值可以取已经存在的变量，记住是否已经存在的
         let [x=0,y=x] = [];//x=0;y=0
         let [x=y,y=0] = [];//报错因为x需要默认值的时候y还不存在
         let [x=y,y=0] = [1];//x=1;y=0;因为x能取到值 所以默认值的赋值操作压根不会执行
        ```
    - 对象的解构赋值
      - 第一部分我们知道数组解构赋值是按照顺序来的，对象就不一样了，对象的属性并没有顺序，对象的解构赋值是按照属性名来的
      ```
      变量名与属性名一样的情况
      let {foo,bar ,line}={foo:'Hello',bar:'es6'};
      foo // Hello
      bar // es6
      line //undefined
      ```
      ---
      ```
      变量名与属性名不一样的情况
      let {foo:fooTest}={foo:'Jason'}
      fooTest // 'Jason'
      ```
      ---
      ```
      当然同名的完全可以理解成
      let {foo:foo ,bar:bar,line:line} = {foo:'Hello',bar:'ES6'}
      ```
      ---
      ```
       对象与数组结合后可以组成嵌套模式
       let {test:[name],mod}={test:['jason'],mod:'hello'}
       mod //'hello'
       name //'jason'
       test//报错 test is not defined
      ```
      ---
      ```
       同样的对象的解构也可以设置默认值，默认值生效的条件是对象的属性值严格等于undefined
       let {x=0}={};
       let {obj={}}={obj:null};
       x //0
       obj //null

       解构失败的话变量被赋予undefined
       let {x}={}
       x //undefined
      ```
      ---
      ```
       如果解构模式是嵌套对象，子对象所在的父属性不存在就会报错
       let {user:{name}}={foo:'test'} // 报错 因为name的父属性不存在
       如果对一个已经声明的变量进行解构赋值一定要注意
       let x;
       {x} = {x:0} //报错 语法错误 javascript会把{x}当作代码块
       
       为了避免以上的错误应该避免大括号位于行首，我们应该将解构赋值放在圆括号内
       let x;
       ({x}={x:0})
       x//0
      ```
      ---
      ```
       对象的解构赋值可以很方便的将已有的方法赋值到变量中方便使用
       let {sin,cos,log,PI}=Math
       sin(PI/6)
       因为数组是一种特殊的对象，所以可以对数组使用对象进行解构
       let arr=[1,2,3]
       let {0:test,[arr.length-1]:name} = arr;
       test//1
       name //3
      ```
    - 字符串的解构赋值
      ```
       字符串也可以解构赋值，因为此时字符串会被当做一个类似数组的对象
       let [first, , , ,last]='12345'
       first // '1'
       last //5

       类似数组的对象都有长度length属性
       let { length } = '12345'
       length//5
      ```
    - 数值与布尔值的解构赋值
      ```
      如果是对数字或者布尔值进行解构，会先将数字或者布尔值转换成对象
       let {toString: s}=123
       s===Number.propertype.toString //true
       ---
       let {toString: s}=true
       s === Boolean.propertype.toString //true

       特别注意undefined 与null无法转换为对象，对他们进行解构都会报错
       let {prop:x}=undefined //报错
       let {prop:y}=null;//报错
      ```
    - 函数参数的解构赋值
      ```
       function test([x,y]){
       return x+y;
       }
       test([1,2])
      ```
 - 圆括号问题
   - 不放圆括号的情况
    - 变量声明语句
       ```
        let [{a}]=[1];
        let {x:(c)}={};
        let ({x:c})={};
        let {{x}:c}={}
 
        let {o:({p:p})}={o:{p:2}}
       ```
    - 函数参数
      ```
       function f([(z)]){return z;}
       function f([(z,(x))]){return x;}
      ```
    - 赋值语句的模式
       ```
        ({p:a})={p:42}
        ({a})=[5]
        [({p:a}),{x:c}]=[{},{}]
       ```
   - 可以使用圆括号的情况
     ```
      可以使用圆括号的情况只有一种：赋值语句的非模式部分，可以使用圆括号
      [(b)]=[3]
      ({p:d}={})
      [(parseInt.prop)]=[3]
       以上三行语句都可以正确执行，因为首先它们是赋值语句而不是声明语句
       其次他们的圆括号都不属于模式的一部分
       第一行语句中，模式是取数组的第一个成员，跟圆括号无关
       第二行语句中，模式是p而不是d
       第三行语句与第一行语句的性质一致
     ```
  -  用途
     - 交换变量的值
      ```
       [x,y]=[y,x]
       交换变量x和y的值
      ```
     - 从函数返回多个值
      ```
       function example(){
       return [1,2,3]
       }
        var [a,b,c]=example()

       //返回一个对象
       function example(){
         return{
           foo:1,
           bar:2
         }
       }
       var {foo,bar}=example();
     ```
     - 函数参数的定义
        ```
        function f([x,y,x]){...}
        f([1,2,30])

        function f({x,y,z}){...}
        f({z:3,y:2,x:90})
        ```
     - 提取json数据
       ```
        var jsonData={
         id:42,status:"OK",
         data:[9888,8999]
        }
        let {id,status,data:number}=jsonData
       ```
     - 数参数的默认值
         ```
          function(url,{
             a=90,
            b=80,
            c=false
          }){

          }
          //指定参数的默认值，就避免了在函数体内部再写var foo=config.foo || 'defau;t.foo'这样  的语句
        ```
      - 遍历Map结构
       任何部署了Iterator接口的对象，都可以用for...of循环遍历。Map结构原生支持Iterator接口，配合变量的解构赋值，获取键名和键值就非常方便
       ```
        var map=new Map()
        map.set('a','aaa')
        map.set('b','bbb')
        for(let[key,value] of map){
          console.log(key+'is'+value)
        }
        //获取键名
        for(let [key] of map){

        }
        //获取键值
        for(let [,value] of map){

         }
      ```
       - 输入模块的指定方法
          - const {SourceMapConsumer,SourceNode}=require("source-map)
#### 扩展运算符
 - es6中引入扩展运算符（...）,它用于把一个数组转化为   用逗号分隔的参数序列，它常用在不定参数个数时的函数   调用，数组合并等情形，因为typeScript是es6的超集，   所以typeScript 也支持扩展运算符
 - 数组扩展用途
   - 可变参数个数的函数调用
     ```
       function push(array,...item){

       }
       function add(...vals){
        let sum=0;
        for(let i=0;i<vals.length;i++>){
          sum+=vals[i]
        }
        return sum
       }
       let arr=[2,3,42,3,4,55,33]
       let sum=add(...arr);
     ```
   - 更便捷的数组合并
      ```
       let arr1=[1,2,3]
       let arr2=[4,6]
       let newArr=[20]
       //es5语法
       newArr=newArr.concat(arr1).concat(arr2)
       //es6语法
       newArr=[20,...arr1,...arr2]
      ```
   - 替代es5的apply方法
     ```
      function f(x,y,x){

      }
      var args=[9,5,6,4]
      //es5的写法
       f.apply(null,args)
      //es6的写法
      f(...args)
      ```
   - 求最大值Math.max()
     ```
      //es5的写法
       Math.max.apply(null,[12,33,99])
      //es6的写法
      Math.max(...[12,33,99]) // Math.max(12,33,99)
     ```
   - 通过push函数，将一个数组添加到另一个数组的尾部
     ```
      //es5的写法
       var arr1=[0,1,2]
       var arr2=[3,4,5]
       Array.prototype.push.apply(arr1,arr2)
       //Es6的写法
       var arr1=[0,2,3]
       var arr2=[3,4,5]
       arr1.push(...arr2)
     ```
   - 新建Date类型
     ```
      //Es5
      new(Date.bind.apply(Date,[null,2015,1,1]))
      //ES6
      new Date(...[2015,1,1])
     ```
   - 与解构赋值结合，生成新数组
     ```
      //es5
      a=list[0] ,rest=list.slice(1)
      //es6
      [a,...rest]=list
      //下面是另一个例子
      const [first,...rest]=[1,2,3,4,5]
      first//1
      rest//[2,3,4,5]
      const [first,...rest]=[]
      first // undefined
      rest // []
      const [first,...rest]=["foo"]
      first // "foo"
      rest //[]
     ```
   - 将字符串转为真正的数组
     ```
      [...'hello'] // ['h','e','l','l','o']
     ```
   - 将实现了Iterator 接口的对象转为数组
     ```
      var nodeList=document.querySelectorAll('div')
      var array=[...nodeList]
     ```
   - Map和Set结构，Generator函数
     ```
      let map=new Map([[1,'one'],[2,'two'],[3,'three']])
      let arr=[...map.keys()] //[1,2,3]
     ```
   - Generator 函数运行后，返回一个遍历对象，因此也可以使用扩展运算   符
     ```
      const go=function *(){
        yield 1;
        yield 2;
        yield 3
      }
      [...go()]//[1,2,3]
      //如果没有Iterator接口的对象，使用扩展运算符，将会报错
     ```
 - 对象扩展用途
   - 扩展运算符--解构赋值
     ```
       对象的解构赋值用于从一个对象取值，相当于将所有可遍历的，但尚未被读取的属性，分配到制定的对象上面，所有的键和它们的值，都会拷贝到新对象上面
       let {x,y,...z}={x:1,y:45,a:90,b:80}
       x //1
       y //45
       z // {a:90,b:80}
       如果等号右边是undefined或null,就会报错,因为它们无法转为对象
       let {x,y,...z}=null //运行报错
       let {x,y,...z}=undefined //运行报错
       解构赋值必须是最后一个参数，否则报错
       let {...x,y,z}=obj //报错
       let {x,...y,...z}=obj //报错

       解构赋值的拷贝是浅拷贝
     ```
   - 扩展运算符--拷贝
     ```
      let z={a:3,b:4}
      let n={...z}
      n//{a:3,b:4}
      //等同于使用Object.assign方法
      let aClone = {...a}
      //等同于
      let aClone=Object.assign({},a)

      上面的例子只是拷贝了对象实例的属性，如果想完整克隆一个对象，还要拷贝对象原型的属性，可以采用下面的方法
      //写法一
      const clone1={
        _proto_:Object.getPrototype(obj),
        ...obj
      }
      //写法二
      const clone2-{
       object.create(Object.getPrototypeOf(obj)),
       obj
      }
     ```
   - 扩展运算符--合并对象
     ```
      let ab={...a,...b}
      // let ab=Object.assign({},a,b)
      如果用户自定义的属性，放在扩展运算符后面，则扩展运算符内部的同名属性会被覆盖掉
      let aWithOverrides={...a,x:1,y:2}
      let aWithOverrides={...a,...{x:1,y:2}}
      let x=1,y=2,aWithOverrides={...a,x,y}
      let aWithOverrides= Object.assign({},a,{x:1,y:2})
      //上面代码中，a对象的x属性和y属性，拷贝到新对象后被覆盖掉
      这用来修改现有对象部分的属性就很方便
      let newVersion={
        ...previousVersion,
        name:'1111'
      }
      如果把自定义属性放在扩展运算符前面，就变成了设置新对象的默认属性
      let aWithDefults={x:1,y:2,...a}
      //对象的扩展运算符后面可以跟表达式
      const obj={
        ...{x>1?{a:1}:{}},
        b:2
      }
      {...{},a:1} //{a:1}

      //如果扩展运算符的参数是null或者undefined，这两个值会忽略，不会报错
      let emptyObj={...null,...undefined}//不报错
      //扩展运算符的参数对象之中，如果有取值函数get，这个函数是会执行的
      let aWithXGetter ={
        ...a,
        get x(){
          throw new Error('not throw yet')
        }
      }
      //会抛出错误，因为x属性被执行了
      let runtimeError={
        ...a,
        ...{
          get x(){
            throw new Error('throw now')
          }
        }
      }
     ```