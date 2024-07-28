# 1. Vue生命周期(理解-以后掌握)

- 生命周期函数的作用   主要记录/监控/改变 vue对象中在各个时期的状态.

- 生命周期函数 是有程序自动调用. 和程序员无关.

- 生命周期函数 名称是固定的 不能随意乱写

- 生命周期状态

  - 初始化状态
    - beforeCreate   vue对象创建前调用
    - ==created==           vue对象创建成功之后调用
  - 挂载状态
    - beforeMount    挂载数据之前执行.
    - ==mounted==           数据挂载后执行   标志着用户可以看到完整页面
  - 修改状态
    - beforeUpdate   函数在数据修改之前执行
    - updated             函数在数据修改之后执行
  - 销毁状态
    - beforeDestroy   vue对象在销毁前执行的函数
    - destroyed          vue对象销毁 

  

  ```javascript
  <!DOCTYPE html>
  <html>
  	<head>
  		<meta charset="utf-8">
  		<title>测试vue生命周期函数</title>
  	</head>
  	<body>
  		
  		<div id="app">
  			<h3 v-text="msg"></h3>
  			<button @click="destroy">销毁</button>
  		</div>
  		
  		<!--引入js函数类库  -->
  		<script src="js/vue.min.js"></script>
  		<script>
  			const app = new Vue({
  				el : "#app",
  				data : {
  					msg: "vue生命周期"
  				},
  				
  				//在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。
  				beforeCreate(){
  					console.log("beforeCreate")
  				},
  				//在实例创建完成后被立即调用
  				created(){
  					console.log("created")
  				},
  				//在挂载开始之前被调用：相关的 render 函数首次被调用。
  				beforeMount(){
  					console.log("beforeMount")
  				},
  				//实例被挂载后调用，这时 el 被新创建的 vm.$el 替换了。
  				mounted(){
  					console.log("mounted")	
  				},
  				//数据更新时调用，发生在虚拟 DOM 打补丁之前
  				beforeUpdate(){
  					console.log("beforeUpdate")
  				},
  				//由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。
  				updated(){
  					console.log("updated")
  				},
  				//实例销毁之前调用。在这一步，实例仍然完全可用
  				beforeDestroy(){
  					console.log("beforeDestroy")	
  				},
  				//实例销毁后调用。
  				destroyed(){
  					console.log("destroyed")
  				},
  				methods:{
  					destroy(){
  						this.$destroy()
  					}
  				}
  			})
  		</script>
  	</body>
  </html>
  
  ```

  # 数组用法

  ![image-20220809095243752](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220809095243752.png)

  # Vue 案例练习

  - 案例需求
    - 准备一个表格
    - 在表格中展现用户列表信息
    - 要求展现 用户的ID号/姓名/年龄/性别/修改/删除
    - 要求准备一个新增表格 要求 当用户点击新增按钮时 实现表格数据的新增
    - 如果用户点击修改操作,则要求实现修改数据的回显.并且点击提交时实现数据修改.
    - 如果用户点击删除按钮 则要求删除当前行数据.

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--1.导入函数类库-->
    <script src="js/vue.min.js"></script>
