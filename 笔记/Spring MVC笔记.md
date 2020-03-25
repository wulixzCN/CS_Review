# `Spring MVC`

## `Spring MVC 和 Structs2`

- 共同点
  - 它们都是表现层的框架，都是基于MVC模型编写的。
  - 它们的底层都离不开原始的ServletAPI
  - 它们处理请求的机制都是一个核心控制器
- 区别
  - Spring MVC的入口是 Servlet， Structs2的入口是 Filter
  - Spring MVC是基于方法设计的，而Structs2是基于类设计的，Structs2每次执行都会创建一个动作类，所以Spring MVC会稍微比Structs2快些
  - Spring MVC更加简洁，而且还支持JSR303，处理ajax的请求更方便
  - Structs2的OGNL表达式使页面的开发效率相比Spring MVC更高些，但执行效率并没有比JSTL提升，尤其Structs2的表单标签，远没有html执行效率高



## `解决maven项目创建慢的问题`

new model时，添加properties： archetypeCatalog  ：internal



## `入门`

```xml 
<!-- 在spring.xml中配置组件  -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.0.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd">
<!-- 开启注解扫描器  -->   //扫描controller中的类
<context: component-scan base-package="com.xx.xxx"/>

<!-- 视图解析器对象  -->
<bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.I nternalResourceViewResolver">
  <property name="prefix" value="/WEB-INF/pages"/>
  <property name="suffix" value=".jsp"/>
</bean>

<!-- 开启SpringMVC框架注解的支持  -->
  <mvc: annotation-driven/>

</beans>
  

```

在web.xml中配置前端控制器

```xml
    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```



## `流程总结`

1. 启动服务器，加载一些配置文件
   - DispathcherServlet对象创建
   - springmvc.xml 被加载
   - HelloController通过IOC创建成对象
   - 开启注解驱动
2. 发送请求，后台处理请求
   - DispathcherServlet控制中心，指挥中心

## `组件分析`

1. 前端控制器（DispathcherServlet）
   - 是整个流程控制的中心，由它调用其他组件处理用户的请求，dispatcherServlet的存在降低了组件之间的耦合性
2. 处理器映射器（HandlerMapping）
   - 负责根据用户请求找到Handler，即处理器
3. 处理器（Handler）
   - 开发中具体业务控制器，由DispatcherServlet把用户请求转发到Handler，由Handler对具体的用户请求进行处理
4. 处理器适配器（HandlerAdapter）
   - 通过HandlerAdapter对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。
5. 视图解析器（ViewResolver）
6. 视图（View）

> SpringMVC框架基于组件方式执行流程，“器” 即为组件

## `RequestMapping`

用于建立请求URL和处理方法之间的对应关系

可以放在方法上，也可以放在类上

属性：

- value 和 path 一样，都指映射的路径

- method：请求方式

  ```java
  method={RequestMethod.POST}
  
  public enum RequestMethod{
    GET, HEAD, POST, PUT, PATCH, DELETE, OPTIONS,TRACE
  }
  ```

- params: 用于指定限制请求参数的条件。它支持简单的表达式。要求请求参数的key和value必须和配置的一模一样



## `请求参数的绑定`

1. 请求参数绑定的说明
   1. 绑定机制
   2. 支持的数据类型
      1. 基本数据类型和字符串类型
      2. 实体类型（JavaBean）
      3. 集合数据类型（List、map集合等）
2. 基本数据类型和字符串类型
   1. 提交表单的name和参数的名称是相同的
   2. 区分大小写
3. 实体类型



## `解决中文乱码问题`

在web.xml中配置过滤器

```xml
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```



##`把数据封装到类中,类中存在List和Map集合`

```java
import lombok.Data;

@Data
public class Account {
    private String username;
    private String password;
    private Double money;
    private List<User> list;
    private Map<String,User> map;
  
 		get...
    set...
    toString...
}
```

```html
<%--把数据封装到Account中,类中存在List和Map集合--%>
<form action="param/saveAccount" method="post">

    user<input type="text" name="username">
    password<input type="text" name="password">   
    money<input type="text" name="money"> 

    userName<input type="text" name="list[0].uname">   
    userAge<input type="text" name="list[0].age">  

    userName<input type="text" name="map['one'].uname">  
    userAge<input type="text" name="map['one'].age">  

    <input type="submit"></input>
</form>
```

