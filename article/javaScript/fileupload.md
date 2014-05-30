##异步文件上传（百度fex webuploader分析）

###上传的3个步骤：

1.获取文件对象

2.post文件对象到服务器

3.返回上传是否成功

### 文件对象的获取方式


####(1)监听file表单的change事件


```html
<input type="file"  id="upload" multiple="multiple" accept="image/gif, image/jpeg"/>
```
```js
var upload = $("#upload");
	upload.on("change",function (e) {
		var files = upload[0].files,
			len = files.length;
		for(var i = 0 ; i < len; i++){
			var file = files[i];
			console.log(file);
		}
	});
```



####(2)拖拽，监听dom的drop事件，获取event的dataTransfer.files


```html
<div class="drag-file-c">
	拖放文件到这里
</div>
```
```js
var dragContainer = $(".drag-file-c");
dragContainer.on("drop",function(e){
		event.preventDefault();
		var files = event.dataTransfer.files,
			len = files.length;
		for(var i = 0; i < len; i++){
			var file = files[i];
			console.log(file);
		}
	});
```



####(3)从剪切板获取,监听paster事件，获取clipboardData


```html
<div class="paste-file-c">
	复制文件到这里
</div>
```
```js
var pasteContainer = $(".paste-file-c");
pasteContainer.on("paste",function(e){
	var e = e.originalEvent || event,
		items = e.clipboardData.items,
		len = items.length;
	for(var i = 0; i < len; i++){
		var item = items[i];
		if(item.kind == 'file'){
			var file  = item.getAsFile();
			console.log(file);
		} 
	}
	e.preventDefault();
	e.stopPropagation();
});
```

####(4)flash文件上传，不支持html5的浏览器的处理方式


flash是业内通用的Uploader.swf，在这个过程中的扮演中转的角色：


![Alt flash](https://raw.githubusercontent.com/liyugit/blog/master/article/javaScript/img/js_flash.png)


###post文件对象到服务器


####(1)formdata + ajax

```js

var formData = new FormData();//定义formdata
formData.append( "filename", file);//文件添加
var xhr = new XMLHttpRequest();
xhr.open( "post", "http://localhost/up.php", true );
xhr.send( formData );//发送~~

```


####(2)js调用flash的方法上传,示意如下：
```js
 flash.exec( 'appendBlob', "filename", file );//文件添加
 flash.exec( 'send', {
                method: "post",
                url: "http://localhost/up.php"
            });//发送
```

####php代码,接收文件

```php

$in = @fopen($_FILES["file"]["tmp_name"], "rb");

后续省略，保存文件到数据库或者磁盘~~~~

```


###服务器返回结果

```php
 
die('{"jsonrpc" : "2.0", "error" : {"code": 102, "message": "Failed to open output stream."}, "id" : "id"}');//失败结果

die('{"jsonrpc" : "2.0", "result" : null, "id" : "id"}');//成功结果

```

ajax收到响应，页面显示上传结果

flash收到响应，调用js的方法，页面显示上传结果


####移动端使用webupload的坑（ququ赞助的内容）

####[移动版的坑](https://github.com/fex-team/webuploader/issues/185)

####移动版用这个custom,[webuploader.custom.js](https://github.com/fex-team/webuploader/blob/master/dist/webuploader.custom.js)

####移动设备上，iOS6+，Android4+ 才可以用，根据用户的机型数据，来决定是否用webupload。

####可以WebUploader.Uploader.support()方法，来判断浏览器是否可以使用组件，

####降级方案，使用传统的file表单提交



###代码中有趣的地方


####进度条和缩略图的实现，前端压缩，等。。


####对于html5，和falsh两种实现方式，保持统一的接口


####在代码运行时自动选择该使用哪种内核


####自定义事件，消息机制，降低耦合


####grunt的使用


####有兴趣的同学可以git上down下代码后，推敲一番~~~

##链接

[gitpress博客用法](http://blog.silverna.org/~posts/gitpress/2013-11-17-gitpress.org%20%E5%9F%BA%E4%BA%8Egithub%E7%9A%84%E6%87%92%E4%BA%BA%E5%8D%9A%E5%AE%A2%E7%B3%BB%E7%BB%9F.md)

[webuploader官网](http://fex.baidu.com/webuploader)

[webuploader git](https://github.com/fex-team/webuploader)

[XMLHttpRequest 2.0的家臣们](http://www.zhangxinxu.com/wordpress/2013/10/understand-domstring-document-formdata-blob-file-arraybuffer/)

[grunt的中文网站](http://www.gruntjs.net/)