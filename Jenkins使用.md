# Jenkins命令

Jenkins:
启动：java -jar jenkins.war --httpPort=8080
WAR/EAR files相对路径是相对于jenkins下的workspace/任务名称

查找文件
find / -amin -10|grep zlib 所有文件夹下最近10分钟访问过的关于zlib的文件

springboot启动脚本

1. 创建对应启动脚本restart.sh

    ```bash
    restart.sh
    source ~/.bash_profile
    killall java
    nohup java -jar /root/springbootdemo/springbootdemo-0.0.1-SNAPSHOT.jar > nohup.log 2>&1 &
    ```

2. 执行启动脚本  
sh /root/springbootdemo/restart.sh
