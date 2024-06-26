# 1. VueJS 学习

- 事件绑定
  - 语法
    - v-on:click="简单的计算"
    - @click="简单计算/事件函数"
    - 函数关键字:   methods : { 函数名:  function(){xxxxxx函数内容}}

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
        <!--1.实现number自增
            功能:
                1.对简单的数据直接进行操作
        -->
        <h3 v-text="num"></h3>
        <button v-on:click="num++">自增1</button>
        <!--简化操作:   @代表 v-on: -->
        <button @click="num++">自增1</button>
        <!--点击事件绑定函数
            规则: 如果其中不需要传递参数,则()可以省略
                 如果需要传递参数 (arg0,arg1,arg2)
          -->
        <button @click="addNum()">自增1</button>
        <button @click="addNum">自增1</button>
    </div>
    <script>
        const app = new Vue({
            el: "#app",
            data: {
                num: 100
            },
            //函数定义
            methods: {
                addNum: function () {
                    //获取当前元素的属性 之后自增
                    this.num++
                }
            }
        })
    </script>
</body>
</html>
```

- 事件修饰符

  - 阻止冒泡  .stop
    - 有时div会进行嵌套,内层div事件执行完成之后,会执行外层的事件,把这种行为 称之为:"事件冒泡"
    - 如果需要阻止时间冒泡 则添加.stop属性

  ```javascript
  <div id="app">
          {{num}}
          <div id="div1" @click="num++">
              <button @click.stop="num++">自增</button>
          </div>
      </div>
  ```

  - 阻止默认行为    .prevent
    - 有些标签的操作可能需要额外的业务处理,不需要默认行为 所以需要阻止默认行为    @click.prevent="事件函数"
    - 常用位置:    a标签    form表单提交

  ```javascript
  <!--阻止默认行为
              需求: 为超链接添加事件,同时不要求超链接跳转
          -->
          <a href="http://www.baidu.com" @click.prevent="toBaidu">超链接百度</a>
  
          <form action="####">
              用户名: <input type="text" name="username">
              <button type="submit" @click.prevent="toBaidu">提交</button>
   </form>
  ```

- 常见vue.js中的事件说明

  - 按键修饰符
    - 回车键触发  @keyup.enter="login"
    - 空格件触发  @keyup.space="login"
    - 鼠标左键触发  @click.left="login"
    - 鼠标右键触发 @click.right="login"

  ![image-20220808102041481](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220808102041481.png)

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
        <!--1.要求:  用户录入完成之后,回车键触发事件-->
        回车键触发: <input type="text" name="username" @keyup.enter="login"><br>
        空格键触发: <input type="text" name="username" @keyup.space="login"><br>
        鼠标左键触发:  
            <img src="image/1.jpg" width="100" height="100" @click.left="login"><br>
        鼠标右键触发:
        <img src="image/1.jpg" width="100" height="100" @click.right="login">
    </div>
    <script>
        const app = new Vue({
            el: "#app",
            data: {
                num: 100
            },
            //函数定义
            methods: {
                login: function (){
                    alert("事件被触发了!!!")
                }
            }
        })
    </script>
</body>
</html>
```

   - 常用事件
     			- @click
          			- @change
          			- @blur

```javascript
<!--
            2.常见事件
                1.点击事件  @click="xxx"
                2.change事件     @change="xxx"
                3.离焦事件      @blur="xxxx"
        -->
        change事件
            <input type="text" name="username" @change="login"><br>
        离焦事件
            <input type="text" name="username" @blur="login">
```

