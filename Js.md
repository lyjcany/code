## 面向对象
就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候一个一个依次调用就可以了
- 举例:
    小明让小红去打酱油 小明就是面向了小红这个对象
## 面向对象的三大特征:
- 封装  
    把不想对外信息隐藏起来,可以提供独特的访问方式给外界访问
- 继承  
    可以让某个类型的对象有另一个类型的对象的属性和方法
    
- 多态
####  如何访问对象的属性:
* 对象.属性名  
    只适合知道了对象的名字,如果属性名是变量,则这种方法无效
* 对象[属性名]  
    如果是字符串,需要用引号,变量可以直接用.
      

```
person.age = 100; // ok
var n = "age";
person.a = 101;	//  no ok 语法错误
person["age"] = 102; // ok
person[n] = 103;  //ok
```
#### 删除对象
- 使用delete操作符
- delete不是方法,后面不要加()
## 创建对象的方式:
- 字面量创建:

```
<script>
    var p={
        name:"张三",
        age:20
    }
</script>
```
* 使用new Object()创建
    
```
<script>
    var p =new Object();
    p.name="张三";
    p.age=20;
    p.speak=function(){
        console.log("说");
    }
</script>
```
* 工厂模式创建对象   
    用函数封装对象返回对象的模式
      
    不能辨别对象的类型
```
<script>
    function person(name,age){
        var p={
            p.name=name;
            p.age=age;
            p.speak=function(){
                console.log("shuo")
            }
        }
        return p;
    }
    var ps=new Person("张三",23);
    p.speak();
</script>
```
* 使用构造函数创建对象:
    
    使用new 关键字去调用
    可以识别对象的具体类型
    属性共享,方法没有共享

```
<script>
    function p(name,age){
        this.name=name;
        this.age=age;
        this.speak=function(){
            console.log("说")
        }
    }
    var o=new p("张三",23);
    o.speak();
    
</script>
```
## 原型
声明一个函数,内存中会自动创建一个原型对象,每个函数都有一个默认的prototype属性, prototype默认有一个constructor指向了声明的那个函数


- __proto__属性可以去范问[[prototype]]

- hasOwnProperty()方法,判断一个属性是否来自本身


* in操作符 判断一个属性是否存在于这个对象中
可以是原型中


* 通过原型创建对象
 属性和方法都共享了 属性不要共享
```
<script>
    function p(){
        
    }
    p.prototype.name="张三";
    p.prototype.age=23;
    p.prototype.speak=function(){
         console.log("说")
    }
    var o=new p();
    p.speak();
</script>
```
- 混合式创建对象

```
<script type="text/javascript">
		function Person(age,name){
			this.age=age;
			this.name=name;
		}
		Person.prototype.speak=function(){
			alert("说"+this.name);
		}
		Person.prototype.read=function(){
			alert("读"+this.age);
		}
		/*
		 属性在构造函数中添加,方法添加在原型中
		 * */
		var p1=new Person(20,"李四");
		p1.speak();
		p1.read();
		var p2=new Person(10,"张三");
		p2.speak();
		p2.read();
	</script>
```
- 动态原型模式创建对象

