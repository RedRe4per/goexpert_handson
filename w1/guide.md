# Setup AWS account, user and AWS CLI

## Level 1 (Intern)
### Task: Set up an AWS account, understand the AWS Management Console layout and set up an IAM user.

#### Step 1: Set up an AWS account
1. Go to the AWS homepage.
2. Click "Create an AWS Account".
3. Fill out the necessary details and click "Continue".
4. Choose "Personal" or "Professional" account type, fill out the rest of the form, and click "Create Account and Continue".

#### Step 2: Understand the AWS Management Console layout
1. Sign in to the AWS Management Console.
2. The navigation bar is at the top, and it contains a list of services and resources.
3. The main panel contains the dashboard which provides an overview of the services and resources.

#### Step 3: Set up an IAM user for yourself
1. Navigate to the IAM dashboard.
2. Click "Users" and then "Add user".
3. Specify a user name and select "AWS Management Console access".
4. Click "Next" to set permissions, add to a group or copy permissions from an existing user.
5. Review the user details and click "Create user".
6. You should now be able to log in with this IAM user instead of the root user.

## Level 2 (Junior)

#### Step 1: Setting up IAM user for AWS CLI
1. Navigate to the IAM dashboard.
2. Click "Users" and then "Add user".
3. Specify a user name and select "Programmatic access".
4. Click "Next" to set permissions. You can either attach existing policies directly, or set up permissions boundaries.
5. Review the user details and click "Create user".
6. Note down the Access key ID and Secret access key.

#### Step 2: Using AWS CLI with IAM user
1. Install AWS CLI on your local machine.
2. Configure it using the command `aws configure`, and input your Access key ID, Secret access key, and preferred region.
3. Now you can interact with your AWS resources using the command line.

## Level 3 (Mid-level)

### Task: Practice Basic AWS CLI Commands

#### Step 1: Configure AWS CLI
1. Open your terminal or command prompt.
2. Type `aws configure` and press Enter. 用完以后host不会出现显示变化,不会像是直接跳进aws里了,但是使用aws s3 ls能显示内容.
3. Enter your AWS Access Key ID when prompted. 这个要去security里找.
4. Enter your AWS Secret Access Key when prompted.
5. Specify your preferred AWS Region (e.g., us-west-2).
6. Choose your default output format (e.g., json, text, or table).

#### Step 2: List S3 Buckets
1. In the terminal, type `aws s3 ls` and press Enter.
2. You should see a list of all S3 buckets in your account.

#### Step 3: Create a New S3 Bucket
1. Type `aws s3 mb s3://<bucket-name>` replacing `<bucket-name>` with a unique bucket name. 
2. Press Enter. You should receive a confirmation message that the bucket has been created.

#### Step 4: Upload a File to S3 Bucket
1. Type `aws s3 cp <file-path> s3://<bucket-name>` replacing `<file-path>` with the local path to the file you're uploading and `<bucket-name>` with the name of your bucket. 本地路径要注意,不行就AI生成.
2. Press Enter. You should see a message indicating successful file upload.

#### Step 5: Describe EC2 Instances
1. Type `aws ec2 describe-instances` and press Enter.
2. You'll receive a JSON response with details of all the EC2 instances in your AWS account.

#### Step 6: Launch a New EC2 Instance
1. Type `aws ec2 run-instances --image-id ami-XXXXX --count 1 --instance-type t2.micro --key-name <your-key-pair> --security-groups <your-security-group>` replacing `ami-XXXXX` with an appropriate AMI ID, `<your-key-pair>` with your SSH key pair, and `<your-security-group>` with your security group name. 
这里要注意的点: ami-XXXXX 要去AWS EC2 console里面,点开Launch Instance后能找到. <your-key-pair>要去EC2 console中左边有Key Pairs点开生成,生成后.pem文件可以自己保存到mac的 `.ssh` 里(推荐但是不强制). 指令里这个不需要后缀. `<your-security-group>` 要去EC2 dashboard左边按他的规则创建.
2. Press Enter. You'll receive details of the instance being launched.
我有个问题.这里实际上我用了aws cli指令来创建了一个ec2实例,而不是在aws上单纯的click.这很好,但是获得ami-XXXXX <your-key-pair> <your-security-group>的过程,我依然要去aws dashboard上有大量的click操作.请问这是为什么?我目前考虑有两种可能性,第一种是我是初学者所以获得这些key或者name的方式不完善; 第二种可能是本身这么做是对的,第一次用这个指令都要这么操作,但是指令跑通后可以复用来创建多个ec2 instance (反之,点击就要点很多次了).请问这两种可能性有哪个是对的?或者说都不对是因为其他原因呢?
你的观察是非常准确的，实际上你提到的两种可能性都有一定的道理。让我们来详细分析一下：