> AccountHandler :

```java
    @RequestMapping("/saveAccount")
    public String saveAccount(Account account){
        System.out.println("doing ...");
        System.out.println("account   :   "+account.toString());
        return "success";
    }
```



## `配置自定义类型转换器`

> 创建类	

```java
public class StringToDateConverter implements Converter<String, Date> {
    /**
     * String source传进来的字符串
     * @param source
     * @return
     */
    @Override
    public Date convert(String source) {
        if (source == null){
            throw new RuntimeException("请传入数据");
        }
        DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
        try {
            return dateFormat.parse(source);
        } catch (Exception e) {
            throw new RuntimeException("数据转换出错");
        }
    }
}
```

配置组件

```xml
<!--    配置自定义类型转换器-->
    <bean id="conversionServiceFactoryBean" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="com.zju.mvc.utils.StringToDateConverter"/>
            </set>
        </property>
    </bean>

    <!--    加载注解驱动-->
    <mvc:annotation-driven conversion-service="conversionService"/>
```



在控制器中使用原生的ServletAPI对象

- 只需要在控制器的方法参数定义 HttpServletRequest 和 HttpServletResponse 对象

```java
    @RequestMapping("testServlet")
    public String testServlet(HttpServletRequest request, HttpServletResponse response){
        
      	System.out.println("request    :   "+request);
        System.out.println("response   :   "+response);
        HttpSession session = request.getSession();
        System.out.println("session   :    "+session);
        ServletContext servletContext = session.getServletContext();
        System.out.println("servletContext    :   "+servletContext);
        return "success";
      
    }
```





## `常用注解`

### `RequestParam`

- 作用：把请求中指定名称的参数给控制器中的形参赋值

- 属性：
  - value：请求参数中的名称
  - required：请求参数中是否必须提供此参数。默认值为true

### `RequestBody`

作用：

- 用于获得请求体的内容。直接使用得到的是 key=value&key=value...结构的数据
- get请求方式不适合。

属性

- required： 是否必须有请求体。

## `PathVariable`

REST风格URL

### `RequestHeader`

---

### `CookieValue`

```JAVA
    @RequestMapping("cookie")
    public String testCookieValue(@CookieValue("JSESSIONID") String cookieValue){
        System.out.println("doing ...");
        System.out.println("cookie Value  :  "+cookieValue);
        return "success";
    }
```



## `ModelAttribute`

- 作用：
  - 用于修饰方法和参数
  - 出现在方法上，表示当前方法会在控制器的方法执行之前，先执行。它可以修饰没有返回值的方法，也可以修饰有具体返回值的方法。



## `SessionAttribute`

参数如果使用 HttpServletRequest  和HttpServlet耦合度太高

```java
@Controller
@RequestMapping("/anno")
@SessionAttributes(value = {"zju_msg"})
public class AnnoHandler {

    /**
     * SessionAttributes 注解  设置值
     * @return
     */
    @RequestMapping("/addsession")
    public String testSessionAttributes(Model model){
        System.out.println("testSessionAttributes  is doing...");
        model.addAttribute("zju_msg","好的");
        return "success";
    }

    /**
     * 获取值
     * @param modelMap
     * @return
     */
    @RequestMapping("/getsession")
    public String getSessionAttributes(ModelMap modelMap){
        System.out.println("getSessionAttributes  is doing...");
        String msg = (String) modelMap.get("zju_msg");
        System.out.println("msg  :  " + msg);
        return "success";
    }

    /**
     * 清除
     * @param status
     * @return
     */
    @RequestMapping("/delsession")
    public String delSessionAttributes(SessionStatus status){
        System.out.println("delSessionAttributes  is doing...");
        status.setComplete();
        return "success";
    }

}
```

```jsp
<%--   对应类中的 session  --%>
     sessionScope    &nbsp;    ${ sessionScope }
 <br>

<%-- 对应方法中的request   --%>
 requestScope   &nbsp;  ${ zju_msg }
 requestScope   &nbsp;  ${ requestScope.get("zju_msg") }
```

## `Date`

