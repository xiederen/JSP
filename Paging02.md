# 分页显示：
## 概述：
专题任务：    
在上一专题分页查询基础上，实现新闻列表的分页显示；  
要实现新闻列表分页显示，需要掌握如何使用JSP页面分页显示数据，了解如何使用可滚动结果集；    
### 分页显示关键点： 
当页面初次打开时，我们应该看到显示某一页的新闻内容；因此，首先，我们要确定当前页；   
有了当前页，接下来我们就可以通过当前页页码来确定首页、上一页、下一页以及末页的页码了；   
思考：      
如果当前页是第一页或最后一页，那用户点击上一页和下一页，页面该怎样显示呢？   
很明显，不可能出现负数，也不可能出现大于总页数；所以，在这里，我们还要对可能出现的异常进行控制；   
还有，要对首页和末页进行控制；当前页码的值如果小于1，则将值修改为1，也就是首页；反之，如果当前页 码大于总页数，就将值修改为末页页码；     


### 升级需求：
添加"GO"按钮以及文本框，可以根据用户输入的页码，跳转到相应页显示新闻信息。   
分析思路：    
做之前，判断用户输入页码的正确性；   
接下来，将用户输入的页码进行传递，显示该页新闻信息；   


思考：      
本案例我们使用的Oracle数据库，使用了Oracle的分页查询方法，通过ROWNUM限制了结果集的大小和起始位置；可是，假设我们有个小的应用系统，需要支持几种不同的数据库，要知道并非所有的数据库都有自己的分页方法的。也就是，有的数据库根本不支持ROWNUM；这时该怎么办呢？     
其实，JDBC2.0为我们提供了一种可滚动结果集；    
它可以实现将结果集光标的前移和后移，从而实现分页。    
不过，这种分页效率很低，因为是将数据先读到内存中，再来选择哪些行；这种方法是为了那些不支持分页的数据库使用的；    
即，将数据先读到内存中，再来通过光标前移和后移控制哪些行显示，从而实现分行。效率低；          


### 总结：
分页显示关键点：   
确定当前页：    
关键代码：     
```java
//获取当前页码
String currntPage=request.getParameter("pageIndex");
if(currntPage==null)
  currntPage="1";
int pageIndex=Integer.parseInt(currntPage);
```

##### 设置首页、上一页、下一页、末页的页码：
关键代码：    
```
<%
  if(pageIndex>1){
%>
<a href="newsDetailList.jsp?pageIndex=1">首页</a>
<a href="newsDetailList.jsp?pageIndex=<%=pageIndex-1%>">上一页</a>
<%
 }if(pageIndex<totalPage){
%>
<a href="newsDetailList.jsp?pageIndex=<%=pageIndex+1>">下一页</a>
<a href="newsDetailList.jsp?pageIndex=<%=totalPage%>">最后一页</a>
<%
 }
%>
```

### 首页与末页的控制：
关键代码：   
```java
//控制首页和末页
if(pageIndex<1)
  pageIndex=1;
else if(pageIndex>totalPage)
  pageIndex=totalPage;
```
### 传递分页页码：
分页显 示，我们开始采用超链接传递页码的形式：  
```java
<a href="newsDetailList.jsp?pageIndex=1">首页</a>
<a href="newsDetailList.jsp?pageIndex=<%=pageIndex-1%>">上一页</a>
```
..省略代码..
这种形式在传递参数时采用get方式，参数长度受限制，尤其是在传递参数比较多时，而且可读性差、容易出错，同时不利于页面复用；
因此，我们可以使用表单提交的方式通过隐藏域传递页码；最后，提交表单即可；包括点击Go按钮的跳转；也是采用这种方式；   
关键代码：     
```java
//编写JavaScript函数，传递页码，提交表单
function page_nav(frm,num){
   frm.pageIndex.value=num;
   frm.submit();
}
...省略代码....
<form action=""  method="">
  ...省略代码...
    <input type="hidden" name="pageIndex" value="1"/>
</form>
...省略代码...
<a href="javaScript:page_nav(document.forms[0],1">首页</a>
<a href="javaScript:page_nav(document.forms[0],<%=pageIndex-1%>">上一页</a>
<a href="javaScript:page_nav(document.forms[0],<%=pageIndex+1%>">下一页</a>
<a href="javaScript:page_nav(document.forms[0],<%=totalPage%>">最后一页</a>
```

### 学习方法：
结合上一专题和这一专题一起学习，复习之前JDBC以及Oracle相关内容，参照视频，分别实现后台分页查询以及前台分页显示；  
 
分布查询和显示有点难度，可是思维很固定，多练多记忆；    