```
<script type="text/javascript">
		function Person(name,age){
			this.name=name;
			this.age=age;
			if (!Person.prototype.eat) {
				Person.prototype.eat=function(){
					alert(this.name+"吃东西");
				}
			}
		}
		var p1=new Person("张三",20);
		var p2=new Person("李四",30);
		p1.eat();
		p2.eat();
		console.log(p1.eat()===p2.eat());
		/*动态原型模式创建对象把属性和方法都封装在构造函数中,只有在需要的时候
		 才去构造函数中初始化和调用
		 * */
	</script>
```
## 原型链
```
<script type="text/javascript">
		function Father(name){
			this.name=name;
		}
		Father.prototype.speak=function(){
			console.log("我是父类型中的方法...");
		}
		function Son(age){
			this.age=age;
			
		}
		Son.prototype=new Father("lisi")
		Son.prototype.constructor=Son;
		var s=new Son(20);
		s.speak();
	</script> 
```
```
<body>
		<script type="text/javascript">
			var Father=function(name){
				this.name=name;
			}
			Father.prototype.speak=function(){
				console.log("我是父类的方法");
			}
			var Son=function(age){
				this.age=age;
			}
			Son.prototype=new Father("李四");
			Son.prototype.constructor=Son;
			Son.prototype.speak=function(){
				console.log("我是zi类的方法");
			}
			var s1=new Son(20);
			s1.speak();
			console.log(s1 instanceof Son);//true
			console.log(s1 instanceof Father);//true
			console.log(s1 instanceof Object);//true
			
			console.log(Object.prototype.isPrototypeOf(s1));
			console.log(Father.prototype.isPrototypeOf(s1));
			console.log(Son.prototype.isPrototypeOf(s1));
			console.log(new Father("").__proto__.isPrototypeOf(s1));//true
		</script>
```
### 面向对象理解函数
```
<script type="text/javascript">
		function abc(){
			var i=1;i++;console.log(i);
		}
		abc();//2
		var f=new Function("var i=1;i++;console.log(i)");
		f();//2
		console.log(abc instanceof Function);//true
		console.log(abc instanceof Object);//true
		//所有对象都是继承自Object
		console.log(Function.prototype===abc.__proto__);//true
		
	</script>
```
### 原型链中继承的问题
```
<script type="text/javascript">
		function Father(name){
			this.name=name;
		}
		function Son(age){
			this.age=age;
		}
		Son.prototype=new Father("王五");
		var s1=new Son(20);
		var s2=new Son(30);
		Son.prototype.name="a";
		console.log(s1.name);
		console.log(s2.name);
	</script>
```
# 函数的借调
```
<script type="text/javascript">
		function Father(name,sex){
//			console.log(this)//window
			this.name=name;
			this.sex=sex;
		}
		Father.prototype.speak=function(){
			console.log("父类的方法");
		}
		function Son(name,age,sex){
			//然后Father函数执行,可以手动指定this的指向,第一个参数就是重新定义
			//函数内部this,后面的参数
			//调用这个方法的同时去更改this的指向
			Father.call(this,name,sex);
			this.age=age;
		}
		var s1=new Son("lisi",20,"男");
		s1.speak();//出错    没有实现真正的继承,借调只能调用构造方法  
					//son对象和Father没有任何的关系
		console.log(s1);
		console.log(s1.name);
		console.log(s1.age);
	</script>
	
```
## 组合继承
```
<script type="text/javascript">
		/*
		 组合继承
		 * :1.子类型的构造函数借调父类型的构造函数
		 * 2.子类型的构造函数的原型对象,仍然改为父类型的对象
		 * */
		function Father(sex){
			this.sex=sex;
		}
		Father.prototype.speak=function(){
			console.log("父类型的方法");
		}
		function Son(name,age,sex){
			Father.call(this,sex);
			//借调父类型的构造函数，相当于把父类型中的属性添加到了未来的子类型的对象中
			this.age=age;
			this.name=name;
		} 
		Son.prototype=new Father();
		var s1=new Son("lisi",20,"nv");
		console.log(s1.name);
		console.log(s1.sex);
		console.log(s1.age);
		console.log(s1);
		s1.speak()
	</script>
```
### 借调的一个应用
```
<script type="text/javascript">
		var arr=[];//Array.prototype ->Object.prototype
		var fun=function(){//Function.prototype    ->object.prototype
			
		}
		console.log(Object.prototype.toString.call(null));
		console.log(Object.prototype.toString.call(undefined));
		console.log(Object.prototype.toString.call(1));
		console.log(Object.prototype.toString.call("a"));
		console.log(Object.prototype.toString.call(arr));
		console.log(Object.prototype.toString.call(fun));
		console.log(Object.prototype.toString.call(/s/));
		
		function Person(){
			
		}
		/**无法测出自定义的类型*/
		console.log(Object.prototype.toString.call(new Person()));
		/*求数组的最大值*/
//		var max=Math.max.call(null,2,3,4);
		//var max=Math.max.call(null,[2,3,4]);//Math.max([])
		var arr=[2,342,3,67645,785];
		var max=Math.max.apply(null,arr);//Math.max(2,342,3....)
		console.log(max);
		
		//ES6 展开运算符:  ...
		console.log(Math.max(...arr));
		
		//ES2016 ** 幂
		console.log(2**5);
	</script>
```
# 作用域链
## 作用域
### 1.全局作用域
- 定义在函数外部的对象作用域
- 函数内部没有声明的也是全局作用域
- 所有的全局变量都会声明提前

