# 1. 内置函数

- 解释: JS中在内部封装了一些现成的函数,提供对应的功能.
- alert函数   弹出框效果    只能输出基本类型的内容    number string   boolean   如果是对象 则以object代替

```javascript
 //1.弹窗口效果
let num = 100
let str = "ssss"
let flag = true
//alert("我是弹出框效果")
let user = {id: 100,name:"tomcat猫"}
alert(flag)
alert(user)
```

- console函数   控制台日志输出    
  - console.log("xxxx"+基本数据类型)
  - console.log("xxxx"+对象)      会出现 xxxx[object object]的现象 
  - console.log(对象)       会展现对象的内容

```javascript
console.log("输入日志信息:"+flag)
console.log("用户信息:"+user)
console.log(user)
```

![image-20220805091948392](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220805091948392.png)

- confirm  消息确认框   弹出框询问用户是否确定    点击确定 返回true   点击取消 返回false.

```javascript
 let result = confirm("你确实删除吗?")
 alert(result)
```

# 2 F12开发者工具说明

- Elements  可以检查当前页面的所有元素

![image-20220805094407914](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220805094407914.png)

- console  浏览器的控制台

![image-20220805094454894](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220805094454894.png)

- network  监控当前页面发起所有网络请求.

![image-20220805094711005](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220805094711005.png)

# 3. 自定义函数

- 语法      

```javascript
function xxxx函数名称(参数列表...){ 函数的内容 }
```

- 用法   先定义 后使用.

```javascript
<script>
       //函数定义
       function hello(){
           alert("你好JS")
       }
       //函数使用
       hello()
</script>
```

- 常见函数定义

```javascript
 //1.无参无返回值的函数写法
       function addNum(){
           let num1 = 100
           let num2 = 200
           let sum = num1 + num2
           alert("结果:"+sum)
       }
       addNum()

       //2.有参无返回值的函数写法
       function addNum2(num1,num2){

           let sum = num1 + num2
           alert("结果:"+sum)
       }
       addNum2(500,600)

       //3.有参有返回值
       function  addNum3(num1,num2,num3){
           //let sum = this.num1 + this.num2 + this.num3
           //return num1 + num2 + num3
           return num1 + num2 + num3
       }
       let result = addNum3(50,50,50)
       alert("三个参数的结果:"+result)
```

# 4. 匿名函数

- 匿名函数 在定义时  不需要编辑函数名称,但是需要通过变量定义函数.

```javascript
 //匿名函数  通过一个变量定义一个函数.
let addNum = function (num1,num2) {
    alert(num1+num2)
}
addNum(100,200)
```

-  匿名函数的使用场景 : 1.Ajax 回调函数中使用   2.在对象定义函数时常用
- window中的匿名函数(重点记忆)  主要解决了JS加载顺序的问题

```javascript
/**
        * 当页面都加载完成之后,才调用该函数!!
        */
window.onload = function (){
    alert("当页面所有的都加载完成之后,才调用该函数")
}

alert("测试执行顺序")
```

# 5 对象的定义

 - new 关键字定义对象     let user = new Object()
   	- user.属性 = 属性值
      	- user.函数名 = function(参数列表) {}

```javascript
 //1.通过new关键字创建对象
       let user = new Object()
       user.id = 100
       user.name = "tomcat猫"
       user.age = 18
       user.addNum = function (num1,num2) {
           return num1 + num2
       }
       console.log(user)
       console.log(user.addNum(100,100))
```

- 对象创建第二种写法  {}
  - let 对象名称 = { key: value,key2: value2,key3: value3}

```javascript
 /**
        * 对象定义方式二   大括号写法
        */
       let user2 = {
           id: 1000,
           name: "mysql",
           age: 18,
           addNum : function (num1,num2) {
               return num1 + num2
           }
       }
       console.log(user2)
       console.log(user2.addNum(200,300))
```



