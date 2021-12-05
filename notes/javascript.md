# JS学习
## 数据类型
+ 基本数据类型——值类型：(数字、字符串、布尔值、null、undefined)
+ 复杂数据类型——引用类型：(对象)
    - 数组
    - 函数
    - 正则表达式
    - Date

## 对象的基本使用
### 创建一个对象
```js
    var student={ 
        name:"李白" , //student有一个name属性，值为"李白"
        grade:"初一" ,
        //a、student有一个say属性，值为一个函数
        //b、student有一个say方法
        say:function(){
            console.log("你好");
        },
        run:function(speed){
            console.log("正在以"+speed+"米/秒的速度奔跑");
        }
    }
```
对象是键值对的集合：对象是由属性和方法构成的 (ps：也有说法为：对象里面皆属性，认为方法也是一个属性)
+ name是属性    grade是属性
+ say是方法     run是方法

### 对象属性操作
#### 获取属性
第一种方式：.语法
+ `student.name`      获取到`name`属性的值，为："李白"
+ `student.say`      获取到一个函数

第二种方式：[]语法
+ `student["name"]`   等价于`student.name`
+ `student["say"]`    等价于`student.say`

两种方式的差异：
+ .语法更方便，但是坑比较多(有局限性)
  - .后面不能使用js中的关键字、保留字(class、this、function。。。)
  - .后面不能使用数字
    ```js
        var obj={};
        obj.this=5; //语法错误
        obj.0=10;   //语法错误
    ```
+ []使用更广泛
    - o1[变量name]
    - ["class"]、["this"]都可以随意使用 `obj["this"]=10`
    - [0]、[1]、[2]也可以使用       
        - `obj[3]=50 = obj["3"]=50`     
        - 思考：为什么obj[3]=obj["3"]
    - 甚至还可以这样用：["[object Array]"]
        - jquery里面就有这样的实现
    - 也可以这样用：["{abc}"]
        - 给对象添加了{abc}属性

#### 设置属性
+ `student["gender"]="男"`等价于`student.gender="男"`
    - 含义：如果student对象中没有gender属性，就添加一个gender属性，值为"男"
    - 如果student对象中有gender属性，就修改gender属性的值为"男"
+ 案例1：`student.isFemale=true`
+ 案例2：`student["children"]=[1,2,5]`
+ 案例3：
    ```js
        student.toShanghai=function(){   
            console.log("正在去往上海的路上")   
        }
    ```

#### 删除属性
+ delete student["gender"]      
+ delete student.gender
  
## 通过构造函数创建对象
构造函数创建对象的例子
+ `var xiaoming = new Object()`     -->   `var xiaoming = {}; ` 
+ `var now = new Date()` 
+ `var rooms = new Array(1,3,5)`    -->   `var rooms = [1,3,5]`
+ `var isMale=/123/;`   ==> `var isMale=new RegExp("123")`
  - isMale是通过RegExp构造函数创建出来的对象
  - isMale是RegExp构造函数的实例

+ 以上例子中，Object、Date、Array都是内置的构造函数

## 自定义一个构造函数来创建对象
+ 构造函数
    ```js
        function Person(name,age){
            this.name=name;
            this.age=age;
        }
        var p1=new Person("赵云",18)
    ```
+ 说明：`p1就是根据【Person构造函数】创建出来的对象`

### 构造函数的概念
+ 任何函数都可以当成构造函数
    `function CreateFunc(){ }`
+ 只要把一个函数通过new的方式来进行调用，我们就把这一次函数的调用方式称之为：构造函数的调用
    - new CreateFunc(); 此时CreateFunc就是一个构造函数
    - CreateFunc();     此时的CreateFunc并不是构造函数
+ new Object()等同于对象字面量{}

### 构造函数的执行过程
1. 创建一个对象`var p1=new Person();` (我们把这个对象称之为Person构造函数的实例)- `_p1 `
2. 创建一个内部对象，`this`，将this指向该实例(_p1)
3. 执行函数内部的代码，其中，操作this的部分就是操作了该实例(_p1)
4. 返回值：
   - a、如果函数没有返回值(没有return语句)，那么就会返回构造函数的实例(p1)
   - b、如果函数返回了一个基本数据类型的值，那么本次构造函数的返回值是该实例(_p1)
       ```js
       function fn(){
       }
       var f1=new fn();    //f1就是fn的实例

       function fn2(){
           return "abc";
       }
       var f2=new fn2();   //f2是fn2构造函数的实例
       ```
   - c、如果函数返回了一个复杂数据类型的值，那么本次函数的返回值就是该值
       ```js
       function fn3(){
           return [1,3,5]; 
           //数组是一个对象类型的值，
           //所以数组是一个复杂数据类型的值
           //本次构造函数的真正返回值就是该数组
           //不再是fn3构造函数的实例
       }
       var f3=new fn3();   //f3还是fn3的实例吗？错
       //f3值为[1,3,5]
       ```

