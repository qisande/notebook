# 后台管理实现

- 界面原型

![image-20220817090130449](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220817090130449.png)

- 图书业务分析

![image-20220817090404838](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220817090404838.png)

- 定义bean对象

![image-20220817090939644](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220817090939644.png)

- 层级代码结构

![image-20220817091414647](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220817091414647.png)

- 图书列表页面跳转

  ![image-20220817092208356](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220817092208356.png)



- 图书列表展现步骤

  - 点击图书管理时 发起新的请求 路径如上图.
  - 准备一个新的BookServlet接收用户的请求
  - 查询所有的图书信息,并且将list集合 保存到Request域中.
  - 当业务执行成功之后转发到新页面 book_manager.html
  - 注意事项:   数据库字段与对象名称不一致 所以查询时使用别名

  ![image-20220817094120821](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220817094120821.png)

  

- 图书新增页面跳转
  - 当用户点击图书新增时,应该跳转到图书新增的页面

```html
<a href="book?method=toAddBook">添加图书</a>


```

![image-20220817101454153](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220817101454153.png)

- 图书新增

  - 用户输入完成数据之后,实现数据提交,其中不要补齐  form表单提交的数据  action/method/name属性

  - 当用户点击提交时 数据提交的地址book?method=addBook  注意提交的方式 post提交

  - 保证每一个name属性的名称 与Book对象的属性一致

  - 接收用户提交的数据 利用对象接收数据.

  - 执行业务操作 实现图书新增入库.

  - 如果入库成功之后,则重定向到图书列表页面

    

- 图书删除操作

  - 当用户点击删除操作时 发起URL请求 

  ```java
  <a th:href="@{/book(method=deleteBook,id=${book.id})}" class="del">删除</a>
  ```

  - Servlet接收用户的请求, 接收id参数.
  - 执行删除的动作  Servlet---Service---Dao
  - 删除成功之后 页面重定向到列表页面

  

- 图书修改操作---数据回显

  - 当用户点击修改操作时,需要传递当前图书的ID号
  - 根据id查询图书信息.
  - 根据Id查询的图书保存到request域中.为修改数据回显做准备
  - 当用户查询结束之后,应该转发到图书修改页面中.
  - 根据模版引擎的语法 动态实现图书数据的回显功能.

- 图书修改操作---提交数据
  - 当用户点击提交时,将form表单的数据提交到后台服务器.
  - url:  book?method=updateBook     method=post 提交
  - 接收用户提交的数据 利用对象接收
  - 之后完成图书的修改操作
  - 如果操作成功之后 ,需要将页面重定向到列表页面

- 公共标签引入

  ![image-20220817140556565](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220817140556565.png)

  图书页面中都会有 图书管理/订单管理等操作,说可以使用标签进行统一定义,之后引入即可.

  - 定义公共标签

  ![image-20220817141100656](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220817141100656.png)

  - 引入标签

  ```html
  <div th:replace="manager/manager :: head"></div>
  ```

  - 修改的页面名称: book_manager.html/book_add.html/book_edit.html
  - 修改图书管理的路径

  ![image-20220817141656683](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220817141656683.png)



- 系统首页展现图书列表数据

  - 当用户访问首页时 默认发起/index.html的请求
  - 访问的IndexServlet, 在该Servlet中实现图书列表的查询
  - 将查询到的数据 保存到request域中.
  - 实现转发到index.html页面中.
  - 之后利用模版引擎的循环遍历操作 遍历列表数据 展现图书信息.

  ![image-20220817143016001](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220817143016001.png)



# Ajax异步调用(重点!!!!!!!!!!)

- 同步和异步问题

  - 同步:  多个任务串行操作,如果前边的任务没有完成,则不能执行后边的任务.       如果业务没有处理完成,则用户看不到完整页面. 
  - 异步:  多个任务并行 相互之间互不干扰.
  - 生活中的案例:   点外卖异步    /处对象(正常的同步|海王异步)
  - ==Ajax特点:     局部刷新   异步访问==       它是实现异步的方式!!!!!

  - Ajax调用时为什么可以异步---异步核心原理

  ![image-20220817154005849](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220817154005849.png)