- 课堂案例练习
  - 知识点:
    - 以后看到文本输入框,第一时间使用双向数据绑定
    - 默认条件下用户录入的数值 其实都是字符串.
    - 如果文本输入框中只能输入数值类型  则设定type="number"

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
       <!--
            需求说明:
                1.准备2个文本输入框   num1   num2
                2. 准备一个总数的p标签  计算结果 num1 + num2
                3. 准备一个计算按钮  进行计算
                4. num2 添加一个回车事件 要求计算结果

            用法技巧:
                1.以后看到文本输入框,第一时间使用双向数据绑定
                2.默认条件下用户录入的数值 其实都是字符串.
                3.如果文本输入框中只能输入数值类型  则设定type="number"
       -->
        num1:  <input type="number" name="num1" v-model="num1"><br>
        num2:  <input type="number" name="num2" v-model="num2" @keyup.enter="addNum">
        <button @click="addNum">计算</button><br>
        <p>总数: <span v-text="sum"></span></p>

    </div>
    <script>
        const app = new Vue({
            el: "#app",
            data: {
                num1: "",
                num2: "",
                sum: 0
            },
            //函数定义
            methods: {
               /* addNum: function (){
                    alert("事件被触发了!!!")
                }*/
                addNum(){
                    //1.需求:  将字符串 转化为数值类型    evel函数
                    //2.需求:

                    //this.sum = eval(this.num1) + eval(this.num2)
                    this.sum = parseInt(this.num1)  + parseInt(this.num2)
                    //this.sum = this.num1 + this.num2
                }

            }
        })
    </script>
</body>
</html>
```

- 属性绑定
  - 语法
    - v-bind:属性名="vue中定义的属性"  `<a v-bind:href="myHref">京东</a><br>`
    - 简化操作  `<a :href="myHref">京东</a><br>`
  - ==核心功能:   动态的修改属性中的数据 使用属性绑定!!!==

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
        <!--1.属性绑定 href标签
            需求:  准备2个按钮  一个京东 一个是天猫
            规则:  通过点击按钮  实现数据的"动态"跳转
            实现原理:
                1.动态的数据必须定义为属性
         -->
        <a href="http://www.jd.com">京东</a><br>
        <!--常规写法-->
        <a v-bind:href="myHref">京东</a><br>
        <!--简化写法-->
        <a :href="myHref">京东</a><br>
        <hr>

        <!--
            实现思路:
                变化的数据  url地址-名称
        -->
        <a v-bind:href="url">{{urlName}}</a><br>
        <button @click="changeUrlJD">京东</button>
        <button @click="changeUrlTM">天猫</button>
    </div>
    <script>
        const app = new Vue({
            el: "#app",
            data: {
                myHref: "http://www.jd.com",
                url: "http://www.jd.com",
                urlName: "京东"
            },
            //函数定义
            methods: {
                changeUrlJD(){
                    this.url = "http://www.jd.com"
                    this.urlName = "京东";
                },
                changeUrlTM(){
                    this.url = "http://www.tianmao.com"
                    this.urlName = "天猫";
                }
            }
        })
    </script>
</body>
</html>
```

- 属性绑定--class样式

  - 控制是否展现   

  ```javascript
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script src="js/vue.min.js"></script>
  
      <!--
          class在页面中 一般控制样式CSS
      -->
      <style type="text/css">
          .red{
              width: 100px;
              height: 100px;
              background-color: red;
          }
      </style>
  
  
  </head>
  <body>
      <div id="app">
          <div class="red"></div>
          <!--
              知识点1:  控制是否展现样式
          -->
          <div :class="{red: flag}"></div>
          <button @click="flag = !flag">切换</button>
      </div>
      <script>
          const app = new Vue({
              el: "#app",
              data: {
                  flag: true
              },
              //函数定义
              methods: {
              }
          })
      </script>
  </body>
  </html>
  ```

  - 关于if判断的说明
    - ==  判断的是内容是否相同  但不能判断类型
    - ===  判断的是 内容和类型是否一致.
  - 控制样式切换

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vue.min.js"></script>

    <!--
        class在页面中 一般控制样式CSS
    -->
    <style type="text/css">
        .red{
            width: 50px;
            height: 50px;
            background-color: red;
        }

        .blue{
            width: 50px;
            height: 50px;
            background-color: blue;
        }
    </style>


