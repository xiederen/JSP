
# 什么是Servlet：
`Servlet=Server+Applet；  `  
Servlet是使用`Java Servlet`应用程序设计接口(API)及相关类和方法的Java程序；     
Servlet是一种运行在服务器端的Java应用程序；        
并不是所有运行在服务器端的Java应用程序可以都称为Servlet；    
只有当一个服务器端的程序使用了`Java Servlet`应用程序接口，也就是`Servlet API`的时候，这个服务器端的程序才能称之为Servlet；        

## Servlet在服务器端的作用：   
它的作用和CGI是非常类似的；      
CGI和Servlet都可以用来在服务器端处理客户请求，并把处理结果返回给客户端；但是Servlet的功能要比CGI要强大的多，而且使用起来比CGI更加方便；      

#### 相对于CGI，Servlet的优点：    
使用Java编写的CGI程序需要为每个请求都启动一个重量级的进程；也就是说，如果我们有10个并发的请求，就需要启动10个这样的进程；如果有100个并发的请求，就需要启动100个这样的进程；这样一来，当请求数量增加时，就会大大降低系统的执行效率；   
那么，用什么办法，可以能够取消这种开销呢？     
即，不管有多少并发的请求，我们只需要启动一个操作系统进程以及一个java虚拟机的映像；那么这样一来基于java的CGI程序的执行效率就能得到很好的改善；       

Servlet就是基于上述思想所产生的；        

与CGI不同，同一个Servlet程序，它只运行在一个进程当中；它并不会因为用户请求数量的增加，而创建多个进程；那么只有一个进程情况下，它是如何处理多个用户的请求：     
答案就是：  线程；    

当多个用户同时访问这个Servlet的时候，就在上面所讲的进程当中创建多个线程，每个线程用来处理一个请求；这样一来，就避免了像CGI程序那样，每个请求都创建一个进程；并且这么多进程，才会重复加载同样的类资源，也会耗费更多的内存，因此，Servlet比CGI程序有更好的执行效率；        

Servlet运行的这个进程会一直处于运行的状态；一直到距Servlet应用关闭的时候，这个进程才会结束运行；    
其次由于Servlet是采用Java语言所编写的，而Java是跨平台的，因此在所有安装Java运行时环境的任何操作系统上，我们都可以使用Servlet；换句话说，Servlet也具有平台独立性；       
因为Servlet是用Java语言编写的，那么在使用Java编写Web应用的时候，就可以很方便的将Servlet与这个应用当中其他的功能集成起来；             比方说，我们可以使用jdbc完成数据库的访问，或者使用JNDI完成目录访问；我们可以把这些功能集成到Servlet当中，这也就是Servlet的集成性；           
除此之外，Servlet本身接口设计也十分精简和合理；而且Servlet得到Web服务器广泛的支持，这将使得Servlet具有更好的扩展性；  比方说，我们可以使用独立的电脑开发以及测试我们的Web应用，测试成功以后，再部署到其他的环境中；例如：我们可以部署到另外一台机器的tamcat当中，或者Apache当中；甚至，我们可以部署到高端服务器集群当中，这些都不再话下；    

#### Servlet的优点：
执行效率高；   
平台独立性；   
集成性；    
可扩展性；      


#### Servlet程序的实现方式：   
在编写Servlet程序的时候一般都要实现`javax.servlet`包当中的Servlet接口；或者我们可以继承这个接口的两个抽象的实现类；它们分别是`javax.servlet`包当中的GenericServlet类，以及`javax.servlet`这个包当中的HttpServlet类；           

#### 实现javax.servlet.Servlet接口：
Servlet接口是所有Java Servlet的基础接口类，规定了必须由Servlet具体类实现的方法集；   
这些方法都是servlet容器来识别和管理；      
也就是说，Servlet接口中定义的方法，在什么时候调用，是由Servlet容器进行管理和控制的。而不是我们自己定义控制；我们所需要做的只需要在自己的Servlet实现类当中指定，在Servlet容器调用这些方法的时候，我们的类要做哪些操作就可以了。     
在Servlet接口中都声明了哪些方法，以及这些方法在什么情况下会被调用？
     
### 在Servlet接口中，最重要的方法有这么几个：

####  init()方法：    
`init()`方法是Servlet的初始化方法，可以把运行这个Servlet所需要初始化的动作放在init方法当中执行；    
初始化Servlet时加载资料；    
初始化Servlet时初始化数据库连接；       
一个Servlet的init方法，仅仅会在Servlet装入Servlet容器的时候执行一次，而不会多次执行；     

##### Servlet的运行机制：       
Servlet要运行，它并不是独立运行的，而是要运行在Servlet容器当中；     
既然Servlet是运行在容器当中的，那么我们就需要在合适的时机把Servlet装入容器当中；      
而这个init方法就是在把Servlet装入Servlet容器的时候，由Servlet容器来调用执行的；      
这个调用过程不是由我们完成，而是由Servlet容器自动完成的；           
同时，因为同一个Servlet只需要装入Servlet容器一次。因此，Servlet当中init方法也只会执行一次。      
不论多少个客户端调用这个Servlet，这个Servlet的init初始化方法只会执行一次，而不会多次执行；     

注意：       
init方法需要传入一个`javax.servlet.ServletConfig`类型的参数，此参数包含了Servlet的初始化参数信息。       
在Servlet容器初始化一个Servlet对象的时候，就会为这个servlet对象创建一个ServletConfig对象。在ServletConfig对象中包含了servlet的初始化参数信息。        


