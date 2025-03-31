# Week 4: Docker & Containerisation

## Level 1 (Intern)
**Task**: Pull a public Docker image and run it. Use Docker commands to list and stop containers.

**Objective**: Execute basic Docker commands and run containers.

在terminal中,可以使用`docker help`来查看docker指令, 并且可以使用 `docker run --help` 来查看指令中 `-h`,`-p`,`-d` 等缩写的意思.

<details>

  <summary>关于常用的 curl 命令</summary>
  <div style="color: lightgray;">


`curl` 是一个强大的工具，广泛用于测试 API、调试网络请求和自动化脚本中。

`curl` 是一个命令行工具，用于从命令行发送 HTTP 请求并获取数据。它支持多种协议，包括 HTTP、HTTPS、FTP 等。以下是 `curl` 的一些常见用途：

### 常见用途

1. **获取网页内容**：
   - `curl http://example.com`：获取指定 URL 的网页内容。

2. **测试本地服务器**：
   - `curl 127.0.0.1` 和 `curl 127.0.0.1:8080` 都是用于向本地服务器发送请求的命令。它们可以与 Docker 一起使用来测试在容器中运行的服务。

3. **下载文件**：
   - `curl -O http://example.com/file.txt`：下载文件并保存到当前目录。

4. **发送数据**：
   - `curl -d "param1=value1&param2=value2" http://example.com/resource`：使用 POST 方法发送数据。

5. **查看响应头**：
   - `curl -I http://example.com`：仅获取响应头信息。

6. **指定请求方法**：
   - `curl -X POST http://example.com`：使用指定的 HTTP 方法（如 POST）。

### 选项和参数

- `-v`：显示详细的请求和响应信息。
- `-H`：添加自定义请求头。
- `-u`：提供用户名和密码进行身份验证。


### `curl 127.0.0.1` 和 `curl 127.0.0.1:8080` 区别

- **`curl 127.0.0.1`**：
  - 默认请求本地服务器的端口 80。
  - 适用于在本地机器上运行的服务，或在 Docker 容器中映射到主机的端口 80 的服务。

- **`curl 127.0.0.1:8080`**：
  - 请求本地服务器的端口 8080。
  - 常用于测试在 Docker 容器中运行的服务，这些服务通过端口映射暴露在主机的 8080 端口上。

### Docker 相关使用

- 当你在 Docker 容器中运行一个服务，并将其端口映射到主机的端口时，可以使用 `curl` 来测试服务是否正常运行。
- 例如，如果你在 Docker 中运行一个 Web 应用，并将其容器的 80 端口映射到主机的 8080 端口，你可以使用 `curl 127.0.0.1:8080` 来访问该应用。
</div>
</details>


### Step by Step Guide:

