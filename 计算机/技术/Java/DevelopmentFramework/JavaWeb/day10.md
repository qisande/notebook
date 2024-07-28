# 学习Thymeleaf模版引擎语法

- 页面从域中动态获取数据
  - request域  取值${key}
  - session域  取值${session.key}
  - application域  取值${application.key}

![image-20220815091423759](images/image-20220815091423759.png)

- 页面中动态获取用户提交的参数

  - 语法:  ${param.参数名称}
  - 编辑页面

  ```html
  <!--动态获取参数信息-->
      <a href="paramServlet?id=100&name=张三">传递id和name的参数</a>
  ```

  - 页面取值

  ```html
   <!--动态获取请求参数-->
       id:   <p th:text="${param.id}"></p>
       name: <p th:text="${param.name}"></p>
  ```

- Servlet注解开发

![image-20220815092615471](images/image-20220815092615471.png)

- 同名提交

```html
<!--同名提交测试-->
    <a href="paramServlet?hobby=赚钱&hobby=吃饭&hobby=疯狂的玩不睡觉">同名提交</a>

 <!--获取同名提交数据-->
<div th:text="${param.hobby}"></div>
<div th:text="${param.hobby[0]}"></div>
<div th:text="${param.hobby[1]}"></div>
```

- 内置对象用法

![image-20220815100606827](images/image-20220815100606827.png)

```html
<!--通过内置对象 获取属性值-->
    <p th:text="${#request.getAttribute('request')}"></p>
    <p th:text="${#session.getAttribute('session')}"></p>
    <p th:text="${#servletContext.getAttribute('application')}"></p>
```

- 公共内置对象

![image-20220815100854740](images/image-20220815100854740.png)

```html
<!--通过公共内置对象取值-->
    <p th:text="${#lists.size(list)}"></p>
    <p th:text="${#lists.isEmpty(list)}"></p>
```

- Ognl表达式语法
  - OGNL：Object-Graph Navigation Language对象-图 导航语言
  - 主要的目的是为了获取对象中的属性和属性的值
  - ==注意事项: 必须有get/set的方法==
  - 对象取值
    - 对象.属性
  - list集合取值
    - list[下标]
  - Map集合取值
    - map.key
    - map.key.属性
    - map.key['变量属性']

```java
 @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       //封装对象之后实现页面取值
       //1.封装User对象
       User user = new User(1001,"不知火舞");
       req.setAttribute("user",user);


       //2.封装为List集合
       List list = new ArrayList<>();
       list.add(user);
       list.add(user);
       req.setAttribute("list",list);

       //3.封装为Map集合
       Map map = new HashMap();
       map.put("user",user);
       req.setAttribute("map",map);
       this.processTemplate("ognl",req,resp);
    }
```

```html
 <h1>对象取值</h1>
     <!--1.获取user对象的数据-->
     <p th:text="${user}"></p>
     id:<p th:text="${user.id}"></p>
     name:<p th:text="${user.name}"></p>

    <!--2.测试list集合取值-->
    <p th:text="${list}"></p>
    <p th:text="${list[0]}"></p>
    <p th:text="${list[0].name}"></p>

    <!--3.从map中获取数据-->
    <p th:text="${map}"></p>
    <p th:text="${map.user}"></p>
    <p th:text="${map.user.name}"></p>
    <!--当name是变量时 使用-->
    <p th:text="${map.user['name']}"></p>
```

- if和unless用法
  - if  如果判断条件为真时   展现标签           v-if类似
  - unless  如果判断条件为假时  展现标签   

```html
<!--4.if和unless测试-->
    <p th:if="${flag}"> 我是展现标签1</p>
    <p th:unless="${flag}"> 我是展现标签2</p>
```

- switch-case 用法

``` html
<!--5.分支结构用法-->
    <div th:switch="${level}">
        <p th:case="1">一级数据</p>
        <p th:case="2">二级数据</p>
        <p th:case="3">三级数据</p>
    </div>
```

