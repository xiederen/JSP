
## JSP页面的组成部分
### `<%@page %>`标签：   
指令元素page指令，它包含了服务器处理该JSP页面时所需要的各种信息；例如：保存该文件的字符集信息；指令元素的内容不会发送到浏览器；   
###<!DOCTYPE>标签：   
它提供了浏览器解析该页面所使用的HTML规范；   
例如：`HTML 4.01 Transitional`规范；   
`HTML标签：CSS标签：javascript标签：`   
页面出现最多的是HTML标签，这些标签都会被发送到浏览器；然后被解析成图形化的界面显示给用户；当然，页面中也可以加入css，用于设置各种标签的样式；也可以加入javascript脚本，用于在客户端实现功能或和用户进行交互；   
### java小脚本： 
页面中还有很多java小脚本；  
它是请求处理阶段要执行的java代码；   
它可以使用`out.print()`或`out.println()`方法产生输出；   

声明：    
以`<%!` 开头的是声明，它可以用来声明全局变量和定义方法；  

表达式：
以`<%=` 开头的是表达式，在请求处理时，这些表达式的值会被转化成字符串，并显示在页面上；   

注意：    
以`<%` 开头的元素都是在服务器端执行，并将执行结果嵌入到html中；然后随整个html文档一起发送到浏览器；
服务器端执行的元素包括了指令元素、java小脚本和动作标签；   


### 常用的内置对象：  
- out:    
out用于向客户端输出数据，输出数据的方法是print()和println()；   
- request：    
request是用户提交的请求对象，它包含客户端请求的相关信息；例如：请求的url，上传的数据等；  
- getParameter():   
可以取得客户端提交的参数；   
- setCharacterEncoding():   
用于设置解析请求内容所使用的字符集；   
- getRequestDispatcher():   
返回一个RequestDispatcher对象，该对象的forward()方法用于转发请求；   
- response:   
是服务器向客户端发送响应信息的对象；   
它存储了服务器向客户端发送的信息；   
例如：html页面，cookie等信息；   
- addcookie():   
用于向客户端发送cookie；   
- sendRedirect():   
用于实现以重定向方式跳转页面；  


### 数据保存：
`request session application，`他们是在服务器端保存数据；   
使用他们存取数据的方法是setAttribute()和getAttribute()方法；    
他们保存的数据生效的时间和可访问范围不一样：      
- request保存的数据只在一次请求中有效；   
- session保存的数据在一次会话中有效；适合保存用户登录信息，权限信息等；   
- application保存的数据可以被所有页面所有用户访问；    
####  cookie：
cookie比较特殊，它是在客户端保存信息；   
灵活使用cookie能提高程序的友好性，但它安全性差，不适合保存密码等敏感数据；     
|名称 |         特点 |          适用场合|
|----|-----|----|
|request|保存的数据只在一次请求中有效；|转发请求时在页面间传递数据；|
|session|保存的数据在一次会话中有效，每个用户都有自己的session，并且只能访问自己的session；|适合保存一次会话有效的数据，如用户登录信息，权限信息等；|
|application|保存的数据可以被所有页面、用户访问；|保存全局性的信息，例如网站访问次数；|
|cookie|在客户端保存信息，会随请求和响应在客户端和服务器之间来回发送；|安全性差，适合保存非敏感数据，只能保存字符串；|


#### 客户端请求新页面
客户端请求新页面的常用方式：
- 使用超链接直接请求新页面；  
- 使用超链接调用javascript请求新页面；  
- submit提交按钮向新页面提交表单；  
- 使用javascript提交表单；    

客户端不但能请求数据，也能提交数据；   
#### 客户端向服务器提交数据的方式：   
提交数据的方式分为get和post两种；
##### get方式：
get方式是把数据附加在url后面；   
示例：   
提交的方式：  
```
index.jsp?uid=admin&pwd=111
```
##### post方式：  
post方式是把数据放入http请求的内容中进行提交，它要求使用表单提交；   
请求新页面和提交数据一般是同时进行的；    


## 小结：
#### get方式提交数据：   
由于提交的数据会显示在url中，所以安全性差，不能提交密码等重要敏感数据，而且只能传递字符串；  
#### post方式提交数据： 
这种方式能提交各种类型数据，例如文件，图片等，而且提交的数据被放入request的内容块中，所以安全性会比较好；由于post方式必须以表单形式提交，所以它适合提交用户输入的数据；    
如果提交请求之前需要先处理，则可以使用元素的属性或事件调用javascript处理后再提交；   

| &nbsp;     |   数据提交方式    |  特点及适用场合|
|-----|-----|----|
|超链接 |  get|只能提交字符串，数据会显示在url后面。适用于比较简单的页面请求，提交的数据为非敏感数据；|
|超链接+js|     get|与上面方式的不同点是：适用于请求新页面前需要使用js进行处理的情况；|
|submit按钮提交表单|一般使用post|能提交各种类型数据，安全性好。适用于提交用户输入的数据，非用户输入的数据可使用hidden元素提交；|
|使用js提交表单 |    一般使用post|与上面方式的不同点是：使用<a>,buttton等元素的属性或事件调用javascript提交表单，适用于提交数据前使用js进行处理时，以及页面上不适合显示submit按钮时；|


#### 处理中文乱码
中文乱码一直是困扰众多java程序员的难题；   
##### 导致乱码的原因：
如果字符的编码和解码没有使用相同的字符集，乱码就产生了；   
tomcat服务器处理字符的默认字符集编码是ISO-8859-1；由于该字符集不支持中文，所以我们要把它改为支持中文的字符集，例如改为最通用的utf-8；同时Eclipse工作空间存储文件的字符集也要改为utf-8；其次，客户端提交的数据，服务器解析客户端提交的字符集也需要设置为utf-8；    
设置好了以后，服务器就会调用我们设置的字符集来转换中文字符了；这样，一般的乱码问题就解决了；但是，也有些地方的字符串，服务器不会自动转换中文字符，例如cookie中的内容，这种情况下，我们就需要手动转换了；使用URLEncoder和URLDecoder就可以实现中英文互相转换，从而解决这个问题；   
这里列举了常见的乱码情况，有时我们还会碰到比这些更加复杂的情况，这时就需要对浏览器、tomcat、Eclipse的工作机制细节进行分析了；   
#### 项目环境设置        
服务器处理JSP文件字符集设置  
工作空间文件字符集设置   

##### get提交字符集设置  
使用 `new String()`重构字符集   
在`server.xml`文件中设置URIEncoding；

##### post提交字符集设置
使用`new String()`重构字符集   
使用`request.setCharacterEncoding()`设置字符集；

##### cookie等其他未转换汉字的情况
使用URLEncoder和URLDecoder转换汉字；