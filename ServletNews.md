
# 新闻管理系统：

概述：   
JSP负责显示，Servlet负责管理；  
回顾到现在为止已经实现的新闻管理系统，且不说功能，该系统无外乎包含两大部分：JSP以及JavaBean。  
接下来，回想使用JSP做了什么：     
首先，接收请求，然后调用相应的JavaBean去处理请求；  
当然，这里的JavaBean，我们用的是service；   
还有就是：显示数据；   
再回想一下使用JavaBean做了什么？  
封装数据和业务逻辑处理；   

看这样两个JSP页面：  
新闻列表显示页面；    
在这个页面中显示与请求处理与调用JavaBean的Java代码混在一起；  

新闻添加处理页面：     
这个JSP页面仅做了接收请求与处理请求的工作，并没有显示；  
可是这时就有个问题：JSP其实是一种负责显示的技术，但是JSP做了很多的事情，如何只专注显示？  

解决问题：   
将显示与接收请求处理请求的工作分开；  
这样，JSP只专注显示，而Servlet负责接收并处理请求；  

### 初识Servlet
Servlet做了什么？  
以新闻添加为例，从刚才的视频中大家可以看到Servlet本身不做业务处理；只是进行逻辑判断，结果由JSP显示，业务处理调用Service；   
Servlet只是接收用户请求拿到新闻信息，并决定调用哪个Javabean去处理请求；最后确定用哪个页面来显示处理返回的数据；   
比如这里使用新闻列表页面来显示了全部的新闻信息；   
##### 从案例中直观感受了Servlet的作用；
Servlet是什么？  
`Server+Applet`，是一种服务器端的Java应用程序；  
Servlet其实是两个单词的合成：server以及applet；   
顾名思义，它就是一种服务器端的Java应用程序；   
强调：  
并不是所有在服务器端的Java应用程序都是Servlet；  
只有当一个服务器端的程序使用了JavaServlet应用程序接口，也就是`Servlet  API`的时候，这个服务器端的程序才能称之为Servlet；      


### Servlet API
主要servlet API介绍：   
#### Servlet接口：`javax.servlet.Servlet`接口：
它位于`javax.servlet`包中；  
它是所有Servlet的基础接口类，它规定了必须由Servlet具体类实现的方法集；  

#### javax.servlet.GenericServlet类：  
它同样位于`javax.servlet`包中，是Servlet的通用版本；  
是一种与协议无关的Servlet。     

#### javax.servlet.http.HttpServlet类：
它位于`javax.servlet.http`包中；   
它是在GenericServlet基础上扩展的基于Http协议的Servlet；  

### 如何创建Servlet？
实现Servlet接口；  
继承GenericServlet类；  
继承HttpServlet类；   

要编写一个Servlet，其实就可以通过实现或者继承以上三个`servlet API`的方式来完成；  


##### 第一个Servlet  
Servlet中主要方法：   
- init()方法是servlet的初始化方法，仅仅会执行一次；  
- service()方法是处理请求和生成响应的方法；   
- destroy()方法在服务器停止并且程序中的Servlet对象不再使用的时候调用，只执行一次；   


#### 方法中的参数类型：
它们也属于`Servlet API`，其中：   
ServletRequest：封装客户的请求信息；  
这和JSP内置对象request作用是一样的；   
ServletResponse：得到请求进行相应的处理，并将处理结果返回给客户端；  
相当于JSP内置对象response的作用；   
Servletconfig：它是初始化一个Servlet对象的时候被创建的；在ServletConfig对象中包含了servlet的初始化参数信息；    


#### Servlet生命周期
包括加载和实例化、初始化、提供服务和请求处理以及销毁；  
##### 加载和实例化：   
加载和实例化由Servlet容器负责；Servlet容器就是用来装载Servlet对象的一种容器；是负责管理Servlet的一类组件；当Servlet容器启动或者在容器检测到客户端请求时，创建Servlet实例；   
##### 初始化：
初始化依然由Servlet容器负责；在实例化后，容器调用Servlet的`init()`方法初始化对象。   
##### 提供服务、请求处理：     
当得到客户端请求并做出处理时，Servlet容器提供服务并请求处理；   

注意：     
针对每个客户请求，容器会开始一个新的线程。多线程响应。这也是Servlet的优点；   
##### 销毁：       
当程序中的Servlet对象不再使用的时候或者WEB服务器停止运行的时候，Servlet容器负责销毁Servlet对象。     
注意：   
整个生命周期都是Servlet容器来负责，而不是我们自己控制的；而且，加载和实例化、初始化和销毁都只进行一次；    


#### 部署运行Servlet：
其实，创建完Servlet后只需要修改web.xml文件就可以完成对Servlet的配置；  
`web.xml`是部署描述文件，在程序运行Servlet时，起到一个"总调度"的作用；   
`web.xml`会告诉容器如何运行Servlet和JSP文件；     
在`web.xml`中需要添加元素<servlet>;      
元素<servlet>可以把Servlet内部名映射到一个Servlet类名，当然这个类名为包名+类名；   
还要添加元素<servlet-mapping>;   
元素<servlet-mapping>把用户访问的URL映射到Servlet的内部名；   
注意：  
<servlet-mapping>与<servlet>中的<servlt-name>必须一致；   

做好了这些工作，部署的工作就搞定了。接下来就可以通过URL访问Servlet了；  


#### 使用Servlet实现新闻信息的添加
分析：   
新闻添加，需要接收添加后的新闻内容并封装成新闻对象；调用Service方法完成业务逻辑，并返回响应页面；也就是添加新闻后的新闻列表页面；这是一个请求和响应的过程；   
因为基于HTTP协议，所以这里的请求会使用到HttpServletRequest对象；这个类位于`javax.servlet.http`包中，它继承与ServletRequest；     
响应则会用`javax.servlet.http.HttpServletResponse`对象，它继承于ServletResponse；  
要拿到新闻信息，需要使用到HttpServletRequest的`getParameter()`方法；  
页面跳转可以使用请求对象的`getRequestDispatcher()`;   
或者响应对象的`sendRedirect()`；    
其实，也可以通过请求对象的`getSession()`可以返回和此次相关联的HttpSession；    