- this关键字用法

  - 如果this在对象的内部使用,则代表当前对象
  - 通过this.属性 获取的就是对象的属性值.

  ```javascript
   /**
          * 对象定义方式二   大括号写法
          */
         let user2 = {
             id: 1000,
             name: "mysql",
             age: 18,
             addNum : function (num1,num2) {
                 return num1 + num2
             },
             getAge: function (){
                 return this.age
             }
         }
         console.log(user2)
         console.log(user2.addNum(200,300))
         console.log("获取当前对象的年龄:"+user2.getAge())
  ```

  

  - 如果this 在script标签中直接使用,则this代表的是当前解析js的唯一对象window对象. 该对象提供了很多的内置函数. window对象就是JS内置核心对象.

  ```javascript
  //单独测试this
  let windows = this
  console.log(windows)
  ```

  # 6 数组写法

  - new Array()

  ```javascript
   let array = new Array()
   array[0] = "数学"
   array[1] = "语文"
   array[2] = "英语"
   console.log(array)
  ```

  - []括号写法

  ```javascript
  //2.[]括号写法
  let array2 = ["安琪拉","亚索","凹凸曼"]
  console.log(array2)
  ```

  # 7 数组常用操作(重点内容)

  - push    向数组末尾追加元素  

  - pop      弹出最后一个元素

  - reverse  实现数组反转

  - splice("起始位置","删除几个元素")    删除操作

  - splice("起始位置","替换的数量","替换后的值")   替换操作

    上述语法 会直接映射原始数组结构

    

  - 字符串和数组的转化
    - join     将数组按照特定连接符拼接为字符串
    - split    将字符串按照特定的字符 拆分为数组

  ```javascript
  <script>
          let array = [1,2,3,4]
  
          //1.追加元素   push 在数组的末尾追加元素    压栈
          array.push(5)
          console.log(array)  //1,2,3,4,5
  
          //2.弹出数据  出栈/弹栈  会直接影响原始数组结构
          array.pop()
          console.log(array)  //1,2,3,4
  
          //3.数组反转
          array.reverse()
          console.log(array) //4,3,2,1
  
          //4.删除
          //4.1 删除第一个元素
          //array.splice("起始位置","删除几个元素")
          //array.splice(0,1)
          //console.log(array) //3,2,1
  
          //4.2 删除第2,3个元素
          //array.splice(1,2)
          //console.log(array)   //4,1
  
          //4.3 删除最后一个元素
          //array.splice(array.length-1,1)
          //console.log(array)     //4,3,2
  
          //5.修改操作
          let array2 = [1,2,3,4]
  
          //5.1 将第二个元素 换位6
          //array2.splice("起始位置","替换的数量","替换后的值")
          //array2.splice(1,1,6)
          //console.log(array2)
  
          //5.2 将第3-4个数据 修改为  100,200,300
          array2.splice(2,2,100,200,300)
          console.log(array2)
  
          //6.数组和字符串的关系转化
          //下列操作 不会对原始数组结构产生影响,需要手动接参
          let array3 = [1,2,3,4]
          let str = array3.join(":")
          console.log(str)        //1:2:3:4
          console.log(typeof str)
  
          //字符串转化为数组
          let newArray = str.split(":")
          console.log(newArray)
  
      </script>
  ```

  # 8 循环遍历写法

  - 常规写法

  ```javascript
   let array = [1,2,3,4,5]
   //循环方式1:
   for (let i=0;i<array.length;i++){
       console.log(array[i])
   }
  ```

  - in关键字
    - 遍历数组   遍历得到的结果是下标
    - 遍历对象   遍历对象得到的是对象的属性

  ```javascript
   //循环方式2: in关键字
          for(index in array){
              console.log("~~~~"+array[index])
          }
  
          //in关键字 遍历对象
          let user = {
              id: 100,
              name: "tomcat",
              age: 18
          }
  
          /**
           * bug说明:  对象.属性 要求属性是定值 不能是变量
           */
          for(key in user){
              console.log("~~~"+key)
              //console.log(user.id)
              //console.log(user.key)
              //如果需要通过变量的方式动态的取值 则使用[[key]]的写法
              console.log(user[[key]])
          }
  ```

  - of关键字  遍历数组 可以直接获取数组中的数据

  ```javascript
   let array3 = [1,2,3,4,5]
   for(num of array3){
       console.log(num)
   }
  ```

  - 总结
    - 如果需要下标/或者需要对象的属性 则使用  in关键字
    - 如果只是常规循环遍历 使用of关键字

  

  # 9 JSON结构介绍

  

  - JSON的使用说明  使用json主要的目的是为了前后端交互 做准备.

  ![image-20220805144515210](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220805144515210.png)

  - JSON概念:  **JSON**(JavaScript Object Notation) 是一种轻量级的数据交换格式。 JSON的本质就是字符串

  - JSON基本结构类型

    - 名称键值对集合 Object类型
      - 无需的键值对集合
      - {}开始和结束
      - key value之间使用 :号连接
      - 多个key-value之间使用 ,号分割

    ![image-20220805145155316](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220805145155316.png)

    ```json
    {"id":100,"name":"tomcat猫","age":18,"flag": true}
    ```

    - ​	key 必须添加引号
    - ​    数值类型和布尔类型 可以直接编辑

    

    - 值的有序列表      数组类型

      - value是有序的
      - 以[]开始和结束
      - 多个value之间,号分割

      ![image-20220805145812613](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220805145812613.png)

      demo 演示:

      ```json
      [100,"张三","你好",true]
      ```

  - JSON的嵌套结构(重点掌握)

    - 值可以是双引号括起来的字符串（*string*）、数值(number)、`true`、`false`、 `null`、对象（object）或者数组（array）。这些结构可以嵌套。
    - ![image-20220805150210464](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220805150210464.png)
    - JSON测试网站:  https://www.json.cn/
    - JSON嵌套结构:

    ```json
    [100,true,null,[1,2,3],{"id":100,"name":"tomcat猫","hobby": ["敲代码","看直播",["躺着","趴着","倒立"]]}]
    ```

  - JSON与JS对象之间的转化

    - JS解析JSON串之后,自动转化为对象

    - JSON.stringify(obj)  将对象转化为JSON串

    - JSON.parse(str)    将字符串转化为对象(JS对象/JSON对象)

    - ```javascript
      <script>
             /*
             *  1.JSON串
             *  2.JS解析JSON串 自动的转化为 JSON对象
             *  3.JSON对象的本质就是JS对象
             */
             let json = {"id": 100,"name":"tomcat猫","age":18}
             alert(typeof json)
             alert("名称:"+json.name)
      
             //将对象转化为JSON串
             let str = JSON.stringify(json)
             alert(typeof str)
      
             //将字符串转化为对象
             let obj = JSON.parse(str)
             alert(typeof obj)
      
      
          </script>
      ```

    - JSON转化的实际意义:   ==**在早期**==时需要将JS对象转化为JSON串之后 可以保留数据的基本结构类型. 当服务器返回数据时,一般都是采用JSON的方式返回. 为了从字符串中获取数据 则最好转化为对象的方式使用  使用对象.属性获取数据.

      现在:  后端服务器直接返回JSON串,由JS解析之后 自动转化为JS对象. 所以可以简化上述的操作.

      ==**具体问题 还需要具体分析:  通过 typeof函数 可以判断是否完成了自动的转化.**==

      

