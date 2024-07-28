# Tomat服务器介绍

- Tomcat是Apache 开源的免费的web应用服务器    开发语言:java

- tomcat服务器安装说明

  - 路径要求:  不要有中文/空格/特殊字符  要求全英文路径

  - tomcat服务器启动方式

    - bin/start.bat文件      要求系统中必须配置环境变量 JDK
    - 通过IDEA 整合tomcat服务器进行启动      IDEA中配置的JDK

  - JDK检查

    ![image-20220810090600654](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220810090600654.png)

  - Tomcat服务器目录结构

  ![image-20231016200147872](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016200147872.png)

  - Tomcat服务器默认加载项目说明:   
    - 默认项目名称必须为ROOT 注意大小写

  ![image-20231016200444085](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016200444085.png)

- Tomcat服务器启动说明

  - 网址:   http://localhost   或者  http://127.0.0.1
  - 端口号:
    - http:默认端口号   80
    - https: 默认端口号 443
    - tomcat服务器: 默认端口号 8080

- Tomcat服务器启动测试
  - 以管理员身份运行 start.bat文件

![image-20231016200457535](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016200457535.png)

![image-20231016200504713](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016200504713.png)

- Tomcat服务器修改端口号    编辑server.xml配置文件

![image-20231016200510965](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016200510965.png)

- 检查8080端口被谁占用

  ![image-20231016200517502](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016200517502.png)

  ![image-20231016200609527](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016200609527.png)

  

- 关于tomcat启动异常说明

  当用户关闭tomcat服务器时,有时由于某种原因导致tomcat服务器未能正常关闭

  - 解决方案:
    - 执行shutdown.bat脚本
    - 打开任务管理器 关闭 java.exe的进程

# 书城项目tomcat服务器发布

- 发布步骤:
  - 将书城项目存入webapps目录下
  - 如果需要默认访问项目则需要将项目名称改为ROOT
  - 如果没有设置默认  http://localhost:8080/bookstore

![image-20231016200615637](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016200615637.png)

![image-20231016201004839](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201004839.png)



# IDEA整合Tomcat服务器

- 配置全局tomcat服务器

![image-20231016201029026](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201029026.png)

- 创建web工程  具体配置方式参见视频/讲义

![image-20231016201036309](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201036309.png)

- Tomcat服务器运行web工程

  - 配置web工程

  ![image-20231016201146073](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201146073.png)

  - 配置tomcat服务

  ![image-20231016201151746](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201151746.png)



# 关于IDEA整合Tomcat服务器问题说明

- 删除多余的war包文件

![image-20231016201156455](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201156455.png)

- 添加war包文件

![image-20231016201200999](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201200999.png)

- 指定out输出目录  web工程需要由xxx.java文件进行编译生成xx.class文件. web项目需要指定统一的输出文件  out目录(一般自动)

![image-20231016201205033](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201205033.png)

个别同学出现 运行键 不能使用的实现 则重启IDEA即可

# IDEA中的Tomcat和Window中的tomcat说明

- windows系统中的tomcat服务器 是本地服务器(真实服务器),如果需要发布项目则必须在webapps目录中添加web工程
- IDEA中的tomcat服务器启动可以理解为 开启了一个单独的线程 其中加载tomcat服务器的副本.其中的配置只对该线程有效.不会影响真实的tomcat服务器. 可以理解为 (沙箱运行)

# Http协议(了解)

- Http 超文本传输协议 规定了浏览器和服务器之间的通讯规则.现在使用广泛

- 报文:   客户端和服务器之间通信传递的内容 叫做报文.

  - 请求报文:     客户端向服务器发起请求的     request
  - 响应报文:     服务器向客户端返回数据的     response

- http通信方式

  - 建立连接
  - 客户端向服务器发送请求
  - 服务器向客户端返回响应
  - 关闭连接