#### service()方法：
service方法用来处理客户端的请求以及生成对客户端的响应。    
在Servlet容器加载了Servlet，并且在这个Servlet对象初始化之后，如果有客户端请求这个Servlet的话，Servlet容器就会调用这个Servlet对象的service()方法去处理客户端的请求以及生成响应；     
在Service方法当中有两个参数：     
```
javax.servlet.ServletRequest
javax.servlet.ServletResponse
```
这两个参数分别对表请求以及响应；       
`javax.servlet.ServletRequest`参数包含了从输入流读取的所有的Servlet请求信息。这里面就包括请求的参数信息；我们通过处理这些请求信息，来生成处理的结果，然后我们把处理结果写入输出流从而生成响应信息，也就是后一个参数ServletResponse；   
ServletRequest包含了Servlet的请求信息。我们可以使用ServletRequest当中的相应的方法来获得这些请求的信息，然后进行相应的处理，比如说我们可以查询数据库和数据库进行交互。然后把处理结果写入到ServletResponse对象中，从而生成相应的响应信息。

#### destory()方法： 
`destroy()`方法也是由Servlet容器进行调用的，同样的，它也只会执行一次。     
destroy是在服务器停止并且卸载Servlet对象的时候由Servlet容器进行调用的；       
在Servlet当中，destroy方法的主要作用就是在Servlet容器卸载Servlet对象的时候执行一些清理操作；       
比方说，我们可以释放一些内存的资源，或者我们可以关闭数据库的连接以及其他的清理操作；      
注意：        
当一个Servlet在运行Service方法的时候可能会产生其他的线程，比方说在Service方法当中可能创建自己的线程；当服务器卸载Servlet的时候，将在所有service()方法调用完成之后，或者是在指定的时间间隔过后调用destroy方法；这个时候，我们需要确认在调用destroy方法的时候，这些自己创建的线程已经终止或者完成。           


#### 其他方法：
`getServletConfig()`方法：         
返回传递到servlet的`init()`方法的ServletConfig对象；      
即，它可以得到servlet对象的初始化信息；      

`getServletInfo()`方法：      
返回描述Servlet对象基本信息的字符串；     


通过实现Servlet接口编写Servlet程序：      
示例：    
```java
public class MyServlet implements Servlet {
 private ServletConfig config;
 public ServletConfig getServletConfig() {
     return config;
}
public void init(ServletConfig config)throws ServletException {
 System.out.println("初始化！");
 this.config=config;
}
public void service(ServletRequest req,ServletResponse res)throws ServletException,IOException {
PrintWriter out=res.getWriter();
out.println("Hello World");
out.close();
}
public void destroy() {
System.out.println("释放资源！");
}
public String getServletInfo() {
 return this.getServletInfo();
  }
}
```

##### 继承javax.servlet.GenericServlet类：    
GenericServlet是Servlet的通用版本，是一种与协议无关的Servlet；    
即，这种Servlet它们不包含对HTTP协议或者任何其他协议的固有支持。如果我们需要创建一个基于某种协议的Servlet，我们可以在GenericServlet的基础上进行扩展。      
HttpServlet就是在GenericServlet基础上扩展的基于Http协议的Servlet。     
GenericServlet类不仅实现了Servlet接口，同时也实现了ServletConfig接口；     

##### 从GenericServlet的源代码片段可以看出：
GenericServlet对于Servlet接口中的四个方法都给出了默认的实现。唯一没有实现的就是Servlet接口中的service方法。因此，如果我们要编写一个通用的Servlet的话，我们只需要继承GenericServlet，并且实现其中抽象的service方法就可以了。      
那么如果我们要实现自己的初始化方法，或者destroy方法，那么，在继承GenericServlet类之后，可以在自己的类中重写GenericServlet当中的init方法、destroy方法以及其他方法；       



##### 继承javax.servlet.http.HttpServlet类：
HttpServlet的声明：    
```java
public abstract class HttpServlet extends GenericServlet implements java.io.Serialiazable {
  ......省略
}
```
HttpServlet代码片段：     
```java
public void service(ServletRequest req,ServletResponse res) throws ServletException,IOException {

HttpServletRequest request;
HttpServletResponse response;
try {
 request = (HttpServletRequest) req;
 response = (HttpServletResponse) res;
} catch (ClassCastException e) {
  throw new ServletException("non-HTTp request or response");
}
 service(request,response);
}
```

在HttpServlet类当中已经实现了GenericServlet类当中的抽象方法service；    
在HttpServlet类中，Service方法是一个处理请求的派发器。真正处理这些请求的方法是`DO***()`方法；   

|方法     |                   描述|
|-----|------|
|doGet |               处理HTTP GET请求|
|doPost|               处理HTTP POST请求|
|doPut  |              处理HTTP PUT请求|
|doDelete|             处理HTTP DELETE请求|
|doHead   |            处理HTTP HEAD请求|
|doOptions |           处理HTTP OPTIONS请求|
|doTrace    |          处理HTTP TRACE请求|

#### doGet方法：
当服务器收到HTTP GET请求的时候，service方法就会调用doGet方法来处理这个请求。   
在HTTP GET请求当中，这个请求所包含的数据信息都是用字符串的形式写在URL的后面的。并且这个字符串的长度是有限制的，这个限制通常是2k的字节。这个限制的非常死。而且用Get方式提交数据的时候，由于数据是直接显示在URL的后面，因此，安全性没有保障。所以，在我们向服务器提交数据的时候，我们往往使用HTTP POST请求。       

