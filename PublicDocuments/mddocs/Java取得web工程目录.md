###Java取得web工程目录-http://www.javaweb.cc
####1.可以在servlet的init方法里
```java
String path = getServletContext().getRealPath("/");
```
这将获取web项目的全路径
例如 :E:\eclipseM9\workspace\tree\
tree是我web项目的根目录
####2.你也可以随时在任意的class里调用
```java
this.getClass().getClassLoader().getResource("/").getPath();
```
这将获取 到classes目录的全路径
例如 : E:\eclipseM9/workspace/tree/WEB-INF/classes/
这个方法也可以不在web环境里确定路径,比较好用
####3.request.getContextPath();
获得web根的上下文环境
如 /tree
tree是我的web项目的root context
- jsp 取得当前目录的路径
```java
path=request.getRealPath("");
```
- 得到jbossWEB发布临时目录 warUrl=.../tmp/deploy/tmp14544test-exp.war/
path=C:\jboss-4.0.5.GA\server\default\tmp\deploy\tmp14544test-exp.war\
```java
String path = (String)request.getContextPath();
```
- 得到项目(test)应用所在的真实的路径 path=/test 
```java
String path = request.getRequestURI();
```
- 得到应用所在的真实的路径 path=/test/admin/admindex.jsp
```java
String savePath=request.getRealPath(request.getServletPath());
```
- 得到当前文件的磁盘绝对路径
```java
//JAVA 取得当前目录的路径
File file=new File("."); 
String path=file.getAbsolutePath();
path=file.getPath();
```
###得到jboss运行目录 path=C:\jboss-4.0.5.GA\bin\
---------------------------------------------
####Java相对路径/绝对路径总结
#####1.基本概念的理解
- **绝对路径**：绝对路径就是你的主页上的文件或目录在硬盘上真正的路径，(URL和物理路径)例如：
C:xyz est.txt 代表了test.txt文件的绝对路径。http://www.sun.com/index.htm也代表了一个URL绝对路径。
- **相对路径**：相对与某个基准目录的路径。包含Web的相对路径（HTML中的相对目录），例如：在
Servlet中，"/"代表Web应用的跟目录。和物理路径的相对表示。例如："./" 代表当前目录,"../"代表上级目录。这种类似的表示，也是属于相对路径。
另外关于URI，URL,URN等内容，请参考RFC相关文档标准。
RFC 2396: Uniform Resource Identifiers (URI): Generic Syntax,
(http://www.ietf.org/rfc/rfc2396.txt)

#####2.关于JSP/Servlet中的相对路径和绝对路径。
######2.1服务器端的地址
服务器端的相对地址指的是相对于你的web应用的地址，这个地址是在服务器端解析的（不同于html和javascript中的相对地址，他们是由客户端浏览器解析的）也就是说这时候在jsp和servlet中的相对地址应该是相对于你的web应用，即相对于http: //192.168.0.1/webapp/的。
其用到的地方有：
***forward***：servlet中的request.getRequestDispatcher(address);这个address是在服务器端解析的，所以，你要forward到a.jsp应该这么写：request.getRequestDispatcher(“/user/a.jsp”)这个/ 相对于当前的web应用webapp，其绝对地址就是：http://192.168.0.1/webapp/user/a.jsp。 sendRedirect：在jsp中<%response.sendRedirect("/rtccp/user/a.jsp");%>
######2.22、客户端的地址
所有的html页面中的相对地址都是相对于服务器根目录(http://192.168.0.1/)的，而不是(跟目录下的该Web应用的目录) http://192.168.0.1/webapp/的。 Html中的form表单的action属性的地址应该是相对于服务器根目录(http://192.168.0.1/)的，所以，如果提交到a.jsp 为：action＝"/webapp/user/a.jsp"或action="<%=request.getContextPath()% >"/user/a.jsp；
提交到servlet为actiom＝"/webapp/handleservlet" Javascript也是在客户端解析的，所以其相对路径和form表单一样。

因此，一般情况下，在JSP/HTML页面等引用的CSS,Javascript.Action等属性前面最好都加上
<%=request.getContextPath()%>,以确保所引用的文件都属于Web应用中的目录。另外，应该尽量避免使用类似".","./","../../"等类似的相对该文件位置的相对路径，这样当文件移动时，很容易出问题。

#####3. JSP/Servlet中获得当前应用的相对路径和绝对路径
######3.1 JSP中获得当前应用的相对路径和绝对路径
根目录所对应的绝对路径:```request.getRequestURI()```
文件的绝对路径 　:```application.getRealPath(request.getRequestURI());```
当前web应用的绝对路径 :application.getRealPath("/");
取得请求文件的上层目录:```new File(application.getRealPath(request.getRequestURI())).getParent()```
######3.2 Servlet中获得当前应用的相对路径和绝对路径
根目录所对应的绝对路径:request.getServletPath();
文件的绝对路径 :```request.getSession().getServletContext().getRealPath
(request.getRequestURI())```
当前web应用的绝对路径 :
```java
servletConfig.getServletContext().getRealPath("/");

ServletContext对象获得几种方式：
javax.servlet.http.HttpSession.getServletContext()
javax.servlet.jsp.PageContext.getServletContext()
javax.servlet.ServletConfig.getServletContext()
```

#####4.java 的Class中获得相对路径，绝对路径的方法
######4.1单独的Java类中获得绝对路径
根据java.io.File的Doc文挡，可知:
默认情况下new File("/")代表的目录为：System.getProperty("user.dir")。
以下程序获得执行类的当前路径
```java
package org.cheng.file; 

import java.io.File; 

public class FileTest ...{ 
public static void main(String[] args) throws Exception ...{ 
System.out.println(Thread.currentThread().getContextClassLoader().getResource("")); 

System.out.println(FileTest.class.getClassLoader().getResource("")); 

System.out.println(ClassLoader.getSystemResource("")); 
System.out.println(FileTest.class.getResource("")); 
System.out.println(FileTest.class.getResource("/")); 
//Class文件所在路径 
System.out.println(new File("/").getAbsolutePath()); 
System.out.println(System.getProperty("user.dir")); 
} 
}
```
######4.2服务器中的Java类获得当前路径
- **(1).Weblogic**
WebApplication的系统文件根目录是你的weblogic安装所在根目录。
例如：如果你的weblogic安装在c:beaweblogic700.....
那么，你的文件根路径就是c:.
所以，有两种方式能够让你访问你的服务器端的文件：
*a.使用绝对路径：*
比如将你的参数文件放在c:yourconfigyourconf.properties，
直接使用``` new FileInputStream("yourconfig/yourconf.properties");```
*b.使用相对路径：*
相对路径的根目录就是你的webapplication的根路径，即WEB-INF的上一级目录，将你的参数文件放
在yourwebappyourconfigyourconf.properties，
这样使用：```new FileInputStream("./yourconfig/yourconf.properties");```

   这两种方式均可，自己选择。
   
- **(2).Tomcat**
在类中输出System.getProperty("user.dir");显示的是%Tomcat_Home%/bin

- **(3).Resin**
不是你的JSP放的相对路径,是JSP引擎执行这个JSP编译成SERVLET
的路径为根.比如用新建文件法测试File f = new File("a.htm");
这个a.htm在resin的安装目录下

- **(4).如何读相对路径哪？**
在Java文件中getResource或getResourceAsStream均可
例：```getClass().getResourceAsStream(filePath);//filePath```可以是"/filename",这里的/代表web
发布根路径下WEB-INF/classes
默认使用该方法的路径是：WEB-INF/classes。已经在Tomcat中测试。
####总结：
通过上面内容的使用，可以解决在Web应用服务器端，移动文件，查找文件，复制
删除文件等操作，同时对服务器的相对地址，绝对地址概念更加清晰。
建议参考URI,的RFC标准文挡。同时对Java.io.File. Java.net.URI.等内容了解透彻
对其他方面的理解可以更加深入和透彻。
这是在Java中去当前项目的根目录的方法
java 代码:
```java
/** *//** 
* TODO 取得当前项目的根目录 
* @author PHeH
*/ 
public class Application ...{ 

/** *//** 
* TODO 获取根目录 
* @return 
* @author PHeH
*/ 
public static String getRootPath()...{ 
//因为类名为"Application"，因此" Application.class"一定能找到 
String result = Application.class.getResource("Application.class").toString(); 
int index = result.indexOf("WEB-INF"); 
if(index == -1)...{ 
index = result.indexOf("bin"); 
} 
result = result.substring(0,index); 
if(result.startsWith("jar"))...{ 
// 当class文件在jar文件中时，返回"jar:file:/F:/ ..."样的路径 
result = result.substring(10); 
}else if(result.startsWith("file"))...{ 
// 当class文件在class文件中时，返回"file:/F:/ ..."样的路径 
result = result.substring(6); 
} 
if(result.endsWith("/"))result = result.substring(0,result.length()-1);//不包含最后的"/" 
return result; 
} 
}
```