1. 初学者阶段的操作
**初学者阶段：**对于初学者来说，使用 AWS 管理控制台（Dashboard）来获取 AMI ID、创建 Key Pair 和 Security Group 是非常常见的做法。这是因为通过图形界面可以更直观地理解 AWS 的各种资源和配置选项。
**学习过程：**通过控制台进行这些操作可以帮助你更好地理解 AWS 的基础设施和服务如何协同工作。这是一个学习的过程，帮助你熟悉 AWS 的生态系统。
2. 自动化和复用
**自动化的好处：**一旦你熟悉了 AWS 的基本概念和资源配置，你可以通过 AWS CLI 或者编写脚本来自动化这些过程。这种方法非常适合需要频繁创建和管理资源的场景。
**复用指令：**使用 AWS CLI 可以轻松地复用相同的命令来创建多个 EC2 实例，尤其是在需要相同配置的情况下。这大大减少了手动点击的工作量，提高了效率。
其他考虑因素
**基础设施即代码（IaC）：**随着经验的积累，你可能会考虑使用工具如 AWS CloudFormation 或者 Terraform 来管理基础设施。这些工具允许你以代码的形式定义和管理 AWS 资源，进一步提高自动化和可重复性。
**CLI 和 SDK 的使用：**AWS 提供了丰富的 CLI 和 SDK 工具，可以用来编写脚本或程序来管理 AWS 资源。这些工具可以帮助你实现更复杂的自动化任务。
总结
初学者阶段使用控制台是正常的，因为它提供了一个直观的学习环境。
随着经验的增加，使用 CLI 和自动化工具是更高效的选择，尤其是在需要频繁创建和管理资源的情况下。
自动化和基础设施即代码是现代云计算管理的趋势，可以显著提高效率和一致性。
因此，你的理解是正确的，初学者阶段通过控制台操作是正常的，而随着经验的积累，自动化和复用 CLI 指令将成为更高效的工作方式。

#### Step 7: List IAM Users
1. Type `aws iam list-users` and press Enter.
2. You'll get a list of IAM users in your AWS account.

#### Step 8: Create a New IAM User
1. Type `aws iam create-user --user-name <new-user-name>` replacing `<new-user-name>` with the desired username for the new IAM user.
2. Press Enter. You will see the details of the newly created IAM user in the output.

#### Step 9: Attach a Policy to an IAM User
1. Type `aws iam attach-user-policy --policy-arn arn:aws:iam::aws:policy/<PolicyName> --user-name <new-user-name>` replacing `<PolicyName>` with the name of the policy you want to attach and `<new-user-name>` with the username of the IAM user.
2. Press Enter. You will receive confirmation that the policy has been attached to the user.

#### Step 10: Delete an IAM User
1. Type `aws iam delete-user --user-name <existing-user-name>` replacing `<existing-user-name>` with the name of the user you wish to delete.
2. Press Enter. You will receive confirmation that the user has been deleted.

#### Step 11: Stop an EC2 Instance
1. Type `aws ec2 stop-instances --instance-ids i-XXXXX` replacing `i-XXXXX` with the Instance ID of the EC2 instance you want to stop.
2. Press Enter. You'll receive confirmation that the instance is stopping.

#### Step 12: Delete an S3 Bucket
1. First, ensure the bucket is empty by typing `aws s3 rm s3://<bucket-name> --recursive` to delete all objects in the bucket.
2. Then, type `aws s3 rb s3://<bucket-name>` to remove the bucket itself.
3. Press Enter. You will receive confirmation that the bucket has been deleted.

Practice these AWS CLI commands to get comfortable with managing AWS services via the command line. Remember to replace placeholder values like `<bucket-name>`, `<file-path>`, `<new-user-name>`, `<existing-user-name>`, `<PolicyName>`, and `i-XXXXX` with actual values from your AWS environment.

**Note:** Always exercise caution when performing delete operations and ensure that you are deleting the correct resources to avoid unintentional loss of data or service interruption.
