# 1. 软件开发的流程

- 需求分析-时间最长的  项目经理-产品经理-CCB
- 组建项目团队    3个开发----1个前端---1UI----2测试----1项目经理----1产品经理----产品助理
  - 3个开发----1前端----1UI---1项目经理
- 产品出原型设计
- web前端构建页面
- UI设计出图
- 后端开发实现业务逻辑
- 测试人员测试.
- 项目发布部署.

开发方式:

​		传统开发:  项目上线不及时. 问题比较多

​		敏捷开发:   需求池.  根据项目紧要程度. 选择开发的内容.

# 2. 自我介绍

名字: 刘昱江
资料地址: 内网通 从磁盘中获取资料
讲义: CSDN 闪耀太阳   专栏: 尚硅谷  
文章位置: https://harrylyj.blog.csdn.net/article/details/123407648
B站地址: 不二子阳  

内网上网账号:   

​	 账号:  jys007   密码: jys007

# 3 知识点讲解

1. WEB  说明:  web 称之为万维网. 
2. web 阶段划分:
   1. web1.0  用户只能单向的接收, 类似于广播收音机
   2. web2.0  用户参与到网络中,可以实时互动.
   3. web3.0  智能设备大量的出现在生活中.  物联网. 
3. javaweb:  使用java技术来解决web中遇到的问题 (javaweb就是技术栈--工具箱)
4. javaweb = web客户端+ web服务器端
5. 软件运行:    程序员写的软件----->软件运行环境 软件服务器(tomcat/weblogic)----- 真实服务器(硬件)
6. web架构设计模式
   1. C/S结构    客户端/服务器       抖音         特点: 需要下载特定的客户端程序			
      1. 需要使用用户本地"算力"解决问题.
      2. 更新需要重新下载.
      3. C/S结构必须要求用户使用特定的设备.  设备关联性强.
   2. B/S结构    浏览器/服务器       京东网站  特点: 不需要安装客户端
      1. B/S结构只需要浏览器 和设备无关.
      2. 对于用户的"算力"要求较低.

# 4. Html知识点讲解

1. html 名称超文本标记语言
2. 页面是由浏览器解析html动态生成.
3. 编辑html的过程,也叫做"画页面"的过程
4. 超文本: 就是一种信息的结构
5. 标记语言:   标签             <p>xxxxxxxx</p>    
   1. 标签是特定的结构,不能自定义
   2. 单标签 不用闭合   双标签: 必须闭合.
   3. 标签可以嵌套.  但是不能交叉嵌套.
   4. 如果标签编辑不正确 浏览器显示不正确 .可能也不报错.   最好边写边测
6.  html标签的基本代码结构 

```html
<!--
    说明: DOCTYPE 文档类型  浏览器解析当前标签时,采用html的方式解析页面.
         如果添加该标签 则默认以html5的方式解析(主流).
         否则以html4的方式解析
-->
<!DOCTYPE html>
<!--
    html是该文件的根标签 必须唯一,以后所有的html标签,都必须位于该标签之内.
    lang="en" 该标签主要是告诉浏览器,我是一个英文页面.
    该标记主要被搜索引擎记录. 和编辑html的文字无关.
-->
<html lang="zh">

    <!--head头部标签
        该标签主要的作用 标记引入的资源.定义标签的基本信息 主要起定义的作用
        例如:
            1.CSS样式表
            2.javaScript js
            3.页面的一些设定
    -->
    <head>
        <!-- 指定浏览器解析该页面的字符集编码格式 -->
        <meta charset="UTF-8">
        <!--浏览器页夹的名称-->
        <title>Title</title>
        <!--标记搜索引擎检索该网站的详情信息-->
        <meta name="description" content="京东JD.COM-专业的综合网上购物商城，为您提供正品低价的购物选择、优质便捷的服务体验。商品来自全球数十万品牌商家，囊括家电、手机、电脑、服装、居家、母婴、美妆、个护、食品、生鲜等丰富品类，满足各种购物需求。"><meta name="description" content="京东JD.COM-专业的综合网上购物商城，为您提供正品低价的购物选择、优质便捷的服务体验。商品来自全球数十万品牌商家，囊括家电、手机、电脑、服装、居家、母婴、美妆、个护、食品、生鲜等丰富品类，满足各种购物需求。"><meta name="description" content="京东JD.COM-专业的综合网上购物商城，为您提供正品低价的购物选择、优质便捷的服务体验。商品来自全球数十万品牌商家，囊括家电、手机、电脑、服装、居家、母婴、美妆、个护、食品、生鲜等丰富品类，满足各种购物需求。">
        <!--标记搜索引擎通过哪些关键字检索该网站-->
        <meta name="Keywords" content="网上购物,网上商城,家电,手机,电脑,服装,居家,母婴,美妆,个护,食品,生鲜,京东">
    </head>
    <!--body标签是页面中的主要的内容.以后所有的标签都必须位于body之内-->
    <body>
        <h1>我是html的入门案例</h1>
    </body>
</html>
```