#### doPost：
在Servlet中为了处理HTTP POST请求，就需要在Servlet中调用doPost方法。   
同HTTP  GET请求不同，POST请求不需要吧传递的参数写在URL的后面，而且POST请求传递的数据量要比GET请求大很多。POST请求传输的数据量在理论上是没有限制的；      
HttpServlet中的doPost方法就是为了处理这类请求的；

#### doPut：
当服务器接收到HTTP PUT请求的时候由service方法调用的。     
处理的请求可以把数据上传到Web服务器当中。也就是说它可以用于上传数据。     
#### doDelete：
HTTP DELETE请求用于从Web服务器中删除数据或者资源。    
这种请求方式使用比较少，因为让用户删除服务器上的资源是一个非常危险的事情，所以不推荐使用；    
doDelete方法就是对这种请求方式的处理方法，但这种方法使用的情况比较少，不推荐使用；     
#### doHead：
处理 `HTTP HEAD`请求；    
HEAD请求和GET请求非常的类似，不同是：    
GET请求，它请求的是页面的所有信息，而HEAD请求只是请求页面的头部信息。也就是说，HEAD请求的信息是GET请求的一部分；    

#### doOptions方法：
处理HTTP OPTIONS请求；    
利用HTTP OPTIONS请求可以取得服务器支持的配置选项。     
#### doTrace：
在我们编写web应用的时候，有的时候往往需要让服务器给我们返回客户端发送的HTTP头信息，以方便我们进行调试。这也就是HTTP TRACE请求的作用。这种请求可以让服务器返回客户端发送过去的HTTP头信息。当HttpServlet接到这样的请求之后就会调用对象内部的doTrace方法。     

说明：   
在HttpServlet类中，这7个do方法都提供了默认的实现。    
如果我们在自己的HttpServlet子类中需要处理Get请求或者Post请求，或者其他类型的请求的话，我们就需要覆写其中相应的方法就可以了。     

Servlet程序的实现方式；    
继承`javax.servlet.http.HttpServlet`类；   

应该如何创建一个基于Http协议的Servlet：     
由于HttpServlet是一个抽象类；因此，如果我们要编写基于Http协议的Servlet程序的时候，并不能够直接使用new关键字实例化一个HttpServlet对象；     
而是，编写一个自己的Servlet类，让这个类继承自HttpServlet，然后在我们自己的Servlet类中实现HttpServlet抽象类中的抽象方法，或者我们覆盖这个类中的已经实现的方法。用这样的方式，就可以创建一个基于Http协议的Servlet；


## Servlet容器：
容器：   
容器就是用来包装或者存放物品的存储器。    

### Servlet容器：
Servlet容器就是用来装载Servlet对象的一种容器。    
也叫Servlet引擎，是能够部署和运行Servlet的一类程序；  是Web服务器或者应用程序服务器的一部分。    
我们所编写的Servlet程序要部署到Servlet容器当中，才能够运行；      
Servlet容器在Servlet的生命周期内负责包容和管理Servlet对象；     
Servlet容器是如何包容和管理Servlet对象：     
用户通过点击某一个超链接或者直接在浏览器中输入URL来访问Servlet的时候，当Web服务器在接收到这样的请求之后，它并不是将这个请求直接交给Servlet对象，而是把这个请求首先交给Servlet容器，在Servlet容器对相应的Servlet进行实例化之后，Servlet容器会调用Servlet对象的service方法对这个请求进行相应的处理；并且，在处理完成之后生成一个响应。然后，这个响应由Servlet容器返回给Web服务器，Web服务器对这个响应进行包装，
然后以HTTP相应的形式发送给Web浏览器进行展示。    

几个概念：   
### Servlet容器：
Servlet容器是负责容纳以及管理Servlet对象的一类容器。它负责servlet实例化、初始化、提供服务以及销毁整个过程；这个过程也称为Servlet对象的生命周期，即，负责管理Servlet的生命周期；     

### Web容器：  
Web服务器：    
Web容器就是Web服务器，它是用来管理和部署Web应用的；    
比方说我们开发了一个网站或者基于Web的应用程序，那么就需要部署到Web服务器上，让他运行起来。    

### 应用服务器：
应用服务器的功能比Web服务器要强大的多。   
因为它除了可以部署web应用之外，还可以部署EJB应用，还可以实现容器管理的事务；   
一般的应用服务器有WebLogic和Websphere等等；它们都是商业服务器，功能十分强大，但是都是收费的；    
而Web服务器的选择就比较多了。    
当我们在一个Web项目中使用了基于Java技术的Web服务器或者使用支持Java的Web服务器的时候，这样的服务器本身都可以作为Servlet容器。    

比方说，Tomcat，Tomcat是一个免费的开放源代码的Web服务器，同时也是Servlet容器。    

Java项目的开发，多数使用Eclipse集成开发环境；      
开发JavaEE项目，习惯使用Eclipse插件集合MyEclipse；   


##### 如何使用Servlet：
Servlet并不是独立运行的，而是要运行在Servlet容器当中，Servlet是由容器进行管理的；
因此，在使用这个Servlet之前，我们首先要对这个Servlet进行必要的配置。以便告诉Servlet容器，如果有客户端请求访问这个Servlet的时候，访问名是什么，以及这个Servlet的类路径是什么。也就是我们开发的Servlet完整的路径是什么；另外，我们还可以告诉Servlet容器，这个Servlet在什么时候加载到容器当中，是在服务器启动的时候加载，还是在第一次使用的时候加载，等等。
这些项目都是可以配置的；    
这样做也提高了Servlet开发的灵活性；     

