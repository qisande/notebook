# 1. JS事件驱动

- 通过事件可以为页面元素添加动作. 通过该动作去执行页面中的业务操作. 页面动态效果
- 常用事件

![image-20220806090944008](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220806090944008.png)

- 入门案例

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>
        function myClick(){
            alert("我是点击事件")
        }
    </script>
</head>
<body>
    <button onclick="myClick()">点击时触发</button>
</body>
</html>
```

- JS事件的常用操作

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>
        function myClick(){
            alert("我是点击事件")
        }

        function myChange(){
            alert("数据修改之后,离焦时触发")
        }

        function  myBlur(){
            alert("离焦时触发")
        }

        function myOnmouseover(){
            //alert("实现超练级跳转!!!!")
            window.location.href ="http://www.baidu.com"
        }
    </script>
</head>
<body>
    <button onclick="myClick()">点击时触发</button>
    <button ondblclick="myClick()">双击时触发</button><br>
    用户名:  <input type="text" name="username" onchange="myChange()">
    密码:  <input type="password" name="password" onblur="myBlur()"><br>
    <!--鼠标移动上去开始触发-->
    <img src="image/32f0f965ca138a13.jpg" width="300px" height="300px" onmouseover="myOnmouseover()">


</body>
</html>
```

- 注意事项:  编辑自定义函数时 一定不要使用关键字,容易陷入死循环.

# 2. JS DOM操作课堂案例讲解

- 模版字符串  
  - 用途: 如果页面中有大量的字符串需要拼接,并且其中需要动态赋值操作.同时需要满足原始代码结构 时  建议采用模版字符串写法.
  - 语法:   使用反引号进行定义   ==``==
  - 取值方式:  ${key}

```javascript
let tds =
                `
                    <td>${id}</td>
                    <td>${name}</td>
                    <td>${sex}</td>
                    <td>
                        <button>删除</button>
                    </td>
                `
```

- 课堂案例代码

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>

        /**
         * 1.获取用户填写的内容
         * 2.获取数据之后 封装为tr标签    1.每个标签自己创建  2.通过innerHtml动态生成
         * 3.将tr标签追加到table中
         */
        function addUser(){
            let id = document.getElementById("id").value
            let name = document.getElementById("name").value
            let sex = document.getElementById("sex").value
            let tr = document.createElement("tr")
            tr.setAttribute("align","center")
            let tds =
                `
                    <td>${id}</td>
                    <td>${name}</td>
                    <td>${sex}</td>
                    <td>
                        <button onclick="deleteUser(this)">删除</button>
                    </td>
                `
            tr.innerHTML = tds
            //将tr标签追加到table中
            document.getElementById("tab1").append(tr)
        }

        /**
         * 删除的策略
         *  根据删除按钮找到tr元素对象.之后将tr标签删除.
         */
        function deleteUser(ele){
            //1.利用内置函数动态获取
            //let btn = event.target
            //console.log(btn) td  tr
            ele.parentElement.parentElement.remove()
        }

    </script>
</head>
<body>
    <h3 align="center">动态表格</h3>
    <table id="tab1" width="65%" border="1px" cellspacing="0" cellpadding="0" align="center">
        <tr>
            <th>序号</th>
            <th>用户</th>
            <th>性别</th>
            <th>操作</th>
        </tr>
        <tr align="center">
            <td>100</td>
            <td>张三</td>
            <td>男</td>
            <td>
                <button onclick="deleteUser(this)">删除</button>
            </td>
        </tr>
    </table>

    <h3 align="center">新增用户</h3>
    <table width="25%" border="1px" cellspacing="0" cellpadding="0" align="center">
        <tr align="center">
            <td>序号</td>
            <td>
                <input id="id" name="id" type="text" value="1001">
            </td>
        </tr>
        <tr align="center">
            <td>姓名</td>
            <td>
                <input id="name" name="name" type="text">
            </td>
        </tr>
        <tr align="center">
            <td>性别</td>
            <td>
                <input id="sex" name="sex" type="text">
            </td>
        </tr>
        <tr align="center">
            <td colspan="2">
                <button onclick="addUser()">添加用户</button>
            </td>
        </tr>
    </table>
</body>
</html>
```

# 3 正则表达式

- **正则表达式用来校验字符串是否满足一定的规则的公式**
- 正则表达式的用途
  - 校验字符串
  - 匹配字符串
  - 替换字符串

- 正则语法介绍
  - 通过new关键字创建对象    let rege = new RegExp("a")
  - 通过/的方式创建对象          let  rege = /a/

```javascript
<script>
        //1.定义正则表达式
        //let rege = new RegExp("a")
        //正则对象创建
        let rege = /a/


        //需要校验的字符串
        let str = "a"
        if(rege.test(str)){  //满足规则 返回true   不满足规则返回false
            alert("满足规则")
        }else{
            alert("不满足规则")
        }
    </script>