7. html的打开方式       
   1. IDEA通过服务器方式打开
   2. 通过浏览器直接打开



# 5. Html常用标签

1. 标题标签    h1-h6   字体加粗   依次减小.    
   1. 特点:  h1标签一个页面中最好只有1个.
2. 段落标签    p   独占一行  行前行末 都会有边距.       古诗词/小说/等场景.
3. 换行标签/空格标签

```html
<!--换行标签用法
            规则: 在页面中手动的敲击空格,浏览器不解析的. 没有效果
            空格:  &nbsp;
            换行:  <br>
         -->
        <p>好好学习,&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>天天向上</p>
```

4. 注释写法:    <!--  xxxxxxx内容   -->

5. 有序列表和无序列表

   1. ol-li    type="排序编号" start="起始位置"   试题/需要排序的内容
   2. ul-li    type="了解一般不用"     标题展现时/商品信息展现

   ```html
    <!--有序标签-->
          <ol type="A" start="4">
              <li>第一</li>
              <li>第二</li>
              <li>第三</li>
          </ol>
   
           <!--无序列表
                 type: disc 黑色圆圈
                 circle 空心圆圈
                 square 黑心方块
           -->
           <ul type="square">
               <li>张三</li>
               <li>李四</li>
               <li>王五</li>
           </ul>
   ```

	6. 关于绝对路径和相对路径的说明``

```html
<!--
1.绝对路径:  以磁盘路径为起始的文件路径
F:\student_workspace2\workspa\day01\day1_3_超链接标签.html
2.相对路径:  是以当前文件为起始的路径信息.(以文件名开头的路径)
写法1:  aa/bb/test.html
写法2:  ./aa/bb/test.html   ./代表当前目录
-->
```

7. a标签用法(重点!!!)
   1. href属性:  指定跳转的资源     
      1. 本资源       绝对路径    相对路径
      2. 网络资源:   http://xxxx.xx.xxx  
   2. target属性:    默认 _self     _blank

```html
<!DOCTYPE html>
<html lang="zh">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <!--
            1.绝对路径:  以磁盘路径为起始的文件路径
                F:\student_workspace2\workspa\day01\day1_3_超链接标签.html
            2.相对路径:  是以当前文件为起始的路径信息.(以文件名开头的路径)
                写法1:  aa/bb/test.html
                写法2:  ./aa/bb/test.html   ./代表当前目录
                上一级目录: ../xxx/xxx/xx.html  ../上一级
                上二级目录: ../../xxx/xx/xx.html   ../../上2级
         -->

        <!--超链接标签   a标签
            href="表示页面跳转"  当用户点击标签时,浏览器会重新加载新的页面
        -->

        <!--1.绝对路径写法
            1.1 IDEA如果使用绝对路径时,不能直接跳转. 因为和IDEA打开页面的方式有关.
            因为IDEA启动的服务器的方式,打开的页面.服务器不能找到F盘....的路径
            1.2 采用浏览器直接打开页面的方式 运行即可.
        -->
        <a href="F:\student_workspace2\workspa\day01\day1_1_入门案例.html">绝对路径</a>

        <!--2.相对路径写法-->
        <a href="day1_1_入门案例.html">相对路径1</a>
        <a href="./day1_1_入门案例.html">相对路径2</a>
        <a href=".\day1_1_入门案例.html">相对路径3</a>

        <hr>
        <!--3.跳转到其它网络资源-->
        <a href="http://www.baidu.com">百度</a>
        <a href="http://www.jd.com">京东</a>
        <a href="http://www.jd.com">性感图片</a>
        <hr>
        <!--4.新页面的打开方式
            target="_self" 新页面在当前页打开  默认值
            target="_blank"  在新窗口打开
            target="_parent"  在框架中的父级页面中展现
            target="_top"     在框架中的最外层页面中展现
        -->
        <a href="http://www.jd.com" target="_top">京东</a>
        
    </body>
</html>
```