1. **Install Docker**
    - Visit [Docker's website](https://www.docker.com/products/docker-desktop) and download Docker Desktop.
    - Install Docker Desktop by following the instructions provided by the installer.
2. **Pull a Docker Image**
    - Open a terminal.
    - Pull an image using the command: `docker pull <image-name>`.
    - For example, to pull nginx image, run `docker pull nginx`.
3. **Run the Docker Image**
    - Run the image using the command: `docker run -d -p 8080:80 <image-name>`.
    - For example, to run nginx, run `docker run -d -p 8080:80 nginx`.
4. **List Docker Containers**
    - Run the command: `docker ps`.
5. **Stop a Docker Container**
    - Find the Container ID from the output of the `docker ps` command.
    - Run the command: `docker stop <container-id>`.

## Level 2 (Junior)
**Task**: Build a custom Docker image for a simple app and deploy it locally.

**Objective**: Learn Docker image creation and management.

### Step by Step Guide:

1. **Create a Dockerfile**
    - Create a file named Dockerfile in your application directory.
    - Fill it with instructions to build your Docker image. A simple Node.js app's Dockerfile might look like https://github.com/AutomationLover/website-visit-count/blob/main/Dockerfile
   
   folk到了我自己的repo,地址是: https://github.com/RedRe4per/website-visit-count

**怎么开始做给定app的 Dockerfile 的调试?**

首先是可以在本机跑这些命令,跑通了再去打包.本机调试就需要先把 GitHub 的 repo 给 clone 下来,也就是和调试 developer 的代码相同的.
  在本机建一个 Developer 之类的专门文件夹, 然后在里面用 `mkdir docker` 来创建专门的文件夹, 然后 `cd` 进去,再 clone. clone 后再 cd 进去那个根文件夹,输入提示中就会出现(main), 这时就可以开始正式调试了.
  
<details> 
  <summary>docker在什么目录下运行都是一样的</summary>
  是的，在 macOS 系统中，无论你在什么目录下运行 Docker 命令，效果都是一样的。Docker 是一个独立的服务，运行在后台，与当前工作目录无关。

### 影响因素

- **当前目录**：只有在你使用相对路径或需要访问当前目录的文件时，当前目录才会影响命令。例如，使用 `docker build .` 时，当前目录会被用作构建上下文。
- **绝对路径**：如果你在命令中使用绝对路径，当前目录不会影响命令的执行。

### 总结

- 启动 Docker 容器的命令在任何目录下执行都是一样的。
- 只有在涉及文件路径时，当前目录才会影响命令的执行。
</details> 

<details> 
  <summary>使用 vim <文件名> 在 terminal 中直接进入文件编辑</summary>
    主要就是按 `esc` 和按 `:` 来输入指令保存和返回.
  在 Vim 中编辑 `app.py` 文件时，你可以使用以下命令来保存和退出：

### 保存并退出

1. **保存并退出**：
   - 按 `Esc` 键进入命令模式。
   - 输入 `:wq` 然后按 `Enter`。

2. **仅保存**：
   - 按 `Esc` 键进入命令模式。
   - 输入 `:w` 然后按 `Enter`。

### 不保存退出

1. **不保存退出**：
   - 按 `Esc` 键进入命令模式。
   - 输入 `:q!` 然后按 `Enter`。

这些命令适用于在 Vim 中编辑任何文件，包括 `app.py`。
</details> 

<details> 
  <summary>关于这个dockerfile的repo</summary>
这个app要启动数据库与后端.
这里面是一个python后端.文件结构是

```
.
├── Dockerfile
├── README.md
├── app.py
├── compose.yaml
└── requirements.txt
```

### 启动redis数据库
  后端需要连接至redis数据库,因为在app.py中有代码:
  ```python
redis = Redis(host='redis', port=6379)
```
所以这里要启动redis的docker image和后端的docker image.
在启动redis的docker image时,用到指令`docker run -d -p 6379:6379 --name=redis redislabs/redismod`,其中`--name=redis`是必要的,因为上面的代码中有语句`host='redis'`,即必须使用名称`redis`来识别.

### 运行后端主app
想运行 app.py (也就是后端主文件),需要用指令 `python3 app.py`,但是直接使用的话会报错 ModuleNotFoundError: No module named 'flask'. 这是因为没有安装依赖(与node.js完全一样的道理).
使用 `pip3 install -r requirements.txt` 来安装.安装后再运行 `python3 app.py` 就能跑起来了.

这里成功后会进入 redis.exceptions 的 ConnectionError的 list 页面.

到此为止,我们之前所有的工作都是为了测试 dockerfile 中的第一步,也就是创建打包 builder.
之后按照 https://github.com/RedRe4per/website-visit-count/blob/main/README.md 指示,运行指令来 build image,检查,run image,即可在本机浏览器输入映射的端口号以查看部署后的网页(此时已经与运行redis数据库的docker image相连了.

</details>

<details> 
  <summary>使用 git log 与 git reset --hard <commit id> 来直接回滚更改的丝滑小连招</summary>
比如刚才为了测试 app.py 文件的可用性与 debug,我修改了很多东西,加了注释等.现在测试完了需要改回去了.

有一种办法可以直接回滚更改.
1. 使用指令 `git log` 来列出文件更新的 commit.
2. 使用 `git reset --hard <commit id>` 来直接回滚到指定版本.

</details>


3. **Build the Docker Image**
    - Run the command: `docker build -t <image-name> .`.
    - For example, to build an image named my-app, run `docker build -t my-app .`.
    - Following steps https://github.com/AutomationLover/website-visit-count/blob/main/README.md, after git clone this repo

4. **Deploy App**
    - Deploy app with docker cli
    - Deploy app with docker compose
    - Deploy app with Docker Development Environments

### 关于docker compose
docker compose这个文件允许同时运行几个 docker image,并且能够指明它们的关系(例如依赖),也就是orchestration.

开启docker compose: `docker compose up -d`

关闭docker compose: `docker compose down`

<details> 
  <summary>详细</summary>
Docker Compose 是一个用于定义和管理多容器 Docker 应用的工具。它使用 YAML 文件来配置应用的服务、网络和卷。以下是 Docker Compose 的一些关键特性：

### 主要特性

1. **多容器管理**：
   - 可以同时启动和管理多个容器。

2. **YAML 配置**：
   - 使用 `docker-compose.yml` 文件定义应用的服务、网络和卷。

3. **简化命令**：
   - 使用简单的命令（如 `docker-compose up` 和 `docker-compose down`）来启动和停止应用。

4. **环境隔离**：
   - 支持在不同环境中运行相同的应用配置。

### 基本用法

1. **定义服务**：
   - 在 `docker-compose.yml` 中定义应用的各个服务。

2. **启动应用**：
   - 使用 `docker-compose up` 启动所有服务。

3. **停止应用**：
   - 使用 `docker-compose down` 停止并移除所有服务。

### 示例 `docker-compose.yml`

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  redis:
    image: redis
```

### 优势

- **简化开发**：通过一个文件管理所有服务，简化开发和测试环境的设置。
- **可移植性**：相同的配置可以在不同环境中使用，确保一致性。

Docker Compose 是管理复杂应用的强大工具，特别适合需要多个服务协同工作的场景。

</details>

## Level 3 (Mid-level)
**Task**: Change Docker file and compose file.
- update port
- update container name
- update python base image
  
升级版本的话可以在 https://hub.docker.com/_/python 来找 image. 找完以后修改 Dockerfile 里面的基础镜像:
  ```python
FROM python:3.10-alpine AS builder  
```
<details> 
  <summary>关于 https://hub.docker.com </summary>
Docker Hub 是一个云端的容器注册表服务，提供以下功能：

1. **镜像存储**：
   - 存储和分发 Docker 镜像。

2. **镜像查找**：
   - 搜索和下载官方和社区提供的 Docker 镜像。

3. **自动构建**：
   - 自动从代码仓库构建镜像。

4. **团队协作**：
   - 管理和共享团队的镜像。

Docker Hub 是 Docker 生态系统的重要组成部分，帮助开发者轻松管理和分发容器化应用。
</details>

**Objective**: Understand parameters in Docker file and compose file.



