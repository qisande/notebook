# 购物车功能实现

- 界面原型讲解

![image-20220819090222354](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220819090222354.png)

- 数据封装
  - 关系型数据库存储    弊端  用户加够的多,但是下单的少  如果存储到数据库中 则会造成海量的数据 解析 查询不易.
  - 非关系数据库   mangoDB/hive   数据按行存储.  占用空间少.
  - 课堂中使用Map集合 模拟非关系型数据库的存储. 

- 购物车功能数据封装解析

  ![image-20220819091653431](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220819091653431.png)

  - CartItem  {Book对象  数量   金额}
  - Cart { Map集合,  总数量,  总金额}
  - 购物车需要与用户绑定  则将Cart保存到 session域中.

- 定义购物车项信息  CartItem

```java
//封装购物项内容
public class CartItem implements Serializable {
    private Book book;      //购物项图书
    private Long count;  //购物数量
    private Double amount;  //购物项总价  不能由用户手动输入,应该由程序计算

}
```

- 封装购物车信息

```java
public class Cart implements Serializable {
    //Map{bookId, cartItem}
    private Map<Integer,CartItem> map = new HashMap<>();  //map集合类似于数据库
    private Long totalCount;        //购物车的总数量
    private Double  totalAmount;    //购物车的总价格
    private Collection cartItems;   //为了页面获取数据方便 添加的属性

}
```

- 用户与购物车绑定
  - 用户与浏览器绑定   将购物车信息保存到Session域中.

![image-20220819093643086](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220819093643086.png)

- 购物项业务逻辑说明
  -  其中的金额 应该是  单价 * 数量 得到的结果  而不能是setxxx设置的.

```java
package com.atguigu.bean;

import java.io.Serializable;
import java.math.BigDecimal;
//封装购物项内容
public class CartItem implements Serializable {
    private Book book;      //购物项图书
    private Long count;  //购物数量
    private Double amount;  //购物项总价  不能由用户手动输入,应该由程序计算

    public CartItem(){

    }

    public CartItem(Book book, Long count) {
        this.book = book;
        this.count = count;
        this.amount = book.getPrice() * count;
    }

    public Book getBook() {
        return book;
    }

    public void setBook(Book book) {
        this.book = book;
        this.amount = book.getPrice() * count;
    }

    public Long getCount() {
        return count;
    }

    public void setCount(Long count) {
        this.count = count;
        this.amount = book.getPrice() * count;
    }

    public Double getAmount() {
        return amount;
    }

    @Override
    public String toString() {
        return "CartItem{" +
                "book=" + book +
                ", count=" + count +
                ", amount=" + amount +
                '}';
    }
}

```

# 购物车新增操作

 -  业务说明

    	- 当用户点击加入购物车时,应该发起Ajax请求实现购物车新增.
        	- ajax请求中必须传递bookId.
        	- 根据bookId 查询图书信息.
     - 通过Session对象 获取购物车信息.
       	- 购物车对象为null, 用户第一次添加购物车 准备购物车对象
       	- 购物车对象不为null, 该购物车对象用户可以直接使用.
    - 通过bookId 判断购物车中是否有该记录.
      - 有记录   则修改购物项的数量
      - 无记录   则添加图书信息  数量为1
    - 用户新增购物车之后.获取购物车的总数量,之后通过SysResult对象返回数量
    - 页面JS 获取到数量之后  应该展现到页面中  默认值为0
    - 由于第一次访问页面 没有发起Ajax请求 所以数量为0  可以通过session对象获取购物车信息 从购物车中获取数量.    使用模版引擎处理.

