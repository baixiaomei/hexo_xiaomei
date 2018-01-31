### 一，我从dev上拉下来的新分支进行开发，，开发完了commit过但是没有push到远程，然后我误将这个分支给删了。git branch -D collectbit

[链接]('http://blog.csdn.net/fdipzone/article/details/50616386')

解决办法：
// 1查找commit版本
git log -g

查不到我那个分支的信息

git reflog 

git branch collectbit (原来分支的名字) 版本号

git branch -a


git checkout collectbit

找回了。