##### Servlet在哪里进行配置：
在Web.xml中对Servlet进行配置；   
Web项目的目录结构中，在`WEB-INF`目录下，有一个`web.xml`文本，这个文件的作用就是用来初始化web项目的配置信息的；
比如说，在Web.xml中我们可以配置欢迎页面等；    
我们开发的Servlet也是在这个文件中进行配置的；     


### 什么是生命周期：
生命周期就是一个事物从产生到消亡的过程；  

普通Java对象的生命周期：     
创建、初始化和使用Java对象，是由我们控制的，Java对象的销毁由Java的垃圾回收机制来进行处理；    

#### Servlet的生命周期：
Servlet对象是由Servlet容器进行管理和控制的；   
Servlet对象的创建、初始化、调用、以及销毁等各步骤都由Servlet容器进行控制；而不是我们自己进行控制的；   
即，Servlet对象的生命周期是由Servlet容器进行管理和控制的；    

#### Servlet生命周期的各个阶段：
- 实例化阶段：
所谓Servlet的实例化是指根据servlet的类生成servlet对象的过程；   
Servlet的实例化是由Servlet容器来完成的；我们并不能控制这个过程；   
根据servlet的实例化时机不同，我们可以把Servlet的实例化分为两种情况：    
1.`web.xml`中设置为Web服务器启动时实例化；    
如果我们在`Web.xml`中配置Servlet的时候，把一个Servlet设置为服务器启动的时候就加载这个Servlet的话，那么当我们的Web服务器，例如说Tomcat启动的时候就会实例化这个Servlet。   
2.客户端第一次请求时实例化；      
如果我们在`web.xml`中配置Servlet的时候，没有指定服务器在启动的时候就加载这个Servlet的话，那么这个Servlet的实例化就不会在Web服务器启动的时候进行，而是当客户端浏览器第一次请求这个Servlet的时候，这个Servlet才会被实例化。    

一个Servlet不论是在Web服务器启动的时候就进行了实例化，还是在客户端请求的时候进行实例化。这个实例化的过程只会进行一次。

### 初始化阶段：
Servlet容器在对一个Servlet进行实例化之后，就会调用这个Servlet对象的init方法对这个Servlet进行初始化；   
init方法在Servlet的整个生命周期中只会执行一次。   
在Servlet的通用版本也就是GenericServlet当中，已经对init方法做了基本的实现；   
因此我们在开发基于GenericServlet或者基于Http协议的Servlet的时候，如果我们没有特殊的初始化需求，就没有必要覆写这个init方法了；    
但是，在实际的开发过程中，除了基本的初始化之外，我们往往还需要做某些个性化的初始化动作；   

注意：   
在覆盖父类的init方法时候，必须要有`super.init(config)`;  这行代码；   
这行代码的作用就是调用父类中基本的初始化操作进行基本的初始化；如果没有，那么这个Servlet的一些配置信息就不能设置到Servlet中了；     

### 服务阶段：
建立Servlet的目的就是为了能够处理客户端的请求，并返回响应；  
当一个客户端浏览器，向Web服务器请求一个Servlet的时候，Servlet容器就会调用这个Servlet对象的`service()`方法，用来处理客户端请求。      
在基于HTTP协议的Servlet中，service方法会根据请求类型的不同调用不同的`do***`方法。      
即，HttpServlet类中的service方法完成了由一个方法向多个方法的转换的过程。而且细化了service方法的功能。    

注意：    
在HttpServlet中提供的`doGet、doPost`方法等等，这些以do开头的方法都是运行在多线程状态下的；即，在web服务器启动之后，可能会有多个客户端浏览器同时访问同一个Servlet，为了提高程序并发处理的能力，Web服务器会开设多个线程来处理不同的请求；而多个线程处理同一个对象的时候，有可能会出现数据并发访问的错误。因此，在Servlet中处理一些共享资源的时候可以考虑加上同步控制。    

### 销毁阶段：
当程序中的Servlet对象不再使用的时候，或者Web服务器停止运行的时候，就需要调用`destroy()`方法销毁这些Servlet对象，destroy方法在Servlet的整个生命周期中只会执行一次。    
不同的Web服务器销毁Servlet对象的时机可能有所不同，但是他们都会调用Servlet的destroy方法；   
在destroy方法中，可以做一些释放资源的清理工作。    
比如说，可以关闭数据库连接、关闭I/O流或者终止一些后台的线程等收尾工作；   


### 配置和部署Servlet：
Servlet技术：   
`web.xml`文件：   
每一个Web项目都需要在`WEB-INF`目录下包含一个`web.xml`文件；在`web.xml`中配置Servlet这个文件称之为部署描述文件；    
这个文件在程序运行Servlet的时候起着一个总调度的角色；它会告诉Servlet容器如何运行Servlet；   
`web.xml`文件：      
XML声明：它会将这个文件标识为XML文件，而不是其他类型的文件；   
`<web-app>`元素：    
这是一个web项目的部署描述文件的根元素。  
一个web项目其他的所有配置元素都放在这个标记之中；   
比如说，用于配置Web项目的欢迎页面的配置项就放在`web-app`元素之间；  
`<welcome-file-list>`配置元素用来配置欢迎页面的元素；这个元素用于让我们定义首页的列表；   
这个配置元素中可以配置多个子项，因为它是一个列表；在这个列表当中，可以配置多个`<welcome-file>`元素；   
如果配置了多个首页文件信息，也就是`<welcome-file>`元素，当配置的第一个欢迎页面没有找到指定的文件的时候，那么Web服务器就会尝试着打开配置的第二个欢迎页面；以此类推；    


