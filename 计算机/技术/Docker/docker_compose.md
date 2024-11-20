# docker-compose.yml 文件

### 按需加载环境变量文件

1. **创建不同的 `docker-compose` 配置文件**

   - `docker-compose.yml`：包含通用的服务定义。
   - `docker-compose.override.yml`（可选）：自动加载的配置文件（用于开发环境）。
   - `docker-compose.prod.yml`：专门为生产环境准备的配置文件。

2. **目录结构示例**

   ```bash
   ├── docker-compose.yml
   ├── docker-compose.prod.yml
   ├── env/
   │   ├── app.dev.env
   │   ├── app.prod.env
   │   ├── db.dev.env
   │   ├── db.prod.env
   │   ├── cache.dev.env
   │   └── cache.prod.env
   ```
   
3. **`docker-compose.yml` 文件 (基础配置)**

   ```yaml
   version: '3.8'
   
   services:
     app:
       image: app-image
       ports:
         - "8080:8080"
     
     db:
       image: db-image
       ports:
         - "3306:3306"
   
     cache:
       image: cache-image
       ports:
         - "6379:6379"
   ```

4. **`docker-compose.override.yml` 文件 (开发环境配置)**

   这是用于开发环境的文件，默认会被加载。如果需要显式地指定 `docker-compose.override.yml`，可以通过命令行来控制。

   ```yaml
   version: '3.8'
   
   services:
     app:
       env_file:
         - ./env/app.dev.env
   
     db:
       env_file:
         - ./env/db.dev.env
   
     cache:
       env_file:
         - ./env/cache.dev.env
   ```

5. **`docker-compose.prod.yml` 文件 (生产环境配置)**

   这是用于生产环境的文件，需要在部署时显式指定。

   ```yaml
   version: '3.8'
   
   services:
     app:
       env_file:
         - ./env/app.prod.env
   
     db:
       env_file:
         - ./env/db.prod.env
   
     cache:
       env_file:
         - ./env/cache.prod.env
   ```

### 命令行加载不同环境的配置文件

#### 开发环境 (默认)

可以直接使用 `docker-compose up`，因为 `docker-compose.override.yml` 会被自动加载：

```
docker-compose up
```

这将加载 `docker-compose.yml` 和 `docker-compose.override.yml` 中的环境变量文件。

#### 生产环境

需要部署生产环境时，使用 `-f` 参数显式指定生产环境的配置文件：

```bash
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up
```

这将加载 `docker-compose.yml` 和 `docker-compose.prod.yml` 中定义的环境变量文件。

### 优先级说明

- 通过多个 `-f` 文件指定时，`docker-compose` 会合并这些文件，后面的文件会覆盖前面的配置。例如，如果 `docker-compose.prod.yml` 中有与 `docker-compose.yml` 不同的配置，生产环境的文件会覆盖默认文件中的配置。

### 使用环境变量

也可以根据不同的环境变量文件来控制哪些文件被加载。例如：

```bash
# 加载开发环境
docker-compose --env-file ./env/app.dev.env up

# 加载生产环境
docker-compose --env-file ./env/app.prod.env -f docker-compose.prod.yml up
```

这样，可以非常灵活地根据不同的需求加载开发或生产环境的变量和配置文件。

# Docker Compose 命令

Docker Compose 提供了一组命令，可以帮助用户启动、停止、构建、查看日志等操作。以下是 Docker Compose 常见命令的详细介绍。

### 1. **`docker-compose up`**

- **作用**：根据 `docker-compose.yml` 文件启动所有服务。

- **常见用法**：

  ```bash
  docker-compose up
  ```
  
  - 启动当前目录下 `docker-compose.yml` 中定义的所有服务。
  
  ```
  docker-compose up -d
  ```

  - 启动服务并在后台运行（即以分离模式运行）。
  
  ```bash
  docker-compose up --build
  ```
  
  - 强制重新构建镜像（如果 `docker-compose.yml` 中定义了镜像构建部分，或者 Dockerfile 发生变化时需要使用这个命令）。

  ```bash
  docker-compose up <service_name>
  ```
  
  - 仅启动指定的服务。
  
- **注意**：

  - `docker-compose up` 会自动创建和启动容器。如果服务已启动，它会检测并重启容器（根据 `docker-compose.yml` 中的设置）。
  - 在启动时，`docker-compose` 会检查是否需要构建镜像、创建网络和挂载卷等。

------

### 2. **`docker-compose down`**

- **作用**：停止并删除容器、网络、卷、镜像等。它是 `docker-compose up` 的反向操作。

- **常见用法**：

  ```bash
  docker-compose down
  ```
  
  - 停止并删除服务容器、网络（由 `docker-compose.yml` 文件定义的）和挂载的卷。
  
  ```bash
  docker-compose down --volumes
  ```

  - 停止并删除服务容器、网络、以及 Docker 卷。
  
  ```bash
  docker-compose down --rmi all
  ```
  
  - 停止并删除服务容器、网络、镜像（所有镜像）。

  ```bash
  docker-compose down --remove-orphans
  ```
  
  - 删除未在 `docker-compose.yml` 文件中定义的孤立容器。
  
