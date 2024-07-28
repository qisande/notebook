# 过滤器讲解

	 - 过滤器什么时候创建   tomcat容器启动 则过滤器创建
  - 过滤器什么时候工作     用户-------Filter------放行---->Servlet
    	 - ​															----拦截------>重定向到xxxx页面

- 使用工作器时,应该注意哪些:      拦截的路径
  - 精确匹配   /user
  - 模糊匹配   /user/*
  - 后缀类型匹配   *.jpg     拦截以xxx结尾的请求.
- 使用过滤器时 必须注意程序是否放行!!!!
- 过滤器链:
  - 用户的请求被多个过滤器拦截,所组成的链路 叫做过滤器链.
  - 所有的过滤器都放行,用户才可以访问目标资源!!!
  - 如果有一个过滤器没有放行 ,则用户无法访问资源!!!
  - 使用过滤器链 其实都是在完成业务操作.   用户登录校验!!!
  - 使用过滤器时的顺序:
    - 如果使用的注解格式 则和程序的加载的顺序有关系  从上到下
    - 如果使用web.xml配置文件的格式 ,则和配置顺序有关  先配置先执行!!!
    - 由于每个过滤器都是一个单独的业务流程,其实研究过滤器的执行顺序 其实意义不大.

# 监听器(使用性----了解其功能即可)

 -  监听器

     -  当监听器监控的对象的状态发生变化时 则会触发监听器

     -  监听器配置

        	- web.xml配置

        ```xml
        <!--监听器是服务器行为和用户的请求路径无关-->  
          <listener>
              <listener-class>Listener.MyServletContextListener</listener-class>
          </listener>
        ```

        	- 注解配置

        ```java
        @WebListener   //通过注解标识该类是一个监听器
        public class MyServletContextListener implements ServletContextListener {...}
        ```

- 课堂案例
  - ![image-20220821100308154](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220821100308154.png)
  - ![image-20220821101415178](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220821101415178.png)

```java
 /**
     * Session如何关闭
     *    直接关闭浏览器  session对象也会被关闭!!!
     *    规则: sessionDestroyed 需要服务器主动感知之后,才能调用.
     *    如果用户直接关闭浏览器(对浏览器有效.). 没有人通知服务器Session对象关闭了.
     *    当session 30分钟之后 服务器感知session关闭 则该方法才会被调用!!!
     *    也可以设定最大的生命周期 但是时间是一个大概值
     * @param httpSessionEvent
     */
    @Override
    public void sessionDestroyed(HttpSessionEvent httpSessionEvent) {
        System.out.println("session对象销毁");
    }