### 配置Servlet：
在`web.xml`中配置Servlet的过程分为两步，分别是：   
- 第一步，把Servlet的实现类的类名及路径映射为一个Servlet别名。  
这个别名，是让Servlet容器来使用的；   
- 第二步，再把这个别名映射为客户端访问此Servlet时的URL规则；   

在`web.xml`中配置Servlet的时候，需要两类配置元素，它们分别是：  
- <Servlet>元素：   
作用：把一个Servlet的实现类的路径和名字映射为一个Servlet的别名；   
基本格式：   
```
<servlet>
<servlet-name>HelloServlet</servlet-name>
<servlet-class>javae.sg.HelloServlet</servlet-class>
<load-on-startup>1</load-on-startup>
<description>我的第一个Servlet</description>
</servlet>
```
在配置元素<Servlet>中，包含了这样几个子元素，分别是：   
```
sevlet-name servlet-class load-on-startup 
description ;
```
- sevlet-name元素：
为这个servlet指定了内部使用的名字；也就是这个Servlet的别名；这个别名是我们自己起的，不一定和类名相同；但是最好在起这个名字的时候能够具有一定的实际意义；    
- servlet-class元素：
指定了我们正在配置的这个Servlet的实现类的完整的类路径以及名字；以便Servlet容器在加载这个Servlet的时候，能够知道去哪里加载以及加载哪个类；   
有了`sevlet-name`以及`servlet-class`这两个配置元素以后，我们就可以把一个Servlet的实现类映射到一个别名上面了。这两个元素是配置Servlet的时候必须要配置的项。    

其他的可选配置项：    
- `load-on-startup`元素：
它指定了我们正在配置的这个Servlet的加载时机。   
加载Servlet就意味着实例化这个Servlet，并调用它的init方法；`load-on-startup`元素通常设置一个正整数；   
这个正整数的值就表示一个Servlet由Web容器装入内存的顺序；   
举例：   
如果有两个Servlet元素都含有`load-on-startup`元素，那么`load-on-startup`元素的值较小的一个将先被加载，而值较大的一个后被加载；   
如果`load-on-startup`子元素的值为负值，那么说明这个servlet在web服务器启动的时候将不会自动被加载，而是在客户端第一次调用servlet的时候才进行加载；   

- description元素：  
用于描述这个Servlet，是对我们正在配置的这个Servlet的一个说明和简介。   


- `<Servlet-mapping>`元素：   
作用：把Servlet的别名映射为访问此Servlet的URL规则；   
在我们配置了<servlet>这个元素之后，我们就成功的把一个Servlet实现类映射为了一个别名；  
但是访问这个Servlet的时候，只需通过URL去访问；   
也就是说，我们为了在Web应用中使用这个Servlet，还需要把这个别名映射为访问Servlet的URL规则；  
即，当一个请求的URL满足什么样的规则的时候，web服务器就会把这个请求交给这个Servlet对象进行处理；这时就需要用到`<servlet-mapping>`元素；   
基本格式：   
```
<servlet-mapping>
<servlet-name>HelloServlet</servlet-name>
<url-pattern>/helloServ</url-pattern>
</servlet-mapping>
```
在`servlet-mapping`元素当中，可以包含并且必须包含这样两个子元素，分别是：  
- `<servlet-name>`元素：   
就是刚才配置<servlet>元素的时候，为Servlet映射的别名。这里的`Servlet-name`，要和刚才设置的那个名字要完全一致，不管大小写还是字母的顺序都要完全一致才可以；    

- `<url-pattern>`元素：
用于设置客户端浏览器在访问servlet的时候，URL规则是怎么样的；  
当一个请求的URL符合设置的规则的时候，servlet容器就会根据`<servlet-name>`元素当中的servlet别名去查找servlet的一个实现类，然后再把这个请求转交给servlet的实现类去进行处理；   

##### <url-pattern>元素的配置方法：
有三种配置规则：   
- 精确匹配(exact match):   
所谓的精确匹配，就是我们在使用浏览器访问一个Servlet的时候，我们输入的URL除了工程名之前的部分之外还要和我们在配置Servlet时候配置的`url-pattern`元素要完全一致。     
示例：   
1.使用一个斜杠，后面加上一个字符串作为访问的URL：   
```java
<servlet-mapping>
<servlet-name>HelloServlet</servlet-name>
<url-pattern>/helloServ</url-pattern>
</servlet-mapping>
```
假如这个工程的名字是TestServlet，Web服务器的端口号为8080；则，在本地访问此Servlet的完整的URL为：      
```
http://127.0.0.1:8080/TestServlet/helloServ
```
其中，`127.0.0.1`是保留的IP地址，指代本机；

2.使用虚拟目录；   
示例：    
```
<servlet-mapping>
<servlet-name>HelloServlet</servlet-name>
<url-pattern>/myServlets/helloServ</url-pattern>
</servlet-mapping>
```
`/myServlets`是一个虚拟的目录，它不一定要确实存在；   
在本地访问此Servlet的完整的URL为：   
```
http://127.0.0.1:8080/TestServlet/myServlets/helloServ
```