- **注意**：

  - 使用 `down` 时，默认只删除由 Compose 创建的容器、网络和挂载的卷，不会删除数据卷和外部网络，除非明确使用 `--volumes` 或 `--rmi` 选项。

------

### 3. **`docker-compose build`**

- **作用**：根据 `docker-compose.yml` 文件构建或重新构建服务的镜像。

- **常见用法**：

  ```bash
  docker-compose build
  ```
  
  - 根据 `docker-compose.yml` 文件构建所有服务的镜像。
  
  ```bash
  docker-compose build <service_name>
  ```

  - 仅构建指定服务的镜像。
  
  ```bash
  docker-compose build --no-cache
  ```
  
  - 不使用缓存重新构建镜像，强制 Docker 重新构建每个步骤。

  ```bash
  docker-compose build --pull
  ```
  
  - 强制拉取镜像（即使本地有镜像）。
  
- **注意**：

  - `docker-compose build` 仅构建镜像，并不会启动容器。如果你希望构建并启动容器，可以使用 `docker-compose up --build`。

------

### 4. **`docker-compose logs`**

- **作用**：查看服务的日志输出。

- **常见用法**：

  ```bash
  docker-compose logs
  ```
  
  - 查看所有服务的日志输出。
  
  ```bash
  docker-compose logs <service_name>
  ```

  - 查看指定服务的日志输出。
  
  ```bash
  docker-compose logs -f
  ```
  
  - 实时查看服务的日志输出（与 `tail -f` 类似，持续显示日志）。

  ```bash
  docker-compose logs --tail="100"
  ```
  
  - 查看最近 100 行日志。
  
- **注意**：

  - `logs` 命令可以帮助你调试应用，查看容器的标准输出和标准错误信息。

------

### 5. **`docker-compose ps`**

- **作用**：查看当前 Compose 项目的容器状态。

- **常见用法**：

  ```bash
  docker-compose ps
  ```
  
  - 显示当前 Compose 项目的所有容器及其状态。
  
  ```bash
docker-compose ps -a
  ```

  - 显示所有容器（包括已停止的容器）。
  
  ```bash
  docker-compose ps -q
  ```
  
  - 仅显示容器 ID。

- **注意**：

  - `ps` 可以帮助你查看当前服务的运行状态、容器 ID、端口映射等信息。

------

### 6. **`docker-compose exec`**

- **作用**：在运行中的容器中执行命令。

- **常见用法**：

  ```bash
  docker-compose exec <service_name> <command>
  ```
  
  - 在指定服务的容器中执行命令。例如，进入容器并打开交互式 shell：
  
  ```bash
docker-compose exec <service_name> bash
  ```

  - 在运行中的容器中执行 `bash`，通常用于调试和操作容器内部。
  
  ```bash
  docker-compose exec <service_name> ls /app
  ```
  
  - 在指定容器的 `/app` 目录中执行 `ls` 命令。

- **注意**：

  - `exec` 命令类似于 `docker exec`，但是它是与 `docker-compose.yml` 中定义的服务直接交互，而不需要容器 ID。

------

### 7. **`docker-compose run`**

- **作用**：运行一个一次性的容器，而不依赖于 `docker-compose.yml` 中的服务定义。

- **常见用法**：

  ```bash
  docker-compose run <service_name> <command>
  ```
  
  - 运行一个临时容器并执行指定的命令。与 `exec` 不同的是，`run` 命令会创建一个新的容器，而 `exec` 会在已有容器中执行命令。
  
  ```bash
  docker-compose run --rm <service_name> <command>
  ```

  - `--rm` 选项会在容器停止后自动删除容器。这个选项对于运行一次性的任务（如数据库迁移、初始化等）非常有用。
  
- **注意**：

  - `docker-compose run` 不会启动依赖的其他服务。它是用来运行临时任务的。

------

### 8. **`docker-compose start`**

- **作用**：启动已停止的服务容器。

- **常见用法**：

  ```bash
  docker-compose start
  ```
  
  - 启动已停止的所有容器。
  
  ```bash
  docker-compose start <service_name>
  ```

  - 启动指定的服务容器。
  
- **注意**：

  - 与 `docker-compose up` 不同，`start` 仅会启动已经存在的容器，而不会重新创建容器。

------

### 9. **`docker-compose stop`**

- **作用**：停止运行中的服务容器。

- **常见用法**：

  ```bash
  docker-compose stop
  ```
  
  - 停止所有正在运行的服务容器。
  
  ```bash
  docker-compose stop <service_name>
  ```

  - 停止指定服务的容器。
  
- **注意**：

  - 停止容器但不删除它们。如果需要删除已停止的容器，可以使用 `docker-compose down`。

------

### 10. **`docker-compose restart`**

- **作用**：重启服务容器。

- **常见用法**：

  ```bash
  docker-compose restart
  ```
  
  - 重启所有服务容器。
  
  ```bash
  docker-compose restart <service_name>
  ```

  - 重启指定服务的容器。
  
- **注意**：

  - `restart` 会停止容器并重新启动它们，常用于调试或在配置更改后需要重新加载服务时。