### 按需加载环境变量文件

1. **创建不同的 `docker-compose` 配置文件**

   - `docker-compose.yml`：包含通用的服务定义。
   - `docker-compose.override.yml`（可选）：自动加载的配置文件（用于开发环境）。
   - `docker-compose.prod.yml`：专门为生产环境准备的配置文件。

2. **目录结构示例**

   ```
   bash复制代码.
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

   ```
   yaml复制代码version: '3.8'
   
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

   ```
   yaml复制代码version: '3.8'
   
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

   ```
   yaml复制代码version: '3.8'
   
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

### 6. **命令行加载不同环境的配置文件**

#### 开发环境 (默认)

可以直接使用 `docker-compose up`，因为 `docker-compose.override.yml` 会被自动加载：

```
bash


复制代码
docker-compose up
```

这将加载 `docker-compose.yml` 和 `docker-compose.override.yml` 中的环境变量文件。

#### 生产环境

需要部署生产环境时，使用 `-f` 参数显式指定生产环境的配置文件：

```
bash


复制代码
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up
```

这将加载 `docker-compose.yml` 和 `docker-compose.prod.yml` 中定义的环境变量文件。

### 7. **优先级说明**

- 通过多个 `-f` 文件指定时，`docker-compose` 会合并这些文件，后面的文件会覆盖前面的配置。例如，如果 `docker-compose.prod.yml` 中有与 `docker-compose.yml` 不同的配置，生产环境的文件会覆盖默认文件中的配置。

### 8. **使用环境变量**

也可以根据不同的环境变量文件来控制哪些文件被加载。例如：

```
bash复制代码# 加载开发环境
docker-compose --env-file ./env/app.dev.env up

# 加载生产环境
docker-compose --env-file ./env/app.prod.env -f docker-compose.prod.yml up
```

这样，可以非常灵活地根据不同的需求加载开发或生产环境的变量和配置文件。