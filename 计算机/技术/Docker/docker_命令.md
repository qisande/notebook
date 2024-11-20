### 1. 镜像相关命令

- **拉取镜像**：

  ```
  docker pull <image_name>
  ```

  从 Docker Hub 或指定的镜像仓库拉取镜像。例如：

  ```
  docker pull ubuntu:latest
  ```

- **列出本地镜像**：

  ```
  docker images
  ```

  显示本地所有的 Docker 镜像。

- **删除镜像**：

  ```
  docker rmi <image_name_or_id>
  ```

  删除本地的 Docker 镜像。例如：

  ```
  docker rmi ubuntu:latest
  ```

- **构建镜像**：

  ```
  docker build -t <image_name> <path_to_dockerfile>
  ```

  从 `Dockerfile` 构建镜像。例如：

  ```
  docker build -t myapp:latest .
  ```

- **为镜像打标签**：

  ```bash
  docker tag <old_image:old_tag> <new_image:new_tag>
  docker tag <IMAGE ID> <new_image:new_tag>
  ```

  

### 2. 容器相关命令

- **运行容器**：

  ```
  docker run -it --name <container_name> <image_name>
  ```

  创建并启动一个新容器。例如：

  ```
  docker run -it --name myubuntu ubuntu
  ```

  常用选项：

  - `-d`：后台运行容器。
  - `-p <host_port>:<container_port>`：将容器端口映射到主机端口。
  - `-v <host_path>:<container_path>`：将主机目录挂载到容器目录。

- **列出正在运行的容器**：

  ```
  docker ps
  ```

  显示当前正在运行的所有容器。

- **列出所有容器**：

  ```
  docker ps -a
  ```

  显示所有容器，包括停止的容器。

- **停止容器**：

  ```
  docker stop <container_name_or_id>
  ```

  停止一个正在运行的容器。

- **启动已停止的容器**：

  ```
  docker start <container_name_or_id>
  ```

  启动已停止的容器。

- **重启容器**：

  ```
  docker restart <container_name_or_id>
  ```

  重启一个容器。

- **删除容器**：

  ```
  docker rm <container_name_or_id>
  ```

  删除一个已停止的容器。使用 `-f` 强制删除正在运行的容器。

- **查看容器日志**：

  ```
  docker logs <container_name_or_id>
  ```

  查看容器的输出日志。

- **进入正在运行的容器**：

  ```
  docker exec -it <container_name_or_id> /bin/bash
  ```

  进入容器的交互式终端。

- **检查容器信息**：

  ```
  docker inspect <container_name_or_id>
  ```

  显示容器的详细配置信息和状态。

### 3. 网络相关命令

- **列出 Docker 网络**：

  ```
  docker network ls
  ```

  显示 Docker 的所有网络。

- **创建网络**：

  ```
  docker network create <network_name>
  ```

  创建一个新的 Docker 网络。

- **连接容器到网络**：

  ```
  docker network connect <network_name> <container_name_or_id>
  ```

  将容器连接到指定的网络。

- **断开容器与网络的连接**：

  ```
  docker network disconnect <network_name> <container_name_or_id>
  ```

  将容器从网络中断开。

- **删除网络**：

  ```
  docker network rm <network_name>
  ```

  删除 Docker 网络。

### 4. 卷相关命令

- **列出卷**：

  ```
  docker volume ls
  ```

  列出所有 Docker 卷。

- **创建卷**：

  ```
  docker volume create <volume_name>
  ```

  创建一个新的 Docker 卷。

- **删除卷**：

  ```
  docker volume rm <volume_name>
  ```

  删除 Docker 卷。

- **查看卷信息**：

  ```
  docker volume inspect <volume_name>
  ```

  查看卷的详细信息。

### 5. 系统管理相关命令

- **查看 Docker 版本**：

  ```
  docker --version
  ```

  或

  ```
  docker version
  ```

- **查看 Docker 系统信息**：

  ```
  docker info
  ```

  显示 Docker 的系统级别信息，包括容器、镜像、网络、卷的数量等。

- **清理未使用的资源**：

  ```
  docker system prune
  ```

  删除所有未使用的容器、网络、镜像（未被任何容器使用）和卷。

