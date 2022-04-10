# Jay
git study
day01

Javascript组成

- ECMAScript:  基础语法 (变量, 表达式, 流程控制语句, 函数, 数组,对象等) 
- DOM: 操作DOM对象, 事件(鼠标, 键盘), DOM对象属性操作等
- BOM: 定时器, 延时器, (location, navigator, history)等, 和浏览器打交道

与现阶段区别

之前学习的是JS基础语法, 这个阶段学习一些高级用法和思想

学习目标

    构造函数的作用
    深入了解JS对象的各种组成关系: 【如：prototype, __proto__  】显式原型和隐式原型
    封装自己的插件！【如：new Swiper()  】
    类的概念 class, 继承的概念, 实例化概念 
    高阶函数(回调函数, 递归函数, 立即执行函数) 闭包
    深拷贝浅拷贝
    ES6新增的语法使用

JS进阶 

前2天学习, 为了面试, 更深层次的理解JS

后2天学习, 为了学习更多的属性和方法

1. 构造函数 - 批量创建对象

- 目的: 创建一套模板 (一个函数), 用于批量创建对象
- 语法: function 大写开头函数名() {this身上绑定属性}
- 使用: new 大写开头函数名()
- 实战: 带着大家封装自己第一个插件 tab导航切换

1.0 为什么使用构造函数 - 目的

批量创建对象

    // 需求: 我想创建3个学生信息对象 (因为都是学生,所以有着共同的属性 => 年龄, 地址, 名字)
    // 1. 一个个创建 过于麻烦 而且不好整体修改
    let obj1 = {
        name: "小黑",
        age: 18,
        address: "上海"
    }
    let obj2 = {
        name: "小智",
        age: 21, 
        address: "北京"
    }
    let obj3 = {
        name: "老北",
        age: 23,
        address: "广州"
    }
    console.log(obj1, obj2, obj3);

工厂函数批量创建

    // 工厂函数批量创建对象
    function createStudent(nameValue, ageValue, address) {
        // 创建一个新对象
        let student = {}; // new Object();
        // 给对象添加对应的属性
        student.name = nameValue;
        student.age = ageValue;
        student.address = address;
        // 返回该对象
        return student;
    }
    
    let zs = createStudent('zs', 18, '铁岭');
    let ls = createStudent('ls', 20, '上海');
    console.log(zs, ls);
    // 不足:
    // 1. 需要手动创建对象和返回这个对象
    // 2. 创建出来的对象类型统一是object, 这样不方便方法共享,继承等...

自定义构造函数

    function Student(nameValue, ageValue, address) {
        this.name = nameValue;
        this.age = ageValue;
        this.address = address;
    }
    
    let zs = new Student('zs', 18, '铁岭');
    let ls = new Student('ls', 20, '上海');
    
    console.log(zs, ls);
    // 优势: 
    // 1. 不需要手动声明对象和返回对象
    // 2. 有具体的类型, 类型是当前的构造函数名称 
    // 小结: 通过构造函数配合new调用,可以实现批量创建对象效果

给实例对象(new出来的对象我们都称之为实例对象)添加公共方法

    // 思考: 如果实例对象需要添加公共方法怎么做?
    function Student(nameValue, ageValue, address) {
        this.name = nameValue;
        this.age = ageValue;
        this.address = address;
        this.sing = function () {
            console.log('我是一只小小小小鸟');
        };
    }
    
    let zs = new Student('zs', 18, '铁岭');
    let ls = new Student('ls', 20, '上海');
    
    console.log(zs, ls);
    // 需求: 给实例对象添加方法
    zs.sing();
    ls.sing();    

2. 原型 