### 目录匹配(directory match):
相对于精确匹配而言，目录匹配是一种相对灵活的配置方式；在这种匹配方式当中，在`url-pattern`元素当中，以斜杠开始，以星号结尾，并且规定了一个虚拟的目录；   
示例：   
```
<servlet-mapping>
<servlet-name>HelloServlet</servlet-name>
<url-pattern>/helloServ/*</url-pattern>
</servlet-mapping>
```
`*`号的意思就是添加其他虚拟目录或者文件名；   
当使用这种配置方式的时候，访问这个Servlet的时候输入的URL就会有多种选择，但只需要保证这个URL当中，除了工程名之外的部分以这个虚拟目录helloServ开始就可以了；     
在这个虚拟目录后面，我们可以添加其他虚拟目录或者文件名，也可以不加；   
在本地访问此Servlet的完整的URL为：    
```
http://127.0.0.1:8080/TestServlet/helloServ/thServlet
```
theServlet是我们在访问的时候随机输入的一个名称。这个名称可以替换为其他的名称；   


### 后缀匹配(extension match):
在这种方式中，`url-pattern`配置元素只限定了访问Servlet的后缀名；而对于虚拟目录、名称都没有要求；即，输入的URL当中，只要后缀名符合配置的`url-pattern`，就可以访问我们所配置的Servlet；   
示例：   
```
<servlet-mapping>
<servlet-name>HelloServlet</servlet-name>
<url-pattern>*.do</url-pattern>
</servlet-mapping>
```
在本地访问此Servlet的完整的URL为：   
```
http://127.0.0.1:8080/TestServlet/helloServ.do
```
注意：    
一个Servlet元素可以同时对应多个`Servlet-mapping`元素；  
当我们在`web.xml`当中为同一个Servlet配置了多个`servlet-mapping`的时候，那么servlet容器首先将按照精确匹配的方式进行匹配查找，如果匹配完成，那么结束匹配；而如果匹配不成功就按照目录匹配以及后缀匹配的顺序依次进行匹配查找；   
那么如果有多个目录匹配方式的配置方式，同时满足要求，将安装最长的一个来进行匹配。也就是最精确额一个来进行匹配，同时，在进行匹配的时候要看请求的URL,如果这个URL以斜线结尾，那么这个请求将会按照目录匹配的方式来进行处理。   


#### 什么是war文件：
要知道如何发布开发好的Web应用，这样，经过发布的Web应用才能为用户服务。  
如何使用war文件的方式部署我们的Web应用：   

war文件：      
war 是`web archive`的缩写，简称WAR，是指打包以后的web应用。   
JAR 是 `JAVA ARCHIVE`的缩写，是指Java应用程序的归档文件。      
因此，JAR文件用于打包Java应用，而WAR用于打包WEB应用。   

#### 如何制作WAR文件：
直接使用命令行生成：  
给Web项目打包的命令：   
`jar cvf war`文件名称`.war`  要打包的文件     
示例：    
当前的路径是在MyWebApp目录下，打包命令为：   
```
jar cvf MyWebApp.war *.*
```

另外，如果使用MyEclipse，还可以直接使用MyEclipse提供的导出菜单，导出为war文件。   
生成了war文件之后，如何来部署这样的war文件：    
不论使用哪种方式，在生成了这个项目的war文件之后，把这个文件直接放在Tomcat的webapps目录下面，然后启动Tomcat就可以了。这就是web项目打包发布的整个过程。    


### Servlet常用接口：

#### HttpServletResponse接口：
- 这个接口是基于Http通信协议的；  
- 这个接口是有关HTTP响应信息的；   
-  这个接口就是用于封装HTTP响应信息，允许我们操作HTTP协议的相关数据，支持Cookie和Session跟踪，并且在这个接口中定义了一系列的描述HTTP状态码的常量；   
Web服务器发送给Web客户端的HTTP响应信息，共分为三个部分：状态行、响应信息头以及消息正文也就是发送的实体内容。  
HttpServletResponse接口就是用于封装这些信息并提供了相应的方法，让我们操作HTTP协议中的相关数据,
包括设置和修改响应头和状态码，这个接口支持Cookie和Session跟踪，并且在这个接口中定义了一系列的描述HTTP状态码的常量；

##### HttpServletResponse接口的常用方法：
常用方法：  
- addCookie(Cookie cookie)   
通过调用这个方法，我们可以把参数中指定的cookie对象添加到这个响应当中；   
Cookie对象的作为：     
cookie对象在我们web开发过程中是经常用到的对象，cookie可以用来进行用户状态的跟踪等；  
在同一个response对象中，我们可以多次调用这个addCookie方法，从而向这个响应当中添加多个cookie对象。     

- containsHeader(String name)    
判断在这个响应当中一个指定名称的Header对象是否已经被设置。那就需要使用containsHeader方法。  
这个方法需要一个字符串类型的参数，这个参数用于指定要检查是否设置的Header的名称。    
这个方法的主要作用是经常用来在设置一个消息头之前，检查这个消息头是否已经存在。    
在web开发当中，我们经常需要跟踪客户的状态；而在Java Servlet API当中引用了Session机制来跟踪客户的状态；    
Session简单的说，就是一个会话；     
当一个会话开始的时候，Servlet容器将会创建一个HttpSession对象，并为这个对象分配一个唯一的标识符，称为Session ID;
这个Session ID是全局的，唯一的，它用来区分不同的session；       
Servlet容器将Session ID作为Cookie保存在客户的浏览器中。每次客户发出HTTP请求的时候，Servlet容器可以从请求对象中读取Session ID，然后根据这个`Session ID`找到相应的HttpSession对象。从而获取客户的状态信息。     