</head>
<body>
    <div id="app">
        <!--
            案例2:  样式的改变  由红色 改为蓝色
        -->
        <div :class="divClass"></div>
        <button @click="changeColor">切换</button>
    </div>
    <script>
        const app = new Vue({
            el: "#app",
            data: {
                divClass: "blue"
            },
            methods: {
                changeColor(){
                    if(this.divClass === "blue"){
                        this.divClass = "red"
                    }else{
                        this.divClass = "blue"
                    }
                }
            }
        })
    </script>
</body>
</html>
```

- 分支结构语法

  - v-if     
    - 如果为true 则展现标签
    - 如果为false  则不展现标签
  - 同类型标签
    - v-else-if
    - v-else
    - 特点: 该标签不允许单独使用,必须配合v-if使用
  - v-show标签
    - 功能: 如果页面元素频繁切换 则建议使用v-show
    - 原理: style="display: none;"

  ```javascript
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script src="js/vue.min.js"></script>
      <style type="text/css">
  
  
      </style>
  </head>
  <body>
      <div id="app">
          <h1 v-if="flag">控制标签是否展现</h1>
          <button @click="flag = !flag">切换</button>
          <hr>
          <!--
              根据用户输入的成绩,动态展现等级
          -->
          成绩: <input type="number" name="score" v-model="score"><br>
          等级:
                  <!--v-if可以单独使用-->
                  <span v-if="score >= 90">优秀</span>
  
                  <!--下列条件必须配合v-if使用-->
                  <span v-else-if="score >= 80">良好</span>
                  <span v-else-if="score >= 70">中等</span>
                  <span v-else>继续努力</span>
  
                  <!--v-show介绍
                     业务说明: 如果需要对一个元素频繁的操作,如果按照默认的
                     方式 浏览器需要重新加载/删除该元素. 这样的效率不高.
                     高效的方式:
                     <h1 style="display: none;">我是h1标签</h1>
                  -->
                  <h1 v-show="flag">我是h1标签</h1>
      </div>
      <script>
          const app = new Vue({
              el: "#app",
              data: {
                  flag: false,
                  score: 0
              },
              methods: {
              }
          })
      </script>
  </body>
  </html>
  ```

  - 循环结构用法
    - 语法   v-for="item in  属性"
    - 功能:  遍历的是对象的属性,而展现的是标签内容
    - 课堂案例讲解
      - 遍历数组
      - 遍历对象
      - 遍历集合
      - 遍历集合的优化方案   v-for 一般配合v-if使用

  ```javascript
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script src="js/vue.min.js"></script>
      <style type="text/css">
      </style>
  </head>
  <body>
      <div id="app">
          <!--
              前提: vue.js中的循环 用来控制标签的.写在html中
                  1.循环遍历展现数组-->
          <p v-for="item in array" v-text="item"><!--{{item}}--></p>
  
          <!--2.获取下标-->
          <p v-for="(item,index) in array">{{index}}~~~~{{item}}</p>
  
          <!--3.遍历对象 参数至于个数有关 和名称无关  形参!!!!-->
          <div v-for="(value,key,index) in user">
              <p>{{index}}~~~{{key}}~~~{{value}}</p>
          </div>
  
          <!--4.遍历集合-->
          <div v-for="user in userList">
              <p v-text="user.id"></p>
              <p v-text="user.name"></p>
              <p v-text="user.age"></p>
          </div>
  
          <!--5.遍历集合优化操作   有时集合数据为空,最好提示用户-->
          <h1 v-if="userList2.length === 0">请添加集合数据</h1>
          <div v-for="item in userList2">
              <h1>xxxxxxx</h1>
          </div>
  
      </div>
      <script>
          const app = new Vue({
              el: "#app",
              data: {
                  array: [1,2,3,4],
                  user: {
                      id: 100,
                      name: "tomcat猫",
                      age: 18
                  },
                  userList: [{
                      id: 100,
                      name: "tomcat猫1",
                      age: 181
                  },{
                      id: 101,
                      name: "tomcat猫2",
                      age: 182
                  }],
                  userList2: []
              },
              methods: {
              }
          })
      </script>
  </body>
  </html>
  ```

- 表单提交与Vue.js进行属性绑定的说明
  - 一般的form表单提交的数据 都可以与vue.js中的属性进行双向数据绑定.
  - 优势:  数据填写之后可以动态的为属性赋值,同时数据也可以回显.操作方便

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vue.min.js"></script>
    <style type="text/css">
    </style>
</head>
<body>
    <div id="app">
        <form action="###" method="get">
            用户名: <input type="text" name="username" v-model="username"><br>
            密码: <input type="password" name="password" v-model="password"><br>
            性别:  <input type="radio" name="gender" value="男" v-model="gender"> 男
                   <input type="radio" name="gender" value="女" v-model="gender"> 女<br>
                   <!--如果是复选框 则建议使用数组接收-->
            爱好:  <input type="checkbox" name="hobby" value="游戏" v-model="hobby"> 游戏
                  <input type="checkbox" name="hobby" value="电影" v-model="hobby"> 电影
                  <input type="checkbox" name="hobby" value="电视剧" v-model="hobby"> 电视剧<br>
            城市:
                  <select name="city" v-model="city">
                      <option value="">---请选择---</option>
                      <option value="北京">北京</option>
                      <option value="上海">上海</option>
                  </select>
        </form>
    </div>
    <script>
        const app = new Vue({
            el: "#app",
            data: {
                username: "admin",
                password: "admin123",
                gender: "女",
                hobby: ["游戏","电视剧"],
                city: "上海"
            },
            methods: {
            }
        })
    </script>
</body>
</html>
```

