# Docker容器备份与迁移

## 1.备份容器:

在远程主机上查看当前容器列表 

```
$ docker ps -a

CONTAINER ID   IMAGE                     COMMAND                  CREATED       STATUS        PORTS                              NAMES

403e6db0c   jenkins/jenkins           "/sbin/tini -- /usr/…"   9 days ago    Up 9 days             0.0.0.0:8080->8080/tcp, :::8080->8080/tcp, 0.0.0.0:50000->50000/tcp, :::50000->50000/tcp             jenkins

a7d775e3d   mysql                     "docker-entrypoint.s…"   10 days ago   Up 9 days             0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp
```

## 2.制作容器备份 

```
docker commit -p <容器ID> <备份名称>
docker commit -p 403e6db0c ghproxy
```

## 3. 查看备份是否成功

```
$ docker images
REPOSITORY                        TAG       IMAGE ID       CREATED          SIZE
qinglong                          latest    0860103ceec8   4 seconds ago    455MB
```

确认已经成功备份

## 4. 将镜像制作成文件

说明: 使用docker save 可能需要使用root权限

保存的命令是:

```
docker save -o [文件名] [镜像]

[zhouhuwei@localhost ~]$ docker save -o jenkins_backup.tar jenkins_backup

[zhouhuwei@localhost ~]$ ls

jenkins_backup.tar
```

备份文件制作完成

## 5. 在本地使用命令将镜像从远程备份到本地

```
$ scp zhouhuwei@192.168.10.10:/home/zhouhuwei/jenkins_backup.tar  /Users/louiezhou/home/sf/DockerImageBackup

zhouhuwei@192.168.10.10's password:

jenkins_backup.tar        100%  639MB   7.7MB/s   01:23
```

备份到本地目录是:`/Users/louiezhou/home/sf/DockerImageBackup`

到此就将镜像备份到本地, 我们去对应目录check下:

```
$ cd /Users/louiezhou/home/sf/DockerImageBackup

$ ls -lh

total 1310752

-rw-------  1 louiezhou  staff   639M 10 18 10:42 jenkins_backup.tar
```

### 1.恢复容器:

为了验证容器是否能正常导入, 先删除docker 里的镜像

```
 docker rmi jenkins
```

### 2.导入

```
docker load -i <镜像文件> 

louie-mac:sf louiezhou$ docker images

REPOSITORY         TAG       IMAGE ID       CREATED          SIZE

jenkins_backup     latest    edfced27a56e   32 minutes ago   444MB
```



### 3.运行镜像

```
docker run -p 8080:8080 -name qinglong -d qinglong
```

### 4. 启动成功

```
903e6db0c   jenkins/jenkins    "/sbin/tini -- /usr/…"     Up 1 min    0.0.0.0:8080->8080/tcp, :::8080->8080/tcp, 0.0.0.0:50000->50000/tcp, :::50000->50000/tcp          jenkins
```