```java
  public static void main(String[] args) {
        Date date = new Date("2014/12/29");
        //c的使用
        System.out.printf("全部日期和时间信息：%tc%n",date);
        //f的使用
        System.out.printf("年-月-日格式：%tF%n",date);
        //d的使用
        System.out.printf("月/日/年格式：%tD%n",date);
        //r的使用
        System.out.printf("HH:MM:SS PM格式（12时制）：%tr%n",date);
        //t的使用
        System.out.printf("HH:MM:SS格式（24时制）：%tT%n",date);
        //R的使用
        System.out.printf("HH:MM格式（24时制）：%tR",date);
    }

Console
      全部日期和时间信息：Mon Dec 29 00:00:00 PST 2014
      年-月-日格式：2014-12-29
      月/日/年格式：12/29/14
      HH:MM:SS PM格式（12时制）：12:00:00 AM
      HH:MM:SS格式（24时制）：00:00:00
      HH:MM格式（24时制）：00:00
```

## `ModelAndView`

```java
@RequestMapping("/modelAndView")
    public ModelAndView testModelAndView(){
        ModelAndView mv = new ModelAndView();
        User user = new User();
        user.setUname("jack");
        user.setAge(24);
        mv.addObject("rose",user);
        mv.setViewName("rose");
        System.out.println("future  ing ......");
        return mv;
    }
```

## `响应json数据`

### `过滤静态资源   ResponseBody`

1. DispatcherServlet会拦截所有的资源，导致一个问题就是静态资源（img，js，css）也会被拦截，从而不能被使用。解决问题就是需要配置静态资源不进行拦截，在springmvc.xml配置文件中添加如下配置

   1. mvc:resources 标签配置不过滤
      1. location 元素表示webapp目录下的包下的所有文件
      2. mapping 元素表示以    /static  开头的所有请求路径，如  /static/a， 或 /static/a/b

   ```xml
   <!--    前端控制器，哪些资源不拦截，设置静态资源不过滤-->
       <mvc:resources mapping="/css/**" location="/css/"/>
       <mvc:resources mapping="/js/**" location="/js/"/>
       <mvc:resources mapping="/images/**" location="/images/"/>
   ```

   2. 使用RequestBody 请求体

   Ajax 请求脚本

   ```jsp
       <script src="js/jquery.min.js"></script>
       <script>
           $(function () {
               $("#btn").click(function () {
                   alert("hello btn")
                   $.ajax({
                       //编写json格式，设置属性和值
                       url: "user/testAjax",
                       contentType:"application/json;charset=UTF-8",
                       data:'{"username":哈哈","password":"123456","age":24}',
                       dateType:"json",
                       type:"post",
                       success:function(data){
                           //date服务器端响应的json的数据，进行解析
                       }
                   })
               });
           });
       </script>
   ```

   RequestBody类

   ```java
     @RequestMapping("/testAjax")
       public void testAjax(@RequestBody String body){
           System.out.println("ajax is doing .... ");
           System.out.println(body);
       }
   ```

   json字符串和JavaBean对象互相转换的过程中，需要使用jackson的jar包

   ```xml
          <dependency>
               <groupId>com.fasterxml.jackson.core</groupId>
               <artifactId>jackson-databind</artifactId>
               <version>2.9.0</version>
           </dependency>
           <dependency>
               <groupId>com.fasterxml.jackson.core</groupId>
               <artifactId>jackson-core</artifactId>
               <version>2.9.0</version>
           </dependency>
           <dependency>
               <groupId>com.fasterxml.jackson.core</groupId>
               <artifactId>jackson-annotations</artifactId>
               <version>2.9.0</version>
           </dependency>
   ```

   ```java
    /**
        * 模封装 json数据到类中
        * @param user
        */
       @RequestMapping("/setJson")
       public @ResponseBody NewUser setJson(@RequestBody NewUser user){
           System.out.println("json is doing .... ");
           //  客户端发送ajax请求，传的是json字符串，后端把json字符串封装到user对象中
           System.out.println(user);
   
           //做响应，模拟查询数据库
           user.setUname("hello");
           user.setAge(30);
           //做响应
           //这里返回的是 一个对象，但是前端需要json格式
           //所以利用 @ResponseBody 进行转换
           return user;
       }
   ```

   ```jsp
   <script src="js/jquery.min.js"></script>
       <script>
           $(function () {
               $("#btn").click(function () {
                   alert("hello btn")
                   $.ajax({
                       //编写json格式，设置属性和值
                       url: "user/setJson",
                       contentType:"application/json;charset=UTF-8",
                       data:'{"uname":"hello","password":"123456","age":24}',
                       dateType:"json",
                       type:"post",
                       success:function(data){
                           //date服务器端响应的json的数据，进行解析
                           alert(data);
                           alert(data.uname);
                           alert(data.password);
                           alert(data.age);
                       }
                   })
               });
           });
       </script>
   ```

   