- Axios介绍
  - 而Axios就是目前最流行的前端Ajax框架。
  - Axios 是一个基于 promise 的 HTTP 库
  - Axios可以实现JSON的转化
- Axios入门案例
  - 导入js类库
  - 实例化Axios对象
  - 发起http请求
  - 接收响应信息---回调函数
  - 异常处理方式
  - 编辑Ajax页面js

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/axios.js"></script>
    <script>
        /**
         * 写法1: 常规写法
         */
        axios({
            url: "axiosServlet",
            method: "get",      //get/post请求类型
            params: {
                id:  1001,
                name: "tomcat猫"
            }
        }).then(function (promise) {   //promise是服务器返回数据的封装对象
            console.log(promise)
            console.log("状态码:"+promise.status)
            console.log("响应数据:"+promise.data)
        }).catch(function (error){
            console.log(error)
        })

    </script>
</head>
<body>
  <h1>Ajax课堂案例-----必会内容!!!!!</h1>
</body>
</html>
```

- 编辑后端Servlet

```java
/**
 * @author 刘昱江
 * @className AxiosServlet
 * @description TODO
 * @date 2022/8/17 16:01
 */
@WebServlet("/axiosServlet")
public class AxiosServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }

    //http://localhost:8080/ajax/axiosServlet?id=1001&name=tomcat%E7%8C%AB

    /**
     * 响应数据的方式3种:
     *  1.转发:  页面
     *  2.重定向: 页面
     *  3.响应字符串数据:  resp.getWriter().write("");
     * @param req
     * @param resp
     * @throws ServletException
     * @throws IOException
     */
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("utf-8");
        String id = req.getParameter("id");
        String name = req.getParameter("name");
        System.out.println("服务器接收客户端数据:"+id+":::"+name);
        String msg = "业务服务器处理完成,响应客户端!!!";
        resp.setContentType("text/html;charset=utf-8");
        resp.getWriter().write(msg);
    }
}
```

- Axios 优化1--- 优化的请求的方式

```javascript
/**
         * 优化写法1: 简化请求方式
         *  axios.post()  //SpringMVC时
         */
        //axios.get("url地址","提交的参数")
        axios.get("axiosServlet",{params: {id: 1002,name: "jerry"}})
             .then(function (promise) {
                 console.log(promise.data)
             })
```

- Axios优化-2    简化Get类型请求方式

```javascript
 /**
         * 优化策略2:  简化get请求参数
         */
        axios.get("axiosServlet?id=1003&name=tomcat")
             .then(function (promise) {
                 console.log(promise.data)
             })
```

- Axios优化-3  优化匿名函数

```javascript
/**
  * axios优化策略3  优化匿名函数
*/
axios.get("axiosServlet?id=1005&name=张伟")
    .then((promise) => {
    console.log(promise.data)
})
```

- Axios优化-4 优化回调函数    async await

```javascript
/**
         * 最终版本1 Axios优化策略4  async　await优化
         *  需求: 通过一行调用获取服务器返回值
         *  建议使用:  async 标识函数  await 标识ajax请求
         */
        async function getMsg(){
            let promise = await axios.get("axiosServlet?id=1006&name=大张伟!!!")
            console.log(promise.data)
        }
        getMsg()
```

- Axios优化-5  优化取值方式

```javascript
 /**
         * 最终版本2  如果想直接获取服务器数据 则建议使用如下操作
         */
        async function getSuperMsg(){
           //let {status: statusCode,data: result}  =  let promise = await axios.get("axiosServlet?id=1006&name=大张伟!!!")
           let {status: statusCode,data: result} = await axios.get("axiosServlet?id=1006&name=大张伟!!!")
           console.log("终极状态码:"+statusCode+"|"+"终极数据:"+result)
        }
        getSuperMsg()
```















