    
	让服务器 启动时候自动 运行 tomcat 了。    
    最简单的方法就是通过startup.sh来自动启动Tomcat。
	
	1)编辑vi /etc/rc.d/rc.local    
    2)文件最后增加内容(假设JDK目录是/usr/java，Tomcat目录是/usr/local/tomcat)    
    export JDK_HOME=/usr/java/jdk1.7.0    
    export JAVA_HOME=/usr/java/jdk1.7.0    
    /usr/tomcat/bin/startup.sh    
    保存退出