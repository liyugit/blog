##关于中文字符串的截取

###普通的php字符串截取方法

substr --- 取得部份字符串 

语法 : string substr (string string, int start [, int length]) 

说明 : 
substr( )传回 string的一部份字符串，由参数 start和 length指定。

如果 start是正数，传回的字符串将会从 string的第 start个字元开始。 

例如

```<?php 

$rest = substr ("abcdef", 1); // returns "bcdef" 

$rest = substr ("abcdef", 1, 3); // returns "bcd" 

?>```

如果 start是负数，传回的字符串将会从 string结尾的第 start个字开始。 

例如

```<?php 

$rest = substr ("abcdef", -1); // returns "f" 

$rest = substr ("abcdef", -2); // returns "ef" 

$rest = substr ("abcdef", -3, 1); // returns "d" 

?> 
```

###但是substr截断，有对中英混排的情况，出现截取半个字符导致乱码的问题

解决方法：利用mb_substr截取字符串不会出现乱码问题

使用方法：

首先 

1.确保你的Windows/system32下有php_mbstring.dll这个文件，没有就从你Php安装目录extensions里拷入Windows/system32里面。 

2.在windows目录下找到php.ini打开编辑，搜索mbstring.dll，找到;extension=php_mbstring.dll把前面的;号去掉，这样mb_substr函数就可以生效了 

mb_strcut函数功能也可以截取字符串长度，下面实例具体看看区别在哪：  

例如：

```<?php 

$str = '这样一来我的字符串就不会有乱码^_^'; 

echo "mb_substr:" . mb_substr($str, 0, 7, 'utf-8'); 

//结果：这样一来我的字 

echo "<br>"; 

echo "mb_strcut:" . mb_strcut($str, 0, 6, 'utf-8'); 

//结果：这样 

?> ```

从上面的例子可以看出，mb_substr是按字来切分字符，而mb_strcut是按字节来切分字符，但是都不会产生半个字符的现象。

####中英混排的内容长度获取


这是WordPress中的一段代码，主要思想就是先用正则将字符串分解为个体单元，然后再计算单元的个数即字符串的长度，代码如下（只能处理utf-
8编码下的字符串）

```<?php

	function utf8_strlen($string = null) {

		// 将字符串分解为单元

		preg_match_all("/./us", $string, $match);

		// 返回单元个数

		return count($match[0]);

	}

	$zhStr = "中国520";

    echo utf8_strlen($zhStr); // 输出：5
?>```


附上一篇关于php正则修饰符(g,i之类)说明文章 ,<a href="http://developer.51cto.com/art/200909/152083.htm" target="_blank">传送门</a>