- encodeURL(String url)   
当客户端浏览器中设置为禁止Cookie的情况下，Servlet容器将无法从客户端浏览器中取得作为Cookie的Session ID，这种情况下，也就无法利用这种方式去跟踪客户状态；    
为了解决这种问题，在Servlet中提供了另外一种跟踪用户状态的机制；如果客户端浏览器不支持Cookie，那么Servlet容器可以重写客户请求的URL，它会把这个`Session ID`添加到URL信息当中；       
这样的话，在web服务器接收到URL的时候，一样可以得到这个`Session ID`。   
encodeURL方法就是HttpServletResponse接口为我们提供的重写URL的方法；这个方法有一个参数，这个参数就是要重写的URL；   
##### 这个方法的执行机制：
- 先判断当前的Web组件是否启用了Session，如果没有启用Session，直接返回参数中指定的url。   
- 如果启用，再判断客户端浏览器是否支持Cooke，如果支持Cookie，则直接返回参数中指定的URL，它可以完全利用Cookie去跟踪客户的状态，这个时候就没有必要重写URL了；如果不支持Cookie，那么就在参数指定的URL中加入`Session ID`信息。然后返回修改之后的URL。    

- sendError(int sc,String msg)   
这个方法用于向客户端发送一个错误的响应；这个响应的状态码就是第一个参数sc所指定的状态码；而第二个参数的意思就是错误的描述信息。    
示例：   
```
resp.sendError(HttpServletResponse.SC_NOT_FOUND,"您请求的资源不存在");
```
请求一个根本不存在的资源的时候，就使用sendError方法；`HttpServletResponse_SC_NOT_FOUND`是资源未找到的状态码；它的意思就是表示我们请求的资源是不存在的；后面的第二个参数是描述错误的描述信息。   

- sendRedirect(String location)
这个方法用于重定向一个URL；  
当我们使用一个URL去访问某一个页面或者请求一个资源的时候，我们把这个请求重定向到另外一个页面或另外一个资源，这就是重定向URL；   
当我们使用sendRedirect方法重定向一个URL之后，我们在客户端浏览器当中看到的URL地址将会变为新的资源的URL地址。而不会保留原来的URL地址。    
在这个方法当中也可以接受相对的URL，但是在发送响应之前Servlet容器会把我们写的相对的URL，转化为绝对的URL。   
如果相对URL不以`/`开头，容器将解释为相对当前请求的路径URL；       
如果是以`/`开头，那么容器就会把它解释为是相对与Servlet容器的根目录环境；      

- set/addHeader(String name,String value)   
这两个方法的作用是使用给定的名称和值去设置或者添加一个响应头。  
 
如何这个时候，要设置的响应头已经存在的话：      
如果我们使用setHeader方法设置一个已经存在的响应头信息，那么原来已经存在的值将会被这个方法的第二个参数所指定的那个值所覆盖；也就是说，原来同名的头信息的值将会丢失；那么如果我们不想让原来已经有的值丢失的话，这个时候我们在使用setHeader方法的时候，可以先使用containsHeader方法去测试一下，看指定名称的头信息是否已经存在。如果已经存在，我们不想覆盖，可以使用另外一个方法，addHeader方法；而如果还没有存在，我们可以使用setHeader方法对它进行设置；      

- setStatus(int sc,String sm)  
用于在没有错误的情况下设置HTTP响应的状态码；   
比方说，我们可以把返回的状态码设置为`SC_OK`，或者`SC_MOVED_TEMPORARILY`，就表示正常。  
如果有错误存在的情况下，并且我们希望调用一个在web项目中定义的错误页面的话，这种情况下我们可以使用sendError方法而不是setStatus方法进行处理。   


HttpServletResponse接口的常用常量：   

|常量    |                         说明|
|----|---|
|SC_OK|请求被正常处理，Http响应的状态码是200；这个状态码表示客户端的请求被服务器正常处理的情况|
|SC_ACCEPIED|请求已经被服务器接受，但是尚未处理，此状态码的值是202；|
|SC_BAD_REQUEST|客户端发送过来的客户端请求有语法错误，此状态码的是是400；|
|SC_UNAUTHORIZED|一个请求没有语法错误，但是试图去访问未经授权的资源，此状态码的值是401；|
|SC_NOT_FOUND|如果一个请求没有语法错误也没有试图访问未经授权的资源的时候，而是试图访问根本不存在的资源；此状态码的值是404；|


### ServletConfig接口：
ServletConfig接口对象包含了servlet的初始化信息。  
在Servlet容器初始化一个servlet对象的时候，servlet容器就会传递一个ServletConfig类型的对象给这个Servlet的init方法，以便在Servlet的init方法当中获得初始化参数。   
ServletConfig接口的常用方法：   
- getInitParameter(String name)   
根据给定的初始化参数名，返回匹配的初始化参数值；     
这个初始化参数值是我们在servlet配置的时候，在web.xml当中进行配置的；如果根据给定的参数名没有找到任何参数值的时候，这个函数将返回null中，也就是控制。 

- getInitParameterNames()
返回所有初始化参数名；即，返回一个Enumeration对象，包含了所有的初始化参数名；   
注意：    
它返回值的类型并不是字符串类型，而是Enumeration这个对象类型；   

- getServletContext()  
返回当前web应用的ServletContext对象；   

