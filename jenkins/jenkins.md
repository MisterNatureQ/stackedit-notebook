# jenkins 安装


## 就是不要让Jenkins作为一个service，而是作为Java web start，通过java -jar Jenkins.jar在windows上启动，就OK了可以考虑使用tomcat 等工具


### 添加命令行 解决 脚本结束 进程就被杀掉的问题 最好不用全局变量 通过set 在需要的地方去昂添加
BUILD_ID
DONTKILLME


[启动命令](https://www.cnblogs.com/wyx123/articles/4106802.html)


## 通过脚本启动 解决 session 0  和 Windows 有GUI 的 exe 无法去显示的问题

java -jar jenkins.war --ajp13Port=-1 --httpPort=8080


@echo off
set BUILD_ID=DONTKILLME
TASKKILL /F /IM Aserver.exe
TASKKILL /F /IM Mserver.exe
TASKKILL /F /IM GServer.exe
start "" C:\AServer.exe
ping 127.0.0.1 -n 10>nul
exit
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY2MDE1MDg3OCwtNzkwMzA3ODBdfQ==
-->