- http关于版本说明

  - http1.0版本  每次发起请求都需要建立连接  效率低
  - http1.1版本  建立长连接的形式  用户建立一次连接之后 可以发起多次请求. 如果常时间没有新的请求  自动断开连接.   多久: 15-30分钟

  

- 关于报文说明

  - 报文基本格式

    - 报文首部
    - 空行
    - 报文主体

  - 请求报文格式说明

    - 请求首行

    ```xml
    GET /?username=admin&password=123456 HTTP/1.1
    ```

    - 请求头

    ![image-20231016201213855](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201213855.png)

    

    - 请求空行   可以理解为分割线
    - 请求体        请求体一般携带参数

- GET请求类型介绍

  - 由于请求参数在请求首行中已经携带了，所以没有请求体，也没有请求空行

  - 请求参数拼接在url地址中，地址栏可见

    [url?name1=value1&name2=value2]，不安全

  - 由于参数在地址栏中携带，所以由大小限制[地址栏数据大小一般限制为4k]，只能携带纯文本

  - get请求参数只能上传文本数据 没有请求体。所以封装和解析都快，效率高， 浏览器默认提交的请求都是get请求

  - 一般情况下  查询操作时首选get请求.   数据提交使用post请求



- POST请求类型
  - POST请求有请求体，而GET请求没有请求体。
  - post请求数据在请求体中携带，请求体数据大小没有限制，可以用来上传所有内容[文件、文本]
  - 能使用post请求上传文件
  - post请求报文多了和请求体相关的配置[请求头]
  - 地址栏参数不可见，相对安全
  - post效率比get低

- POST请求类型报文结构

  - 请求首行

  ```xml
  POST / HTTP/1.1
  ```

  - 请求头信息

  ![image-20231016201218656](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201218656.png)

  - 请求空行     一般不可见
  - 请求体

  ![image-20231016201224118](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201224118.png)

  

- 响应报文介绍

![image-20231016201227336](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201227336.png)

​	响应体信息:

![image-20231016201246322](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201246322.png)



- 补充知识 网络模型 OSI模型

![image-20231016201256979](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201256979.png)

==口诀:   物数网传会表应==



# Servlet

- Servlet介绍     Servlet是java实现前后端交互的机制.

- Servlet入门案例

  - 导入jar包文件

    - 要求: web项目中的jar包文件 必须位于WEB-INF

    ![image-20231016201318990](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201318990.png)

    

  - 准备业务类实现Servlet接口

  ![image-20231016201333539](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20231016201333539.png)

  

  - 编辑web.xml配置文件

  ```xml
   <servlet>
          <servlet-name>HelloServlet</servlet-name>
          <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>HelloServlet</servlet-name>
          <!--访问该Servlet的路径信息要求必须/开头
              localhost:8080/xxxxx/hello
          -->
          <url-pattern>/hello</url-pattern>
      </servlet-mapping>
  ```

  

  - 编辑index.html发起http请求 实现前后端交互.

  ```html
  <body>
      <h1>Servlet入门案例</h1>
      <!--
          web应用中 指定请求主机地址和端口号的写法 绝对路径
      -->
      <a href="http://localhost:8080/day07_servlet/hello">入门案例1</a><br>
      <!--相对路径写法-->
      <a href="hello">入门案例2</a>
  </body>
  ```

- 关于Servlet访问注意事项

  - lib包必须位于WEB-INF中
  - web.xml 中的路径 必须以/开头
  - 用户如何访问服务器  
    - http://主机地址:端口号/项目路径(上下文) /业务路径
    - http://localhost:8080/day07_servlet/hello
  - 页面请求地址
    - 全路径写法 http://localhost:8080/day07_servlet/hello
    - 绝对路径写法:  /day07_servlet/hello
    - 相对路径写法:   hello

  ```html
   <!--
          web应用中 指定请求主机地址和端口号的写法 绝对路径
      -->
  <a href="http://localhost:8080/day07_servlet/hello">入门案例1</a><br>
      <a href="/day07_servlet/hello">入门案例1</a><br>
      <!--相对路径写法-->
      <a href="hello">入门案例2</a>
  ```

