# Java小脚本和EL/JSTL
## 概述：   
本专题优化显示新闻信息代码，使用EL增加程序的可读性；前面的专题中，程序代码已经很复杂，并且结构混乱；   
我们需要使用EL简化这些代码；要实现代码的简化，需要掌握EL的基本语法，并使用EL访问作用域对象；    

以下两段代码都可显示新闻列表标题，哪一段更好？     

代码一：    
```java
<table>
  <%
    for(News news : list) {
  %>
    <tr><td><%=news.getTitle()%></td></tr>
   <%
      }
    %>
</table>
```
代码二：     
```java
<table>
  <c:forEach var="news" items="${list}">
 <tr><td><c:out value="${news.title}"/></td></tr>
  </c:forEach>
</table>
```
### Java小脚本缺点：
第一段代码使用java小脚本实现，通过这段代码不难发现，在jsp中嵌入java代码，会导致程序结构混乱，可读性差，编写过程容易出错，且不易维护。      

### EL表达式的优势：
相对于java小脚本，下面这段代码则更为简洁清晰，容易理解。代码中有些表达式和标签；正是因为运用了这些表达式和标签，使得我们的代码变简单了，这些表达式叫EL表达式；标签叫做JSTL标签；    


#### EL表达式的语法：
```
${EL表达式}
```
这是EL表达式的通用格式，$和{}缺一不可，表达式写在{}中；   
例如： `${username}`      
使用这个表达式就可以取得变量username的值。       

### .操作符：
用来访问对象的属性；   
例如：` ${news.title}`  
取得了news对象的title属性的值；   

### []操作符：
也能用来访问对象的属性：   
例如： `${news["title"]}`   
和`${news.title}`的效果是相同的；      
还能用来访问数组或集合的元素；       
例如：` ${newsList[0]}`       
可取得newsList的第一个元素；      


在书写EL表达式的时候要注意区分大小写；   

##### 经验：
- EL区分大小写；   
- 使用EL前，必须先将对象存入作用域中。将对象存入作用域不会复制对象，作用域只保存该对象的引用，所以对服务器的性能几乎没有影响；     


### EL运算符：
```
()              改变执行的优先级，例如${3*(4+5)}
+ - * / %       算术运算符，例如${a+b}
== != > >= < <=    关系运算符，例如${a==b}
&& || !         逻辑运算符，例如${true&&false}
?:              条件运算符，例如${a?b:c}
empty   用于检测变量是否存在，是否等于null，
```
例如： `${empty name}`   

这些运算符和java语言中的运算符很相似，例如：`()运算符，算术运算符，逻辑运算符`；他们的使用和java语言大同小异；值得注意的是，类似于`==，<`等运算符有可能与HTML标签冲突，我们一般会使用其他符号代替；    
例如：`{a==b}` 会写成`{a eq b}`


### EL的功能：
取得JavaBean对象的属性：   
例如：` ${news.title} `    
取得了news对象的title属性的值；    
 
取得数组，`List，Map`类型对象的元素：       
例如：` ${newsList[0]} `      
可取得newsList的第一个元素；

使用各类运算符对原始数据进行简单处理：     
例如： `${totalRecordCount/pageSize}`          
求页面数；    

屏蔽一些常见的异常：     
例如：` ${username}`    
即使username不存在，也不会报错；     

能实现简单的自动类型转换：        
例如：      
`${news}` 相当于` (News)request.getAttribute("news")`     
取news对象时无需像java那样来进行强制类型转换；      

### EL访问作用域：
在前面的专题中，已经学过在request作用域中存入数据：    
```
request.setAttribute("news",news);
```
使用java小脚本取出数据：       
```
request.getAttribute("news");
```
刚才又学习了使用EL取出数据：
`${news}`    
其实，这个代码的完整写法应该是这样的：         
```
${requestScope.news}
```
这个代码的意思是，从request作用域中查找并取出news对象。     
我们还学习过其他的作用域，包括会话作用域，程序作用域，页面作用域；      
使用java程序访问这些作用域的代码，都是使用`getAttribute()`方法来取值的。       

那使用EL来取值的代码该怎么写呢？            
不难发现，访问作用域的EL表达式都有Scope单词，这个单词就是范围、作用域的意思；      
如果不指定作用域，则系统会按照`page，request，session，application`的顺序进行查找； 

|作用域 |  Java代码取值|
|----|-----|        
|请求作用域    |request.getAttribute("news");|
|会话作用域    |session.getAttribute("username");|
|程序作用域    |application.getAttribute("count");|
|页面作用域   |pageContext.getAttribute("userNum");|

|作用域        |EL取值|
|----|-----|   
|请求作用域       |${requestScope.news}|
|会话作用域       |${sessionScope.username}|
|程序作用域       |${applicationScope.count}|
|页面作用域       |${pageScope.userNum}|


#### pageContext
开发人员可以在各种作用域中保存值和对象，然后使用EL表达式访问作用域。    
EL还可以访问页面上下文，页面上下文又叫pageContext，它提供了对jsp页面相关的对象的访问；    
在实际开发中，使用pageContext的机会不多；     
