
# 过滤和监听：  
专题任务：   
使用过滤器解决乱码；   
使用监听器统计在线人数；   

## 什么是过滤器
在之前的新闻管理系统中不管是使用JSP还是Servlet，都要设置请求和响应的编码方式，显得很繁琐；  
有更方便的办法解决这个问题吗？   
其实用过滤器就可以解决这个问题；   

## 什么是过滤器：
过滤器是向Web应用程序的请求和响应添加功能的Web服务组件；通俗点说，过滤器也是一个类；这个类可以在访问JSP或是其他web资源之前先访问到请求，还可以在访问了JSP或是其他web资源之后得到响应，进而对响应进行控制；  
使用过滤器可以在整个网站中统一的集中处理请求和响应；这样就可以把整个网站中所有的设置请求和响应的编码工作，放到过滤器中解决了；   
### 过滤器的工作方式：
用户访问web资源的时候，发送的请求会先经过过滤器；  
这时，我们可以对请求对象进行控制和操作，然后过滤器将用户的请求发送至Web资源；  
访问web资源之后，Web资源响应时还会经过过滤器；   
这时，我们可以对响应对象进行控制和操作，最后过滤器将Web资源的响应发送给用户；  

### 小结：
#### 过滤器的使用步骤：
- 首先要建立一个实现了Filter接口(javax.servlet.Filter)的类；  
- 然后在doFilter方法中实现过滤的行为；  
- 在doFilter方法中需要调用下一个过滤器或Web资源；  
- 最后，还要在`web.xml`中配置过滤器，filter中的`filter-name`用来指定过滤器名；要与`filter-mapping`中的`filter-name`一致；`filter-class`是过滤器的完全限定名；`url-pattern`用来指定过滤器映射的Web资源，它有这样几种写法：   
```
完全匹配：  /index.jsp
目录匹配：  /admin/*
扩展名匹配：  *.do
全部匹配：  /*
```
在匹配的时候会首先查找完全匹配，如果找不到，再找目录匹配，然后是扩展名匹配，最后是全部匹配；   


### 过滤器的生命周期：
过滤器也有和Servlet一样的两个方法，就是init和destroy；其实不光是这两个方法，过滤器与Servlet的生命周期都是很像的；  
在访问Web资源之前，容器负责创建过滤器的实例，来完成实例化的工作，实例化仅需做一次；    
在真正的过滤工作之前会调用`init()`方法来完成初始化。初始化的工作也仅做一次；      
接下来，每当用户提交请求时，会调用`doFilter()`方法，完成过滤的工作，在整个生命周期中过滤方法会被调用多次；     
在停止使用过滤器之前，由容器调用过滤器的`destroy()`方法，允许过滤器完成必要的清除和释放资源；这个阶段就是销毁；销毁在生命周期中也只做一次；   


##### 初始化参数和过滤器链
跟Servlet一样，我们可以为过滤器设置初始化参数；  
```
<filter>
.......
  <init-param>
      <param-name>Encoding</param-name>
      <param-value>UTF-8</param-vaule>
  </init-param>
</filter>
```
也可以在init方法中读取过滤器的初始化参数；   
```
init(FilterConfig fConfig) {
  String encoding=fConfig.getInitParameter("Encoding");
}
```

在一个Web应用中，可以创建多个过滤器，这些过滤器会形成一个过滤器链，来进行多个过滤操作或是检查。  
当客户端请求Web资源时，过滤器1会进行过滤，而后调用过滤器2，过滤器中的最后一个过滤器过滤请求后，会访问Web资源；然后，再返回到过滤器2，再返回到过滤器1，最后把响应提供给客户端；    
#### 过滤器链的配置：
过滤器在过滤器链中的先后顺序取决于`web.xml`中`filter-mapping`的顺序；写在前面就先过滤，写在后面的就后过滤；   


##### 过滤器常用的场合   
我们一般可以在以下几种场合使用过滤器：  
需要对请求、响应进行统一处理的时候；   
对请求进行日志的记录和审核时；   
对请求中数据进行屏蔽和替换的时候；   
对数据进行加密和解密的时候；    
在这几种情况下都可以使用过滤器来实现；   



## 什么是监听器：   
监听器是Web应用程序事件模型的一部分，当Web应用中的某些状态发生改变时，会产生相应的事件，监听器可以接收这些事件，并可以在事件发生时做一些相关的处理；     

#### 使用监听器统计在线人数 
使用过滤器可以对访问的资源进行过滤；    
如果想在访问资源的过程中，在发生特定事件时，进行对应的操作，该怎么办呢？    
我们可以使用监听器来监听某些特定的事件，从而在事件发生时执行相应的操作；     
#### 实现在线用户统计的步骤：     
- 创建用户类实现HttpSessionBindingListener接口；  
- 在valueBound和valueUnbound方法中实现用户数量的统计；   
- 在`web.xml`中配置监听器；   


### 小结
统计在线用户数量的功能：  
首先，创建类实现监听器接口：   
```
javax.servlet.http.HttpSessionBindingListener
```
然后，在监听器的方法(valueBound 和 valueUnbound方法)中实现用户数量的统计功能；    
当把用户存入session的时候，valueBound方法自动调用，在此对用户数量加1；   
```
valueBound(HttpSessionBindingEvent arg0) {
   //用户对象存入session时自动调用
   //用户数量加1
}
```
当用户对象从session中删除时，valueUnbound方法自动调用；对用户数量进行减1操作；   
```
valueUnbound(HttpSessionBindingEvent agr0) {
   //用户对象从session中删除时自动调用
   //用户数量减1
}
```
#### valueUnbound方法会在三种情况下自动调用：
- 第一种：   
调用使session失效的方法：`session.invalidate()`      

- 第二种：   
session超时   

- 第三种：   
调用setAttribute重新设置了别的对象；或是调用removeAttribute移除了这个属性；   

在`web.xml`中配置监听器  
监听器也是需要在web.xml中进行配置的；  
```
<listener>
 <listener-class>监听器完全限定名</listener-class>
</listener>
```
listener中的listener-class里面指定的是监听器的完全限定名；   
