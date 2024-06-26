# Servlet知识

 - 简化代码标准格式
	- 为了简化业务代码,通常情况下 会由 doGet调用doPost方法 (标准结构)

```java
  /**
     * 如果用户访问get/post执行的都是相同的业务逻辑,会有什么问题?
     * 问题说明:
     *      由于类型不同,则导致业务操作重复编写.
     * 优化:
     *      尽可能少写重复代码.
     */

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("执行了100行业务代码");
    }
```

- HttpServletRequest(重点!!!)
  - 是ServletRequest的子接口
  - 内部请求报文信息封装为对象 通过HttpServletRequest进行获取
  - 虽然写的是httpServletRequest接口,但是在运行期由容器创建实现类对象进行参数传递.
  - 获取参数 getParameter
    - req.getParameter("key");
    - 该方法获取的都是String数据类型,如果需要其它类型,则手动转化
    - 该方法每次只能获取一个参数信息
  - 获取同名提交的数据
    - req.getParameterValues("hobby");
    - 通过数组的形式保存数据.
    - 一般同名提交时使用该方法
  - getParameterMap
    - 通过一个方法 获取所有的请求数据 Map<String,String[]>
    - 该方法一般配合框架使用, 通过该方法接收参数,利用框架内部的API动态的转化为封装的对象

```java
//http://localhost:8080/day08/hello?username=123&password=456
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //1.获取客户端提交的参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");

        //2.接收同名提交的参数
        String[] hobbies = req.getParameterValues("hobby");
        System.out.println("获取客户端数据:"+username+":"+password+":"+ Arrays.toString(hobbies));

        //3.直接获取请求对象中的所有参数
        Map<String, String[]> parameterMap = req.getParameterMap();
        parameterMap.forEach((key,value) -> {
            System.out.println(key+":"+ Arrays.toString(value));
        });
    }
```



- 获取URL相关的参数

````java
//4.获取URL地址信息
String http = req.getScheme();
String serverName = req.getServerName();
int serverPort = req.getServerPort();
String contextPath = req.getContextPath();
System.out.println(http+serverName+serverPort+contextPath);
````

- 获取请求头信息   大小写都可以  但是建议粘贴复制

```java
 //5.获取请求头信息
        String host = req.getHeader("host");
        String agent = req.getHeader("User-Agent");
        String referer = req.getHeader("Referer");
        System.out.println(host);
        System.out.println(agent);
        System.out.println(referer);
```

- 请求转发的实现

  - 需求: 当用户访问servlet之后,需要返回特定的页面给用户,则可以使用转发的方式实现.
  - 实现方式

  ```java
   //6.转发成功页面给用户
          req.getRequestDispatcher("success.html").forward(req,resp);
  ```

  ![image-20220812104309626](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220812104309626.png)

  

- Request域可以实现数据共享
  - 用户发起一个请求,由ServletA ,将请求转发给ServletB.  并且测试request域对象数据是否共享.
  - Request域可以实现数据的共享. 保证用户使用的是同一个Request对象.

1. ServletA中保存数据

![image-20220812112032761](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220812112032761.png)

	 2. ServletB中获取数据

![image-20220812112052524](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220812112052524.png)



- HttpServletResponse(重点!!!)

  - 该接口是ServletResponse接口的子接口,封装了响应报文的信息
  - HttpServletResponse是接口,运行期使用是容器创建的实现类对象
  - 服务器响应给客户端数据
    -   resp.getWriter().write("你好世界!!!!");
    - 乱码解决:

  ```java
    /**
       * 数据响应时会有乱码问题
       */
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          System.out.println("servlet调用成功,向客户端返回数据");
          resp.setContentType("text/html;charset=utf-8");
          resp.getWriter().write("你好世界!!!!");
      }
  ```

  - 重定向

    - 用户访问自己的Servlet之后,要求用户的页面跳转到百度.
    - 语法:

    ```java
     resp.sendRedirect("http://www.baidu.com");
    ```

  # 转发和重定向的区别(重点)

  - 转发调用

  ![image-20220812115452954](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220812115452954.png)

  

  - 重定向调用

  ![image-20220812115513729](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220812115513729.png)

  

  ![image-20220812115027080](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220812115027080.png)

  

  - 关于WEB-INF的说明
    - 如果页面保存到web目录下,则通过浏览器可以直接访问.如果用户直接通过浏览器访问业务页面则不安全. 所以业务页面不能直接放到web目录下.
    - web项目准备了一个目录WEB-INF,该目录是受保护的.用户不能通过浏览器直接访问. 只能通过服务器==内部跳转(转发)==访问.

