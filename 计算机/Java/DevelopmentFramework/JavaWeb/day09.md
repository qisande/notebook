# MVC设计思想

- 知识铺垫
  - 如果有大量的代码需要维护,需要准备一个合理的结构保存代码,尽量保证代码的耦合性低(==松耦合--高内聚-低耦合==).

- MVC设计思想
  - M model   代表后端服务器中的数据 可以理解为实体对象POJO/数据持久化操作.
  - V   view      代表用户可视化的内容   一般代指页面 
  - C   Controller      控制视图和持久层的数据的流转
- 设计目的:     尽可能实现代码的松耦合!!!

![image-20220813090943370](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220813090943370.png)



# 层级代码结构

- 基于MVC设计思想 代码需要有一个具体操作的代码结构 即层级代码结构

- 结构说明

  - Controller层    实现数据的流转和控制   主要来源于前端页面
  - Service层         主要实现==业务==处理
  - Dao层                主要控制数据 CURD操作 与数据库进行关联 核心知识jdbc操作数据库.

- 面试题:   

  - 层级代码结构是基于MVC思想而来,具体的代码实现的方式!!!

  

- 层级代码结构调用原理
  - 标准化文档:   接口文档

![image-20220813093537920](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220813093537920.png)



# 书城项目搭建

 -  创建项目

 -  拷贝文件

    ![image-20220813100319670](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220813100319670.png)

- 配置信息

  - 修改资源目录

  ![image-20220813100345361](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220813100345361.png)

  - 配置虚拟路径

  ![image-20220813100426023](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220813100426023.png)

  - 修改端口号

  ![image-20220813100454705](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220813100454705.png)

  ![image-20220813100513107](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220813100513107.png)

# 项目结构说明

- ==BaseDaoImpl==   在内部将dbUtils进行保证 通过泛型的方式实现了增删改查的操作. 以后我们无需编辑基本的增删改查 只需要调用方法即可.
- JDBCTools       为了实现后期事务的控制 需要动态的获取连接
- 以后代码的调用结构:    用户---->Servlet------Service-----Dao-----数据库中.

# 用户登录实现

- 业务说明

  - 当用户点击登录按钮时,应该提交用户名和密码传递给后端服务器
  - 通过LoginServlet获取其中的用户名和密码的参数 查询数据库进行校验
  - 当用户名和密码正确 则重定向到系统首页 index.html
  - 如果用户名和密码错误  则转发到登录页面(暂时这样写 后期还要优化)

- 页面分析 

  - 请求提交的方式

  ![image-20220813105313181](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220813105313181.png)

- 编辑LoginServlet

  - 编辑web.xml

  ```xml
   <servlet>
         <servlet-name>LoginServlet</servlet-name>
         <servlet-class>com.atguigu.servlet.LoginServlet</servlet-class>
     </servlet>
      <servlet-mapping>
          <servlet-name>LoginServlet</servlet-name>
          <url-pattern>/loginServlet</url-pattern>
      </servlet-mapping>
  ```

  - 编辑LoginServlet

  ```java
  /**
       * url: localhost:8090/bookstore_02/loginServlet
       * 参数: username  password
       * 业务:
       *      根据用户名和密码查询数据库
       *  结果:
       *      有值:  true   重定向到系统首页
       *      没值:  false  转发到登录页面
       */
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
         /* String username = req.getParameter("username");
          String password = req.getParameter("password");
         */
          Map<String, String[]> parameterMap = req.getParameterMap();
          //beanUtils 专门负责将map集合中的参数封装为实体对象的,参数名称必须与属性名称一致 并且有get/set方法
          User user = new User();
          try {
              BeanUtils.populate(user,parameterMap);
          } catch (Exception e) {
              e.printStackTrace();
              throw new RuntimeException(e);
          }
  
          //返回的真实的数据库对象
          User userDB = userService.findUserByUP(user);
          //判断查询是否有值
          if(userDB == null){  //说明用户名和密码错误
              req.getRequestDispatcher("/pages/user/login.html")
                      .forward(req,resp);
          }else{
              //重定向到系统首页
              //resp.sendRedirect(req.getContextPath());
              resp.sendRedirect(req.getContextPath()+"/index.html");
          }
      }
  ```

- 编辑LoginService

  - 接口
  - 实现类

  ```java
  public class UserServiceImpl implements UserService {
  
      //面向接口编程!!!!
      //准备持久层对象
      private UserDao userDao = new UserDaoImpl();
  
  
      @Override
      public User findUserByUP(User user) {
          //1.需要将用户密码加密处理
          String md5Pass = MD5Util.encode(user.getPassword());
          user.setPassword(md5Pass);
          //2.根据用户名和密码进行查询
          return userDao.findUserByUP(user);
      }
  }
  ```