- 编辑页面

  - 引入页面JS    index.html

  ```javascript
  <!--加入js-->
      <script src="static/script/vue.js"></script>
      <script src="static/script/axios.js"></script>
  ```

  	- 编辑CartItem

  ```java
  package com.atguigu.bean;
  
  import java.io.Serializable;
  import java.math.BigDecimal;
  //封装购物项内容
  public class CartItem implements Serializable {
      private Book book;      //购物项图书
      private Long count;  //购物数量
      private Double amount;  //购物项总价  不能由用户手动输入,应该由程序计算
  
      public CartItem(){
  
      }
  
      public CartItem(Book book, Long count) {
          this.book = book;
          this.count = count;
          this.amount = book.getPrice() * count;
      }
  
      public Book getBook() {
          return book;
      }
  
      public void setBook(Book book) {
          this.book = book;
          this.amount = book.getPrice() * count;
      }
  
      public Long getCount() {
          return count;
      }
  
      public void setCount(Long count) {
          this.count = count;
          this.amount = book.getPrice() * count;
      }
  
      public Double getAmount() {
          return amount;
      }
  
      @Override
      public String toString() {
          return "CartItem{" +
                  "book=" + book +
                  ", count=" + count +
                  ", amount=" + amount +
                  '}';
      }
  }
  
  ```

  - 编辑Cart

  ```java
  package com.atguigu.bean;
  
  import java.io.Serializable;
  import java.math.BigDecimal;
  import java.util.*;
  
  public class Cart implements Serializable {
      //Map{bookId, cartItem}
      private Map<Integer,CartItem> map = new HashMap<>();  //map集合类似于数据库
      private Long totalCount;        //购物车的总数量
      private Double  totalAmount;    //购物车的总价格
      private Collection cartItems;   //为了页面获取数据方便 添加的属性
  
  
      public void addCart(Book book) {
  
          if(map.containsKey(book.getId())){  //买过这本书
              //更新数量信息
              long count = map.get(book.getId()).getCount() + 1;
              map.get(book.getId()).setCount(count);
  
          }else{//用户第一次购买
              CartItem cartItem = new CartItem(book,1L);
              map.put(book.getId(),cartItem);
          }
      }
  
      /**
       * 在购物车中添加getTotalCount方法 获取总数量
       *   作用:
       *      1.手动调用获取总数信息
       *      2.为了转化JSON时 获取总数!!!!
       */
      public Long getTotalCount() {
          //1.获取所有的购物项
          Collection<CartItem> cartItems = getCartItems();
          long count = 0;
          for (CartItem cartItem : cartItems){
              count += cartItem.getCount();
          }
          //为属性的总数赋值.
          this.totalCount = count;
          return totalCount;
      }
  
      /**
       * 1.手动调用方法获取所有的购物项内容!!
       * 2.准备getXXX方法 目的 转化JSON时 可以动态获取所有的购物项内容
       * @return
       */
      public Collection getCartItems() {
          this.cartItems = map.values();
          return cartItems;
      }
  }
  
  ```

  - 编辑CartServlet

  ```java
  package com.atguigu.servlet;
  
  import com.atguigu.bean.Book;
  import com.atguigu.bean.Cart;
  import com.atguigu.service.BookService;
  import com.atguigu.service.impl.BookServiceImpl;
  import com.atguigu.vo.SysResult;
  import com.fasterxml.jackson.databind.ObjectMapper;
  
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  
  /**
   * @author 刘昱江
   * @className CartServlet
   * @description TODO
   * @date 2022/8/19 10:32
   */
  @WebServlet("/cart")
  public class CartServlet extends BaseServlet{
  
      private BookService bookService = new BookServiceImpl();
      private static final ObjectMapper MAPPER = new ObjectMapper();
      /**
       *  let {data: sysResult} = await axios.get("cart?method=addCart&bookId="+bookId)
       */
      public  void addCart(HttpServletRequest req, HttpServletResponse resp) throws Exception {
          int bookId = Integer.parseInt(req.getParameter("bookId"));
          Book book = bookService.findBookById(bookId);
          //获取购物车对象!!!!
          Cart cart = getCartSession(req,resp);
          //实现购物车新增
          cart.addCart(book);
          //获取购物车的总数量
          long count = cart.getTotalCount();
          //ajax调用 返回业务数据  count
          String json = MAPPER.writeValueAsString(SysResult.success(count));
          resp.getWriter().write(json);
      }
  
      /**
       * 封装一个方法 从Session中获取购物车
       * 原则: java中一般操作的都是对象的引用!!!!!   操作了具体的对象  则引用的数据也会发生变化(同一个对象)
       */
      public Cart getCartSession(HttpServletRequest request,HttpServletResponse response){
          //从session对象中获取购物车信息
          Cart cart = (Cart) request.getSession().getAttribute("cart");
          if(cart == null){   //如果购物车为null 则证明第一次加够  准备购物车对象
              cart = new Cart();
              //将创建的cart对象保存到session域中
              request.getSession().setAttribute("cart",cart);
          }
          //最终返回购物车对象!!!
          return cart;
      }
  
      /**
       * axios.get("cart?method=getTotalCount")
       */
      public void getTotalCount(HttpServletRequest request,HttpServletResponse response) throws Exception{
          Cart cart = getCartSession(request, response);
          long count = cart.getTotalCount();
          String json = MAPPER.writeValueAsString(SysResult.success(count));
          response.getWriter().write(json);
      }
  }
  
  ```

  - 编辑页面JS

  ```javascript
  <script>
        const app = new Vue({
          el: "#app",
          data: {
            totalCount: "[[${session.cart==null ? 0 : session.cart.totalCount}]]",
            //totalCount: 0
          },
          methods: {
            async addCart(){
              //event.target  通过dom操作 获取当前元素对象   <button id="8">加入购物车</button>
              let bookId = event.target.id
              let {data: sysResult} = await axios.get("cart?method=addCart&bookId="+bookId)
              if(sysResult.flag){
                alert("新增购物车成功")
                //获取服务器的业务数据
                let count = sysResult.data
                this.totalCount = count
              }else{
                alert("添加购物车失败!!!")
              }
            },
            async getTotalCount(){
              let {data: sysResult} = await axios.get("cart?method=getTotalCount")
              let count = sysResult.data
              this.totalCount = count
            }
          },
          created(){
            //this.getTotalCount()
          }
        })
      </script>
  ```

  

