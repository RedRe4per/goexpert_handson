# Week 3: Deploy Statistic webpage with CICD

## Level 1 (Intern)
**Task**: Create CodeCommit repository

**Objective**: Understand how to set up a CodeCommit repository
<details>
  <summary>CodeCommit repository是什么</summary>

AWS CodeCommit 创建的仓库与 GitHub 上的代码库非常相似。AWS CodeCommit 是一个托管的源代码控制服务，支持 Git 协议，允许你在 AWS 云中安全地存储和管理代码。以下是一些 AWS CodeCommit 仓库的特性，与 GitHub 仓库的相似之处：

1. **Git 支持**：
   - CodeCommit 完全兼容 Git，你可以使用 Git 命令行工具或任何支持 Git 的客户端（如 GitHub Desktop、Sourcetree 等）与 CodeCommit 仓库进行交互。

2. **版本控制**：
   - 你可以在 CodeCommit 中进行常规的 Git 操作，如克隆、提交、推送、拉取、分支管理和合并等。

3. **安全性**：
   - CodeCommit 提供了与 AWS IAM 集成的细粒度权限控制，允许你定义谁可以访问和操作仓库。
   - 数据在传输和存储时都经过加密，确保代码的安全性。

4. **集成**：
   - CodeCommit 可以与其他 AWS 服务集成，如 AWS CodeBuild、AWS CodePipeline 和 AWS Lambda，支持持续集成和持续交付（CI/CD）工作流。

5. **Web 界面**：
   - CodeCommit 提供了一个 Web 界面，允许你查看代码、提交历史、分支和合并请求（类似于 GitHub 的 Pull Request）。

虽然 CodeCommit 和 GitHub 在功能上有很多相似之处，但它们的生态系统和集成方式可能有所不同。选择使用哪种服务通常取决于你的具体需求、现有的 AWS 基础设施以及对特定功能的偏好。
</details>

### Step by Step Guide:

1. **Create a CodeCommit repository**
    - Go to the AWS Management Console and open the CodeCommit console at https://console.aws.amazon.com/codecommit/.
    - Choose `Create repository`.
    - In `Repository name`, enter a name for your repository.
    - (Optional) In `Description`, enter a description for your repository.
    - Choose `Create`.
      
<details>
  <summary>CodeCommit已经不让新用户创建repo了,怎么解决?</summary>

AWS CodeCommit is no longer available to new customers. Existing customers of AWS CodeCommit can continue to use the service as normal. https://aws.amazon.com/blogs/devops/how-to-migrate-your-aws-codecommit-repository-to-another-git-provider/
里面介绍的是怎么把AWS codecommit上面的项目迁移到其他平台😭

根据公告，AWS CodeCommit 不再对新客户开放，这表明 AWS 可能在未来不打算对 CodeCommit 进行新的功能开发或扩展。然而，AWS 仍然会为现有客户提供服务，并继续在安全性、可用性和性能方面进行投资。这通常意味着：

现有客户支持：现有的 CodeCommit 用户仍然可以继续使用该服务，AWS 会继续提供支持和维护。

不再新增功能：AWS 可能不会为 CodeCommit 引入新的功能或重大更新，这意味着该服务可能会逐渐被其他更受欢迎或更广泛使用的服务所取代。

鼓励迁移：AWS 提供了迁移指南，帮助用户将他们的代码库转移到其他 Git 提供商（如 GitHub、GitLab 或 Bitbucket），这表明 AWS 可能希望用户逐步转向其他平台。

总的来说，这种策略通常是因为 AWS 可能希望将资源和精力集中在其他更具战略意义的服务上，或者因为市场上已经有其他更成熟的解决方案可以满足用户的需求。对于现有用户来说，虽然可以继续使用 CodeCommit，但考虑迁移到其他平台可能是一个更长远的选择。
</details>


2. **Create Git Crednetial**
    - Service `User` -> your admin account -> Security credentials -> HTTPS Git credentials for AWS CodeCommit 
