# 一些Linux命令

**性能监控命令**：
vmstat    可以用来监控虚拟内存。可对操作系统的虚拟内存、IO、CPU等多个指标的整体情况进行监视。

**查看服务列表命令**：
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

**修改linux默认日期显示格式**
临时修改：
export TIME_STYLE='+%Y-%m-%d %H:%M:%S'
永久修改：
echo "export TIME_STYLE='+%Y-%m-%d %H:%M:%S'" >> /etc/profile
source /etc/profile
