# Week 4: Docker & Containerisation

## Level 1 (Intern)
**Task**: Pull a public Docker image and run it. Use Docker commands to list and stop containers.

**Objective**: Execute basic Docker commands and run containers.

在terminal中,可以使用`docker help`来查看docker指令, 并且可以使用 `docker run --help` 来查看指令中 `-h`,`-p`,`-d` 等缩写的意思.

<details>

  <summary>关于常用的 curl 命令</summary>


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
<details>
  <summary>关于这个dockerfile的repo</summary>
这里面是一个python后端.文件结构是

```
.
├── Dockerfile
├── README.md
├── app.py
├── compose.yaml
└── requirements.txt
```

  后端需要连接至redis数据库,因为在app.py中有代码:
  ```python
redis = Redis(host='redis', port=6379)
```
所以这里要启动redis的docker image和后端的docker image.
在启动redis的docker image时,用到指令`docker run -d -p 6379:6379 --name=redis redislabs/redismod`,其中`--name=redis`是必要的,因为上面的代码中有语句`host='redis'`,即必须使用名称`redis`来识别.
</details>

3. **Build the Docker Image**
    - Run the command: `docker build -t <image-name> .`.
    - For example, to build an image named my-app, run `docker build -t my-app .`.
    - Following steps https://github.com/AutomationLover/website-visit-count/blob/main/README.md, after git clone this repo

4. **Deploy App**
    - Deploy app with docker cli
    - Deploy app with docker compose
    - Deploy app with Docker Development Environments


## Level 3 (Mid-level)
**Task**: Change Docker file and compose file.
- update port
- update container name
- update python base image

**Objective**: Understand parameters in Docker file and compose file.