- getServletName()  
返回当前Servlet的名字；   
这个名字就是在`web.xml`文件中配置的<servl<vlet-name></vlet-name>ervlet-name>`子元素的值。     
如果没有为servlet配置`servlet-name`子元素，当调用`getServletName()`方法的时候，则会返回Servlet实现类的名字。  


#### Servlet中的初始化参数：
它包含一对参数名和参数值；  
在`web.xml`当中配置<servlet>元素的时候，可以通过<servlet>的子元素<init-param>元素的子元素来设置初始化参数。   
<init-param>元素的<param-name>子元素设定参数名，<param-value>子元素设定参数值。   


### ServletContext接口：
这个接口是Servlet对象和Servlet容器之间进行通信的接口；  
Servlet容器在启动一个Web应用的时候就会为这个web应用创建一个ServletContext对象；  
每个Web应用都拥有唯一的一个ServletContext对象；   
同一个Web应用下的所有Servlet对象都共享一个ServletContext；   
Servlet对象可以通过共享的同一个ServletContext对象来访问Servlet容器当中的各种资源。  
在ServletContext接口中，定义了很多个用于Servlet对象与Servlet容器进行通信的方法；   
#### 根据这些方法作用的不同，分为：   
##### 在Web应用范围内存取共享数据的方法：   

- setAttribute(String name,Object object)     
把一个Java对象和一个属性名绑定，并存放到ServletContext中；  
这样一来，这个参数名以及对象就可以在整个Web范围内进行共享了；     
参数name指定要绑定的属性名；       
参数object表示要共享的数据；      

- getAttribute(String name)   
使用setAttribute方法将一个Java对象存放到ServletContext中之后，如何得到刚才设置的Java对象：就要使用getAttribute方法；
这个方法根据给定的属性名返回一个Object类型的Java对象；      
其中，这个给定的属性名要和设置这个属性的时候的属性名相一致；   
如果Servlet容器中不存在指定名称的属性的话，这个方法将会返回null 值；   

- getAttributeNames()  
当我们向ServletContext中设置了多个属性的时候，我们可以通过getAttributeNames()方法返回一个Enumeration对象，这个对象包含了所有存放在ServletContext中的属性名；   

- removeAttribute(String name)   
当我们决定不再需要某一个属性的时候，就可以使用removeAttribute方法根据这个方法的参数指定的属性名，从ServletContext中删除和这个参数匹配的属性；   

##### 访问当前Web应用资源的方法：
这些资源包括当前Web应用的初始化参数，以及其他一些在`web.xml`中配置的信息等。  

- getInitParameter(String name)   
根据指定的参数名称返回Web应用范围内的初始化参数值；  
eb应用范围内的初始化参数值也可以在`web.xml`文件中进行配置；在`web.xml`文件当中，我们可以直接在<web-app>根元素下，来定义<context-param>元素表示应用范围内的初始化参数。    
如果调用getInitParameter方法的时候，指定名称参数不存在，就返回null值，也就是空值。   
在ServletConfig接口当中的getInitParameter方法所返回的初始化参数在`web.xml`配置Servlet的时候进行配置；也就是说，在Servlet元素下面有一个子元素，那就是<init-parameter>元素，在这里进行配置；    
而ServletContext接口当中的getInitParameter方法，是在<web-app>根目录下进行定义的；  

- getInitParameterNames()   
返回一个Enumeration对象，这个对象中包含了所有当前Web应用范围内的初始化参数的参数名；   

- getServletContextName()  
返回当前Web应用的名字，这个名字是指在web.xml配置文件中<display-name>元素的值；  

- getRequestDispatcher(String path)  
返回一个用于向其他Web组件转发请求的RequestDispatchar对象；  
返回的这个RequestDispatcher对象可以用来把请求转发到指定的资源当中；  
而这个指定的资源可以是静态的，也可以是动态的；   


##### 访问Servlet容器相关信息的方法：

- getContext(String uriPath)  
访问同一个Servlet容器中，其他Web应用的ServletContext对象；  
通过调用这个方法，就可以在当前的Servlet当中，访问同一个服务器中不同的Web应用的信息。  
作用：    
根据指定的URL返回同一个Web服务器上指向这个URL的ServletContext对象；   

#####  访问Web容器相关信息的方法：

- getMajorVersion()  
返回Servlet容器支持的`Java Servlet API`的主版本号；  

- getMinorVersion()  
返回Servlet容器支持的`Java Servlet API`的次版本号；  

- getServletInfo()  
返回Servlet容器的名字和版本；   

##### 访问服务器端文件系统的方法：
这些方法的作用主要是用于访问指定路径下的资源。  

-  getRealPath(String path)   
这个方法需要传入一个字符串类型的变量，表示一个路径；  
这个方法会根据参数指定的路径（虚拟路径）返回该路径在服务器上的绝对路径；即，文件系统中的一个真实的路径；     

- getResource(String path)  
返回一个映射到参数指定的路径的URL；  

- getResourceAsStream(String path)  
返回一个用于读取参数指定的文件的输入流；  
也就是把文件读到输入流中去。   

- getMimeType(String file)  
返回参数指定的文件的MIME类型；   

##### 输出日志的方法：

- log(String msg)  
向Servlet的日志文件中写日志；  
其中写的内容就是参数指定的字符串；  

- log(String msg,Throwable throwable)   
向Servlet的日志文件中写错误日志，以及异常的堆栈信息；  
在向Servlet的日志文件中写日志的时候，它不但写入指定消息，而且它把异常的堆栈信息输出到Servlet日志文件中；因此，有两个参数的log方法，它是向servlet日志文件写错误日志的，它不但把第一个参数字符串写到日志中，而且最重要的是把异常的堆栈信息打印到日志文件当中，这样就方便了我们对错误的排查；     