</head>
<body>
    <div id="app">
        <h3 align="center">动态表格</h3>
        <table id="tab1" width="65%" border="1px" cellspacing="0" cellpadding="0" align="center">
            <tr>
                <th>序号</th>
                <th>用户</th>
                <th>性别</th>
                <th>操作</th>
            </tr>
            <tr align="center">
                <td colspan="4" v-show="userList.length === 0">
                    <h3>集合中没有数据,请新增</h3>
                </td>
            </tr>
            <tr align="center" v-for="(user,index) in userList">
                <td v-text="user.id">100</td>
                <td v-text="user.name">张三</td>
                <td v-text="user.sex">男</td>
                <td>
                    <button @click="updateUserMethod(user,index)">修改</button>
                    <button @click="deleteUserMethod(index)">删除</button>
                </td>
            </tr>
        </table>

        <h3 align="center">新增用户</h3>
        <table width="25%" border="1px" cellspacing="0" cellpadding="0" align="center">
            <tr align="center">
                <td>序号</td>
                <td>
                    <input  name="id" type="text" v-model="addUser.id">
                </td>
            </tr>
            <tr align="center">
                <td>姓名</td>
                <td>
                    <input  name="name" type="text" v-model="addUser.name">
                </td>
            </tr>
            <tr align="center">
                <td>性别</td>
                <td>
                    <input name="sex" type="text" v-model="addUser.sex">
                </td>
            </tr>
            <tr align="center">
                <td colspan="2">
                    <button @click="addUserMethod">添加用户</button>
                </td>
            </tr>
        </table>

        <h3 align="center">修改用户</h3>
        <table width="25%" border="1px" cellspacing="0" cellpadding="0" align="center">
            <tr align="center">
                <td>序号</td>
                <td>
                    <input name="id" type="text" v-model="updateUser.id">
                </td>
            </tr>
            <tr align="center">
                <td>姓名</td>
                <td>
                    <input  name="name" type="text" v-model="updateUser.name">
                </td>
            </tr>
            <tr align="center">
                <td>性别</td>
                <td>
                    <input  name="sex" type="text" v-model="updateUser.sex">
                </td>
            </tr>
            <tr align="center">
                <td colspan="2">
                    <button @click="updateSubmit">修改用户</button>
                </td>
            </tr>
        </table>
    </div>
    <script type="text/javascript">


        const app = new Vue({
            el: "#app",
            data: {
                //1.定义list列表数据
                userList: [],
                //2.定义user对象
                addUser: {
                    id: "",
                    name: "",
                    sex: ""
                },
                updateUser: {
                    id: "",
                    name: "",
                    sex: "",
                    //为了根据下标进行修改操作 ,可以提前定义下标的属性
                    index: 0
                }
            },
            methods: {
                addUserMethod(){
                    //实现数据新增
                    this.userList.push(this.addUser)
                    //新增之后将原始数据 清空 v-model绑定的对象中没有属性时会自动创建属性
                    this.addUser = {}
                },
                updateUserMethod(user,index){
                    //console.log(user)
                    //console.log(index)
                    this.updateUser = user
                    this.updateUser.index = index
                },
                updateSubmit(){
                    //数据的修改操作  1.修改后的user对象  2.修改数据的下标
                    let index = this.updateUser.index
                    //修改数组中的数据
                    this.userList.splice(index,1,this.updateUser)
                    //console.log(this.userList)
                    //清空修改操作
                    this.updateUser = {}
                    console.log(this.userList)
                },
                deleteUserMethod(index){
                    this.userList.splice(index,1)
                },
                init(){
                    //初始化数据  未来的数据来源 从数据库中获取
                    let userList = [{id: 1001,name:"安琪拉",sex:"女"},{id: 1002,name:"王昭君",sex:"女"},{id: 1003,name:"貂蝉",sex:"女"}]
                    this.userList = userList
                }
            },
            created(){  //程序自动完成的调用  利用生命周期函数用法 自动完成业务调用
                this.init()
            }
        })
    </script>