2.0 内存浪费问题

    // 构造函数
    function Person(){
        this.sing = function(){
            console.log("学习唱歌");
        };
    }
    
    // 1. 判断多个对象之间的sing值是否相等 - 相等证明是同一个内存空间
    // 判断是否相等:
    // 基础类型判断: 值, 类型是否相等
    // 引用类型判断: 内存地址是否相等
    let p1 = new Person();
    let p2 = new Person();
    console.log(p1.sing === p2.sing); // false (不相同)
    
    // 2. 如果有一万个实例对象, 那么会创建1万个对应的函数, 但是函数功能是一样的, 岂不是浪费?
    // 可以回忆一下, 所有的数组都可以使用push,pop等方法... 难不成是每一个数组自己都有一个自己的push么??



    解决方式: 将函数提取出来存到一个变量里面去
    1. 写法不优雅
    2. 如果有多个公共方法很难维护

2.1 prototype - 原型属性

- prototype是什么?
  1. 函数本质上也是对象,所以函数也有属性, prototype是函数的一个属性, 取值是一个对象
  2. 每一个函数天生就有prototype这个属性,是JS内置的
- prototype能干什么?
  1. 所有由该构造函数实例化的对象都来共享这个prototype原型对象里面的值

    // 1. Person其实也是一个对象
    function Person(){
    }
    // 查看Person的prototype属性 - 值是一个对象(原型对象)
    console.log(Person.prototype);
    
    // 2. 尝试在函数的原型属性上, 添加我们需要共享的数据
    Person.prototype.sing = function(){
        console.log("学习唱歌");
    }
    
    // 3. 尝试判断
    let p1 = new Person();
    let p2 = new Person();
    console.log(p1.sing === p2.sing); // true
    
    // 总结:
    // 在原型对象上添加, 让所有实例对象共享, 可以保证内存的不浪费

注意: 判断sing的值是否相等, 不要写成了sing()

prototype是函数特有的属性, 只有函数能使用

2.2 __proto__ 隐式原型

问:  为什么每一个实例对象都可以共享到原型对象上的方法呢?

- __proto__是什么?
  对象的一个属性,并且是每一个对象都有的一个属性
- __proto__能干什么?
  可以通过__proto__访问到构造函数的原型对象
  当一个实例访问属性时, 在自己身上没有找到时, 会自动根据 __proto__ 找到原型对象, 去找原型对象上的属性

    function Person(){
    }
    Person.prototype.sing = function(){
        console.log("学习唱歌");
    }
    // 1. 构造函数里的属性 - 直接在对象上
    let per = new Person();
    // 2. 打印实例对象的隐式原型 通过查询实例对象的隐式原型可以直观的看到这个对象的原型上有什么
    console.log(per.__proto__);
    
    // 总结:
    // "对象的__proto__" 等于 "构造函数的prototype属性"
    console.log(per.__proto__ === Person.prototype);   
    

- __proto__, 构造函数, prototype 关系



课堂练习

    制作一个构造函数,由这个构造函数实例化的对象都拥有sayHi方法
    

重点:

1. 构造函数, 原型, 实例对象的关系图
2. 属性, 写在构造函数中添加, 方法绑定在原型prototype上 (共享)

2.3 原型链

问: 下面代码的zs为什么可以调用toString() 方法

原型链, 就是每个对象通过__proto__去寻找它自身没有的属性/方法来进行调用, 逐级查找, 直到对象的顶端, 没有就报错, 有就调用执行

    function Person(){
    }
    Person.prototype.sing = function(){
        console.log("学习唱歌");
    }
    
    let zs = new Person();
    
    // zs调用一个不存在的方法
    console.log(zs.toString());
    console.log(zs);
    

查找过程

- zs 对象本身
- zs的__proto__ 的对象本身
- zs的__proto__的__proto__ 指向的对象 (Object的原型属性)
- zs的__proto__ 的__proto__的__proto__的对象找 (发现是null)
- 直接报错: xxxx.xxx is not a function

访问对象属性, 会通过隐式原型逐级查找, 这个链式关系,叫做原型链

