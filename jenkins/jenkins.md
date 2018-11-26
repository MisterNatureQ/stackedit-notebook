# jenkins 安装


## 就是不要让Jenkins作为一个service，而是作为Java web start，通过java -jar Jenkins.jar在windows上启动，就OK了


###添加命令行 解决 脚本结束 进程就被杀掉的问题 最好不用全局变量 通过set 在需要的地方去昂添加
BUILD_ID
DONTKILLME

[启动命令](https://www.cnblogs.com/wyx123/articles/4106802.html)



## 通过脚本启动 解决 session 0  和 Windows 有GUI 的 exe 无法去显示的问题

java -jar jenkins.war --ajp13Port=-1 --httpPort=8080
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxNTMyNjE3OF19
-->