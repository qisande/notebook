# 1.表格用法

- 表格标签   table
- border属性: 边框线
- cellpadding   单元格的内填充
- cellspacing     单元格和单元格之间的距离
- tr标签      定义表格的行
- th标签     定义单元格  加粗和居中的效果  表头元素定义
- td标签      定义普通的单元格.
- colspan  跨列  向右扩展    
- rowspan 跨行  向下扩展   注意事项: 会对原始的表格产生影响 需要提前删除多余的标签


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <!--

    -->
    <table border="1px" cellpadding="0" cellspacing="0" width="80%" align="center">
        <tr >
            <th colspan="4">
                <h1>惹事精</h1>
            </th>
        </tr>
        <tr>
            <th>序号</th>
            <!--<th rowspan="3">序号</th>-->
            <th>ID号</th>
            <th>名称</th>
            <th>年龄</th>
        </tr>
        <tr align="center">
            <td>1</td>
            <td>1001</td>
            <td>老妖婆</td>
            <td>81</td>
        </tr>
        <tr align="center">
            <td>2</td>
            <td>1002</td>
            <td>老普</td>
            <td>79</td>
        </tr>
        <tr align="center">
            <td>3</td>
            <td>1002</td>
            <td>老拜</td>
            <td>99</td>
        </tr>
    </table>
</body>
</html>
```

# 2.表单标签

- 一般向服务器提交数据,首选表单  注册/登录
- form表单的基本结构
  - form标签
  - action属性:   数据提交的地址  可以是本地服务器,可以是远程服务器.
  - method属性:  标识提交方式
    - GET提交       url?name=value&age=value2&sex=女
    - POST提交     URL  数据通过请求头的方式进行携带. 后续介绍
  - 提交按钮:  属性  type="submit"  设定唯一的提交按钮.

```html
<form action="服务器地址" method="post">
        用户名: <input type="text" name="name">
        <button type="reset">重置</button>
        <button type="submit">提交</button>
    </form>
```

form表单中常用提交标签:

	- 文本输入框   

```html
用户名: <input type="text" name="name" value="admin">
```

	- 密码输入框

```html
密码:   <input type="password" name="password">
```

- 单选框

```html
 性别:
<!--
关于单选标签说明:
1.单选标签必须写默认值value属性
2.单选操作是 name属性必须相同 才能互斥
3.label标签 通过点击文字关联单选框  for="id值"
4.checked 默认选中
-->
<input type="radio" name="gender" value="男" id="man">
<label for="man">男</label>
<input type="radio" name="gender" value="女" id="woman" checked>
<label for="woman">女</label><br>
```

- 复选框(多选框)

```html
爱好:
<!--
type="checkbox" 复选框
name属性:  要求相同
value:  标识当前选项的值
提交方式:    key=value1&key=value2&key=value3 其中key相同
-->
<input type="checkbox" name="hobby" value="study">学习
<input type="checkbox" name="hobby" value="wan" checked>旷课
<input type="checkbox" name="hobby" value="game">打游戏
<input type="checkbox" name="hobby" value="rap">rap
```

- 下拉框  select标签   name属性    配合  option标签  value属性

```html
所在城市:
                <select name="city">
                    <option value="">---请选择---</option>
                    <option value="上海">上海</option>
                    <option value="成都">成都</option>
                    <option value="台湾" selected>台湾</option>
                </select>
```



- 日期框

```html
 生日:
<!--由于各个浏览器的内核不一致所以解析的插件效果也不相同.一般引入第三方的时间插件-->
<input type="date" name="birthday">
```

- 文件框

```html
 图片:
<!--
注意事项:  文件的上传的类型,必须为post 不能为get.
因为:get提交最多允许4k的数据.
-->
<input type="file" name="img">
```

- 文本域  用户如果需要输入大量的文字信息时使用该功能.

```html
 详情信息:
 <textarea name="info" style="width: 300px;height: 300px;"></textarea>
```

- 隐藏域    一般多用于修改操作.  在修改操作中 id主键是唯一标识符.

```html
 <!--根据id修改: 隐藏域的功能-->