# `SpringMVC实现文件上传`

## `本地文件上传`

1. 导入文件上传的jar包

```xml
<!--        文件上传-->
        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.3.1</version>
        </dependency>
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.4</version>
        </dependency>
```

2. 传统文件上传方式

```jsp
<h3> file upload</h3>
<form action="user/fileupload" method="post" enctype="multipart/form-data">
    choose file : <input type="file" name="upload"><br/>
    <input type="submit" value="upload">
</form>
```

```java
  /**
     * 文件上传
     * @return
     */
    @RequestMapping("/fileupload")
    public String fileupload(HttpServletRequest request) throws Exception {
        System.out.println("file upload ing...");
//        使用 fileupload组件
//        上传的位置
        String path = request.getSession().getServletContext().getRealPath("/uploads");
        File file = new File(path);
//        判断该路径是否存在
        if (!file.exists()){
            file.mkdirs();
        }
//        解析request对象，获取上传文件项
        DiskFileItemFactory factory = new DiskFileItemFactory();
        ServletFileUpload upload = new ServletFileUpload(factory);
//        解析request
        List<FileItem> items = upload.parseRequest(request);
        for (FileItem item : items){
            if (item.isFormField()){
                //说明这是普通表单
            }else {
                //说明是文件上传项
                //获取上传文件的名称
                String filename = item.getName();
                //把文件的名称设为唯一值
                String uuid = UUID.randomUUID().toString().replace("-", "");
                filename = uuid + "_" +filename;
                //完成文件上传
                System.out.println("filename   :   "+filename);
                System.out.println("path   :   "+path);
                item.write(new File(path, filename));
                //删除临时文件
                item.delete();
            }
        }
        return "success";
    }
```

3. Spring MVC上传文件的方式

   1.  SpringMVC框架提供了MultipartFile对象，该对象表示上传的文件，要求变量名称必须和表单file标签的name属性名称相同。

   2. 代码：

      ```xml
      <!--    配置文件解析器对象 ，这里 id名称必须是  multipartResolver-->
          <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
              <property name="maxUploadSize" value="10485760"/>
          </bean>
      ```

      ```jsp
      <form action="user/springupload" method="post" enctype="multipart/form-data">
          SpringMVC的文件上传: <input type="file" name="springupload"><br/>
          <input type="submit" value="upload">
          <h4> 注：  name值必须和方法中的形参名字相同</h4>
      </form>
      ```

      ```java
       /**
           * Spring MVC 文件上传
           * 前端 name值必须和方法中的形参名字相同
           * @param request
           * @param springupload
           * @return
           */
          @RequestMapping("/springupload")
          public String springFileUpload(HttpServletRequest request, MultipartFile springupload) throws Exception {
              System.out.println("spring  upload  ing");
              String path = request.getSession().getServletContext().getRealPath("uploads");
              File file = new File(path);
              if (!file.exists()){
                  file.mkdirs();
              }
              //不用再需要解析request
              //获取上传文件的名称
              String filename = springupload.getOriginalFilename();
              String uuid = UUID.randomUUID().toString().replace("-", "");
              filename = uuid + "_" +filename;
              //完成文件的上传
              springupload.transferTo(new File(path,filename));
              return "success";
          }
      ```

## `跨服务器的文件传输`

1. 导入jar包

```xml
<!--         跨服务器文件上传-->
        <dependency>
            <groupId>com.sun.jersey</groupId>
            <artifactId>jersey-core</artifactId>
            <version>1.18.1</version>
        </dependency>
        <dependency>
            <groupId>com.sun.jersey</groupId>
            <artifactId>jersey-client</artifactId>
            <version>1.18.1</version>
        </dependency>
```

```jsp
<h3> 跨服务器文件上传  file upload</h3>
<form action="user/serverupload" method="post" enctype="multipart/form-data">
    跨服务器的文件上传: <input type="file" name="upload"><br/>
    <input type="submit" value="upload">
    <h4> 注：  name值必须和方法中的形参名字相同</h4>
</form>
```