</body>
</html>
```

# 用户登录数据校验

- 将页面改造为vue.js的页面结构
- 看到用户名和密码框使用双向数据绑定
- 根据用户输入的内容动态提示用户.
- 如果用户名或密码没有输入则不允许用户登录跳转.
- 具体代码参数课堂案例

![image-20220809142842730](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220809142842730.png)

# 用户注册数据校验

- 将页面改造为vue.js的结构
- 使用双向数据绑定 封装数据
- 用户添加完成之后开始校验  使用正则表达式校验数据的有效性
- 如果校验选项都正确,则跳转页面 否则阻止默认行为.



# 项目发布流程

![image-20220809155107966](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220809155107966.png)

	- 静态资源一般需要部署到tomcat服务器内部
	- tomcat服务器一般部署到Linux系统的服务器中.
	- 而一般服务器内部需要大量的修改操作 建议使用**配置文件**的方式进行管理

# 配置文件(了解)

- 常见配置文件
  - properties     jdbc.properties  常用的
  - xml文件         是tomcat服务器中使用的配置文件
  - yaml文件        SpringBoot框架中使用的配置文件  xxx.yml
  - json                 一般用于前后端数据交互

- properties语法介绍
  - 数据结构:   key=value
  - 数据类型:   都是String类型   无需手动添加引号

```properties
atguigu.jdbc.url=jdbc:mysql://192.168.198.100:3306/bj1026
atguigu.jdbc.driver=com.mysql.cj.jdbc.Driver
atguigu.jdbc.username=root
atguigu.jdbc.password=root
```

- XML介绍

  - XML是eXtensible Markup Language，翻译过来就是==**可扩展标记语言**==。所以很明显，XML和HTML一样都是标记语言，也就是说它们的基本语法都是标签。

  - 针对于xml配置文件  用途 好多框架都支持xml的配置语法. 只不过一套框架一个配置文件格式.

  - 例子:   Mybatis框架  xml配置文件     mybatis自己的标签

    ​		    Spring框架     xml配置文件    spring自己的标签

    

  - 了解java程序如何读取xml配置文件 为以后学习tomcat服务器做铺垫
    - 导入课前资料的jar包
    - 创建解析器对象
    - 通过反射机制  类加载器读取xml配置文件
    - 通过解析器获取document对象
    - 通过document获取根标签
    - 通过根标签获取子级元素  和 属性值
  - 学习xml解析方式 有什么意义??
    - 掌握tomcat服务器是如何加载server.xml配置文件的.
    - 通过读取配置文件中的内容,将数据封装到指定的对象中 开始运行程序.!!!!
    - 通过将数据写入配置文件中 ==**可以实现代码和业务的解耦!!!!**==

```java
package com.atguigu.xml;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.io.InputStream;
import java.util.List;

/**
 * @author 刘昱江
 * @className TestXml
 * @description TODO
 * @date 2022/8/9 16:26
 */
public class TestXml {

    public static void main(String[] args) throws DocumentException {
        //1.创建解析器对象
        SAXReader saxReader = new SAXReader();
        //2.获取配置文件的IO流信息
        InputStream inputStream =
                TestXml.class.getClassLoader().getResourceAsStream("user.xml");
        //3.获取文档对象模型
        Document document = saxReader.read(inputStream);
        //4.获取根标签
        Element rootElement = document.getRootElement();

        //5.获取所有的子级标签
        List<Element> elements = rootElement.elements();
        //System.out.println("子元素数量:"+elements.size());

        //6.获取子标签的内容
        for(Element element : elements){
            //7.获取元素的属性值
            String id = element.attributeValue("id");
            Element idEle = element.element("id");
            String userId = idEle.getText();
            Element nameEle = element.element("name");
            String userName = nameEle.getText();
            Element sexEle = element.element("sex");
            String userSex = sexEle.getText();
            System.out.println("子元素的属性:"+id+"|Id号:"+userId+"|name:"+userName+"|性别:"+userSex);

            /**
             * 根据xml配置文件 读取其中的信息 则可以按照自己的规则实现数据封装,之后按照特定的业务
             * 逻辑运行!!!!
             */
            User user = new User(userId,userName,userSex); //模拟赋值操作
            String name = user.getName();                  //模拟tomcat运行
            System.out.println("从业务调用的流程获取数据:"+name);
        }
    }

    static class User{
        private String id;
        private String name;
        private String sex;
        public User(){

        }
        public User(String id, String name, String sex) {
            this.id = id;
            this.name = name;
            this.sex = sex;
        }

        public String getName(){
            return this.name;
        }
    }
}

```