```

- 监听器的实际应用的场景
  - 当用户操作满足某些条件时,监听器才会被触发 所以一般用于数据的监控和记录. 日志的控制
  - 容器(框架)之间的调用   Spring/SpringMVC框架之间的整合
  - 架构设计时 可能用到监听器.  对于初学者 使用监听器的概率不高



# 书城项目订单功能实现

- 订单页面原型设计

![image-20220821110012651](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220821110012651.png)

- 业务说明

  - 跳转的页面名称  checkout.html
  - 要求用户必须登录   过滤器
  - 实现购物车数据的入库操作,并且返回订单编号.
  - 订单表和订单项表介绍

  ![image-20220821110428841](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220821110428841.png)

  订单表:

  ![image-20220821110452196](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220821110452196.png)

  订单项表:

  ​	![image-20220821110532330](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220821110532330.png)

  关联关系:   一个订单可以有多个订单项  

  	- 入库操作   第一步完成order入库, 之后完成OrderItem入库动作  
  	- 入库2张表
		
  	- 规则: 当完成订单入库操作时,需要同时将order_id号传给订单项完成入库操作. 如此操作才能实现订单和订单项的关联

- 封装实体对象Order

![image-20220821111345414](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220821111345414.png)

- 封装实体对象OrderItem

![image-20220821111411173](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220821111411173.png)

- 搭建层级代码结构

![image-20220821112019308](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220821112019308.png)



- 订单入库功能实现
  - 通过session获取Cart对象,从Cart中 获取CartItem对象   从CartItem中获取图书book对象.
  - 通过Session获取BOOK_USER对象
  - 当用户点击去结算时,应该发起同步请求,利用模版引擎实现页面跳转.
  - 根据Cart/CartItem 实现Order/OrderItem的入库操作. 2张表同时入库
  - 注意事项:  order表中的order_id 必须与order_item表中的order_id一致
  - 购物车中的数据添加到订单之后 应该清空购物车信息

- 我的订单业务实现
  - 当用户点击我的订单时,要求跳转到order.html页面中
  - 根据userId 查询当前的用户的订单信息.
  - 一个用户可以有多个订单信息,所以采用list集合接收.

- 要求用户数据的回显

  - 通过session获取用户信息

- 将公共的标签进行统一的定义

  - 定义公共标签   cart_right.html

  ```html
  <!DOCTYPE html >
  <html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <base th:href="@{/}">
  </head>
  <body>
      <div class="header-right" th:fragment="cart_right">
          <h3>欢迎<span th:text="${session.BOOK_USER.username}">张总</span>光临尚硅谷书城</h3>
          <div class="order"><a href="order?method=findOrderByUserId">我的订单</a></div>
          <div class="destory"><a href="user?method=logout">注销</a></div>
          <div class="gohome">
              <a href="index.html">返回</a>
          </div>
      </div>
  </body>
  </html>
  ```

  - 引用公共标签
    - cart.html
    - checkout.html
    - order.html

  ```html
  <div th:replace="cart/cart_right :: cart_right"></div>
  ```



# 关于书城项目中的事务的控制

- 业务说明

  - 当用户实现订单和订单项入库操作时 应该满足 一起入库否则一起回滚. 应该满足数据库事务的一致性/原子性.
  - 数据库中的ACID原则!!!  		      口诀 :  原一隔持

  ![image-20220821153733985](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220821153733985.png)

- 思考:  项目中的事务控制 原来 JDBC是自动提交   现在如何通过web项目中的技能点实现事务的控制???



- ThreadLocal介绍

  - 名称: 本地线程变量
  - 作用:  可以在同一个线程内实现数据共享.
  - 规则:  ThreadLocal数据与线程绑定, 不会出现线程安全性问题  

  ![image-20220821161012935](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220821161012935.png)



# Cookie介绍

- Cookie介绍
  - ==Cookie是客户端保存服务器数据的一个文本文件.== 
  - 通常情况下 该文件是加密的
  - 每次用户发起请求时,都会携带Cookie信息.
  - Cookie的数据持久化到磁盘中 所以可以实现永久保存.
  - Cookie的数据类型 key-value 
  - 浏览器读取Cookie时 只能获取当前域名的Cookie  不能获取其它网站的
- Cookie入门案例

```java
 @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.创建Cookie对象
        Cookie cookie = new Cookie("msg","我是cookie信息");
        //2.将cookie保存到浏览器中
        resp.addCookie(cookie);
        //3.重定向到首页
        resp.sendRedirect(req.getContextPath()+"/index.html");
    }
```

![image-20220821170449737](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220821170449737.png)

- Cookie 基本API

  - name:  cookie的名称
  - value:  cookie的值
  - Domain:   这个cookie对哪个网站有效 默认 localhost    www.jd.com
  - path:    用户发起的请求    是否携带cookie
    - 默认值  上下文    当前网站发起请求时 都可以携带cookie信息
    - 如果  path:  day15       cookie对哪个路径有效!!!!
    - 但是 url地址: http://localhost:8080/day18/xx/xx     cookie无效
  - Max-age:  Cookie的生命周期  单位秒   默认是Session级别
    - 默认 session  当浏览器关闭之后  Cookie销毁      maxAge = -1
    - '>0'   Cookie可以存活  xx  秒
    - =0     Cookie立即删除

  - Cookie获取的方法

  ```java
  @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          //1.获取Cookie
          Cookie[] cookies = req.getCookies();
          //2.获取指定的cookie  msg!!!
          if(cookies !=null && cookies.length>0){
              for(Cookie cookie : cookies){
                  if(cookie.getName().equals("msg")){
                      String value = cookie.getValue();
                      System.out.println("用户获取cookie的值:"+value);
                      break;  //跳出循环
                  }
              }
          }
  
          //2.重定向到首页
          resp.sendRedirect(req.getContextPath()+"/index.html");
      }
  ```

  - Cookie的作用
    - 记录用户信息    各大视频网址ＶＩＰ   1个月登录一次!!  
    - 支持断点续播    
    - 后续项目中 遇到的SSO 单点登录项目时 使用!!!!
    - 可以在客户端保存 一些业务数据

  











