# 使用CKEditor编辑新闻内容；
## 概述：  
完成任务：    
掌握为新闻添加图片自动：   
使用`commons-fileupload`上传图片；   
会实现所见即所得的新闻编辑：   

## 为新闻添加图片：
我们已经完成了新闻的基本数据的添加了，但是我们在很多网站上都会发现，很多新闻都会配有图片，这样的话新闻会更有吸引力和说服力。那怎样为新闻添加图片呢？  
为新闻添加图片的时候会涉及到文件的上传，如果我们自己写文件上传的功能会需要写大量的代码，而且很容易出现异常。这时，我们可以直接使用第三方提供的一些组件来实现文件的上传功能。   
### 第三方组件：   
它不是我们自己写的组件，也不是Java提供的，而是由一些第三方的组织提供的一些组件，就是第三方组件。   
能够实现文件上传功能的组件中`commons-fileupload`使用的比较广泛；        
####  commons-fileupload ：
它是apache公司提供的一个开源的、免费的文件上传组件，实现上传功能；  
使用`commons-fileupload`实现为新闻添加图片的步骤：      
- 第一步：   
需要下载`commons-fileupload-1.2.2.jar`和`commons-io-2.4.jar`，并且导入项目中；    
- 第二步：   
准备添加新闻的页面，把表单提交到JSP；    
- 第三步：   
在JSP中实现文件上传和新闻添加；     
如何在JSP中实现文件上传；    

### 小结：
为新闻添加图片：  
首先，我们要准备新闻的提交页面；   
然后，提交到JSP，在JSP中实现文件的上传和新闻的添加；注意：      
在新闻的提交页面中，我们需要在表单中指定属性：     
```
enctype="multipart/form-data"
```
并且，修改表单的提交方式为post；   
接下来，就可以把表单提交到JSP来实现图片文件的上传了；  
### 在JSP中实现文件上传需要注意以下几步：        
- 第一步：   
检查请求的类型；   
```java
boolean isMultipart=ServletFileUpload.isMultipartContent(request);
if(isMultipart)   //当enctype="multipart/form-data",并且method="post",则isMultipart为true；
```
- 第二步：
声明需要的对象；     
```java
DiskFileItemFactory factory=new DiskFileItemFactory();
ServletFileUpload upload=new ServletFileUpload(factory);
```
- 第三步：   
转换请求对象；
```java
List<FileItem> items=null;
items=upload.parseRequest(request);
```
- 第四步：   
保存上传的文件；    
```java
if(!item.isFormField()){
  File fullFile=new File(item.getName());
  File uploadFile=new File(realPath,fullFile.getName());
  item.write(uploadFile);
}
```
- 第五步：   
获取普通自动；    
```java
if(item.isFormField()){
  fieldName = item.getFieldName();
  if (fileName.equals("title")){
  news.setTitle(item.getString("UTF-8"));
    }....
}
```

##### 所见即所得新闻编辑：
我们的新闻已经有了图片，但单调的文字效果依然表现力不强。真正的新闻系统中需要有一种可以简单的、可视化的对内容进行编辑的功能。
编辑新闻内容就像编辑word那样所见即所得；  
那么怎样对新闻进行所见即所得的编辑呢：   
##### 可以使用CKEditor这个第三方组件。    
CKEditor是一个非常受欢迎的，可以在网页中实现所见即所得的编辑器。        
CKEditor的使用非常的方便：          
##### CKEditor的使用步骤；         
首先下载CKEditor；      
然后把解压到项目中。    
接下来就可以在页面中引入CKEditor；   
引入以后，就可以使用CKEditor编辑新闻的内容了；   
编辑好新闻内容后，就可以提交到JSP，把新闻存入数据 库中；       

### 小结：
将CKEditor解压到项目中以后，就可以在页面中使用CKEditor了；    
在页面中使用CKEditor的时候，先在页面中引入`ckeditor.js`文件；       
```javascript
<script type="text/javascript"
src="ckeditor/ckeditor.js"></script>
```
之后，在页面中加入textarea，使之成为CKEditor；      
```html
<textarea id="newscontent" name="newscontent"
class="ckeditor"></textarea>
```
在使用CKEditor的时候，可以通过`config.js`文件，配置CKEditor；   
```java
CKEDITOR.editorConfig=function(config)
{
  config.language='zh-cn'; //配置语言，代表中文；
  config.uiColor='#AADC6E'; //背景颜色
  config.width='auto';   //宽度
  config.height='300px';  //高度
  config.skin='office2003' 
         //皮肤：v2,kama,office2003
};
```
 当然，如果我们不做设置，CKEditor也会有自己默 认的设置；   
 
最后，CKEditor有几个文件夹我们可以了解一下；      
lang文件夹：存放多国语言文件；   
`_samples文件夹：存放官方提供的Demo；`
skins文件夹：存放CKEditor皮肤的文件夹；   


#### 大字段：
数据库中存放新闻内容的字段类型：   
这里使用的是clob；   
##### clob类型：
在`oracle 10g`以前的版本中处理这样的大字段时会有问题，但`oracle 10g`之后的版本就已经解决了这个问题。  
以后如果遇到使用老版本oracle的情况，可以使用下载资源中的方法来解决大字段的问题；     