![image-20220802151838744](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220802151838744.png)

8. 图片标签    img   src="资源路径  本地资源/网络资源"    width/height  单位:像素 px

```html
 <img src="./image/1.jpg" width="300px" height="300px" alt="如果图片加载异常时,以文字的方式代替">
        <img src="https://img14.360buyimg.com/n0/jfs/t1/156859/7/16544/128222/6052b37eE5c0b191d/9e8d3a6939a65041.jpg.avif" width="300px" height="300px">

```

	9. 音频/==视频介绍==

```html
<audio controls="controls">
    <source src="image/迎着风.mp3" type="audio/ogg">
</audio>
<br>
<video width="320" height="240" controls="controls">
    <source src="image/苹果.mp4" type="video/mp4" />
</video>
```

10. div(重点!!!)

    1. div不是展现页面内容的,而是定义页面布局的.

    2. div是块级标签,也把div称之为盒子模型.   可以进行嵌套,

    3. 浏览器解析html 是解释执行  按照行逐条解析,逐条展现.

    4. 块级标签:   ==**独占一行   前后回车换行**==    

       如果设定了大小,则大小固定的. 

       如果没有设定大小则以其中包含的内容决定.

    5. 行级标签:   ==不会独占一行   向左看齐      标签的大小由内容决定.==

    6. div默认独占一行,向左看齐. 如果需要并列div则必须设置浮动效果 float

       1. 如果前一个div没有浮动,则独占一行.  之后的div设定了浮动,也是在另一行展现.
       2. 如果前一个div设置了浮动效果,而后一个div没有设置浮动.则必然出现覆盖的现象, 浮动的覆盖未浮动的.
       3. 如果多个div都设置了浮动效果. 则并排展现.

       ![image-20220802165540732](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220802165540732.png)

       

    ```html
     <div style="height: 400px; width: 900px; background-color: darkgrey">
            <div style="height: 400px;width: 300px; background-color: cyan;float: left"></div>
            <div style="height: 400px;width: 300px; background-color: coral;float: left"></div>
            <div style="height: 400px;width: 300px; background-color: seagreen;float: left"></div>
        </div>
    ```

    效果展现:

    ![image-20220802165601369](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220802165601369.png)

    

11.  字符实体  有些html标签 在页面中不能直接编辑.必须使用转义标签代替.则该标签叫做字符实体.

```html
<h3> 请问下列是标题标签的是?</h3>
    <ol type="A">
        <li>&lt;h1&gt;xxxxxx&lt;/h1&gt;</li>
        <li>&lt;p&gt;xxxxxx&lt;/p&gt;</li>
        <li>&lt;div&gt;xxxxxx&lt;/div&gt;</li>
    </ol>
```

![08dd137878b945aba8bddf401eebe154](https://images73.oss-cn-beijing.aliyuncs.com/img/08dd137878b945aba8bddf401eebe154.png)

# 作业

1. 通过超链接 图片标签  和视频标签用法   制作自己的家庭影院.
2. 预习: table表格    form表单用法.
3. 主要复习 mysql 数据库查询操作