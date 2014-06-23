##异步文件上传（百度fex webuploader试用报告）

####为什么选择这个组件，因为在这里，找到了亮点！

### 亮点一，添加文件的多样化


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


###亮点二，缩略图预览(pc全浏览器实现)

####(1)html5，生成bash64

```js

img.src = canvas.toDataURL();

```
####(2)flash+php


支持canvas的，用flash代理实现，支持bash64的，多请求一次服务端（php），帮忙处理成解码base64普通图片的url


###亮点三，图片的前端压缩

####(1)canvas缩放

```js
drawImage(image, x, y, width, height)

```
####(2)jpg图片降低质量,和Photoshop的功能类似

```js

canvas.toDataURL( type, quality / 100 );

```
####(3)flash处理

省略500字...


###亮点四，分片上传

####(1)file对象的切分

```js

file.slice( block.start, block.end );

```
####flash做切分


再次省略500字...


####比较遗憾的他没有实现秒传和断点续传..[原因](https://github.com/fex-team/webuploader/issues/142)


####亮点五，移动端也可以用，不过有坑（ququ赞助的内容）

####移动版的坑:[https://github.com/fex-team/webuploader/issues/185](https://github.com/fex-team/webuploader/issues/185)

####移动版js,[webuploader.custom.js](https://github.com/fex-team/webuploader/blob/master/dist/webuploader.custom.js)

####移动设备上，iOS6+，Android4+ 才可以用，根据用户的机型数据，来决定是否用webupload。

####可以WebUploader.Uploader.support()方法，来判断浏览器是否可以使用组件，

####降级方案，使用传统的file表单提交




我不是组件的作者，我只是代码的搬运工，有bug可以找原作者（貌似人很好,响应很快！）[webuploader git](https://github.com/fex-team/webuploader)



##链接

[webuploader官网](http://fex.baidu.com/webuploader)

[webuploader git](https://github.com/fex-team/webuploader)

[XMLHttpRequest 2.0的家臣们](http://www.zhangxinxu.com/wordpress/2013/10/understand-domstring-document-formdata-blob-file-arraybuffer/)

[grunt的中文网站](http://www.gruntjs.net/)