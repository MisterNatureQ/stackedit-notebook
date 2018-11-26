# jenkins 安装


## 就是不要让Jenkins作为一个service，而是作为Java web start，通过java -jar Jenkins.jar在windows上启动，就OK了




通过脚本启动
java -jar jenkins.war --ajp13Port=-1 --httpPort=8080
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc4MTM0NzQ2OF19
-->