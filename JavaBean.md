
# 使用JavaBean实现新闻列表 
## 编写JavaBean：
添加新闻的add()方法：     
```java   
public void add(int id,int categoryId,String title,String summary,String content,Date createdate) {
  //方法体 
}
```
我们将要添加的新闻信息作为参数一一传入；       
可是，如果新闻字段特别多，比如100个字段时，我们也要这样定义100个参数吗？     
显然很麻烦，实际上我们可以把这些字段封装在一个新闻对象中；这样参数只要传递一个对象就可以了。这个对象就是JavaBean对象；    
### JavaBean：
说到底，JavaBean实际上就是一个Java类，是Java开发的可以跨平台的重(chong)用组件；   
作用：     
在JSP程序中常用来封装业务逻辑和数据库操作。       
而在开发里面，程序员们要处理的无非是业务逻辑和数据的处理；而这两种操作都要用到JavaBean，因此JavaBean很重要；         

### 编写service：
在编写的代码中：     
dao包中的NewsDao接口以及NewsDaoImpl实现类，主要负责和数据操作相关的事情；    
比如：操作数据库，增、删、改、查新闻信息；     
而实际开发中，我们往往除了dao包，还会创建一个名为service的包；在这个包里存放和业务逻辑相关的操作。    
service包中的接口和类对dao的方法进行封装和调用，主要负责和业务逻辑相关的操作；      
举例：      
假设CategoryDao接口，这是一个和操作新闻类别相关的接口；在这个接口里，定义了根据类别删除新闻信息的方法，还有一个删除新闻分类的方法；       
需求：删除某新闻类别，该怎么做？          
要删除某新闻类别，肯定要调用Dao中的删除新闻分类的方法；但是，新闻分类都没有了，那该类别下的新闻也应该删除，不然数据就不一致了；因此，还要调用Dao删除某类别下新闻信息的方法。当然，删除的时候还得先删除新闻信息，然后删除新闻分类。因为要先删子表内容。
这两个方法的调用，可以将其封装为一个方法叫`deleteCategory()`;       
然后将这个方法放在一个Service中就可以了；当然，这个Service可以定义为一个接口；     
 
### 新闻列表显示页面：  
需求：   
创建新闻系统后台的新闻列表页面，将数据库中所有新闻信息在该页面显示：     
JSP页面显示数据：     
```
<%out.print();%> 或者  <%out.println();%>
<%=%>  <%变量%>
```

### <jsp:useBean>   
新闻列表页面实现了，但是，JSP显示时将大量Java代码嵌套其中；实际上，我们可以通过JSP动作标签简化这样的写法；    
##### JSP动作标签：        
通过动作标签，程序员可以在JSP页面中把页面的显示功能部分封装起来，使这个页面更简洁和易于维护。它是一种类似HTML标签的东西。
### <jsp:useBean>
作用：      
装载一个将在JSP页面中使用的JavaBean，发挥Java组件重(chong)用的优势；     
语法：    
```
<jsp:useBean id="name" class="package.class" scope="scope">
```
Id用于创建JavaBean的引用名；    
class用于指定JavaBean的类；    
scope用于指定JavaBean的范围；    
 

### <jsp:include>
思考：     
增加新闻信息页面和新闻列表显示页面的左侧和上半部分一模一样，要把页面代码全部重新写一遍或者复制一遍？   
JSP中页面复用的方式：      
动作标签` <jsp:include>`       
作用：      
它可以把指定文件插入正在生成的页面中。        
语法：    
```
<jsp:include page="URL">
```
URL: 引入的页面；      


#### <%@include file="URL"%>
作用：实现页面复用；    
file属性：代表要引入的页面路径；     

#### <jsp:include>和<%@include file="URL"%>的区别：    
- `<jsp:include>`为动态包含，会将被包含页面的结果包含进来；也就是：先处理，再包含；       
- `<%@include%>`为静态包含，将被包含页面的内容包含进来。也就是：先包含，再处理；          
 

### 页面跳转：   
两种方式：   
```java
Request.getRequestDispatcher().forward()
```
是转发；代表服务器端跳转；      
一次请求，多个页面间跳转；URL不会改变，可以共享request范围内的数据；   
##### response.sendRedirect()   
是重定向；代表客户端跳转；   
代表两次请求过程，多个页面间跳转URL会改变；获取不到request范围内的值；    

#### 第三种页面跳转方式：
JSP动态标签：` <jsp:forward>`      
它能达到和转发一样的效果；    
语法：  
```
<jsp:forward page="URL">
```
page属性代表要跳转到的页面；
