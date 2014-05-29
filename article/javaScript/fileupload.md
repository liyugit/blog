##文件上传

### 文件上传的方式

####传统

```
<input type="file" name="upfile" id="upload" multiple="multiple" accept="image/gif, image/jpeg"/>
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

####拖拽

```
<div class="drag-file-c">
	拖放文件到这里
</div>
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

####从剪切板获取

```
<div class="paste-file-c">
	复制文件到这里
</div>
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