```

- 正则表达式 内容较多 不方便记忆 所以 随用随查

- 常用正则语法

  - 不确定次数控制
    - \  转义字符   
    - ^  开始
    - $  结束
    - '* '   任意次   >=0
    - '+'    至少1次   >=1
    - '?'     0次或者1次 

  ```javascript
  /**
           * 案例1:
           *      要求字符a至少出现1次
           */
          let rege = /^a+$/
          let str = "aaaa"
          if(rege.test(str)){
              alert("校验通过")
          }else{
              alert("校验失败")
          }
  ```

  - 确定次数的控制
    - {n}     只允许出现n次
    - {n,}    次数    >=n
    - {n,m}  次数     >=n  同时   <=m

```javascript
 /**
         * 只允许 a字符 出现 2,5次
         */
        let rege = /^a{2,5}$/
        let str = "aaaaaa"
        if(rege.test(str)){
            alert("校验通过")
        }else{
            alert("校验失败")
        }
```

- 万能通配符   . 点

```javascript
 /**
         * 要求用户输入5个字符 内容不限
         * 校验输入的字符是否为.
         */
        //let rege = /.{5}/
        let rege = /^\.$/
        let str = "a"
        if(rege.test(str)){
            alert("校验通过")
        }else{
            alert("校验失败")
        }
```

- 字符集合用法
  - [xyz]     匹配一个字符 只能是xyz的其中一个
  - [^xyz]   匹配一个字符 不能是xyz的其中一个
  - [z-a]      匹配一个字符  所有的小写字母
  - [A-Z]     匹配一个字符  所有的大写字母
  - [0-9]      匹配一个字符  所有的数字
  - [a-zA-Z0-9]  匹配一个字符 要求必须是字母和数字
  - [^a-z]    匹配一个字符  不能是小写字母

  ```javascript
  /**
         * 匹配三个字符 第一个字符 小写字母
         *            第二个字符  数字
         *            第三个字符  任意字符
            前3个字符英文和数字,后边的随意
         */
        //let rege = /^[a-z][0-9].$/
        let rege = /^[a-zA-Z0-9]{3}.*$/
        let str = "???!!!"
        if(rege.test(str)){
            alert("校验通过")
        }else{
            alert("校验失败")
        }
  ```

- 或和分组写法
  - |   或的业务要求
  - () 分组   其中可以写多个条件

```javascript
/**
         * 要求1个字符 要么是数字,要么是a
         * @type {RegExp}
         */
        let rege = /^([0-9]|a)$/
        let str = "b"
        if(rege.test(str)){
            alert("校验通过")
        }else{
            alert("校验失败")
        }
