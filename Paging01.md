# 分页查询：
## 概述：
回顾之前实现的新闻列表显示页面，看见了6条新闻信息；       
可是，当数据量很大时，可能有几万、几十万甚至上百万条新闻时，受 页面的限制用户必须拖动页面才能浏览更多的新闻数据；这样，也就就显得很冗长。那有没有一种显示方式，即能显示多条信息，又不需要拖动页面；          
分页显示新闻列表；    
## 分页优点：
使用分页后使数据更加清晰直观显示、同时不受数据量的限制、页面不再冗长； 
分页如何实现：   
解决方案：       
每次翻页的时候只从数据库里检索出本页需要的数据；   
不用担心每次翻页都查询数据库；即便这样，因为每次查询出的记录数很少，网络传输量不大，这要比我们把所有数据都查出来之后再分页这么做，效率要高；即，效率也会提高；    

### 分页查询的步骤：
- 第一步：      
确定每页显示的数据数量；       
这个数量定义一个变量存储即可；   
- 第二步：    
计算显示数据的总数量；     
关键代码：     
```java
//编写查询新闻纪录总数量的sql语句
String sql="select count(*) from news_detail";
//借助JDBC:执行sql语句，返回结果；
int totalCount=0;
Object[] params={};
ResultSet rs=this.executeSQL(sql,params);
//...省略代码....
totalCount=rs.getInt(1);
```
- 第三步：  
计算显示的页数；   
页面=总数量/每页显示的数据数量(+1);  
即，如果不能整除，页数还要加1；也就是，不管整除后余几条新闻，都要单独加1页；    
关键代码：    
```java
//总页数
private int totalPageCount=1;
//页面大小，即每页显示纪录数
private int pageSize=0;
//纪录总数
private int recordCount=0;
//....省略代码....
//设置总页面=总数量/每页显示的数据数量(+1)
private void setTotalPageCountByRs(){
 if(this.recordCount%this.pageSize==0)
this.totalPageCount=this.recordCount/this.pageSize;
else if(ths.recordCount%this.pageSize>0)
this.totalPageCount=this.recordCount/this.pageSize+1;
else
   this.totalPageCount=0;
}
```
- 第四步：   
编写分页查询sql语句；   
结合之前学过的Oracle的分页查询，编写分页查询sql语句；   
关键代码：    
```java
//页码：currPageNo 每页显示的记录数：pageSize
SELECT id,title,author,createdate FROM
(SELECT id,title,author,createdate,ROWNUM rn FROM news_detail) a  AND a.rn<=currPageNo * PageSize
```
- 第五步：  
实现分页查询；   
```java
//分页获取新闻信息
List<News> newsList=new ArrayList<News>();
//分页查询sql语句
String sql="SELECT id,title,author,createdate FROM
(SELECT id,title,author,createdate,ROWNUM rn FROM news_detail) a  WHERE a.rn>=? AND a.rn<=?";
//调用工具类Page相应方法，分别设置当前页码、每页显示纪录数
Page page=new Page();
page.setCurrPageNo(pageNo);
page.setPageSize(pageSize);
//计算sql语句的起始纪录数以及结束纪录数据的行数
int startRow=page.getStartRow();
int endRow=page.getEndRow();
//借助JDBC分页查询纪录
Object[] params={startRow,endRow};
ResultSet rs=this.executeSQL(sql,params);
//....省略代码...
```
### 分页查询小结：   
分页查询关键点：  
我们在查询新闻总数量的时候，需要借助前面JDBC的内容。而计算页数时，声明了一个Page工具类实现，这样就可以将这个功能独立出来，利于复用。        
在编写分页查询sql语句时，思考每页记录起始行数和结束行数有什么特点？     
和每页显示的记录数以及当前页码有何关系？      
想清楚这两个问题，分页查询的sql语句就自然 水到渠成了；       

### 分页查询总结：
实现分页查询的关键操作：  
计算分页总页数：    
```
总记录数/每页显示的记录数整除：
总页数=总记录数/每页显示的记录数；
总记录数/每页显示的记录数不整除：
总页数=总记录数/每页显示的记录数+1；
编写分页查询sql语句：
借助Oracle学过的分页查询知识，我们在这里只需要确定分页后的起始记录数，以及结束记录数；
起始记录数：（当前页码-1）*每页显示的记录数+1
结束计算数：当前页码*每页显示的记录数；
```
剩下的分页查询操作只需要借助JDBC来完成就可以了；


### JDBC执行存储过程：
思考：   
当业务逻辑比较复杂或者对性能要求比较高时，DBA一般通过存储过程实现，如何调用存储过程呢？   

示例：   
新闻分页查询：查询新闻总记录数的存储过程(getNewsCount)如何执行？   
假设我们将查询新闻总记录数的数据库操作通过存储过程完成，那JDBC如何执行这个存储过程呢？    
其实，可以借助一个接口CallableStatement实现；    
#### java.sql.CallableStatement ：
用于执行SQL存储过程的接口；  
此时sql语句的形式是：   
```
sql:{call<procedure-name>[(<arg1>,<arg2>,...]}
```
而CallableStatement可以通过数据库连接对象的`prepareCall()`方法获得:
```
conn.prepareCall(sql);
```
如果此时的存储过程有输出参数，还可以通过它的`registerOutParameter()`方法将输出参数注册为JDBC类型：
```
registerOutParameter(int parameterIndex,int sqlType)
```