1. 如何判断一个数据是否是复杂数据类型？

   使用排除法：
    + 看它的值是不是数字、字符串、布尔值、null、undefined
    + 如果不是以上5种值，那就是复杂数据类型

2. 为什么要理解构造函数的返回值？

   String是一个内置函数：a、String() b、new String()

   一个函数通过new调用，或者不通过new调用，很多时候会有截然不同的返回值

3. 我们如何分辨出一个对象到底是不是某个构造函数的实例？

   var isTrue=xxx instanceof Person

4. 如何识别xxx对象是哪个构造函数的实例？

   xxx.__proto__属性，也是对象，该对象中一般会有一个constructor属性，这个值指向PPP，那么xxx就是PPP的构造函数

5. typeof运算符不能用来判断对象的构造函数

## 继承
通过【某种方式】让一个对象可以访问到另一个对象中的属性和方法，我们把这种方式称之为继承  `并不是所谓的xxx extends yyy`

### 为什么要使用继承？
有些对象会有方法(动作、行为)，而这些方法都是函数，如果把这些方法和函数都放在构造函数中声明就会导致内存的浪费

```js
function Person(){
    this.say=function(){
        console.log("你好")
    }
}
var p1=new Person();
var p2=new Person();
console.log(p1.say === p2.say);   //false
```

### 原型链继承1
```js
Person.prototype.say=function(){
    console.log("你好")
}
```
缺点：添加1、2个方法无所谓，但是如果方法很多会导致过多的代码冗余

### 原型链继承2
```js
Person.prototype={
    constructor:Person,
    say:function(){
        console.log("你好");
    },
    run:function(){
        console.log("正在进行百米冲刺");
    }
}
```
注意点：
+ a、一般情况下，应该先改变原型对象，再创建对象
+ b、一般情况下，对于新原型，会添加一个constructor属性，从而不破坏原有的原型对象的结构

### 拷贝继承(混入继承)
场景：有时候想使用某个对象中的属性，但是又不能直接修改它，于是就可以创建一个该对象的拷贝

```javascript
var o1 = {age:2};
var o2 = o1;
o2.age = 18;
```

1. 修改了o2对象的age
2. o2对象跟o1对象是同一个对象
3. o1对象的age属性也被修改了

如下代码中，如果使用拷贝继承对代码进行优化会非常和谐

```
var o3 = {gender:"男",grade="初三",group:"第五组",name:"张三"};
var o4 = {gender:"男",grade="初三",group:"第五组",name:"李四"};
```

1. 已经有了o3对象

2. 创建o3对象的拷贝：for...in循环

   ```javascript
    //a. 取出o3对象中每个属性
   for(var key in o3){
       //key是o3中每个属性
       //b. 获取到对应属性值
       var value = o3[key];
       //c. 把属性值放入o4
   	o4[key] = value;
   }
   
   ```

3. 修改克隆对象，把该对象的name改为“李四”

   ```javascript
   o4.name="李四"；
   ```

浅拷贝和深拷贝
 + 浅拷贝只是拷贝一层属性，没有内部对象
 + 深拷贝其实是利用了递归的原理，将对象的若干层属性拷贝出来
    
实现：
```javascript
var source={name:"李白",age:15}
var target={};
target.name=source.name
target.age=source.age;
```

上面的方式很明显无法重用，实际代码编写过程中，很多时候都会使用拷贝继承的方式，所以为了重用，可以编写一个函数把他们封装起来：

```
function extend(target,source){
        for(key in source){
            target[key]=source[key];
        }
        return target;
}
extend(target,source)
```

由于拷贝继承在实际开发中使用场景非常多，所以很多库都对此有了实现
- jquery：`$.extend`
- es6中有了对象扩展运算符仿佛就是专门为了拷贝继承而生：
    ```javascript
        var source={name:"李白",age:15}
        var target={ ...source }
        var target2={...source,age:18}
    ```

### 原型式继承
+ 场景：
  - 创建一个纯洁的对象
  - 创建一个继承自某个父对象的子对象
      ```javascript
      var parent = {age:18,gender:"男"};
      var student = Object.create(parent);
      console.log(student)
      ```
 
+ 使用方式：
  - 空对象：Object.create(null)
    ```javascript
    var o1={ say:function(){} }
    var o2=Object.create(o1);
    ```

`var o3={}` o3并不纯洁

### 借用构造函数实现继承