- 购物车列表页面跳转

  - 编辑页面 实现url跳转

  ```html
  <a  href="cart?method=toCart" class="cart iconfont icon-gouwuche"> 购物车<div class="cart-num" v-text="totalCount">3</div></a>
  ```

  - 编辑Servlet实现页面跳转

  ```java
  /**
       * 跳转到购物车列表页面
       * url:  cart?method=toCart
       * web/WEB-INF/pages/cart/cart.html
       */
      public void toCart(HttpServletRequest request,HttpServletResponse response) throws Exception{
  
          this.processTemplate("cart/cart",request,response);
      }
  ```

- 购物车列表展现
  - 用户发起请求 获取购物车信息
  - 后端服务器应该从Session中获取购物车对象.
    - 获取购物项信息
    - 获取总数量
    - 获取总金额
  - 获取到Cart对象之后 通过json的方式 业务返回

- 继续购物实现

  - 跳转页面到首页

  ```html
  <a href="index.html">继续购物</a>
  ```

- 清空购物车

  - 发起ajax请求 删除Session中的cart对象

  

- 购物车删除操作

  - 当用户点击删除操作时,应该携带bookId 之后发起ajax请求删除数据

- 购物车数量的修改操作

  - 用户的输入框 应该使用双向数据绑定
  - 要求用户输入有效的数量信息  正则表达式校验
  - 获取bookId和数量信息count  之后发起Ajax请求 实现数据更新操作.
  - 如果点击-  如果只剩一个时 应该调用删除操作

  

- 计算精度问题说明

![image-20220819162205149](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220819163621208.png)



- 过滤器介绍

  - Filter 是JAVAWEB 的三大组件之一   
    - Servlet
    - Filter  过滤器
    - listener  监听器  
  - Filter主要作用  过滤敏感数据/控制程序的流转(登录的验证!!!),Filter是先于Servlet执行 如果程序放行才能访问Servlet.
    - 放行  程序才会访问Servlet服务
    - 拦截  则必须配合重定向的方式 跳转新的页面 否则程序卡死 阻塞
    - 

  ![image-20220819163621208](https://images73.oss-cn-beijing.aliyuncs.com/img/image-20220819162205149.png)

   -  Filter的配置

      	- web.xml写法

      ```xml
      <!--配置Filter-->
          <filter>
              <filter-name>CharacterFilter</filter-name>
              <filter-class>com.atguigu.filter.CharacterFilter</filter-class>
          </filter>
          <filter-mapping>
              <filter-name>CharacterFilter</filter-name>
              <!--拦截的路径 /*拦截所有的请求-->
              <url-pattern>/*</url-pattern>
             <!-- <url-pattern>/cart/*</url-pattern>
              <url-pattern>/order/*</url-pattern>
              <url-pattern>/addCart</url-pattern>
              <url-pattern>*.jpg</url-pattern>-->
          </filter-mapping>
      ```

      

      	- 注解写法

      ```java
      @WebFilter(filterName="CharacterFilter",value = {"/*"})
      ```

      

  - 过滤器实现乱码解决

  ```java
  package com.atguigu.filter;
  
  import javax.servlet.*;
  import javax.servlet.annotation.WebFilter;
  import javax.servlet.annotation.WebServlet;
  import java.io.IOException;
  
  /**
   * @author 刘昱江
   * @className CharacterFilter
   * @description TODO
   * @date 2022/8/19 16:39
   */
  //@WebFilter(filterName="CharacterFilter",value = {"/*"})
  @WebFilter("/*")
  public class CharacterFilter implements Filter {
  
  
      @Override
      public void init(FilterConfig filterConfig) throws ServletException {
          System.out.println("过滤器对象创建时调用");
      }
  
      @Override
      public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
          //System.out.println("过滤器执行Servlet前");
          servletRequest.setCharacterEncoding("utf-8");
          servletResponse.setContentType("text/html;charset=utf-8");
          //表示程序放行
          filterChain.doFilter(servletRequest,servletResponse);
          //System.out.println("过滤器执行Servlet后");
      }
  
      @Override
      public void destroy() {
          System.out.println("过滤器销毁!!!");
      }
  }
  ```

  

- 过滤器实现用户校验

  - 如果用户没有登录 则重定向到登录页面

  - 如果用户登录了 .则放行请求.

  - 业务要求:

    - 拦截 购物车业务  /cart/*     /订单业务  /order/*
    - 检查Session中是否有User对象  有对象已登录 则放行  否则拦截重定向到登录页面

    

  









