# Week 4: Docker & Containerisation

## Level 1 (Intern)
**Task**: Pull a public Docker image and run it. Use Docker commands to list and stop containers.
**Objective**: Execute basic Docker commands and run containers.

## Level 2 (Junior)
**Task**: Build a custom Docker image for a simple app and deploy it locally.
**Objective**: Learn Docker image creation and management.

## Level 3 (Mid-level)
**Task**: Change Docker file and compose file.
**Objective**: Understand parameters in Docker file and compose file.

-------------------

### Docker常用的操作指令
1. `docker pull nginx` 拉取nginx镜像
2. `docker run -d -p 8080:80 nginx` 在8080端口启动ngix镜像.
   
<details>
  <summary> 关于8080:80</summary>
是的，`docker run -d -p 8080:80 nginx` 中的 `:80` 是指容器内部的端口。

### 解释

- **`8080:80`**：
  - `8080`：主机上的端口。
  - `80`：容器内部的端口。

### 工作原理

- 当你运行这个命令时，Docker 会启动一个 Nginx 容器。
- 容器内部的 Nginx 服务监听的是端口 `80`。
- 通过 `-p 8080:80`，你将主机的 `8080` 端口映射到容器的 `80` 端口。
- 这样，你可以通过访问 `http://localhost:8080` 来访问容器中运行的 Nginx 服务。

这种端口映射允许你从主机访问容器内的服务。
</details>

3. `curl 127.0.0.1:8080`
4. `docker ps`, `docker ps -a` 列出镜像. `-a`是all,包括停止的镜像
5. `docker logs <docker id>` 用于查看 Docker 容器日志的命令
6. `docker attach <docker id>` 附加到一个正在运行的 Docker 容器的终端会话

<details>
  <summary>解释</summary>
`docker attach c4f1d8382bfa` 用于附加到一个正在运行的 Docker 容器的终端会话。

### 解释

- **`docker attach`**：命令用于连接到一个正在运行的容器。
- **`c4f1d8382bfa`**：这是容器的 ID（或名称）。

### 用途

- 直接与容器的标准输入、输出和错误流交互。
- 适合需要与容器内应用进行交互的场景。

### 注意事项

- 附加到容器后，输入和输出会直接连接到容器的终端。
- 如果容器没有运行交互式终端，可能无法看到输出。
- 使用 `Ctrl + P`，然后 `Ctrl + Q` 可以安全地从容器分离而不停止容器。

`docker attach` 适用于需要实时与容器交互的情况。
</details>

7. `docker restart <docker id>` 重新启动镜像
8. **`docker run -it ubuntu /bin/bash`** 直接在本机进入docker容器内部,可以直接在容器内使用bash指令.

<details>
  <summary>解释</summary>
`docker run -it ubuntu /bin/bash` 命令用于启动一个新的 Ubuntu 容器，并在其中打开一个交互式 Bash shell。

### 解释

- **`docker run`**：启动一个新的容器。
- **`-it`**：
  - `-i`：保持标准输入打开，以便与容器交互。
  - `-t`：分配一个伪终端。
- **`ubuntu`**：使用 Ubuntu 镜像。
- **`/bin/bash`**：在容器中运行 Bash shell。

### 用途

- 启动一个 Ubuntu 容器，并进入一个交互式 Bash 会话。
- 适合需要在容器内手动执行命令或进行调试的场景。

### 结果

- 你会进入容器的 Bash shell，可以像在本地终端一样执行命令。
- 退出 Bash shell（使用 `exit`）会停止并退出容器。
</details>