<input type="hidden" name="id" value="100">
<!--禁用   当标签禁用时,该属性不能提交数据-->
<input type="text" name="id2" value="100" disabled>
```

- 按钮操作

```html
按钮操作:
<button type="button">普通按钮</button><br>
<input type="button" value="普通按钮2">
```

# CSS样式(能看懂即可)

- CSS 叫做层叠样式表   可以实现页面的美化.

- CSS引入方式

  - 行内样式

  ```html
  <!--1.行内样式-->
      <p style="height: 30px; width: 400px;background-color: seagreen;color: gold">CSS样式测试</p>
  ```

  - 内部样式  如果该样式被多个标签同时使用,则可以采用内部样式的方式定义

  ```css
   <!--如果样式可以被多个标签使用,则样式可以抽取.
          1.type="text/css"  标签的内容,只能写文本和css样式
      -->
      <style type="text/css">
  
          /*内部样式  2.指定哪些标签有效*/
          p {
              height: 30px;
              width: 100px;
             /* font-size: 150px;*/
              font-family: 宋体;
              color: coral;
              background-color: darkgrey;
          }
      </style>
  ```

  

  - 外部样式  如果样式可以修饰多个页面中的多个元素 则使用外部样式

    - 定义css文件  新建p.css文件

    ```css
    #p2 {
        height: 30px;
        width: 100px;
        /* font-size: 150px;*/
        font-family: 宋体;
        color: coral;
        background-color: darkgrey;
    }
    
    .red {
        height: 30px;
        width: 100px;
        /* font-size: 150px;*/
        font-family: 宋体;
        color: coral;
        background-color: red;
    }
    
    p {
        height: 30px;
        width: 100px;
        /* font-size: 150px;*/
        font-family: 宋体;
        color: coral;
        background-color: blue;
    }
    ```

    - 引入外部样式

    ```html
    <!--
            rel="stylesheet" 引入的文件是一个样式表
            href=""          引入资源路径
            type="text/css"  不写则默认
        -->
        <link rel="stylesheet" href="css/p.css" type="text/css">
    ```

- 关于样式优先级的说明

  - 行内样式  >  内部样式   > 外部样式

- 选择器

  - 标签选择器    p {}
  - id选择器         #id{}
  - 类选择器        .class值 {}

  

- 盒子模型属性讲解

  - border div的边框线
  - margin  外边距   div与其它元素的边距   margin不会影响当前div的大小

  ```css
  #div2 {
      width: 100px;
      height: 100px;
      border: 1px solid red;
      background-color: rosybrown;
      /*
      基准点:  左上方是基准点
      外边距 上 右 下 左  分别为20像素  */
      /*margin: 20px 20px 20px 20px;*/
      /*2个参数:   参数1: 上下结构   参数2: 左右结构*/
      /*margin: 20px 20px;*/
      /*1个参数:  上右下左 都是20像素*/
      /*margin: 20px;*/
      margin: auto;    /*水平居中*/
      margin-top: 20px;
  }
  ```

  - padding  内边距    内部元素和border之间的距离. 会影响当前div的大小(慎重)   能用margin不要使用padding

  ```css
   #div2 {
              width: 200px;
              height: 200px;
              border: 1px solid red;
              background-color: rosybrown;
              /*padding: 20px 20px 20px 20px;*/  /*顺时针 20像素*/
              /*padding: 20px 20px;*/  /* 上下  左右20像素*/
              /*padding: 20px;*/
              padding-top: 40px;
          }
  ```

- CSS课堂案例练习

![image-20220803150230235](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220803150230235.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style type="text/css">
        /*说明:如果同时为多个标签指定样式,则使用,号分割*/
        html,body{
            width: 100%;
            height: 100%;
            margin: 0;
            padding: 0;
        }

        #div_parent {
            width: 100%;
            height: 100%;
            background-color: darkgrey;
        }

        #top{
            height: 15%;
            background-color: cyan;
        }

        #left {
            width: 20%;
            height: 85%;
            background-color: seagreen;
            float: left;
        }

        #main {
            height: 75%;
            width: 80%;
            background-color: rosybrown;
            float: left;
        }

        #bottom {
            height: 10%;
            width: 80%;
            background-color: coral;
            float: right;
        }
    </style>

</head>
<body>
    <!--
        说明: 在h5的页面中 外层div使用%的形式不生效,
        如果想要生效则必须指定边界
    -->
    <div id="div_parent">
        <div id="top"></div>
        <div id="left"></div>
        <div id="main"></div>
        <div id="bottom"></div>
    </div>
</body>
</html>
```

