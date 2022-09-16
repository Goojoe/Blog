# Back up and restore data

*Estimated reading time: 2 minutes*

使用以下程序来保存和恢复你的镜像和容器数据。如果你想重置你的虚拟机磁盘或将你的Docker环境转移到新的计算机上，例如，这很有用。

> 我应该备份我的容器吗？
>
> 如果你使用卷或绑定挂载来存储你的容器数据，可能不需要备份你的容器，但要确保记住创建容器时使用的选项，如果你想在重新安装后以相同的配置重新创建你的容器，可以使用[Docker Compose file](https://docs.docker.com/compose/compose-file/)

## 	保存你的数据

1. 用[`docker container commit`](https://docs.docker.com/engine/reference/commandline/commit/)将你的容器提交到一个镜像中。

    提交一个容器会将容器文件系统的变化和容器的一些配置，例如标签和环境变量，作为一个本地镜像存储起来。请注意，环境变量可能包含敏感信息，如密码或代理认证，所以在将生成的镜像推送到注册表时，应该注意。

    还要注意，连接到容器的卷中的文件系统变化不包括在镜像中，必须单独备份。

    如果你使用[命名卷](https://docs.docker.com/storage/#more-details-about-mount-types)来存储容器数据，如数据库，请参阅存储部分的[备份、恢复或迁移数据卷](https://docs.docker.com/storage/volumes/#backup-restore-or-migrate-data-volumes)页面。

2. 使用[`docker push`](https://docs.docker.com/engine/reference/commandline/push/)来推送任何你在本地构建并希望保留的镜像到[Docker Hub注册库](https://docs.docker.com/docker-hub/)。

    请确保将[仓库的可见性配置为 "私有"](https://docs.docker.com/docker-hub/repos/#private-repositories)，以便不被公众访问的图像。

    另外，使用[`docker image save -o images.tar image1 [image2 ...\]`](https://docs.docker.com/engine/reference/commandline/save/)来保存任何你想保留的图像到本地的tar文件。

备份数据后，你可以卸载当前版本的Docker Desktop并[安装不同的版本](https://docs.docker.com/desktop/release-notes/)或将Docker Desktop重置为出厂默认值。

## 恢复你的数据

1. 使用[`docker pull`](https://docs.docker.com/engine/reference/commandline/load/)来恢复你推送到Docker Hub的镜像。

    如果你把镜像备份到本地的tar文件，使用[`docker image load -i images.tar`](https://docs.docker.com/engine/reference/commandline/load/)来恢复以前保存的镜像。

2. Re-create your containers if needed, using [`docker run`](https://docs.docker.com/engine/reference/commandline/load/), or [Docker Compose](https://docs.docker.com/compose/).

Refer to the [backup, restore, or migrate data volumes](https://docs.docker.com/storage/volumes/#backup-restore-or-migrate-data-volumes) page in the storage section to restore volume data.