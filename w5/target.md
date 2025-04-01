# Week 5: Docker & Containerisation

## Level 0 (prerequisites)
**Task**: Create a Key Pair and an EC2 Instance

**Objective**: To create a key pair for SSH access and then create an EC2 instance using this key pair.


## Level 1 (Intern)
**Task**: Deploy in app in ECS with AWS CLI and AWS console

**Objective**: Create ECR, and push image; create ECS cluster, and run a task


## Level 2 
**Task**: Create pipeline to build docker and push image to ECR
**Objective**: Learn pipeline to build docker and push image to ECR



## Level 3 
**Task**: Create pipeline to deploy image in ECR to ECS
**Objective**: Learn pipeline to deploy image in ECR to ECS


### 我对这三个服务的理解
1. EC2（Elastic Compute Cloud）
    类似于：虚拟机（VM）。
    功能：提供可扩展的计算能力，类似于在云中运行的物理服务器。
2. ECS（Elastic Container Service）
    类似于：容器编排服务。
    功能：管理和运行 Docker 容器，但不等同于 Kubernetes（K8s）。ECS 是 AWS 的容器编排服务，类似于 Kubernetes，但更紧密集成在 AWS 生态系统中。
3. ECR（Elastic Container Registry）
    类似于：容器镜像存储。
    功能：存储和管理 Docker 容器镜像，类似于 Docker Hub。
**总结**
EC2：虚拟机服务。
ECS：容器编排服务（类似于 Kubernetes，但不是 Kubernetes）。
ECR：容器镜像存储服务。

