## 先搞清楚this代表什么
1. this是JavaScript的关键字之一。它是**对象**自动生成的一个内部对象，只能在**对象**内部使用。随着函数使用场合的不同，this的值会发生变化。
2. **this指向什么，完全取决于什么地方以什么方式调用。而不是创建时指向什么**。但是有一个总的原则，那就是this指的是，调用函数的那个对象。

## this绑定规则
一般有四种绑定规则：默认绑定、隐形绑定、显性绑定、new绑定
#### 1. 默认绑定
```javascript
function foo(){
	var a = 1;
	console.log(this.a);
	}
var a = 10;
foo();		// 输出10
```
这种就是典型的默认绑定，我们看foo被调用时的情景，“光杆司令”，它前面没有任何的调用对象，像这种直接调用函数的方式就是默认绑定，非严格模式下一般绑定到window对象上，严格模式下是绑定到undefined上。

#### 2. 隐性绑定
隐性绑定就是被调用函数前面有明显的调用对象，上代码：
```javascript
function foo(){
	console.log(this.a);
}
var obj = {
	a: 10,
	foo: foo
}

foo();			// 由“默认绑定”可知输出为window.a，即 undefined
obj.foo();		// 输出为10
```
obj.foo()这种调用方式就是明显的带有调用对象的情况，obj被称为**上下文对象**，foo函数里的this默认绑定到上下文对象上，等价于输出obj.a，所以为10。
此外，如果是链性的关系，比如 xx.yy.obj.foo();, 上下文取函数的直接上级，即紧挨着的那个，或者说对象链的最后一个。

#### 3. 显性绑定
先来看看隐性绑定的问题：每次调用obj.foo()时，可以看到一个隐藏的前提就是obj中必须先有foo方法，如果上下文对象中不存在被调用的函数那么隐性绑定会失败的（即报错：Uncaught TypeError: obj.foo is not a function at eval），由于**不可能每个对象都要加某个被调用的函数**，这样扩展性很差，接下来就是显性绑定发挥作用的时候，显性绑定就是强制绑定的意思：通过call、apply、bind三种函数。

#### 4. new绑定
```javascript
function Person(name){
	this.name = name;
}
// Person虽然是构造函数但也是普通函数，直接调用它就和普通函数没什么不同
Person("Kobe");
console.log(window.name);		// 输出为 Kobe

// Person作为构造函数被调用（红宝石书：6.2 创建对象 6.2.2 构造函数模式）
var person = new Person("Lebron");
console.log(person.name);		// 输出为 Lebron
```
使用**new**操作符创建新实例，会经历以下4个步骤：
1. 创建一个新对象；
2. 将构造函数的作用域赋值给新对象（因此**this**就指向了这个新对象）；
3. 执行构造函数中的代码；
4. 返回新对象。

**注意：在构造函数模式下下构造函数，如果构造函数最后显式地返回一个对象类型的变量，则构造函数里新创建的对象无法被返回，则this在构造函数里绑定的新对象将丢失。**

## 箭头函数的this绑定
箭头函数与普通函数的区别：
1. 箭头函数不适用上面的四种绑定，而是**完全根据外部作用域来决定this**。（他的父级仍适用上面的四种绑定规则）
2. **箭头函数的this绑定后无法被修改**
3. 箭头函数中的this，指向与一般function定义的函数不同，比较容易绕晕，箭头函数this的定义：箭头函数中的this是在定义函数的时候绑定，而不是在执行函数的时候绑定。

```javascript
function foo(){
    return ()=>{
        console.log(this.a);
    }
}
foo.a = 10;

// 1. 箭头函数关联父级作用域this

var bar = foo();            // foo默认绑定
bar();                      // undefined 哈哈，是不是有小伙伴想当然了

var baz = foo.call(foo);    // foo 显性绑定
baz();                      // 10 

// 2. 箭头函数this不可修改
//这里我们使用上面的已经绑定了foo 的 baz
var obj = {
    a : 999
}
baz.call(obj);              // 10
```
另一个例子：
```javascript
var people = {
    Name: "海洋饼干",
    getName : function(){
        console.log(this.Name);
    }
};
var bar = people.getName;

bar();    // undefined
```
--------------------------------------修改后------------------------------------------
```javascript
var people = {
    Name: "海洋饼干",
    getName : function(){
        return ()=>{
            console.log(this.Name);
        }
    }
};
var bar = people.getName(); //获得一个永远指向people的函数，不用想this了,岂不是美滋滋？

bar();    // 海洋饼干 
```
另一个例子：
```javascript
var obj= {
    that : this,
    bar : function(){
        return ()=>{
            console.log(this);
        }
    },
    baz : ()=>{
        console.log(this);
    }
}
console.log(obj.that);  // window
obj.bar()();            // obj
obj.baz();              // window
```
1.我们先要搞清楚一点，obj的当前作用域是window,如 obj.that === window。
2.如果不用function（function有自己的函数作用域）将其包裹起来，那么默认绑定的父级作用域就是window。
3.用function包裹的目的就是将箭头函数绑定到当前的对象上。函数的作用域是当前这个对象，然后箭头函数会自动绑定函数所在作用域的this，即obj。
4.function与箭头函数的区别在：function又创建了一个新的作用域，而箭头函数没创建新的作用域，沿用父级作用域（即继承的是父执行上下文里面的this）

以上笔记多是参考自：https://segmentfault.com/a/1190000011194676 ，里面还有一些习题很不错，有空复习一下。
（我只是借鉴来做私人笔记，不知道算不算转载，侵删）