```
<script type="text/javascript">
  	alert(a);//undefined
	var a = "李四";
</script>
```
###  2.局部作用域
- 定义在函数内部的对象的作用域
- 形参的作用域也是局部作用域
## 作用域链
顾名思义，就是由作用域组成的链，是一个类似链状的数据结构。作用域就是对上下文环境(执行环境)的数据描述

---
执行环境定义了变量或函数有权访问的其他数据，决定了它们各自的行为。每个执行环境都有一个与之关联的 变量对象（variable object），环境中定义的所有变量和函数都保存在这个对象中。

---
 在函数执行过程中，每遇到一个变量，都会检索从哪里获取和存储数据，该过程从作用域链头部，也就是从活动对象开始搜索，查找同名的标识符，如果找到了就使用这个标识符对应的变量，如果没有则继续搜索作用域链中的下一个对象，如果搜索完所有对象都未找到，则认为该标识符未定义，函数执行过程中，每个标识符都要经历这样的搜索过程。
 
```
function sayName(name){
    return name;
}
var say = sayName('jozo');
```
这段代码包含两个作用域：a.全局作用域；b.sayName函数的作用域，也就是只有两个变量对象，当执行到对应的执行环境时，该变量对象会成为活动对象，并被推入到执行环境作用域链的前端，也就是成为优先级最高的那个。 看图说话：

---
![image](http://note.youdao.com/noteshare?id=7fe5b75759604e63137b48bcb46b517e&sub=69814A2E14CB47E085673ABE6343C48A)
## 闭包
- 只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"。
- 在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁

### 闭包的特点:
- 1.可以访问外层函数的局部变量
- 2.读取的局部变量的值一定是最终的值

---
### 闭包的应用:
#### 1.封装私有对象

```
 var p = (function () {
        var person = {
            name: "王二麻子",
            height:160,
            sex: "不明"
        }
        return {
            getName : function () {
                return person.name;
            },
            setName : function (name) {
                person.name = name;
            },
            setHeight : function (height) {
                if(height > 200 || height < 100){
                    throw  Error("发育不良或者是激素吃多了!!!")
                }else{
                    person.height = height;
                }
            }
        }
    })();

    console.log(p.getName());
    p.setHeight(80);
```
#### 2.匿名函数的自执行

```
(function () {
	alert("匿名函数自执行")
    
})();
```
#### for循环的问题

```
<body>
	<input type="button" value="按钮1"	>
	<input type="button" value="按钮2"	>
	<input type="button" value="按钮3"	>
	<script type="text/javascript">
		var btns = document.getElementsByTagName("input");
		for (var i = 0; i < 3; i++) {
			btns[i].onclick = function () {
				alert("我是第" + (i + 1) + "个按钮");
			};
		}
	</script>
</body>
```
发现在点击三个按钮的时候都是弹出 我是第4个按钮。	为什么呢？闭包导致的！ 每循环一次都会有一个匿名函数设置点击事件，闭包总是保持的变量的最后一个值，所以点击的时候，总是读的是 i 的组后一个值4

---
- 解决方法1:

```
<script>
    var btns = document.getElementsByTagName("button");
    for(var i = 0; i < btns.length; i++){
        btns[i].index = i;
        btns[i].onclick = function () {
            console.log(this.index);
        }
    }

</script>
```
- 解决方法2:
 
```
<script type="text/javascript">
		var btns = document.getElementsByTagName("input");
		for (var i = 0; i < 3; i++) {	
			(function (num) {
				btns[i].onclick = function () {
					console.log(num);
				}
			})(i);
		}
	</script>
```