- Servlet生命周期
  - 执行构造方法     用户第一次访问时执行   创建Servlet对象
  - init方法        当用户访问时 执行完构造方法之后自动调用.   一般用来指定默认值.
  - service    用户的业务操作,  调用业务就会执行.
  - destroy 方法    当tomcat服务器关闭前 执行销毁动作 释放资源.

```java
package com.atguigu.servlet;

import javax.servlet.*;
import java.io.IOException;

/**
 * @author 刘昱江
 * @className HelloServlet
 * @description TODO
 * @date 2022/8/10 15:13
 */
public class HelloServlet implements Servlet {

    public HelloServlet(){
        System.out.println("1.创建Servlet对象");
    }

    /**
     * 通过实现Servlet接口 重写其中的方法.
     * 其中service 方法是执行的业务操作.
     */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("3.你好Servlet,客户端调用服务器成功!!!");
    }

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("2.执行初始化操作");
    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {
        System.out.println("4.执行销毁方法");
    }
}

```

- 修改Servlet创建的时期
  - 默认条件下 用户第一次访问Servlet时,该对象才会被创建 类似于懒加载机制.
  - 通过web.xml可以控制Servlet什么时候创建

```xml
<!--WEB.XML 主要实现web应用的配置 初始化数据/用户的请求和servlet的映射关系-->
    <servlet>
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
        <!--修改Servlet的启动顺序 数字由小到大 越小越先执行-->
        <load-on-startup>1</load-on-startup>
    </servlet>
```

- 初始化操作说明

  - init方法执行值 其中servletConfig对象 一个servlet独享一个配置对象.

  ```xml
  <!--WEB.XML 主要实现web应用的配置 初始化数据/用户的请求和servlet的映射关系-->
      <servlet>
          <servlet-name>HelloServlet</servlet-name>
          <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
          <init-param>
              <param-name>name</param-name>
              <param-value>mysql</param-value>
          </init-param>
          <!--修改Servlet的启动顺序 数字由小到大 越小越先执行-->
          <load-on-startup>1</load-on-startup>
      </servlet>
  ```
  - 取值方式

```java
 @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        String name = servletConfig.getInitParameter("name");
        System.out.println("2.执行初始化操作 获取数据:"+name);
    }
```

- ServletContext接口介绍
  - tomcat容器启动时,会创建唯一的一个ServletContext对象. 也叫上下文对象. 代表当前web应用.
  - Servlet对象内部可能有多个, 但是上下文对象独一份.
  - ServletContext对象 tomcat服务器启动时创建 ,tomcat服务器关闭时销毁.
  - 可以利用上下文对象(ServletContext) 可以实现多个Servlet的数据共享!!!
  - ==小结:   利用上下文对象 主要获取 getContextPath()/实现数据共享==

```java
 //1.获取上下文对象
        ServletContext servletContext = servletConfig.getServletContext();
        String contextPath = servletContext.getContextPath();
        System.out.println("获取项目的上下文路径:"+contextPath);
        String realPath = servletContext.getRealPath("index.html");
        System.out.println("获取index.html的绝对路径地址:"+realPath);
        String servletName = servletContext.getInitParameter("servletName");
        System.out.println("获取成员变量:"+servletName);
```

# Servlet中常用接口和类(重点练习)

- Servlet接口   该接口是Servlet中最为重要的根级接口,其中定义了生命周期方法.(特别记忆)
- GenericServlet    是一个抽象性类  其中只有一个抽象方法 service().  所以如果只关心业务操作  可以使用该抽象类
- ==HttpServlet==    如果用户需要对请求类型进行控制   则首选httpServlet
  - doGet()       用户get请求时调用
  - doPost()       用户post请求时调用













