# 使用JSTL显示新闻列表；
### 概述：   
专题任务：    
实现这个任务，需要掌握JSTL的基本用户和核心标签库；       

## JSTL介绍：
在上一专题中使用EL显示新闻列表的代码，不难发现，EL虽然在一定程度上简化了jsp页面代码，但由于EL不能实现循环，分支等结构，所以仍需要在页面上嵌入java代码；       
如何解决这个问题呢？      
使用JSTL就可以完全解决这个问题；             
`[JSTL]`     
JSTL是JSP标准标签库的简称，它提供的标签能在一定程度上代替java代码；例如：`<c:forEach/>`标签能实现java语言支持的for循环的功能；

### JSTL使用步骤：
首先，必须下载`jstl.jar`和`standard.jar`包；      
建议下载高版本，下载完解压后，将`jstl.jar`和`standard.jar`这两个包复制到`WEB-INF/lib`目录下；       
最后，在JSP页面中添加taglib指令；例如：     
```java
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```

#### <c:out/>标签：
问题：     
在数据库中插入一条数据：     
我们插入的内容里面包含了`<a></a>`一对超链接标签。但显示页面上，`<a></a>`却无法显示；这是什么原因呢？          
这是因为浏览器把`<a></a>`解析为一对超链接标签，而不是要显示的文字；如何解决这个问题呢？         
使用`<c:out/>`标签可以很容易解决这个问题；       
##### <c:out/>标签的语法：
```
<c:out value="value" default="default" />
```

### 小结：   
`<c:out/>`常用于需要转义和需要默认值的内容输出，语法如下：           
```
<c:out value="value" default="default" escapeXml="true"/>
```
value ：要显示的值； default：默认值；      
escapeXml：是否转义；          

`<c:set/>`和`<c:remove/>`使用频率不高，语法如下：    
```
<c:set var="uid" value="admin" scope="request"/>
```
var：存入的变量名； value：存入的变量值；      
scope：存入的作用域；         

`<c:remove var="uid" scope="request"/>`        
var：移除的变量名； scope：移除的变量所在的作用域；   


### JSTL标签分类：
`<c:out/> <c:set/> <c:remove/>`都是以c开头，因为它们都属于JSTL中的核心标签库；       
其实，JSTL中还包含其他标签库：国际化/格式化标签库；          
XML标签库；数据库标签库；函数标签库；         
每种标签库都有自己的资源标识符号前缀；         
如果我们需要使用核心标签库中的标签，就要在页面中添加taglib指令；指令中需要指定标签库的uri和前缀：           
```
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```
如果我们需要使用国际化/格式化标签库，页面上必须加入这样的taglib指令：          
```
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" parefix="fmt" %>
```
|标签库名称|     资源标识符(uri)|   前缀(prefix)|
|------|------|-----|
|核心标签库|http://java.sun.com/jsp/jstl/core|c|
|国际化/格式化标签库|    http://java.sun.com/jsp/jstl/fmt|  fmt|
|XML标签库      |  http://java.sun.com/jsp/jstl/xml  |x|
|数据库标签库|         http://java.sun.com/jsp/jstl/sql|  sql
|函数标签库      | http://java.sun.com/jsp/jstl/functions|  fn



### <c:forEach/>标签  
在前面的专题中，已经使用java小脚本显示新闻列表，由于使用了java中的循环语句，使得页面代码很混乱；我们可以使用`<c:forEach/>`标签来代替循环语句，简化代码结构；      
##### <c:forEach/>标签的基本语法结构：
```
<c:forEach var="varName" item="items" varStatus="varStatus">
</c:forEach>
```
items中是集合对象，var是集合中的元素名，varStatus中是当前循环的状态信息，例如循环的索引号；    


### <c:if/>标签
我们在前面的专题中，已经使用java小脚本实现隔行变色，由于使用了java中的if语句，使得页面代码很混乱；所以，我们可以使用`<c:if/>`标签来代替if语句，简化代码结构；          
`<c:if/>`标签的基本语法结构：     
```
<c:if test="condition" var="varName" scope="scope">
</c:if>
```
#### test为判断条件；
当test中表达式的结果为true时，则会执行<c:if/>标签中的内容；如果为false，则不执行。    
var用来存放判断结果，可省略；     
scope是指判断结果var的存放的作用域；       
scope的可选值为`page/request/session/application`，可省略；     


`<c:url/>  <c:param/>  <c:import/>`     
在实际开发中，经常需要构造一个url地址，使用`<c:url>`和`<c:param>`来构造url地址非常方便；      

语法：        
```
<c:url value="value"/>

<c:param name="name" value="value"/>

<c:import url="url"/>
```
`<c:url value="value"/>`的value属性可以指定要构造的url；这个url可以是相对路径，也可以是绝对路径；           
`<c:param>`用来给url附加参数，name属性是参数的名称，value属性则是参数的值；          
`<c:import>`用于在jsp页面中导入一个url地址指向的资源内容；其作用类似于`<jsp:include>`，其属性url中指定要导入的资源的url地址；    
    

#### <fmt:formatDate/>标签
在前面的专题中，已经使用java中SimpleDateFormat格式化时间，其代码还是比较麻烦的，而使用`<fmt:formatDate/>`标签来格式化时间就简单多了；
##### <fmt:formatDate/>标签的语法：
```
<fmt:formatDate value="date" pattern="yyyy-MM-dd HH:mm:ss"/>
```
Value用来指定时间对象，pattern用来指定时间格式；        

#### 小结：
|JSTL标签 |                 功能|
|----|-----|
|<c:out/>|输出文本内容到out对象，常用于显示特殊字符，显示默认值；|
|<c:set/>|在作用域中设置变量或对象属性的值；|
|<c:remove>|在作用域中移除变量的值；|
|<c:if/>|实现条件判断结构；|
|<c:forEach/>|实现循环结构；|
|<c:url/>|构造url地址|
|<c:param/>|在url后附加参数；|
|<c:import/>|在页面中嵌入另一个资源内容；|
|<fmt:formatDate/>|格式化时间；|
|<fmt:formatNumber/>|格式化数字；|


### 使用JSTL升级分页功能
在前面的专题中，我们已经实现了分页显示新闻列表的功能；在列表下边我们有一个分页功能条。如果其他页面也需要使用分页功能条的话，需要重新做一次；显然重新实现是浪费时间的；如果我们能把功能条单独做成一个页面，通过被其他页面包含的形式，实现重(chong)用，这才是理想的解决方案；       