总结:

    为什么要有构造函数 ? => 批量创建对象
    为什么需要原型对象(prototype) ? => 让所有实例, 共享方法, 节省内存
    为什么需要有隐式原型(__proto__)? => 让对象, 方便的调用更多的属性和方法
    
    1. 为什么所有的日期对象都有getFullYear,getMonth,getDate...等方法 
       => 定义在 Date.prototype 中, 只需要定义一次
    
    2. 为什么所有的数组都有push,pop,join,sort...等方法  
       => 定义在 Array.prototype 中, 只需要定义一次
       
    3. 为什么所有的数组, 对象, 字符串,数字 都可以使用toString
       => 都是因为有我们的原型, 原型链
    

2.4 instanceof 使用

作用:  用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上 

语法:  实例对象  instanceof  构造函数

    // 需求: 判断复杂数据的类型
    function Person(){}
    let per = new Person();
    console.log(per instanceof Person); // true
    
    // ----------------------------------------
    let obj = {};
    let arr = [];
    console.log(obj instanceof Array); // false
    console.log(arr instanceof Array); // true
    
    console.log(obj instanceof Object); // true
    console.log(arr instanceof Object); // true
    

2.5 课上思考

    请正确的往每个小数组里, 添加一个数字, 保证代码不错 [{}, [], {}, []],使用循环判断
    重点: 判断是否是数组, 是数组才往里面加
    思路:
    1. 封装一个方法, 判断数据类型
    2. 调用方法判断, 添加
    

3. this相关 

this是一个关键字, 在函数中使用.  (全局的this是window),会根据函数的调用方式不同最终的指向也不同

绝大部分情况下: 函数的this是无法通过定义函数的上下文就可以知道的, 需要通过调用才能够知道 



来源: 你不知道的 javascript

- 万能口诀: 谁调用 this 就是谁 (除了 new 以外)

    // 普通函数调用(默认绑定)
    function myFn() {
        console.log(this); 
    }
    myFn();
    // 因为前面省略了window. => window.myFn()
    
    // 计时器/定时器函数中 - this指向window对象 (默认绑定)
    setTimeout(function () {
        console.log(this); // window对象
    }, 2000);
    // window调用setTimeout上的函数执行
    
    // ---------------------------------------------------------------
    
    // 事件处理函数中 - this指向事件源 (隐式绑定)
    let btn = document.querySelector("button");
    btn.onclick = function () {
        console.log(this); // btn
    }
    // btn调用onclick方法执行, 调用者是btn, 点击按钮 => btn.onclick(); 
    
    // 对象中函数调用 - 找准函数调用者 (隐式绑定)
    let obj = {    
        fn: function () {
            console.log(this); 
        }, 
    }
    obj.fn();
    
    // ---------------------------------------------------------------
    // new 调用特殊记忆
    function Person(){
        console.log(this);
    }
    
    new Person(); // 这里this指向的new出来的新对象
    Person(); // 这样调用this是window
    
    // 总结: 
    // 看好函数在调用时, 点前面的调用者
    // new调用函数时, 构造函数内this指向的是new新创建的空对象
    

- 面试题

    let obj = {
        a: {
            fn: function () {
                console.log(this); 
            },
            b: 10
        }
    }
    obj.a.fn();
    let temp = obj.a.fn;
    temp();
    
    // -------------------------------------------------------------
    
    function Person(theName, theAge){
        this.name = theName;
        this.age = theAge;
    }
    Person.prototype.sayHello = function(){ // 定义函数
        console.log(this); 
        // console.log('我叫' + this.name + '我今年, ' + this.age + '岁');
    }
    
    let per = new Person("小黑", 18);
    per.sayHello(); // 调用函数
    

