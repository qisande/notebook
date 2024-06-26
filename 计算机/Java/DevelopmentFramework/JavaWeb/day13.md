# Vue.js和Axios的整合案例(异步调用的模版)

- 客户端和服务器端 如何保证传递数据的一致性  高效的沟通
  - 例子:  购物时 买的东西通常都需要一个包裹.其中为了方便运输.但是其中的内容是什么  只有拆开包裹才能看到.
  - 思考: 如何实现客户端和服务器 数据传递   高效  安全
    - 为了保证客户端和服务器进行有效的沟通 ==首选JSON==
    - 为了保证每次数据返回时格式都是固定的("包裹") 定义了系统统一返回值对象SysResult对象.
  - 构建SysResult对象
    - 属性  flag   布尔类型     true 表示业务执行成功   false 表示业务执行失败.
    - 属性 msg    String 类型   返回服务器的提示信息.
    - 属性 data    Object类型   服务器返回的业务数据
- 创建SysResult对象

```java
package com.atguigu.vo;

import java.io.Serializable;

/**
 * @author 刘昱江
 * @className SysResult
 * @description TODO
 * @date 2022/8/18 10:32
 */
public class SysResult implements Serializable {

    private Boolean flag;
    private String msg;
    private Object data;

    /**
     * 重载的注意事项:  参数不要出现耦合 否则必定报错  一般通过多个参数进行区分.
     */
    public static SysResult success(String msg,Object data){

        return new SysResult(true,msg,data);
    }

    public static SysResult success(Object data){

        return new SysResult(true,"业务调用成功",data);
    }

    public static SysResult success(){

        return new SysResult(true,"业务调用成功",null);
    }

    public static SysResult fail(){

        return new SysResult(false,"业务调用失败",null);
    }



    public Boolean getFlag() {
        return flag;
    }

    public void setFlag(Boolean flag) {
        this.flag = flag;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    public Object getData() {
        return data;
    }

    public void setData(Object data) {
        this.data = data;
    }

    public SysResult() {

    }

    public SysResult(Boolean flag, String msg, Object data) {
        this.flag = flag;
        this.msg = msg;
        this.data = data;
    }
}

```

- 客户端和服务器数据交互的格式JSON的实现
  - 为了实现数据的交互采用SysResult对象进行数据封装.
  - 为了交互的高效 采用JSON的结构封装数据.
  - 知识点:   
    - JS 如何转化JSON    
      - JSON.stringify(user)   将js对象转化为JSON
      - JSON.parse(json1)       将JSON串转化为JS对象
    - Java如何转化JSON
      - mapper.writeValueAsString(user);    将js对象转化为JSON
      - mapper.readValue(json,User.class);  将json转化为对象

![image-20220818104917776](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220818104917776.png)

- Java实现JSON的转化     使用sysResult是为了统一数据
  - 利用SpringMVC中提供工具API实现  jackson   ObjectMapper
  - JSON转化的规则: 是根据对象中的Get方法获取的属性和属性的值
    - 获取方法名 getJava() ---- 去除get----首字母小写---java充当key,  该方法的返回值 充当value    {"java": "xxxxxxx"}
    - 注意事项:  JSON转化时 只有Get方法有关和属性无直接关系. 
    - 有时业务复杂时 需要单独的get方法 支持!!!!!!

- 案例实现-用户列表展现

  - 利用生命周期函数 触发查询函数
  - 准备查询函数 findUserList   发起Ajax请求异步获取 list集合数据
  - 后端服务器接收用户额findUserList请求.查询数据获取数据信息. 封装到SysResult对象中. 之后再将对象转化为JSON. 之后通过Resp对象返回.
  - 通过回调函数通知js数据返回. 根据返回值的状态  执行列表展现




# 书城项目-用户唯一性校验

- 书城项目改造
  - 导入vo对象SysResult

![image-20220818150519974](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220818150519974.png)

					- - 导入JSON jar包

![image-20220818150619222](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220818150619222.png)

- 	- 导入axios.js

![image-20220818151714179](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220818151714179.png)

​		如果导入静态资源文件,则需要删除out目录.重新发布

- 用户唯一性校验

  - 业务说明: 当用户添加完成用户名之后,应该校验用户名是否存在.应该发起异步请求.校验数据 如果数据库中没有该用户名则 √,否则应该提示用户用户名已存在,请更换!!!

  ![image-20220818151045010](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220818151045010.png)

  

  - 实现思路:
    - 用户添加完成数据之后,通过事件触发
    - 校验用户填写的数据是否有效  正则表达式判断
    - 通过异步方式发起ajax请求校验用户是否存在
      - true   用户可以使用    √    标识符 为1
      - false  用户名已存在    提示用户  标识符 为0

  

  

  - 验证码功能实现

    - Kaptcha是一个基于SimpleCaptcha的验证码开源项目,主要生成验证码的.

    - 使用步骤:

      - 导入jar包

      ![image-20220818155118613](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220818155118613.png)

      - 配置servlet

      ```xml
      <!--准备验证码功能 Servlet-->
          <servlet>
              <servlet-name>KaptchaServlet</servlet-name>
              <servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>
          </servlet>
          
          <servlet-mapping>
              <servlet-name>KaptchaServlet</servlet-name>
              <url-pattern>/kaptcha</url-pattern>
          </servlet-mapping>
          
      ```

      - 通过url地址访问服务器.

    ![image-20220818155328367](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220818155328367.png)

  ​		

  - 验证码功能说明

    - 该功能会在session中保存一个数据.  key=""   value="当前最新的验证码"
    - 之所以使用session 目的就是为了和用户绑定.
    - key源码:

  - ```java
    return this.properties.getProperty("kaptcha.session.key", "KAPTCHA_SESSION_KEY");
    ```

  - 动态获取验证码

  ```java
   //  /user?method=getCode
      public void getCode(HttpServletRequest req, HttpServletResponse resp) throws Exception {
          //如果用户调用了验证码功能,则KaptchaServlet会将验证码 保存到session中
          // key=KAPTCHA_SESSION_KEY
          //可以通过session 获取当前的验证码信息
          String code = (String) req.getSession().getAttribute("KAPTCHA_SESSION_KEY");
          System.out.println("验证码:"+code);
      }
  ```

  

- 用户实现验证码校验
  - 动态生成验证码     /kaptcha
  - 图片如果想要刷新 则动态修改 `<img  src="属性">`
  - 当用户输入验证码之后离焦 触发js函数.  获取用户输入的内容,将数据发送到后台服务器 进行数据校验
  - 后台服务器接收参数之后,从session中拿到真实的验证码,之后对比.
  - true   返回 SysResult.success()
  - false  返回  SysResult.fail()
  - js 根据返回值进行前端数据校验.
    - true   前端显示 √    标识符  1
    - false  提示用户验证码错误  标识符0

















​	