3. **Clone the repository**
    - On the `Repositories` page, choose the name of the repository you want to clone.
    - Choose `Clone URL`, and then choose `Clone HTTPS`.
    - Open a terminal and navigate to the local directory where you want to clone the repository.
    - Run the `git clone` command followed by the HTTPS URL you copied. For example, `git clone https://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo` and input the username and password created in previous step.

## Level 2 (Junior)
**Task**: Build pipeline to push updated index.html file to bucket www.yourdomain.click via git push

**Objective**: Understand how to use CodePipeline to automate the deployment process.

### Step by Step Guide:

1. **Create a CodePipeline**
    - Go to the AWS Management Console and open the CodePipeline console at https://console.aws.amazon.com/codepipeline/.
    - Choose `Create pipeline`.
    - In `Pipeline name`, enter a name for your pipeline.
    - Choose `Next`.

2. **Configure the source stage**
    - In `Source provider`, choose `AWS CodeCommit`.
    - In `Repository name`, choose the name of your CodeCommit repository.
    - In `Branch name`, choose the name of the branch `main` that you want the pipeline to use.
    - Choose `Next`.

3. **Skip the build stage**
    - Choose `Skip build stage`.
    - In the warning message that's displayed, choose `Skip`.

4. **Configure the deploy stage**
    - In `Deploy provider`, choose `Amazon S3`.
    - In `Bucket`, choose the name of the S3 bucket where your index.html file is stored.
    - tick "Extract file before deploy"
    - Choose `Next`.

5. **Review the pipeline**
    - Review the details of your pipeline.
    - Choose `Create pipeline`.

6. **Push changes to the CodeCommit repository**
    - Navigate to your local project directory.
    - Make some changes to your index.html file.
    - Use `git add` to stage the changes, `git commit -m "Your message"` to commit the changes, and `git push` to push the changes to your CodeCommit repository.
    - Once the changes are pushed, your pipeline will automatically start and deploy the updated index.html file to your S3 bucket.

## Level 3 (Mid-level)
**Task**: Add a testing phase in your pipeline to validate your changes before the deployment.

**Objective**: Understand how to add a testing phase in the CodePipeline.

### Step by Step Guide:

1. **Create a CodeBuild project**
    - Go to the AWS Management Console and open the CodeBuild console at https://console.aws.amazon.com/codebuild/.
    - Choose `Create project`.
    - In `Project name`, enter a name for your project.
    - In `Source`, select `AWS CodeCommit` and choose your repository.
    - In `Environment`, choose a managed image and select the `Ubuntu` operating system, `Standard` runtime, and `aws/codebuild/standard:4.0` image.
    - In `Buildspec`, choose 'Use a buildspec file', and in your repository, include a buildspec file that contains commands to validate your index.html file.
    - Choose `Create build project`.

2. **Add the build stage to your pipeline**
    - Go to the CodePipeline console at https://console.aws.amazon.com/codepipeline/.
    - Choose the name of your pipeline.
    - Choose `Edit`.
    - In the pipeline structure area, choose `+ Add stage`, and then enter a name for the stage.
    - In the new stage, choose `+ Add action group`.
    - In the `Add action` dialog box, in `Action name`, enter a name.
    - In `Action provider`, choose `AWS CodeBuild`.
    - In `Input artifacts`, choose `SourceArtifact`.
    - In `Project name`, choose the name of your CodeBuild project.
    - Choose `Done`.

3. **Update the deploy stage**
    - In the deploy stage of your pipeline, for `Input artifacts`, choose `BuildArtifact`.

4. **Save your pipeline**
    - Choose `Done`, and then choose `Save`.

Now, whenever you push changes to your CodeCommit repository, your pipeline starts automatically. The pipeline fetches the latest version of your index.html file, validates it using the commands in your buildspec file, and if the validation is successful, it deploys the index.html file to your S3 bucket.
