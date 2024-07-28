# Servlet代码优化

- 问题描述

  - 一个Servlet只能处理一个业务逻辑   100个业务 岂不是要100个Servlet!!!
  - 如果不用注解  1个Servlet 8行配置
  - Servlet中的参数默认条件下都是String类型,如果类型不一致 需要手动转化
  - 字符集编码需要手动处理....
  - 每个Servlet都需要编辑doGet  doPost 并且代码固定    

- 优化策略

  - 通过一个url请求地址 访问同一个Servlet 通过不同的参数 标识不同的业务逻辑
  - 例子:   
    - http://localhost:8080/user?method=findUserList
    - http://localhost:8080/user?method=addUser
    - http://localhost:8080/user?method=findUserById&id=9
    - http://localhost:8080/user?method=updateUser
    - http://localhost:8080/user?method=deleteUserById&id=9
    - 总结Servlet只要写一个即可.之后通过method的参数执行业务调用即可.
    - 注意事项:  用户的参数名称 必须为method!!!!
  - 具体参见代码

- 优化策略2.0版本

  - 要求通过方法名称,动态的执行方法
  - 实现思路:  利用反射机制 自动调用方法
  - 反射案例API

  ![image-20220816110717167](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220816110717167.png)

  - ==注意事项:  必须保证 method参数中的方法 与实际的方法名称 一致!!!!==
  - 案例实现

  ![image-20220816112223313](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220816112223313.png)



- Servlet优化3.0

  - 优化策略:   优化doGet/doPost   利用公共API BaseServlet 实现代码优化
  - 实现方式

  ```java
  package com.atguigu.servlet;
  
  import javax.servlet.ServletException;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  import java.lang.reflect.Method;
  
  /**
   * @author 刘昱江
   * @className BaseServlet
   * @description TODO
   * @date 2022/8/16 11:27
   * 说明:
   *      该BaseServlet中 抽取了doGet/doPost方法
   *      以后其它的Servlet只需要继承该类  就可以得到其中的方法 包括反射机制
   *      其中的this 代表用户调用的真实的Servlet  通过url地址调用 真实的Servlet实例化对象
   */
  public class BaseServlet extends ViewBaseServlet{
  
      private static final String METHOD = "method";
  
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          doPost(req,resp);
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          //解决post请求乱码问题
          req.setCharacterEncoding("utf-8");
          //解决不走模版引擎的乱码问题
          resp.setContentType("text/html;charset=UTF-8");
  
          //1.获取用户的方法名称
          String method = req.getParameter(METHOD);
          //2.获取当前对象的类型
          try {
              Method declaredMethod = this.getClass().getDeclaredMethod(method, HttpServletRequest.class, HttpServletResponse.class);
              declaredMethod.setAccessible(true); //暴力反射
              //3.调用方法  一般情况下 都是对象.方法(参数)
              declaredMethod.invoke(this,req,resp);
          } catch (Exception e) {
              e.printStackTrace();
              System.out.println("~~~~~~请检查method的方法名称是否匹配!!!!!!");
              throw new RuntimeException(e);
          }
      }
  }
  
  ```

  - 以后Servlet的格式

  ![image-20220816113811782](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220816113811782.png)

  

  - 优化策略4.0 

    - 利用注解动态标识方法

    ![image-20220816114134664](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220816114134664.png)

    

# 书城项目功能实现

- 用户登录实现 
  - 修改登录按钮url地址

```html
 <a href="user?method=toLogin" class="login">登录</a>
 <a href="user?method=toRegist" class="register">注册</a>
```

						- - 实现用户登录和注册的页面跳转

![image-20220816142332091](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220816142332091.png)



- 用户登录实现

  - 用户点击登录按钮 实现数据提交  注意请求路径的写法 

  - url地址:   action="user?method=login"

  - 用户提交 在UserServlet中准备方法接收请求.

  - 利用对象接收用户数据.

  - 根据用户数据 查询数据库是否有该记录

  - 有结果:    说明登录成功   重定向到系统首页

  - 没有结果:   说明用户登录失败  转发到登录页面  提示用户用户名或密码错误.

  - 页面中回显域中数据    th:text=${msg}

  - 页面数据展现问题说明

    - 在返回页面时 模版引擎开始工作  将提示信息 显示为用户名或密码错误.
    - 当页面交给浏览器去解析 展现时, 则vue.js开始工作 再次将用户的提示信息改为 请输入用户名和密码
    - 解决方案:  要求vue.js 动态获取request域中的数据
    - vue.js从模版引擎中取值

    ```html
    msg: '[[${msg==null ? "请输入用户名和密码" : msg}]]'
    ```

    - 兼容性问题:   

      - 模版引擎中会解析[[]]的结构 不管该字符位于什么位置  
      - 图中 的[[]]会被模版引擎解析  即使在注释的位置 所以写的时候注意.

      ![image-20220816152210749](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220816152210749.png)

- 用户注册功能

  	- 用户点击登录注册按钮之后 实现用户入库操作
  	- 通过form表单提交数据.
  	- 通过regist方法 接收用户信息
  	- 调用 service ----dao  实现数据入库
  	- 如果入库成功 重定向到登录页面

  

- 实现用户信息回显

  - 业务说明

  ![image-20220816155306148](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220816155306148.png)

  	- 用户在未登录时,应该展现 登录和注册功能
  	- 如果用户登录成功之后, 应该回显用户信息.
  	- 2组标签不能同时展现.
   - 判断依据: 判断用户是否登录  session.BOOK_USER 是否为null
     	- null:  未登录
        	- 不为null:  回显登录数据
  - 登录前风格

![image-20220816160423281](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220816160423281.png)

		- 登录后风格

![image-20220816160351705](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220816160351705.png)



- 用户注销操作
  - 删除Session中的用户信息.
  - 如果用户点击退出 则重定向到系统首页



# 知识总结

	- vue.js中获取模版引擎的代码结构

```html
msg: '[[   ${msg == null ? "xxxx"  :  msg}  ]]'
```

- [[]]的语法  模版引擎会解析  不管位置在哪里...

- 书城项目调用原则

  - ==url地址:  http://localhost:8080/bookstore03/user业务名?method=方法名==
  - ==利用反射机制 根据方法名称调用方法  必须  method=值 与类中的方法名称一致==

  

  







