# Week 2: Manual Deploy Statistic Website

## Level 1 (Intern)

### Task: Create an S3 bucket and upload a simple HTML file. 

#### Step 1: Create an S3 Bucket
1. Go to the S3 service in AWS Console.
2. Click "Create Bucket", give it a unique name and select us-east-1. (the bucket name using the URL you are going to use in Zone A record setting in step2, e.g. "www.{{ yourname }}.{{ devopsNN }}.learnby.click")
3. Leave the rest as default and click "Create".

<details>
  <summary>关于放网站的S3 bucket命名问题</summary>

存储桶名称与域名匹配：如果你希望通过自定义域名（例如，www.example.com）访问你的静态网站，那么你的 S3 存储桶名称应该与该域名完全匹配。这意味着你需要创建一个名为 www.example.com 的 S3 存储桶。存储桶名称必须是全局唯一的，并且遵循 DNS 命名约定。**并且不能是 IP 地址的格式**（例如，192.168.1.1）。

网站域名与 S3 存储桶名称需要完全匹配的原因主要是为了简化域名解析和访问过程。

DNS 解析：当你希望通过自定义域名（例如，www.example.com）访问 S3 上托管的静态网站时，S3 需要知道哪个存储桶与该域名相关联。通过让存储桶名称与域名匹配，AWS 可以直接将域名请求路由到正确的存储桶。

CNAME 记录：在使用自定义域名时，你通常需要在你的域名提供商处设置 CNAME 记录，将你的域名指向 S3 的网站终端节点。匹配的存储桶名称使得这个过程更加直接和无缝。

简化配置：匹配的名称减少了配置错误的可能性，因为它**消除了在域名和存储桶名称之间进行映射的需要**。

一致性和可读性：使用匹配的名称可以提高配置的一致性和可读性，特别是在管理多个域名和存储桶时。

通过确保存储桶名称与域名匹配，你可以更轻松地配置和管理静态网站托管，并确保用户能够通过预期的域名顺利访问网站。
</details>

#### Step 2: Upload a HTML file
Here's a simple "Hello World" HTML file: 
```html
<!DOCTYPE html>
<html>
<body>

<h1>Hello World</h1>

</body>
</html>
```
1. Save this as `index.html`.
2. Go to your S3 bucket and click "Upload".
3. Choose the `index.html` file and click "Upload".

#### Step 3: Set permissions to allow public view
1. Click on your `index.html` file in the S3 bucket.
2. Go to the "Permissions" tab.
3. Click "Edit", uncheck "Block all public access", and save.


## Level 1 (Option)
### Task: Configure permissions and versioning for the S3 bucket.

#### Step 1: Set up versioning
1. Go to your S3 bucket.
2. Click on the "Properties" tab.
3. Scroll down to the "Versioning" section and click "Edit".
4. Check "Enable versioning" and save.

#### Step 2: Update S3 bucket file content
1. Create a new `index.html` file with different content.
2. Upload this new file to the S3 bucket. It should have the same name as the previous file.
3. The new file becomes the current version.

## Level 2 (Junior)

### Task: Setup Route53 Records for a Static Web Page Hosted in an S3 Bucket

Before setting up Route53 records, make sure you have a static web page hosted on an S3 bucket with Static Website Hosting enabled, and you have registered a domain with Route 53 or another domain registrar.

#### Step 1: Enable Static Website Hosting on S3
1. Go to the Amazon S3 service in the AWS Management Console.
2. Click on the bucket that you want to use for your static website.
3. Navigate to the "Properties" tab.
4. Scroll down to the "Static website hosting" section and click "Edit".
5. Select "Enable" for Static website hosting.
6. Fill in the "Index document" field with the name of your index file (e.g., "index.html").
7. Optionally, set an "Error document" that will be returned when an error occurs.
8. Click "Save changes".

