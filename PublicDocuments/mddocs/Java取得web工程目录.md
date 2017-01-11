从request获取各种路径总结 
```java
request.getRealPath("url"); // 虚拟目录映射为实际目录

request.getRealPath("./");    // 网页所在的目录
request.getRealPath("../"); // 网页所在目录的上一层目录
request.getContextPath();    // 应用的web目录的名称
```
如 *http://localhost:7001/bookStore/*

```/bookStore/ => [contextPath] (request.getContextPath())```

获取Web项目的全路径 
```java
String strDirPath = request.getSession().getServletContext().getRealPath("/");
String strDirPath2 = request.getServletContext().getRealPath("/");
```
以工程名为TEST为例：
(1)得到包含工程名的当前页面全路径：request.getRequestURI() 
结果：/TEST/test.jsp

(2)得到工程名：request.getContextPath() 
结果：/TEST

(3)得到当前页面所在目录下全名称：request.getServletPath() 
结果：如果页面在jsp目录下 /TEST/jsp/test.jsp

(4)得到页面所在服务器的全路径：application.getRealPath("页面.jsp") 
结果：D:\resin\webapps\TEST\test.jsp

(5)得到页面所在服务器的绝对路径：absPath=new java.io.File(application.getRealPath(request.getRequestURI())).getParent(); 
结果：D:\resin\webapps\TEST
2.在类中取得路径：
(1)类的绝对路径：Class.class.getClass().getResource("/").getPath() 
结果：/D:/TEST/WebRoot/WEB-INF/classes/pack/

(2)得到工程的路径：System.getProperty("user.dir") 
结果：D:\TEST
3.在Servlet中取得路径：
(1)得到工程目录：request.getSession().getServletContext().getRealPath("") 参数可具体到包名。 
结果：E:\Tomcat\webapps\TEST

(2)得到IE地址栏地址：request.getRequestURL() 
结果：http://localhost:8080/TEST/test

(3)得到相对地址：request.getRequestURI() 
结果：/TEST/test