# 10 DOM操作(理解重点--基础)

- DOM是Document Object Model的缩写，意思是『文档对象模型』——将HTML文档抽象成模型，再封装成对象方便用程序操作。(面向对象的思想)
- 解析: 可以利用DOM对象实现JS对页面Html的控制
- DOM树:  DOM将整个html页面,根据父子关系,抽象为一个树形结构. 有了这个结构可以方便的操作页面中的元素信息.

![image-20220805160010503](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220805160010503.png)

- DOM操作入门案例---看懂即可

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>
        window.onload = function (){
            let p = document.getElementById("p1")
            console.log(p)
        }
    </script>
</head>
<body>
    <h1>DOM操作</h1>
    <p id="p1">我是DOM操作的入门案例</p>
</body>
</html>
```

- 根据选择器获取元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>
        window.onload = function (){
            //1.根据ID获取元素
            let p = document.getElementById("p1")
            console.log(p)

            //2.根据标签名称获取元素
            let pArray = document.getElementsByTagName("p")
            console.log(pArray[0])

            //3.根据name属性获取元素
            let p2 = document.getElementsByName("pName")[1]
            console.log(p2)

            //4.class属性获取元素
            let p1Class = document.getElementsByClassName("pClass")[0]
            console.log(p1Class)
        }
    </script>
</head>
<body>
    <h1>DOM操作</h1>
    <p id="p1" name="pName" class="pClass">我是DOM操作的入门案例1</p>
    <p id="p2" name="pName" class="pClass">我是DOM操作的入门案例2</p>
</body>
</html>
```