#### Step 2: Configure Bucket Policy to Allow Public Read Access
1. Still in your bucket, go to the "Permissions" tab.
2. Click "Bucket Policy".
3. Enter a policy that grants public read access to the bucket. Here is an example policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
    }
  ]
}
```

Replace `YOUR-BUCKET-NAME` with the actual name of your S3 bucket.

4. Click "Save" to apply the bucket policy.

#### Step 3: Register a Domain with Route 53 (Skip if you already have a domain)
1. Go to the Route 53 service in the AWS Management Console.
2. In the "Domain Registration" section, click "Register Domain".
3. Search for the domain name you want and follow the prompts to register it.
<details>
  <summary>Domain（域名）和Hosted Zone（托管区域）</summary>


在 AWS Route 53 中，"domain" 和 "hosted zone" 是两个相关但不同的概念。理解它们的区别有助于更好地管理你的域名和 DNS 设置。

Domain（域名）：
域名是你在域名注册商（如 Google Domains、AWS Route 53 等）注册的唯一名称，例如 www.tunespire.com。
域名注册通常包括设置域名的注册信息和选择域名的 DNS 服务器。
Hosted Zone（托管区域）：
托管区域是一个容器，用于存储与特定域名相关的 DNS 记录集。
在 AWS Route 53 中，托管区域用于管理域名的 DNS 设置，例如 A record(redirect domain to IPv4 address)、CNAME 记录(redirect domain to ther domain)、MX(Mail Exchange Record,指定负责接收电子邮件的邮件服务器) 。AAAA 记录(同A record但是是IPv6), NS 记录（Name Server Record, 指定哪个 DNS 服务器是该域名的权威服务器）
托管区域可以是“公有”或“私有”的。公有托管区域用于管理互联网可访问的域名，而私有托管区域用于在 VPC 内部管理域名。


关系和使用：
当你在 AWS Route 53 中注册一个域名时，AWS 会自动为你创建一个对应的托管区域。
你可以在托管区域中添加和管理 DNS 记录，以控制域名的解析方式。例如，你可以添加 A 记录将 www.tunespire.com 指向某个 IP 地址，或添加 CNAME 记录将 app.tunespire.com 指向另一个域名。
托管区域的设置决定了用户在访问你的域名时，DNS 服务器如何将域名解析为 IP 地址或其他资源。
因此，托管区域在域名解析过程中扮演了关键角色，帮助你管理和配置域名的 DNS 记录，而域名本身则是你在互联网上的标识。
</details>


#### Step 4: Create Hosted Zone in Route 53
1. In the Route 53 dashboard, click on "Hosted zones".
2. Click "Create hosted zone".
3. Enter your registered domain name in the "Domain Name" field.
4. Choose "Public Hosted Zone" for the type.
5. Click "Create".

#### Step 5: Set Up Route 53 Records for Your Domain
1. After creating your hosted zone, you will be taken to its management page.
2. Click "Create Record".
3. In the "Record name" field, specify the subdomain if required (e.g., www), or leave it blank for the root domain.
4. Set the "Record type" to "A".
5. Set the "Alias" toggle to "Yes".
6. Select the S3 bucket.
7. Click "Create records".


#### Step 6: Update Name Servers for Your Domain (if using third-party registrar)
1. If you registered your domain with Route 53, it will automatically use AWS name servers. Skip to the next step.
2. If you're using a third-party domain registrar, log in to your account there.
3. Navigate to the section where you can set or update the domain's name servers.
4. Replace the current name servers with the ones provided by AWS in the Route 53 hosted zone you created. There are typically 4 name servers to update.
5. Save changes on your registrar's website. It may take up to 48 hours for the new name servers to propagate and for your DNS changes to take effect globally.


#### Step 7: Verify the Domain Points to Your S3 Bucket
1. Once the DNS changes have propagated, open a web browser.
2. Type your domain name into the address bar, and press Enter.
3. Your browser should now display the static website hosted on your S3 bucket. If it does not, confirm that you have set up the S3 bucket and Route 53 records correctly, and that you have given sufficient time for DNS propagation.

## Level 3 (Mid-level)
### Task: let HTTPS works

You can try to follow up with https://docs.aws.amazon.com/AmazonS3/latest/userguide/website-hosting-custom-domain-walkthrough.html

1. Creat cloudfront distribute

   set up Origin with the S3 bucket static web page, (e.g. static page of  http://hello-world-devops15.s3-website-ap-southeast-2.amazonaws.com/, using "hello-world-devops15.s3-website-ap-southeast-2.amazonaws.com",)

2. Update cloudfront distribute
    
   General -> Settings -> Edit 

   Add "Alternate domain name (CNAME)" with "www.{{ yourname }}.{{ devopsNN }}.learnby.click"

   Create SSL certificate with  "www.{{ yourname }}.{{ devopsNN }}.learnby.click"

   During Creating SSL certificate, you may need to add CNAME.

   OK