```java
//success位于web目录下
//req.getRequestDispatcher("/success.html").forward(req,resp);
//success位于WEB-INF目录下
req.getRequestDispatcher("/WEB-INF/success.html").forward(req,resp);
```

- web阶段乱码解决

  - 请求乱码

    - GET请求    tomcat8以下的版本需要手动设定字符集编码格式

    - GET请求    如果使用tomcat8及以上的版本 则tomcat内部已经定义了规则  所以无需修改!!!

    - POST请求:  都会有乱码问题,必须手动处理

      - 原因说明:  使用post提交 表单提交/文件/音频/视频等媒体文件. 如果都设置为utf-8则会导致媒体文件损坏 不能使用. 所以需要手动指定.

      - req.setCharacterEncoding("utf-8");

  ```java
  //post请求乱码的解决方案
  req.setCharacterEncoding("utf-8");
  String name = req.getParameter("name");
  String sex = req.getParameter("sex");
  ```

  - 响应乱码

    - 响应乱码的说明  是服务器数据交给客户端,之后客户端获取数据在页面中进行展现的.
    - 易错分析:  resp.setCharacterEncoding("utf-8"); 只能保证从resp对象中获取数据不乱码
    - 解决方案:    服务器通过响应报文--响应头 设定字符集编码格式.

    ```java
    resp.setHeader("Content-Type", "text/html;charset=utf-8");
     resp.setContentType("text/html;charset=utf-8");
    ```

# 关于URL地址讲解说明

![image-20220812143902096](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220812143902096.png)

- url是`uniform Resource Locater`的简写，中文翻译为`统一资源定位符`  
  - ==协议名称://主机地址/应用名称/资源名称==
- uri是`统一资源标志符(Uniform Resource Identifier， URI)`
  - web应用下的资源     ==/应用名称/TestServlet==     没有主机地址!!!!!

- 相对路径和绝对路径问题

  - 知识回顾:

    - 相对路径:  对象对于当前地址
    - 绝对路径:  以盘符为开头      C:/xxx/xxx/xxx.jpg

  - WEB应用中绝对路径是以项目的根目录为开始. 没有盘符的概念.

  - 相对路径和绝对路径说明:

    - 相对路径由于基准位置发生了变化 导致所有的路径都要修改 所以项目中不建议使用相对路径.

    - 以后在页面中尽量使用绝对路径.

    - WEB项目中的浏览器解析   ==/开头   只代表主机地址==

      - 全路径写法

      ```html
      <a href="http://localhost:8080/day08_2_url/a/A.html">A页面</a>
      ```

      - 绝对路径写法

      ```html
      <a href="/day08_2_url/a/A.html">A页面</a>
      ```

    - 服务器内部

      - 转发时 /  ==**服务器解析**==    代表服务器的根目录  out/项目路径 

      ![image-20220812154309666](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220812154309666.png)

      

      ```java
       @Override
          protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              System.out.println("转发操作成功!!!!");
              req.getRequestDispatcher("/a/A.html").forward(req,resp);
          }
      ```

      - 如果转发时的请求地址与web.xml中配置的url-pattern路径一致,则执行该Servlet操作.

      ![image-20220812155205627](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220812155205627.png)

      ![image-20220812155227304](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220812155227304.png)

      - 重定向时  ==/ 浏览器解析==    /代表主机地址   所以配合URI(上下文/访问路径)进行编辑

      ```java
      @Override
          protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              System.out.println("用户执行重定向操作");
              ///day08_2_url
              System.out.println(req.getContextPath());
              resp.sendRedirect(req.getContextPath()+"/a/A.html");
          }
      ```



 - base标签用法
   	- base标签可以将公共的路径进行抽取
      	- base标签必须位于head标签内部
   	- 使用base标签之后 会对其它路径产生影响   a标签/form表单/img标签
   	- 使用base标签之后. 其余不标签不能以/开头. 否则按照自己的路径跳转.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <base href="/day08_2_url/">