+ 场景：适用于2种构造函数之间逻辑有相似的情况
+ 原理：函数的call、apply调用方式
+ 局限性：Animal（父类构造函数）的代码必须完全适用于Person（子类构造函数）

```javascript
function Animal(name,age,gender){
    this.name=name;
    this.age=age;
    this.gender=gener;
}
function Person(name,age,gender,address){
    // Animal.call(this,name,age,gender);
    //等价于
    Animal.apply(this,[name,age,gender]);
    //this.name=name;
    //this.age=age;
    // this.gender=gender;
    this.address=address;
}
```
+ 寄生继承、寄生组合继承
  
## 原型链（家族族谱）
+ 概念：JS里面的对象可能会有父对象，父对象还会有父对象，。。。。。祖先
+ 根本：继承
    - 属性：对象中几乎都会有一个__proto__属性，指向他的父对象
        -意义：可以实现让该对象访问到父对象中相关属性
+ 根对象：`Object.prototype`
    - var arr=[1,3,5]
    - arr.__proto__：Array.prototype
    - arr.__proto__.__proto__找到了根对象

    ```javascript
        function Animal(){}
        var cat=new Animal();
        //cat.__proto__：Animal.prototype
        //cat.__proto__.__proto__:根对象
    ```
+ 错误的理解：万物继承自Object？

## 闭包
### 变量作用域
+ 变量作用域的概念：就是一个变量可以使用的范围
+ JS中首先有一个最外层的作用域：称之为全局作用域
+ JS中还可以通过函数创建出一个独立的作用域，其中函数可以嵌套，所以作用域也可以嵌套
```js
var age=18;//age是全局作用域中
function f1(){
    var name="张三";//name是f1函数内部声明的变量，所以作用域在f1内部
    console.log(name);//可以访问到name变量
    console.log(age);//age是全局作用域，所以也可以访问
}
console.log(age);//也可以访问
```
```js
    //多级作用域
    //1级作用域
    var gender="男";
    function fn(){
        console.log(age);//age:undefined undefined为初始值，既然有值，说明可以访问
        console.log(height);//height不是在该作用域内部声明，故不能访问
        //2级作用域
        return function(){
           //3级作用域
            var height=180;
        }
        var age=5;
    }
```
注意：
- 变量的声明和赋值是在两个不同的时期的。
- fn函数在执行的时候，首先找到函数内部所有变量、函数声明，把他们放在作用域中，给变量一个初始值undefined --变量可以访问
- 逐行执行代码过程中，如果有赋值语句，对变量进行赋值

### 作用域链
+ 由于作用域是相对于变量而言的，而如果存在多级作用域，这个变量又来自于哪里？这个问题就需要好好地探究一下了，我们把这个变量的查找过程称之为变量的作用域链
+ 意义：查找变量（确定变量来自于哪里，变量是否可以访问）
+ 简单来说，作用域链可以用以下几句话来概括：(或者说：确定一个变量来自于哪个作用域)
    1. 查看当前作用域，如果当前作用域声明了这个变量，就确定结果
    2. 查找当前作用域的上级作用域，也就是当前函数的上级函数，看看上级函数中有没有声明
    3. 再查找上级函数的上级函数，直到全局作用域为止
    4. 如果全局作用域中也没有，我们就认为这个变量未声明(xxx is not defined)
```js
function fn(callback){
    var age=18;
    callback();
}
fn(function(){
    console.log(age);
    //分析：age变量
    //1.查找当前作用域并没有
    // 2.查找上一级作用域，全局作用域
})
```
注意：看上一级作用域，不是看函数在哪里调用，而是看函数在哪里编写，因为这种特别，我们通常会把作用域说成是**词法作用域**
+ 举例1：
    ```javascript
        var name="张三";
        function f1(){
            var name="abc";
            console.log(name);
        }
        f1();
    ```

+ 举例2：
    ```javascript
        var name="张三";
        function f1(){
            console.log(name);
            var name="abc";
        }
        f1();
    ```

+ 举例3：
    ```javascript
        var name="张三";
        function f1(){
            return function(){
                console.log(name);
            }
            var name="abc";
        }
        var fn=f1();
        fn();
    ```

+ 举例4：
    ```javascript
        var name="张三";
        function f1(){
            return {
                say:function(){
                    console.log(name);
                    var name="abc";
                }
            }
        }
        var fn=f1();
    ```

