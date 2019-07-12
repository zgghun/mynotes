# 一些Linux命令

**性能监控**：
vmstat 可以用来监控虚拟内存。可对操作系统的虚拟内存、IO、CPU等多个指标的整体情况进行监视。

**查看服务列表**：
chkconfig --list

**已经安装软件查找**：
yum list installed | grep mysql 或者
rpm -qa |grep mysql

**查看安装位置**：
rpm -qal | grep mysql

**文件查找**：
find / -name mysql_config.conf

**查看文件大小**：
du -h -d 1

**查看ip信息**：
ip addr

**修改linux默认日期显示格式**：
临时修改：
export TIME_STYLE='+%Y-%m-%d %H:%M:%S'
永久修改：
echo "export TIME_STYLE='+%Y-%m-%d %H:%M:%S'" >> /etc/profile
source /etc/profile

**通过源码安装软件**：

1. 下载***.tar.gz文件
2. 解压 tar zxf ***.tar.gz
3. cd 解压目录
4. 设置安装配置参数，如设置安装路径 ./configure PREFIX=/opt/***
5. 编译 make
6. 安装 make install

**SpringBoot项目启动**：
`nohup java -jar jeesuite-config-server.jar > config-server.out 2>&1 &`

**利用cat查看指定行**:  
从100行开始显示 cat | tail -n +100
显示前500行 cat filename | head -n 500
显示第100到500 cat filename | head -n 500 | tail -n +100

