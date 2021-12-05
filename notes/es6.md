# ES6(常用的、重点的)
## 模板字符串
模板字符串的基本用法
```js
    //基本用法
    var s1 = ` abc `
    var s2 = " abc "
    //模板字符串解决了一些痛点：
    //字符串拼接
    var s3=" a "+s1+" b "+s2;
    var s4=` a ${s1} b ${s2}`
    var s5=`<div>
                <p>
                    <span>222</span>
                    <span>${s1}</span>
                    <span>${s2}</span>
                </p
            </div>
            `
```

## 解构赋值
对象的解构赋值
```js
    var obj={name:"张三",age:18}

    var {name,age}=obj; 
    //生成2个变量，
    //  name值来自于obj.name、
    //  age值来自于obj.age

    var {name:title}=obj;
    //生成一个变量：title，值来自于obj.name
```

函数参数的解构赋值
```js
    function f1(obj){
        console.log(obj.age);
        console.log(obj.height)
    }
    //等价于
    function f1({ age,height }){
        console.log(age);
        console.log(height)
    }

    f1({age:5,height:180})
```

补充：属性的简写
```js
    var a = 3 ; 
    var c = 10;
    var b = { a,c } ;   
    //b对象有一个a属性，a属性的值，来自于a变量  ，
    //还有一个c属性，c属性的值来自于c变量
    console.log(b);//{a: 3, c: 10}
```

## 函数的扩展
### rest参数
+ 使用背景：es6的
+ 优点：arguments是伪数组，而rest参数是真数组
    ```js
        function fn(...args){
            //验证args是不是数组？
            console.log(args instanceof Array);//true
            console.log(Object.prototype.toString.call(args));//[object Array]
            console.log(Array.isArray(args));
            console.log(args);  //数组：[1,2,3,4,5]
        }
        fn(1,2,3,4,5)
    ```
 + 判断数据类型
   + typeof只能判断：数字、字符串、布尔、undefined、函数
   + `Object.prototype.toString.call()`   
   + Array.isArray()
   + isNaN()
   + isInfinity()
  
### 箭头函数
+ 场景：用于替换匿名函数
+ 基本用法：
    ```js
        //匿名函数
        div.onclick=function(){
            console.log("你好")
        }
        //箭头函数
        div.onclick=()=>{
            console.log("你好")
        }
    ```
+ 有一个参数的箭头函数
    ```js
        var fn=(a)=>{
            console.log("abc");
        }
        //等价于：
        var fn=a=>{
            console.log("abc");
        }
    ```
+ 有2个及更多参数的箭头函数
    ```js
        var f=(a,b,c)=>{
            console.log("abc")
        }
    ```

+ 箭头函数和普通匿名函数有哪些不同？
    - 函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
    - 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
    - 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
    - （不常用）不可以使用yield命令，因此箭头函数不能用作 Generator 函数。 
        - generator函数现在经常用async替代
    ```js
    var p={
        age:18,
        run:()=>{
            setTimeout(()=>{
                console.log("run",this);//this指向window
            },100)
        },
        say(){
            setTimeout(()=>{
                console.log("say",this);//this指向p
            },100)
        },
        //推荐使用的方式
        travel:function(){
            setTimeout(()=>{
                console.log("travel",this);//this指向p
            },100)
        }
    }
    ```
    
## 对象的扩展
+ `Object.assign`：实现拷贝继承
  ```js
    var source={age:18,height:170,className:"三年级"};
    var newObj=Object.assign({},source);
  ``` 
+ 对象扩展运算符
    ```js
        var obj1={ age:5,gender:"男" }
        var obj2={ ...obj1 }
        var obj3={ ...obj1 , age:10 }
        var s1=[1,3,5,7,9];
        var s2=[...s1];
    ```

## Promise
为什么要有promise：解决回调地狱的问题
### 回调地狱
```js
    $.get("/getUser",function(res){
        $.get("/getUserDetail",function(){
            $.get("/getCart",function(){
                //...
            })
        })
    })

    function f1(){
        return new Promise((resolve)=>{
            setTimeout(()=>{
                console.log("第一步");
                //异步执行完毕，必须告诫外界执行结束
                resolve();
            },1000)
        })
    }
    function f2(){
        return new Promise((resolve)=>{
            setTimeout(()=>{
                console.log("第二步");
                //到这里异步操作就已经结束了，如何让外界得知
                resolve();
            },1000)
        })
    }
    f1().then(res=>{
        //return promise对象
       return f2();
    }).then(res=>{
        setTimeout(()=>{
            console.log("结束");
        },1000)
    })
```

