### 1. **镜像相关命令**

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

### 2. **容器相关命令**

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

### 3. **网络相关命令**

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

### 4. **卷相关命令**

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

### 5. **系统管理相关命令**

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

### 6. **Docker Compose 相关命令**

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