- 循环结构
  - 语法:   th:each = "变量的值,状态信息status  : ${key}"
  - status
    - status.count     序号   从1开始
    - status.index     索引   从0开始
    - status.odd  奇数    status.even 偶数      布尔类型值

```html
<!--6.循环结构用法
        将list集合中的变量 循环遍历输出
        vue.js中     v-for="user对象,索引index in list"
    -->
    <div th:each="user,status : ${list}">
        序号:  <span th:text="${status.count}"></span>
        索引:  <span th:text="${status.index}"></span>
        id:   <span th:text="${user.id}"></span>
        name: <span th:text="${user.name}"></span>
    </div>
```

- 模版引擎实现页面的嵌套

  - 定义公共页面     th:fragment="jd_head"

  ```html
  <div id="jdHead"  th:fragment="jd_head">
        <h1>我是京东商城的头部</h1>
      </div>
  ```

  - 引入标签

  ![image-20220815113033045](images/image-20220815113033045.png)

  ```html
   <!--<div id="head">
          <h1>我是京东商城的头部</h1>
      </div>-->
      <!--
          th:insert = "封装标签的页面逻辑名 ::  th:fragment定义的id名称"
      -->
      <div id="oldHead" th:include="head :: jd_head"></div>
  
      <div id="main">
          <h1>我是京东商城的主体</h1>
      </div>
  ```

- index路径跳转说明

  - 从模版引擎代码的结构 可以使用indexServlet
  - 从保护资源的目的 使用indexServlet
  - 使用servlet不能影响原有的代码结构

```java
@WebServlet(value = {"/","/index.html"})
public class IndexServlet extends ViewBaseServlet{

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.processTemplate("index",req,resp);
    }
}
```

- 案例查询
  - 通过点击a标签实现页面跳转  findUserList
  - 创建一个FindUserListServlet 用于数据的查询.
  - 通过servlet---service----dao 查询user表中的list集合
  - 将list集合的数据保存到request域中.
  - 使用模板类引擎实现转发到index.html
  - 之后使用模版引擎的遍历操作 实现表格数据展现

- 案例新增

  - 当用户点击提交按钮时,应该实现数据提交  用户名/密码/邮箱
  - 用户表单提交的地址  action="addUser"   提交方式post提交
  - 创建一个AddUserServlet接收用户提交的数据
  - 利用工具API接收对象  
  - 之后servlet  --- service(密码加密)  --- dao
  - 当用户新增操作完成之后,需要展现列表信息 所以采用重定向的方式 实现列表展现    http://localhost:8080/day10/findUserList

  

- 案例编辑-查询用户信息

  - 当用户点击修改按钮时,应该将当前id值作为参数 实现数据查询
  - 创建FindUserIdServlet 接收id信息
  - 根据Id查询数据库   servlet---service----dao
  - 将查询到的user对象 保存到request域中
  - 将页面转发到  index.html

- 案例编辑---修改用户信息

  - 当用户点击提交操作时, 会提交三个数据   其中password可能为null
  - 创建UpdateUserServlet  接收用户提交的数据
  - 如果是修改操作,则必须要求传递主键Id,但是该Id不能被修改,所以使用隐藏域的形式!!!!
  - 执行业务操作  Servlet---Service(密码加密)---Dao
  - 用户密码操作
    - 有密码    加密 执行修改密码的sql
    - 没有密码    只修改username和email
  - 如果业务执行成功 ,则应该重定向到用户列表页面
  - http://localhost:8080/day10/findUserList
  - 注意事项:   如果对象为null 记得添加判断条件 否则程序 加载终止了!!!!
  - 检查的技巧   哪行没有解析  则哪行报错!!!!

- 案例删除

  - 用户点击删除按钮时 发起请求 

  - ```html
    <a th:href="@{/deleteUserById(id=${user.id})}"><button>删除</button></a>
    ```

  - 准备DeleteUserById的Servlet 接收用户的参数

  - 接收用户信息,执行业务操作

  - Servlet----Service ----Dao

  - 如果业务执行成功  则重定向到列表页面

  