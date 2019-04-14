# Docker常用命令

## 1. 启动mysql容器

```bash
docker run --name mysql55 \
-p 8100:3306 \
-e MYSQL_ROOT_PASSWORD=password \
-v /opt/docker-file/mysql55/my.cnf:/etc/my.cnf \
-v /opt/docker-file/mysql55/persist-data:/var/lib/mysql \
-d mysql/mysql-server:5.5.62
```

说明
    --name mysql55 给容器取个别名
    -p 把宿主机器的ip映射到容器开放的ip
    -e 用于设置容器中的环境变量，这里设置了数据库root账号的密码
    -v 用于把宿主机上的文件/空间映射给容器，让容器使用宿主机的文件/空间，
         这里利用外部my.cnf设置mysql的编码为utf-8，并且把容器中数据库的文件保存到宿主机上，防止删除容器后，数据就不在了
    -d 后台运行容器，并返回容器ID

数据库启动后，远程访问在mysql下执行：

1. bash-4.2# mysql -uroot -proot
2. mysql>use mysql;
3. mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
4. mysql>flush privileges;

## 进入启动了的容器
docker exec -it containerID /bin/bash

## 容器与宿主机文件复制

docker cp /opt/xxxx.conf container_name:/path/to/save
docker cp container_name:/source/file /opt/file