3.1 课上练习

    练习: 定义ChuanTool构造函数, 属性接受2个数字(numA, numB), 定义2个方法(getMax, randomNum), getMax方法返回numA和numB其中比较大的值, 另一个方法返回一个随机数(范围就是numA到numB, 假设numA是小于numB的) 
    要求: 所有由ChuanTool构造函数new出来的对象都可以共享到getMax和randomNum方法.
    
    定义2个实例对象, 分别调用返回打印结果
    function ChuanTool(theNumA, theNumB){}
    
    let obj1 = new ChuanTool(10, 20);
    let obj2 = new ChuanTool(40, 80);
    obj1.getMax(); // 20
    obj2.getMax(); // 80
    obj1.randomNum(); // 10 - 20的随机数 
    obj2.randomNum(); // 40 - 80的随机数
    
    // 1. 方法应该写在谁的身上? 共享方法?
    // 2. 如何在共享方法里面获取实例对象的属性
    

3.2 课上练习2

要点整理:

    构造函数: 负责给实例对象添加属性.
    原型对象: 负责给实例对象添加公共方法.
    实例对象: 我们需要用的,通过 `new 构造函数`得到.
    
    属性的取值不同: 在new 调用创建对象的时候传递进去
    公共方法的参数不同: 在实例对象调用该方法的时候传递进去
    如果公共方法需要用到当前对象自身的属性的时候, 可以通过this获取
    

练习

    批量创建老师对象
    属性: 工号, 姓名, 爱好  (实例自己的属性)  直接在构造函数中通过 this.code = code, this.name= name 添加即可
    公共方法: 
    	上课: 每个老师负责不同的阶段课程,     在原型对象中添加的, 复用
    

4. 案例 - 封装tab栏 

最终效果: 用户调用new Tab()函数, 传递对应的选择器,不同的数据就可以得到一个新的tab栏

    <div class="box" id="box">
        <div class="header"></div>
        <div class="main"></div>
    </div>
    
    new Tab('#box', arr1, arr2) // arr1是header数据数组 arr2是main数据数组
    



4.0 案例 - 封装tab栏 - 标签和css

准备好标签结构和CSS样式

    <style>
        .box {
            border: 1px solid #000;
            box-sizing: border-box;
        }
        .box * {
            box-sizing: border-box;
        }
        .box .header {
            width: 100%;
            height: 50px;
            display: flex;
            border-bottom: 1px solid #222;
        }
        .box .header span {
            transition: all 0.3s;
            background-color: #fff;
            line-height: 50px;
            text-align: center;
            cursor: pointer;
            font-size: 16px;
            border-right: 1px solid #000;
            flex: 1;
        }
    
        .box .header span.active,
        .box .header span:hover {
            background-color: #ccc;
        }
    
        .box .main {
            width: 100%;
        }
    
        .box .main div {
            width: 100%;
            text-align: center;
            font-size: 60px;
            font-weight: 800;
            display: none;
        }
        .box .main div.active {
            display: block;
        }
    </style>
    
    <body>
        <div class="box">
            <div class="header">
                <span class="active">tab1</span>
                <span>tab2</span>
                <span>tab3</span>
            </div>
            <div class="main">
                <div class="active">内容1</div>
                <div>内容2</div>
                <div>内容3</div>
            </div>
        </div>
    </body>
    

4.1 案例 - 封装tab栏 - 根据数据动态创建标签

    // 1. 获取标签
    let box1 = document.querySelector('#box1')
    // 2. 准备数据
    let headerArr = ['tab1', 'tab2', 'tab3', 'tab4']
    let contentArr = ['内容1', '内容2', '内容3', '内容4']
    // 3. JS生成如上结构的标签
    // 创建标签
    let headerDiv = document.createElement('div')
    let mainDiv = document.createElement('div')
    // 添加节点
    box1.appendChild(headerDiv)
    box1.appendChild(mainDiv)
    // 设置属性
    headerDiv.className = 'header'
    mainDiv.className = 'main'
    // 遍历
    for (let i = 0; i < headerArr.length; i++) {
      let theSpan = document.createElement('span')
      let theDiv = document.createElement('div')
      // 添加节点
      headerDiv.appendChild(theSpan) // 导航span
      mainDiv.appendChild(theDiv) // 内容
      // 添加属性: 如果是第零项, 要高亮激活
      if (i === 0) {
        theSpan.className = 'active'
        theDiv.className = 'active'
      }
      // 添加内容
      theSpan.innerHTML = headerArr[i]
      theDiv.innerHTML = contentArr[i]
    }
    