### Promise函数基本用法
```js
    var promise=new Promise((resolve,reject)=>{
        $.get("/getUser",res=>{
            resolve(res)
        })
    })
    promise.then(res=>{
        console.log(res);
    })
```

### Promise函数实现多层回调
```js
    new Promise((resolve,reject)=>{
        $.get("/getUser",res=>{
            resolve(res)
        })
    }).then(res=>{
        //用户基本信息
        return new Promise(resolve=>{
            $.get("/getUserDetail",res=>{
                resolve(res)
            })
        })
    }).then(res=>{
        //用户详情
        return new Promise(resolve=>{
            $.get("/getCart",res=>{
                resolve(res)
            })
        })
    }).then(res=>{
        //购物车信息
    })
```

### Promise函数错误处理
+ 第一种方式
    ```js
        new Promise((resolve,reject)=>{
            $.ajax({
                url:"/getUser",
                type:"GET",
                success:res=>{
                    resolve(res);
                },
                error:res=>{
                    reject(res)
                }
            })
        }).then(resSuccess=>{
            //成功的返回值
        },resError=>{
            //失败的返回值
        })
    ```

+ 第二种方式（推荐使用这一种，reject的错误和代码发生的粗误都可以捕捉到）
    ```js
        new Promise((resolve,reject)=>{
            $.ajax({
                url:"/getUser",
                type:"GET",
                success:res=>{
                    resolve(res);
                },
                error:res=>{
                    reject(res)
                }
            })
        }).then(resSuccess=>{
            //成功的返回值
        }).catch(resError=>{
            //失败的返回值
        })

    ```

## async 
await可以执行异步操作，但是await必须在async函数内执行
```js
    function f1(){
        return new Promise((resolve)=>{
            setTimeout(()=>{
                console.log("第一步");
                //异步执行完毕，必须告诫外界执行结束
                resolve();
            },1000)
        })
    }
    (async function(){
        await f1();
        console.log("第二步");
    })()

    async function get(){
        console.log('开始执行');
        var res = await timer()
        console.log('执行结束：',res);
    }
    function timer(){
        return new Promise((resolve,reject)=>{
            setTimeout(()=>{
                resolve("你好");
            },1000)
        })
    }
    get();
```
错误处理，只能使用try-catch
```js
    function q(){
        return new Promise((resolve,reject)=>{
            setTimeout(()=>{
                reject("你好");
            },1000)
        })
    }
    (async function(){
        try{
            let res=await q();
            console.log(res);
        }catch(e){
            console.log(e);
        }
    })()
```
## class
### 定义一个类
```js
    class Person {
        constructor(name,age) {
            this.name=name;
            this.age=age;
        }
    }
    //相当于：
    function Person(name,age){
        this.name=name;
        this.age=age;
    }

```
### 添加实例方法
```js
    //不使用class添加实例方法
    function Person(){

    }
    Person.prototype.run=()=>{
        console.log("run");
    }
    //使用class添加实例方法
    class Person {
        constructor(name,age) {
            this.name=name;
            this.age=age;
        }
        //定义方法
        say() {
            console.log("大家好，我叫："+this.name+"，今年："+this.age+"岁");
        }
        travel(){
            console.log("坐着飞机去巴厘岛");
        }
    }
```

### 添加静态方法
```js
    class Animal {
        constructor(){

        }
        static age=18;
        static born(){
            console.log("小呆萌出生了")
        }
    }
    //访问静态方法
    Animal.born();
    console.log(Animal.age);
```

### 类的继承
```js
    class Person {
        constructor(name){
            this.name=name;
        }
    }
    class Student extends Person {
        constructor(name,grade){
            super(name);    //调用父类构造函数
            this.grade=grade;
        }
    }
```

## module
### 基本用法
+ 导出模块：
    ```js
        //common.js
        export default { name:"abc" }
    ```

+ 导入模块：
    ```js
        //b.js
        import common from "common.js"

        console.log( common.name ) //"abc"
    ```

### 模块有多个导出
```js
    //person.js
    export const jim = { country :"France" }
    export const tony = { color:"gray" }
    //默认的导出
    export default { name:"abc" }
```
```js
    //index.js
    import person , { jim , tony } from "person.js"

    //person：{ name:"abc" }
    //jim：{ country :"France" }
    //tony：{ color:"gray" }
```

### 模块导入导出取别名
```js
    //person.js
    export const tony = { color:"gray" }
    export { tony as Tony }

    //index.js
    import { Tony } from "person.js"
    import { Tony as man} from "person.js"

    console.log(man)    //{ color:"gray" }
```