### 6. Docker Compose 相关命令

如果你使用 `docker-compose.yml` 来管理容器，可以使用以下命令：

- **启动所有服务**：

  ```
  docker-compose up
  ```

  使用 `-d` 后台运行服务。

- **停止所有服务**：

  ```
  docker-compose down
  ```

- **查看服务日志**：

  ```
  docker-compose logs
  ```

### 7. 配置 docker 国内镜像源

docker 的镜像源文件配置在 `/etc/docker/daemon.json`处，如果没有的话我们就创建一个然后再修改。

```bash
sudo vim  /etc/docker/daemon.json
```

在配置文件中添加需要的镜像源链接，如下所示：

```json
{
  "registry-mirrors": [
    "https://jobl969e.mirror.aliyuncs.com",
    "https://docker.m.daocloud.io",
    "https://docker.1panel.live",
    "https://docker.mirrors.ustc.edu.cn/",
    "https://hub-mirror.c.163.com",
    "https://registry.docker-cn.com"
  ]
}
```

重启 docker，注意由于走的是守护程序daemon，所以daemon进程也需要重启。

```bash
sudo systemctl daemon-reload		#重启daemon进程
sudo systemctl restart docker		#重启docker
```

最后我们再验证一下是否修改成功，运行

```bash
docker info
```

在长串info信息中如果出现类似下文的内容：

```bash
Registry Mirrors:
  https://jobl969e.mirror.aliyuncs.com/
  https://docker.m.daocloud.io/
  https://docker.1panel.live/
  https://docker.mirrors.ustc.edu.cn/
  https://hub-mirror.c.163.com/
  https://registry.docker-cn.com/
```

镜像拉取测试

```bash
docker pull hello-world
```

### 8. Docker将镜像导出到本地,上传至内网服务器上

有两种方法，一种是通过容器，一种是通过镜像，其实本质是一样的，容器的实质就是镜像

方法一：通过容器

1 首先使用docker`ps -a` 查看本机上的所有容器

```bash
docker ps -a
```

2 导出镜像

使用 docker export 命令根据容器 id 将镜像导成一个文件

```bash
docker export 容器id > image.tar
```

上面命令执行之后，我们便可以通过 ls 命令在当前目录下发现 image.tar

3 导入镜像

使用 docker import 命令将这个镜像导进来

```bash
docker import 容器名 < image.tar
```

通过 docker images 命令查看镜像是否导入

```bash
docker images
```

方法二：通过镜像

1 通过 docker image 查看本机上的所有镜像

```bash
docker images
```

2 找到要上传的镜像的 id, 使用 docker save 命令将镜像保存为一个文件

```bash
docker save 镜像id > image.tar
```

docker save 可以将多个 image 打包成一个文件

```bash
docker save -o image.tar 镜像1 镜像2
```

3 通过 docker load 载入镜像

```bash
docker load < image.tar
```

这两种方案的差别

#### **1，文件大小不同**

export 导出的镜像文件体积小于 save 保存的镜像

#### **2，是否可以对镜像重命名**

- docker import 可以为镜像指定新名称
- docker load 不能对载入的镜像重命名

#### **3，是否可以同时将多个镜像打包到一个文件中**

- docker export 不支持
- docker save 支持

#### **4，是否包含镜像历史**

- export 导出（import 导入）是根据容器拿到的镜像，再导入时会丢失镜像所有的历史记录和元数据信息（即仅保存容器当时的快照状态），所以无法进行回滚操作。
- 而 save 保存（load 加载）的镜像，没有丢失镜像的历史，可以回滚到之前的层（layer）。

#### **5，应用场景不同**

- docker export 的应用场景：主要用来制作基础镜像，比如我们从一个 ubuntu 镜像启动一个容器，然后安装一些软件和进行一些设置后，使用 docker export 保存为一个基础镜像。然后，把这个镜像分发给其他人使用，比如作为基础的开发环境。
- docker save 的应用场景：如果我们的应用是使用 docker-compose.yml 编排的多个镜像组合，但我们要部署的客户服务器并不能连外网。这时就可以使用 docker save 将用到的镜像打个包，然后拷贝到客户服务器上使用 docker load 载入。