</head>
<body>
   <a href="a/A.html">访问A页面</a>
   <a href="a/b/B.html">访问B页面1</a>
   <a href="/a/b/B.html">访问B页面2</a> <!--错误写法-->
</body>
</html>
```

# 关于IDEA 编译问题说明

​	![image-20220812145542329](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220812145542329.png)

该问题是IDEA编辑web工程中的常见现象. 由于没有及时编译 导致文件不正确. 一般将out目录中文件删除,之后重启即可.

# JDBC操作数据库的案例练习

- 导入数据库

  - 课前资料地址 :   0数据库资料  atweb.sql
  - 导入数据库

  ![image-20220812163348380](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220812163348380.png)



- 课堂案例练习

```java
package com.atguigu.test;

import com.alibaba.druid.pool.DruidDataSourceFactory;
import com.atguigu.bean.User;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.junit.Test;

import javax.sql.DataSource;
import java.io.IOException;
import java.io.InputStream;
import java.util.List;
import java.util.Properties;

/**
 * @author 刘昱江
 * @className JDBCTest
 * @description TODO
 * @date 2022/4/19 18:12
 */
public class JDBCTest {

    public DataSource getDataSource() throws Exception {
        //1.创建配置对象
        Properties properties = new Properties();
        InputStream inputStream =
                JDBCTest.class.getClassLoader().getResourceAsStream("druid.properties");
        properties.load(inputStream);
       return DruidDataSourceFactory.createDataSource(properties);
    }

    /**
     * 查询id=13的数据
     */
    @Test
    public void testSelectById() throws Exception {
        QueryRunner queryRunner = new QueryRunner(getDataSource());
        String sql = "select * from demo_user where id=?";
        /*结果为一时使用*/
        BeanHandler<User> beanHandler = new BeanHandler<>(User.class);
        User user = queryRunner.query(sql, beanHandler,13);
        System.out.println(user);
    }

    /**
     * 想查询所有的数据信息
     */
    @Test
    public void testSelect() throws Exception {
        QueryRunner queryRunner = new QueryRunner(getDataSource());
        String sql = "select * from demo_user";
        /*封装的数据有多条 则使用BeanListHandler*/
        BeanListHandler<User> listHandler = new BeanListHandler<>(User.class);
        List<User> list = queryRunner.query(sql, listHandler);
        list.forEach(System.out::println);
    }

    /**
     * 新增用户
     */
    @Test
    public void testInsert() throws Exception {
        QueryRunner queryRunner = new QueryRunner(getDataSource());
        String sql = "insert into demo_user values(null,?,?,?)";
        int rows = queryRunner.update(sql,"星期五",20,"男");
        if(rows>0)
            System.out.println("执行成功!!!!");
        else
            System.out.println("新增失败!!!!");
    }

    /**
     * 将id=6164的数据改为 name="星期6"  age=3000  sex="其它"
     */
    @Test
    public void testUpdate() throws Exception {
        QueryRunner queryRunner = new QueryRunner(getDataSource());
        String sql = "update demo_user set name=?,age=?, sex=? where id=?";
        queryRunner.update(sql,"星期6",3000,"其它",6164);
        System.out.println("执行成功!!!");
    }

    //删除案例自己测试
}

```