- 表单提交常用属性
  - number属性  可以将数据自动转化为数值类型   字母除外
    - 如果输入的是数字 则可以实现转化
    - 如果先输入字母后输入数字 则按照字符串拼接的方式处理 string
    - 如果先输入数字,后添加字母则默认为数值类型,则自动去除字母
    - 建议使用.number属性时 文本框的类型也是number 匹配度更高
  - trim属性        可以去除前后多余的空格
  - lazy属性         当输入结束之后.离焦时触发  懒加载的方式

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vue.min.js"></script>
    <style type="text/css">
    </style>
</head>
<body>
    <div id="app">
        年龄:  <input type="number" name="age" v-model.number="age">
        <button @click="check">校验属性是否有效</button><br>
        个人信息:
            <input type="text" name="info" v-model.trim="info">
        <button @click="checkInfo">校验长度</button><br>

        <!--
            默认条件下 浏览器实时加载. 如果数据操作频繁 性能有所消耗!!!
            建议使用 .lazy的方式 实现懒加载控制
        -->
        懒加载测试:  {{username}}<br>
            <input type="text" name="username" v-model.lazy="username">
    </div>
    <script>
        const app = new Vue({
            el: "#app",
            data: {
                age: 0,
                info: "",
                username: ""
            },
            methods: {
                check(){
                    alert(typeof this.age)
                },
                checkInfo(){
                    alert(this.info.length)
                }
            }
        })
    </script>
