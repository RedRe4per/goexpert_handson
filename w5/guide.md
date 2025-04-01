# Week 5 Jumphost, ECS and CICD: 
## Level 0 (prerequisites)
**Task**: Create a Key Pair and an EC2 Instance

**Objective**: To create a key pair for SSH access and then create an EC2 instance using this key pair.

### Step by Step Guide:

**Step 1**: Create the key pair
- To create a key pair, use the following command:
```bash
aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem
```
- This command will create a key pair named `MyKeyPair` and save the private key in a file named `MyKeyPair.pem`.

> 可以使用命令 `ls -al | grep pem` 来查看自己的所有 key-pairs 以及权限(不分 region). </br>
> 也可以用 (GUI) AWS console 创建 key-pair,然后下载. </br>
> 这里实际上就是把公钥上传给了 AWS, 私钥保留在了 local.

**Step 2**: Create the EC2 instance
- To create an EC2 instance
   Select "Amazon Linux", and MyKeyPair

>这里要注意 EC2 instance 和 key-pair 都是有地区限制的,必须要在同一个区域 AWS 才能找到.</br>
>如果进错区了,用 `aws configure` 重新登录并且选择区域.

<details>
  <summary>关于 EC2 instance 创建时的 network 以及 subnet</summary>
EC2 instance 创建的时候选项里有 network (也就是vpc, private network), 还有网段 subnet, subnet之间要用官关(gateway)沟通.


在创建 EC2 实例时，网络和子网的选择是关键配置。以下是简要说明：

VPC（Virtual Private Cloud）
作用：定义一个隔离的网络环境。
选择：创建实例时选择一个 VPC，决定实例所在的网络。
Subnet
作用：VPC 内的子网，决定实例的 IP 地址范围。
选择：选择一个子网，决定实例的可用区和网络位置。
通信
子网间通信：同一 VPC 内的子网可以直接通信。
网关：用于连接 VPC 与外部网络（如互联网）。
总结
VPC：定义网络边界。
Subnet：定义网络位置和可用区。
网关：连接内外网络。
通过这些配置，你可以控制 EC2 实例的网络环境和访问权限。
   
</details>

<details>
  <summary>关于 EC2 instance 的 public ip 和 private ip</summary>
public ip和private ip是有映射关系的.从外网只能访问public ip,而内网才能用private ip.
可以通过 `ping ec2-54-242-192-230.compute-1.amazonaws.com`, 也就是 `ping <Public IPv4 DNS>` 来查到.   