```

- 邮箱正则表达式  /^ [a-zA-Z0-9_-]+@([a-zA-Z0-9-]+[.]{1})+[a-zA-Z]+$/

![image-20220806120127697](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220806120127697.png)

- 正则匹配和替换

  - 规则说明:  
    - 默认条件下匹配和替换只能处理一个字符.
    - 如果需要全部匹配 则需要添加关键字 /g  全文匹配

  - 匹配规则说明

  ```javascript
    //匹配输入的文字中是否有 "国"
          let str  = "中华人民共和国,国家,中国"
          let rege = /国/g
          let result = str.match(rege)
          console.log(result.join(","))
          //一般使用于全文检索.
  ```

  - 字符串替换说明

  ```javascript
  //字符串替换
  let str2 = "你个大**,我是你**,你个小**"
  let rege2 = /[*]/g
  let result2 = str2.replace(rege2,"X")
  console.log(result2)
  ```

  - 使用场景介绍
    - 上传文章时,需要进行文字过滤, 检查是否有敏感信息. 使用匹配功能.
    - 弹幕的评论 ,将敏感词语进行替换.

- 关于正则表达式案例练习

  - 要求:  准备一个from表单 要求用户输入用户名和密码
  - 要求1:  用户名只允许输入 英文和数字 5-20位
  - 要求2:  密码 只允许 小写字母数字_@符 3-20位
  - 要求3:  如果用户输入不满足条件 则弹框提示.

  

  ```javascript
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script>
          function checkUsername(ele){
              //获取用户的输入信息
              let input = ele.value
              //定义正则表达式  i表示不区分大小写
              let rege = /^[a-z0-9]{5,20}$/i
              if(!rege.test(input)){
                  alert("请正确输入用户名!!!")
              }
          }
  
          function checkPassword(ele){
              //获取用户的输入信息
              let input = ele.value
              //定义正则表达式  i表示不区分大小写
              let rege = /^[a-z0-9@_]{3,20}$/
              if(!rege.test(input)){
                  alert("请正确输入密码!!")
              }
          }
      </script>
  </head>
  <body>
      <h1>正则案例练习</h1>
      <form action="#" method="post">
          <table border="1px" cellpadding="0" cellspacing="0" width="35%">
              <tr align="center">
                  <td>
                      用户名:
                  </td>
                  <td>
                      <input type="text" name="username" id="username" onchange="checkUsername(this)">
                  </td>
              </tr>
              <tr align="center">
                  <td>
                      密码:
                  </td>
                  <td>
                      <input type="password" name="password" id="password" onchange="checkPassword(this)">
                  </td>
              </tr>
              <tr align="center">
                  <td colspan="2">
                      <button type="submit">提交</button>
                  </td>
              </tr>
          </table>
      </form>
  </body>
  </html>
  ```

- JS小结

  - 变量的定义   var let  const
  - js数据类型    number  string  boolean  Object 
  - 内置函数    alert  console  confirm   window.onload
  - 循环写法    常规for      in关键   of关键字
  - 数组操作方式    
  - 对象的写法    1.new关键字   2.简化操作  {}  []  //
  - DOM操作
    - id选择器
    - 标签选择器
    - 类选择器
  - 事件绑定
    - 单击事件 
    - 数据改变
    - 离焦事件

  - 正则表达式
    - 校验   ^ $
    - 匹配/替换      /rege/ig  不分区大小写    全文匹配 

# VUE.js学习



- **Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架**
- 渐进式:   开发从简单到复杂 如果有特殊的需求 则引入额外的插件即可.
- VUE.js优势
  - 组件  就是一个单独的页面    这个页面中可以写html/css/js
  - 优势:  
    - 可以将整个页面化整为零
    - 组件可以复用
    - 相当于传统的页面 vue.js更加简洁高效

![image-20220806153623684](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220806153623684.png)

- VUE.js入门案例
  - 引入vue.js 函数类库
  - 准备根标签div
  - 创建vue对象
  - 利用插值表达式动态取值

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <!--1.引入类库-->
    <script src="js/vue.min.js"></script>
</head>
<body>
    <!--2.准备根标签   vue.js需要指定区域进行渲染
       以后开发时都要在div的内部完成
    -->
    <div id="app">
        <!--4.从vue对象的内部获取属性值-->
        {{msg}}
    </div>

    <script>
       const APP = new Vue({
         //el表示将来要绑定的div
         el: "#app",
         //vue对象内部定义的属性
         data: {
           //key: value
           msg: "vue入门案例"
         }
       })
    </script>
</body>
</html>
```

- VUE.JS指令学习
  -  v-text   在未加载成功之前 不展现数据.    dom innerText
  - v-html   以标签的方式展现内容                 dom innerHtml
  - v-pre  想展现标签本身 跳过编译效果
  - v-once  元素只需要被渲染一次,以后不变
  - ==v-model  双向数据绑定== 
    - 属性变化元素变化  
    - 元素变化属性变化

- 核心思想 MVVM思想

  - M model 数据   在vue中可以理解为属性
  - V view  视图      在页面中看到的内容
  - VM  视图模型控制    实现双向数据绑定的核心机制.     
  - 理解:  传统页面数据变化 则必须=="刷新"==

  ![image-20220806170354037](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220806170354037.png)

![image-20220806170421419](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220806170421419.png)

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vue.min.js"></script>
</head>
<body>

    <div id="app">
        <!--1.说明: 如果浏览器未解析完成,则以原始结构展现
            这样的方式用户体验不好 所以需要优化.
        -->
        {{name}}
        <p v-text="name"></p>

        <!--2.v-html用法: 以标签的方式展现数据-->
        <div v-html="h1_html"></div>

        <!--3. v-pre指令  想展现标签本身-->
        <hr>
        <span v-pre>{{name}}</span>

        <!--4. v-once  页面标签只渲染一次-->
        <h3 v-text="hello" v-once></h3>

        <!--5.双向数据绑定测试-->
        用户名: <input type="text" name="username" v-model="username">
    </div>

    <script>
        const  app = new Vue({
            el: "#app",
            data: {
                name: "今天太热了!!!",
                h1_html: "<h1>今天外边40度 快熟了!!!</h1>",
                hello: "欢迎来到西安!!",
                username: "admin"
            }
        })
    </script>
</body>
</html>
```