- 获取子元素写法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>
        window.onload = function (){

            //1.获取div1  获取子元素
            let childrens = document.getElementById("div1").children
            console.log(childrens)

            //2.获取div1  第一个孩子
            let p1 = document.getElementById("div1").firstElementChild
            console.log(p1)
        }
    </script>
</head>
<body>
    <h1>DOM操作</h1>
    <div id="div1">
        <p>老大</p>
        <p>老二</p>
        <p>老三</p>
    </div>
</body>
</html>
```

- 关于属性取值和赋值操作

  - 标签中的默认属性,可以通过元素.属性的方式获取数据.
  - 如果标签中的属性是自定义的 则需要通过pEle.getAttribute("name") 获取数据.

- ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script>
          window.onload = function (){
              //1.获取p标签中的属性值
              let pEle = document.getElementById("p1")
              let id = pEle.id
              let name = pEle.name
              let name2 = pEle.getAttribute("name")
              console.log(id+":"+name+":"+name2)
  
              //获取文本信息
              let text = pEle.innerText
              console.log(text)
  
              //为属性和文本赋值
              pEle.id = "p100"
              pEle.setAttribute("name","pName2")
              pEle.innerText = "修改数据!!!!!"
              console.log(pEle)
          }
      </script>
  </head>
  <body>
      <h1>DOM操作</h1>
      <p id="p1" name="pName">我是p标签</p>
  </body>
  </html>
  ```

- 元素追加操作

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>
        window.onload = function (){
            //写法1:
            //1.创建tr标签
           let trEle = document.createElement("tr")
           trEle.setAttribute("align","center")

           //2.创建td标签
            let tdEle1 = document.createElement("td")
            tdEle1.innerText = 200
            let tdEle2 = document.createElement("td")
            tdEle2.innerText = "李四"
            let tdEle3 = document.createElement("td")
            tdEle3.innerText = 18

           //3.将td追加到tr标签中
            trEle.append(tdEle1,tdEle2,tdEle3)
            console.log(trEle)

           //4.将tr标签追加到table中
            document.getElementById("tab1").append(trEle)

            //写法2:  利用innerHtml的方式简化代码
            let tr2 = document.createElement("tr")
            tr2.setAttribute("align","center")
            //定义的html的字符串
            let tds = "<td>300</td><td>麻子</td><td>20</td>"
            // <tr>
            //   <td>300</td><td>麻子</td><td>20</td>
            // </tr>
             tr2.innerHTML = tds
             document.getElementById("tab1").append(tr2)

        }
    </script>
</head>
<body>
    <h1>DOM操作</h1>
    <table id="tab1" border="1px" cellpadding="0" cellspacing="0" width="500px">
        <tr>
            <th>编号</th>
            <th>名称</th>
            <th>年龄</th>
        </tr>
        <tr align="center">
            <td>100</td>
            <td>张三</td>
            <td>18</td>
        </tr>
    </table>
</body>
</html>
```