4.2 案例 - 封装tab栏 - 点击切换

    // 给每个span添加点击事件
    let spanList = document.querySelectorAll('.header>span')
    let divList = document.querySelectorAll('.main>div')
    for (let i = 0; i < spanList.length; i++) {
      spanList[i].index = i // 存下标
      spanList[i].onclick = function() {
        // 通过 this.index ,可以拿到 span 对应的索引
        // 索引找到的div, 让对应的 div 出现 (排他)
        for (let j = 0; j < divList.length; j++) {
          spanList[j].className = ''
          divList[j].className = ''
        }
        divList[this.index].className = 'active'
        this.className = 'active'
      }
    }
    

4.3 案例 - 封装tab栏 - 构造函数封装

目前是已经完成了一个tab切换的效果, 但是不可复用, 如果又有一个tab效果, 又需要再写一遍 (不合理) 

我们联想 swiper, 并没有每次都重新手写一整个轮播图, 而是直接 new 一下就完了, 所以我们也可以进行构造函数的封装 

    // 定义构造函数
    function CreateTab (query, headerArr, contentArr) {
    
    }
    // 定义init方法 (初始化标签和类名)
    // 定义addClick方法 - 绑定点击效果
    let tab = new CreateTab('#box1', ['tab1', 'tab2', 'tab3'], ['内容1', '内容2', '内容3'])
    tab.init()
    tab.addClick()
    

传参优化, 传一个对象

    let tab = new CreateTab('#box1', {
      headerArr: ['tab1', 'tab2', 'tab3'], 
      contentArr: ['内容1', '内容2', '内容3']
    })
    

完整代码

    // 1. 创建构造函数
    function CreateTab(query, configObj) {
      this.el = document.querySelector(query) // 获取到容器标签, 存到el变量中
      this.headerArr = configObj.headerArr
      this.contentArr = configObj.contentArr
    
      this.init()
      this.addClick()
    }
    // 2. 定义init方法 - 初始化标签和类名
    CreateTab.prototype.init = function () {
      // 创建标签
      let headerDiv = document.createElement('div')
      let mainDiv = document.createElement('div')
      this.el.appendChild(headerDiv)
      this.el.appendChild(mainDiv)
      headerDiv.className = 'header'
      mainDiv.className = 'main'
      // 遍历
      for (let i = 0; i < this.headerArr.length; i++) {
        let theSpan = document.createElement('span')
        let theDiv = document.createElement('div')
        // 添加节点
        headerDiv.appendChild(theSpan) // 导航span
        mainDiv.appendChild(theDiv) // 内容
        // 添加属性: 如果是第零项, 要高亮激活
        if (i === 0) {
          theSpan.className = 'active'
          theDiv.className = 'active'
        }
        // 添加内容
        theSpan.innerHTML = this.headerArr[i]
        theDiv.innerHTML = this.contentArr[i]
      }
    }
    
    
    // 3. 定义addClick方法 - 绑定点击效果
    CreateTab.prototype.addClick = function () {
      // 给每个span添加点击事件
      let spanList = this.el.querySelectorAll('.header>span')
      let divList = this.el.querySelectorAll('.main>div')
      for (let i = 0; i < spanList.length; i++) {
        spanList[i].index = i // 存下标
        spanList[i].onclick = function () {
          // 通过 this.index ,可以拿到 span 对应的索引
          // 索引找到的div, 让对应的 div 出现 (排他)
          for (let j = 0; j < divList.length; j++) {
            spanList[j].className = ''
            divList[j].className = ''
          }
          divList[this.index].className = 'active'
          this.className = 'active'
        }
      }
    }
    
    let tab = new CreateTab('#box1', {
      headerArr: ['tab1', 'tab2', 'tab3'], 
      contentArr: ['内容1', '内容2', '内容3']
    })
    

5. 作业