### 闭包的问题
```javascript
    function fn(){
        var a=5;
        return function(){
            a++;
            console.log(a);
        }
    }
    var f1=fn();
    //执行到上一行，fn函数完毕，返回匿名函数
    // 一般认为函数执行完毕，变量就会释放
    // 但是由于此时由于js引擎发现匿名函数要使用a变量
    //所以a变量并不能得到释放
    // 而是把a变量放在匿名函数可以访问到的地方去了
    f1();//6
    f1();//7
    f1();//8
```
```js
function q2(){
    var a={};
    return function(){
        return a;
    }
}
var t3=q2();
var o5=t3();
var o6=t3();
console.log(o5==o6);//true
var w3=q2();
var o8=w3();
console.log(o5==o8);//false

```
### 闭包问题的产生原因
+ 函数执行完毕后，作用域中保留了最新的a变量的值
+ 闭包内存释放
  ```js
  function f1(){
      var a=5;
      return function(){
          a++;
          console.log(a);
      }
  }
  var q1=f1();
  //要想释放q1里边保存的a,只能通过释放q1
  q1=null;//q1=undefined;
  ``` 
闭包的应用场景
+ 模块化
+ 防止变量被破坏

### 函数的4种调用方式
1. 函数调用
   ```js
   var age=18;
   var p={
       age:15,
       say:function(){
           console.log(this.age);
       }
   }
   var s1=p.say;
   s1();//函数调用--> this: window 输出18
   ```
   结论：函数内部的this指向window
2. 方法调用
   ```js
   var age=18;
   var p={
       age:15,
       say:function(){
           console.log(this.age);
       }
   }
   p.say(); //打印结果15
    //
   function Person(){
       this.age = 20;
   }
   Person.prototype.run=function(){
       console.log(this.age);
   }
   var p1=new Person();
   p1.run(); //打印结果20
    //
   var clear=function(){
       console.log(this.length);
   } 
   var length=50;
   var tom={c:clear,length:100};
   tom.c();//打印100 this指向tom
   ``` 
   结论：由于clear函数被当成tom.c()这种方法的形式调用，所以函数内部的this指向调用该方法的对象：tom
3. new调用（构造函数）
   ```js
   //1
   function fn(name){
       this.name=name;
   }
   //通过new关键字来调用的，这种方式就是构造函数调用方式
   var _n=new fn("小明");
   //2
   function jQuery(){
       var _init=jQuery.prototype.init;
       return new _init();
   }
   jQuery.prototype={
       constructor:jQuery,
       length:100,
       init:function(){
            //1.this指向init构造函数的实例
            //2.如果本身没有该属性，那么去他的原型对象中去找
            //3.如果原型对象中没有，那么就去原型对象的原型对象中查找，最终一直找到根对象(Object.prototype)
            //4.最终没有找到 ，则属性值为：undefined
           console.log(this.length);
       }
   }
   jQuery.prototype.init.prototype=jQuery.prototype;
   jQuery();//结果为100
   ``` 
4. 上下文方式（call,apply,bind）
   ```js
   var name=21;
   function f1(){
       console.log(this.name);
   }
   f1.call([1,3,5]);
   f1.apply(this);
   f1.call(5);
   //总结
   //call函数的第一个参数：
   //1.如果是一个对象类型，那么函数内部this指向对象
   //2.如果是undefined、null,那么函数内部this指向window
   //3.如果是数字-->this指向new Number(数字)，字符串-->this指向new String(字符串)，布尔，this指向new Boolean(布尔)

   //bind是ES5(ie9+)
   var obj={
       age:18,
       run:function(){
           setTimeout((function(){
               console.log(this.age)
           }).bind(this),500)
           //通过执行了bind方法，匿名函数本身并没有执行，只是改变了该函数内部的this的值，指向obj
       }
   }
   obj.run();
   //bind基本用法
   function speed(){
       console.log(this.seconds);
   }
   //执行bind方法之后产生了一个新函数，这个新函数里边的逻辑和原来还是一样的，唯一的不同是this指向{seconds:100}
   var speedBind=speed.bind({seconds:100});
   speedBind();//打印100
   ```
   + call和apply都可以改变函数内部this的值
   + 传参的形式不同
        ```js
        function toString(a,b,c){
            console.log(a+" "+b+" "+c);
        }
        toString.call(null,1,3,5);
        toString.apply(null,[1,3,5])
        ```
在ES6的箭头函数之前的时代，想要判断一个函数内部的this指向谁，就是根据上边的四种方式来决定的
+ bind方法实现
    ```javascript
        //target表示新函数的内部的this值
        Function.prototype._bind=function(target){
            
            //利用闭包创建一个内部函数，返回那个所谓的新函数
            return ()=>{
                //执行fn里边的逻辑
                this.call(target)
            }
            //等价于
            // var _that=this;
            // return function(){
            //     _that.call(target);
            // }
        }
        function fn(){
            console.log(this);
        }
        var _fn=fn.bind({age:18});
        
    ```
1. bind方法放在函数的原型中 `fn.__proto__ === fn的构造函数.prototype`
2. 所有的函数对象的构造函数都是Function