然后是VPC,有多个AZ(availability zone)
![Screenshot 2025-04-01 at 10 50 15 pm](https://github.com/user-attachments/assets/4bf3497c-d15e-461f-a2f2-8369fc288e38)

</details>

<details>
  <summary>关于 AMI location</summary>
在EC2 details里面有这一项.它是对应创建EC2 instance时选择的配置,实际上就是虚拟机休眠时候的配置文件.

如果使用terraform创建虚拟机,我们会需要它.
</details>

**Step 3**: Connect to the EC2 instance
- To connect to the EC2 instance from AWS console
- run `chmod 400 MyKeyPair.pem`, if you get error `Permissions 0644 for 'MyKeyPair.pem' are too open.`
- more to read for Linux file permission in https://www.linuxfoundation.org/blog/blog/classic-sysadmin-understanding-linux-file-permissions

<details>
  <summary>用 chmod 400 MyKeyPair.pem 指令来增加 private key 的安全程度</summary>
`chmod 400 devops0401kp2.pem` 命令用于设置文件的权限，使得只有文件所有者可以读取该文件，而没有写入或执行权限。
400是三位数,第一位表示owner有什么权限,第二位表示group有什么权限,第三位表示其他人有什么权限.

### 解释
- **`chmod`**：更改文件权限的命令。
- **`400`**：权限设置。
  - `4` 表示读权限。
  - `0` 表示没有写或执行权限。
- **`devops0401kp2.pem`**：要更改权限的文件。

### 用途
- 常用于保护私钥文件，确保只有文件所有者可以读取，防止未经授权的访问。

改之前和改之后,可以用 `ls -al | grep pem` 或者 `ls -al | grep <key-name>` 来查看权限. </br>
改之后就只有 user 可读并且不可修改, 其他人都不可读. 让 private key 很安全.
</details>

> Connect 到 EC2 instance: 在EC2 console 左上点击 connect, 选择 SSH, 然后使用 example 生成的指令 `ssh -i "devops0401kp2.pem" ec2-user@ec2-54-242-192-230.compute-1.amazonaws.com` 连接. 提示确认yes后,出现鸽子提示,即登录成功. 此时

**Step 4**: set up AWS configure
run aws configure, get credential from you administrator user, and set region to us-west-2

```bash
aws configure
```
tips:
you may have set up AWS configure in your laptop, you can fetch aws_access_key_id and aws_secret_access_key via
```bash
ls -al ~/.aws
cat ~/.aws/credentials
```
>这里可以用 scp 的方法把本机的 aws credentials 都复制过来, 也可以直接重新登录.

**Step 5**: set up ecs-cli

```bash
sudo curl -Lo /usr/local/bin/ecs-cli https://amazon-ecs-cli.s3.amazonaws.com/ecs-cli-linux-amd64-latest
```

```bash
sudo chmod +x /usr/local/bin/ecs-cli
```

```bash
ecs-cli --version
```
**Step 6**: set up git and docker

```bash
sudo yum update -y
```

```bash
sudo yum install git -y
```

```bash
git --version
```

```bash
sudo yum install docker -y
```

```bash
docker --version
```
To read more for Yum in https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-yum#ch-yum

## Level 1 (Intern)
**Task**: Deploy in app in ECS with AWS CLI and AWS console

**Objective**: Create ECR, and push image; create ECS cluster, and run a task

### Step by Step Guide:

**Step 1**: Create ECR

```bash
aws ecr create-repository --repository-name web --region us-west-2
```
check this ECR from AWS console, and "view push commands"

**Step 2**: Clone repo

```bash
git clone https://github.com/AutomationLover/goexpert_handson.git
```

```bash
cd goexpert_handson/w5/repo/
```

**Step 3**: Push image to ECR
follow the cli in "view push commands" in previous Step.
Open AWS console https://us-west-2.console.aws.amazon.com/ecr/private-registry/repositories?region=us-west-2, select `web` repo, you will find bottom `You can find "view push commands" on `


create image under folder "goexpert_handson/w5/repo/", and then push this docker image to ECR following step in "view push commands"

if some docker issue, please try restart

```bash
sudo service docker restart
```

If some permission issue for docker command, pls add sudo before docker.
If you do not want to add sudo, run 
```bash
sudo usermod -aG docker ec2-user
```
logout and then login

>使用上面的 `sudo service docker restart` 就行.

the sudo usermod -aG docker ec2-user command is used to add the user "ec2-user" to the "docker" group, allowing that user to run Docker commands without needing to use sudo or switch to the root user.
- `usermod`: This is the command used to modify a user account. It is typically followed by various options and arguments to specify the modifications to be made.
- `-aG docker`: These options are used to add the user to a specific group. In this case, the -a option indicates that the user should be added to the specified group, and the -G option is used to specify the group name, which is "docker" in this command.

Check ECR repo and you should find image there.

**Step 4**: Create ECS cluster AWS console
Create cluster
Cluster name: myfargate
Create



**Step 5**: Create task-definition
- Task definition family: web
- CPU: 0.25; Memory: 0.5GB

Container 1:
- name:  web
- Image: URL, copy from ECR, format like 839775502365.dkr.ecr.us-west-2.amazonaws.com/web:latest
- Container port: 8000

Click Add Container

Container 2:
- name:  redis
- Image: redislabs/redismod
- Add Port Mappings
- Container port: 6379



**Step 6**: Deploy task to ECS cluster
- Deploy -> Create service 
  - name: myWebCntService
  - Add Load Balancer (OPTIONAL)
   - Type: Application Load Balancer
   - Load balancer name: "myWebCountLB"


If error of IAM do below
- codebuild-my-project-service-role role add permission AmazonEC2ContainerRegistryPowerUser

Click link in create new Task, and click Configuration tab, you can find the Public IP. Then visit this IP with port 8000
http://{public_ip}:8000/hit

Notes:
Decommission process. To remove service/tasks 1st, and then delete Cluster

## Level 2 (skip in class)
**Task**: Create pipeline to build docker and push image to ECR
**Objective**: Learn pipeline to build docker and push image to ECR

**Step 1**: Create repo in CodeCommit


**Step 2**: Push code to CodeCommit
get the code from https://github.com/AutomationLover/goexpert_handson/tree/main/w5/repo

**Step 3**: Create pipeline
update the user id the https://github.com/AutomationLover/goexpert_handson/blob/main/w5/files/buildcommand.yaml

**Step 4**: Update app.py

Update app.py with version

**Step 5**: Run task and check

Run task in ECS cluster and check the version.

## Level 3 (skip in class)
**Task**: Create pipeline to deploy image in ECR to ECS
**Objective**: Learn pipeline to deploy image in ECR to ECS

Please refer to https://docs.aws.amazon.com/codepipeline/latest/userguide/ecs-cd-pipeline.html 
**Step 1**: Create repo in CodeCommit


**Step 2**: Push code to CodeCommit
get the code from https://github.com/AutomationLover/goexpert_handson/tree/main/w5/repo
Replace below, with your own AWS account ID and region, and push the code to CodeCommit repo 
https://github.com/AutomationLover/goexpert_handson/blob/main/w5/repo/buildspec.yml#L11

**Step 3**: Create pipeline
- step 1: select Build custom pipeline
- step 2: give Pipeline name "myCountWebPipeline"
- step 3:
   - select source provider "AWS CodeCommit", and
   - the repo you created before
   - branch "master"
- step 4:
   - Select "Other build providers"
   - Create project "myProject"
      - Select "Use a buildspec file"   
      - Remove the selection of "CloudWatch logs - optional"
      - Click "Continue to CodePipeline"
      - Others refer to "On the Step 3: Add build stage page, for Build provider choose AWS CodeBuild, and then choose Create project." in https://docs.aws.amazon.com/codepipeline/latest/userguide/ecs-cd-pipeline.html 

  
   - Double check the region is the same region of CodeCommit and ECR in previous step
     
- step 5:
   - Deploy provider: select "Amazon ECS"
   - Double check the region is the same region of CodeCommit and ECR in previous step
   - Select Cluster name, which you created in previous step
   - Select Service name, which you created in previous step
 
- step 6
   - Review and Confirm
   - If IAM error, which did not allow visit ECR, The CodePipeline wizard created an IAM role for the CodeBuild build project, called codebuild-build-project-name-service-role. For this tutorial, the name is codebuild-hello-world-service-role. Because the buildspec.yml file makes calls to Amazon ECR API operations, the role must have a policy that allows permissions to make these Amazon ECR calls.  check https://docs.aws.amazon.com/codepipeline/latest/userguide/ecs-cd-pipeline.html#code-build-perms
