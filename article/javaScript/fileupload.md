##异步文件上传（百度fex webuploader分析）

###上传的3部曲

1.获取文件对象

2.post文件对象到服务器

3.返回上传是否成功

### 文件对象的获取方式

####监听file表单的change事件

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

####拖拽，监听dom的drop事件，获取event的dataTransfer.files

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

####从剪切板获取,监听paster事件，获取clipboardData

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
