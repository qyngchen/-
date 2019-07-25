##docker使用笔记
###dockerfile
    FROM baseimage:0.0.1
	ENV TZ Asia/Shanghai
	ENV db.name smart_office
	WORKDIR /usr/jar
	ADD wiseservice-1.0-SNAPSHOT.jar /usr/jar
	CMD java -server -Xms4096m -Xmx4096m -Xmn2048m -XX:+UseConcMarkSweepGC -XX:+UseCMSCompactAtFullCollection -XX:CMSInitiatingOccupancyFraction=70 -XX:+CMSParallelRemarkEnabled -XX:+CMSClassUnloadingEnabled -XX:SurvivorRatio=8 -XX:+DisableExplicitGC -jar /usr/jar/wiseservice-1.0-SNAPSHOT.jar