```java
/**
     * 跨服务器上传
     * @param upload
     * @return
     * @throws Exception
     */
    @RequestMapping("/serverupload")
    public String serverFileUpload( MultipartFile upload) throws Exception {
        System.out.println("跨服务器上传 ing");
        //定义上传文件服务器的路径
        String path ="http://127.0.0.1:9090/storage/uploads/";
        //不用再需要解析request  获取上传文件的名称
        String filename = upload.getOriginalFilename();
        String uuid = UUID.randomUUID().toString().replace("-", "");
        filename = uuid + "_" +filename;
        //创建客户端的对象
        Client client = Client.create();
        //和图片服务器进行连接
        WebResource webResource = client.resource(path + filename);
        //上传文件
        webResource.put(upload.getBytes());
        return "success";
    }
```

---

> ！！！！
>
> 注意
>
> ！！！
>
> 报错：  405 Method Not Allowed
>
> 
>
> 修复方法
>
> tomcat文件夹的conf下的web.xml把只读去掉   ！！！ 注意是 服务器tomcat的！！

---

# Spring MVC中的异常处理

> 抛出自己的异常

```java
   @RequestMapping("/testException")
    public String except() throws MyException {
        System.out.println("exception ....");
        try {
            int a = 10 /0 ;
        } catch (Exception e) {
//            打印错误信息
            e.printStackTrace();
            throw new MyException("不能 ➗ 0  ... ");
        }
        return "success";
    }
```

> 创建自己的异常类

```java
public class MyException extends Exception {
    //提示信息
    private String message;
    @Override
    public String getMessage() {
        return message;
    }
    public void setMessage(String message) {
        this.message = message;
    }
    public MyException() {
    }
    public MyException(String message) {
        this.message = message;
    }
}
```

> 创建异常解决方案 (异常处理器)

```java
public class MyExceptionResolver implements HandlerExceptionResolver {
    /**
     * 处理异常业务逻辑
     * @param httpServletRequest
     * @param httpServletResponse
     * @param o
     * @param ex
     * @return
     */
    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception ex) {
        MyException mye = null;
        if (ex instanceof MyException){
            mye = (MyException) ex;
        }else {
            mye = new MyException("系统正在维护中...");
        }

        ModelAndView mv = new ModelAndView();
        mv.addObject("errorMesg",mye.getMessage());
        mv.setViewName("error");
        return mv;
    }
}
```

> 配置文件中配置异常处理器（IOC）

```xml
<!--    配置异常处理器-->
    <bean id="myExceptionResolver" class="com.zju.mvc.exception.MyExceptionResolver">
    </bean>
```

# `Spring MVC中的拦截器`

1. 编写拦截器的类，需要实现 HandlerInterceptor接口
2. 配置拦截器

```java
//编写拦截器的类

public class MyInterceptor implements HandlerInterceptor {
    /**
     * 预处理，controller方法执行前
     * return true 表示放行，执行下一个拦截器，如果没有，执行controller中的方法
     * return false 不放行
     * @param request
     * @param response
     * @param handler
     * @return
     * @throws Exception
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("MyInterceptor  执行前 ...");
//        request.getRequestDispatcher("/WEB_INF/pages/interceptorerror.jsp").forward(request,response);
//        return false;
        return true;
    }
    /**
     * 后处理方法，controller执行之后，success.jsp执行之前
     * @param request
     * @param response
     * @param handler
     * @param modelAndView
     * @throws Exception
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        request.getRequestDispatcher("/WEB-INF/pages/interceptorerror.jsp").forward(request,response);
        System.out.println("MyInterceptor  执行后 ...");
    }

    /**
     * success.jsp 执行后执行
     * @param request
     * @param response
     * @param handler
     * @param ex
     * @throws Exception
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("MyInterceptor  执行 最最后 ...");

    }
}
```

```xml
<!--    配置拦截器s-->
    <mvc:interceptors>
        <!--    配置拦截器-->
        <mvc:interceptor>
<!--            配置要拦截的具体的方法-->
            <mvc:mapping path="/user/*"/>
<!--            不要拦截的方法-->
<!--            <mvc:exclude-mapping path=""/>-->
<!--            配置自己的拦截器-->
            <bean id="MyInterceptor" class="com.zju.mvc.interceptor.MyInterceptor"></bean>
        </mvc:interceptor>
    </mvc:interceptors>
```