</body>
</html>
```

- 计算属性
  - 需求:  有时某些计算需要被重复调用,其中参数不变,结果不变 .但是方法需要执行多次 性能低.
  - 要求: 能否只执行一次计算,存入缓存 提高用户的效率.
  - 功能:  计算属性:  当有重复计算时,只计算一次 保存到缓存中,以后都从缓存中获取数据 效率高.
  - 语法:
    - 关键字: computed
    - 使用时 通过方法名获取数据  不需要添加()
    - 计算属性对相于方法,内部有缓存效果,如果参数发生变化 则缓存同步更新.

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vue.min.js"></script>
    <style type="text/css">
    </style>
</head>
<body>
    <div id="app">
        <!--
            1.要求用户将输入的内容进行反转
            实现思路:
                1.将字符串拆分为数组
                2.将数组反转
                3.将数组拼接
        -->
        {{msg}}<br>
        {{rev()}}<br>
        {{rev()}}<br>
        {{revCom}}<br>
        {{revCom}}<br>
        录入: <input type="text" v-model.lazy="msg">

    </div>
    <script>
        const app = new Vue({
            el: "#app",
            data: {
                msg: ""
            },
            methods: {
                rev(){
                    console.log("函数执行!!!")
                    return this.msg.split("").reverse().join("")
                }
            },
            //计算属性
            computed: {
                revCom(){
                    console.log("计算属性执行!!!!")
                    return this.msg.split("").reverse().join("")
                }
            }
        })
    </script>
</body>
</html>
```

- 监听器案例
  - 如果需要对用户输入的内容进行监控,根据业务调用方法时建议使用监听器
  - 案例:  当用户输入内容 "后空翻"时,及时通知用户!!!!
  - 用法:
    - 双向数据绑定属性
    - watch 属性属于vue.js 
    - 监听器中的方法 与属性保持一致

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vue.min.js"></script>
    <script>
       // alert("警告!!!!你男朋友有异常  请捕获!!!")
    </script>
</head>
<body>
    <div id="app">
         你的聊天内容:  <input type="text" v-model.lazy="msg">
    </div>
    <script>
        const app = new Vue({
            el: "#app",
            data: {
                msg: ""
            },
            methods: {
            },
            //监控的是属性的变化
            watch : {
                msg(value){
                    if(value === "后空翻"){
                        alert("警告!!!!你男朋友有异常  请捕获!!!")
                        alert("进行异步调用  ajax远程发送")
                    }else{
                        console.log(value)
                    }
                }
            }
        })
    </script>
</body>
</html>
```

- 过滤器

  - 目的:  主要用于用户的页面数据展现
  - 实际用途:   格式化代码/格式化时间/格式化字母大小写
  - 案例:   将用户输入的内容 全部转化为大写字母
  - 语法:
    - 创建过滤器 Vue.filter("过滤器名称","函数(参数)")
    - 其中的函数的参数 就是需要过滤器处理的内容.必须传递.
    - 使用过滤器时,必须添加return关键字.
    - 用户使用时  使用 |      `{{msg | upper}}`

  ```javascript
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script src="js/vue.min.js"></script>
      <script>
      </script>
  </head>
  <body>
      <div id="app">
          {{msg | upper}}<br>
          用户输入 <input type="text" v-model.lazy="msg">
      </div>
      <script>
          //定义全局过滤器
          //Vue.filter("过滤器的名称",function (){"执行过滤器任务  必须添加return关键字"})
          /*Vue.filter("upper",function (oldValue) {
              //console.log(oldValue)
              return oldValue.toUpperCase()
          })
  
          Vue.filter("lower",function (oldValue) {
              //console.log(oldValue)
              return oldValue.toLowerCase()
          })*/
  
  
          const app = new Vue({
              el: "#app",
              data: {
                  msg: ""
              },
              methods: {
              },
              //定义局部过滤器
              filters: {
                  upper(oldvalue){
                      return oldvalue.toUpperCase()
                  }
              }
          })
      </script>
  </body>
  </html>
  ```

- 匿名函数简化写法  箭头函数

```javascript
 /*函数的简化操作 箭头函数
        *  1.箭头函数去除function关键字
        *  2.如果只有一个参数时,参数的括号可以省略
        *  3.多个参数时 建议保留  规则如此.
        * */
        Vue.filter("lower",(oldValue) => {
            return oldValue.toLowerCase()
        })
```