- 编辑UserDao

  - 接口
  - 实现类

  ```java
  public class UserDaoImpl extends BaseDaoImpl implements UserDao {
  
  
      @Override
      public User findUserByUP(User user) {
          String sql = "select * from user where username=? and password=?";
          return this.getBean(User.class,sql,user.getUsername(),user.getPassword());
      }
  }
  ```

# 加密算法介绍

- MD5 介绍:  **MD5信息摘要算法**（英语：MD5 Message-Digest Algorithm），一种被广泛使用的密码散列函数   生成128位（16[字节](https://baike.baidu.com/item/字节/1096318)）的散列值

- 特点:

  - 可由明文加密为密文,不能反向解密
  - 通过函数计算 无解 所以不能反向解密

- 常识

  - java中常见hash码      8位16进制数    0-9   A-F
  - 问8位16进制数 有多少种排列组合?    0000000-FFFFFFFF    = (2^4) ^ 8  2^32
  - 如果密码相同, 采用相同的hash算法 问 结果是否相同?    相同
  - 如果密码不同,采用相同的hash算法  问题 结果是否相同?   可能相同!
  - hash碰撞问题:  不同的数据经过相同的算法可能出现相同的结果.
  - hash碰撞问题: 能否避免   不可以的  只能降低其概率     增大hash的长度.

  

# 用户注册的实现

- 实现步骤:
  - 用户按照要求输入完 数据之后 点击注册按钮实现form表单提交
  - 后端服务器通过RegistServlet接收用户的请求参数.
  - 通过beanUtils 实现对象的转化.
  - 调用业务层Service实现数据入库前准备   密码进行加密处理
  - 调用持久层 实现数据入库操作 调用baseDaoImpl中的方法 实现持久化操作.
  - 通过影响的行数判断业务执行是否成功.
    - 大于0   表示执行操作成功    重定向到登录页面
    - 不大于0   表示新增失败        转发到注册页面
- 具体代码参见项目



# Thymeleaf模版工具类(难点!!!)

- 问题描述:
  - 常规操作要么跳转页面.要么返回特定的文字信息.暂时不能实现页面和文字同时返回的功能!!!
- Thymeleaf介绍
  - Thymeleaf是模版引擎,主要功能是在静态页面中动态渲染页面数据.
- 物理视图和逻辑视图
  - 物理视图
    - /pages/user/login.html
    - /pages/user/regist.html
  - 逻辑视图
    - 前缀:  /pages/user/
    - 逻辑名:  login或regist
    - 后缀: .html
    - ==逻辑视图 = 前缀 + 逻辑名 + 后缀==

- 模版引擎工作原理
- 核心知识点:     如果需要在页面中动态展现服务器数据则使用模版引擎!!!!

![image-20220813161620856](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220813161620856.png)

- 入门案例步骤:
  - 导入jar包
  - 准备web.xml配置文件   定义前缀和后缀
  - 准备ViewBaseServlet (粘贴复制 理解就好 都是现成代码 无需记忆)
  - 编辑 HelloServlet 实现业务调用   web.xml的8行配置
  - 准备WEB-INF/hello.html   通过模版引擎的方式获取数据   th:text=${key}
  - 具体代码参见案例

# Thymeleaf API学习(重点!!!)

	- th:text     类似于 v-text    显示域中的数据
	- th:value   可输入的标签中 动态修改value的属性值(动态获取value值)

```html
 <!--2.修改input的value值-->
    用户名:  <input type="text" name="username" th:value="${usernameValue}">
```

- th:href    动态的获取上下文路过

  - ```html
    <a th:href="@{/index.html}">跳转到首页</a>   <!-- / 代表上下文 -->
    ```

  - base标签:    `<base th:href="@{/}"> `  配合相对路径  提取   上下拼接 为直接路径

  - ```html
    <a href="index.html">优化处理:  base标签 配合相对路径</a>
    ```

- Thymeleaf中操作域对象

  	- 常见域对象的介绍

  ![image-20220813164228318](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220813164228318.png)

  	- request域  同一个request请求内部实现数据共享
   - session域
     	- 理解:  会话机制  在**同一个浏览器**内部实现数据共享
        	- 会话有效期:  默认30分钟
        	- 关闭会话:    关闭浏览器 Session对象销毁.
  	- application 应用域   在同一个tomcat服务器内部使用数据共享
  	- 安全性 实用性:   request域 > Session域   > application域

  - 测试案例 参见代码

  



