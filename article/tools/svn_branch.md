##svn 分支开发与合并
###创建分支

比如，你现在负责一个项目，svn的主干目录地址为:https://localhost/game/trunk/,

现在有一个页面改版的需求，创建一个叫mod_page的分支

```svn copy -m "页面改版" https://localhost/game/trunk/  https://localhost/game/branches/mod_page```

在-m后面加上注释是很好的习惯，方便结合svn log 命令很快找到版本号之类的信息。


```svn co https://localhost/game/branches/mod_page```

checkout下分支，开始工作吧 ！！

###分支合并

####从主干合并到分支

当你在分支上开发了好几天的时候，你的同事告知他刚刚在主干有修改上线，这个时候就必须把他在主干上的改动，合并

到目前你正在开发的分支上来

```cd branches/mod_page```

进入分支目录

```svn merge --dry-run   -r 13881:14094 https://localhost/game/trunk/```

建议在svn  merge之前先加上参数 ```--dry-run```,做一个合并的测试，并不是真正的合并，只是让你可以看到哪些文件发生了变化，是否有合并冲突

```13881```为分支创建前的trunk版本号，```14094```为trunk当前版本号(在主干目录，svn up 命令可以顺便获取到)

分支创建前的版本号,忘了怎么办？在分支目录下，执行 ```svn log --stop-on-copy``` 命令，可以看到创建分支后trunk的版本号，

这个版本号减去1就可以算出分支创建前的trunk版本号

没有问题就去掉--dry-run参数，正式合并吧

```svn merge -r 13881:14094 https://localhost/game/trunk/```

解决冲突，提交代码

```svn ci -m "svn merge -r 13881:14094 https://localhost/game/trunk/"```

####从分支合并到主干

开发完成上线了，这个时候代码就应该合并到主干了


```cd trunk```, 进入主干目录

合并，同样建议先加上参数 ```--dry-run```，做一个合并测试

```svn merge -r 13882:14094 https://localhost/game/branches/mod_page```

13882是分支开始的版本号(```svn log --stop-on-copy```获得)，14094是分支结束的版本号(svn up 命令可以顺便获取到)

(3)解决冲突，提交代码

```svn ci -m "svn merge -r 13882:14094 https://localhost/game/branches/mod_page"```


###svn的不常用操作

####代码回滚

(1)代码改动还没有被提交（commit）。

这种情况下，使用svn revert就能取消之前的修改。

svn revert用法如下：

```svn revert [-R] something```

其中something可以是（目录或文件的）相对路径也可以是绝对路径

(2)改动已经提交，回滚到历史版本

当前的最新版本号28，回滚到25，如果想要更详细的了解情况，可以使用
 
 ```svn diff -r 28:25 [something]```
 
 回滚到版本号25：
       
 ```svn merge -r 28:25 something```
 
 为了保险起见，再次确认回滚的结果：
 
 ```svn diff [something]```
 
 发现正确无误，提交。
 
 提交回滚：
 
 ```svn commit -m "代码回滚28到25" ```