# 课堂案例练习题

1. 以下( B)标签用来建立无序列表

   A.<ol></ol>

   B.<ul></ul>

   C.<dI></dl>

   D.<il></il>

2. 表单中的数据要提交到哪里处理由表单的（ C）属性指定?

   A.method   

   B. name  

   C. action  

   D. 以上都不对 

3. 页面中需要增加链接，正确的HTML代码是(A )

   ​    A. `<a href="http://www.baidu.com">百度</a>`

   ​    B. `<a name="http://www.baidu.com">百度</a>`

   ​    C. `<a url="http://www.baidu.com">百度</a>`

   ​    D. `<a>http://www.baidu.com`</a>` 

4.  下面关于HTML描述错误的是(C)

    ​    A. HTML文件必须由<html>开头，</html>标记结束

    ​    B. 文档头信息包含在<head></head>中

    ​    C. 在<head></head>之间可以包含<title>和<body>等信息

    ​    D. 文档体包含在<body>和</body>标记之间

5. html语法中哪条命令用于使一行文本折行，而不是插入一个新的段落？ B

   A.  <td>

   B.  <br>

   C.  <p>

   D.  <h1>

6. html中注释的写法正确的是（A）

   A.  <!-- 这是注释的内容 -->

   B.  //这是注释的内容

   C.  /*这是注释的内容*/

   D.  --这是注释的内容

7. 下列是HTML常用标记(ABC)

   ​	A. <body>

   ​    B. <head>

   ​    C. <html>

   ​    D. <book>

8. 关于HTML语法下列说法错误的是（ABD）

   ​    A. 标签严格区分大小写

   ​    B. 标签可以嵌套且可以交叉嵌套

   ​    C. 属性必须有值，且属性值必须加引号

   ​    D. 注释可以嵌套

# JavaScript学习(难点知识!)

-  JavaScript（简称“JS”） 是解释型轻量级的函数式编程语言。

- JS的作用:  控制页面的动态效果.

- 现在几乎所有的浏览器都支持(兼容)JS 所以和浏览器的版本关系不大(IE除外)

- 入门案例

  - 必须位于script标签内部编辑.
  - script标签 没有位置的要求.  但是一般放到head中 或者body最后
  - alert("xxxx")    有弹出框的效果

- JS引入方式

  - 内部引入     在页面中通过编辑 script标签 执行JS

  - 外部引入

    - 1.创建xxx.js文件   在其中编辑js的内容

    ```javascript
    /*在script标签内部 添加js的内容 */
    alert("你好世界!!!!")
    ```

    - 引入js文件

    ```javascript
    <!--2.外部引入JS-->
    <script type="text/javascript" src="js/hello.js"></script>
    ```

  - 注意事项:   一旦通过src引入js文件,则该标签内部 不能编辑JS

- JS变量声明
  - var关键字     以后不建议使用   没有作用域范围
  - let 关键字     建议使用 有作用域范围  ES6以后推出的.
  - const关键字   定义常量   数据不允许被修改.
  - js严格区分大小写   都使用小写字母.
- JS常见报错
  - not defined     没有声明变量
  - undefine         字符串操作 没有赋初始值
  - NaN                 数值操作 没有赋初始值

- JS中的数据类型
  - number  数值类型       let  num = 100
  - string 数据类型            let  str = "hello"
  - boolean 布尔类型       true/false    可以实现自动的类型转化 
  - 为假的:   0   ""   null  undefined    除此之外都为真

```javascript
//布尔类型测试
//为假的:   0   "" null  undefined 除此之外都为真
let flag = "undefined"
if(flag){
    alert("结果为真")
}else{
    alert("结果为假")
}
```





