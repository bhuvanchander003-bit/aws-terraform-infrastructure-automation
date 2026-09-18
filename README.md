# AWS Infrastructure Automation with Terraform

## 📌 Project Overview

This project demonstrates how to automate the deployment of AWS infrastructure using **Terraform Infrastructure as Code (IaC)**.

Instead of manually creating AWS resources through the AWS Management Console, Terraform configuration files are used to define and provision the infrastructure automatically.

The project provisions AWS resources such as:

* Amazon EC2
* Amazon S3
* AWS IAM
* Security Group
* Apache Web Server

Terraform commands such as `terraform init`, `terraform validate`, `terraform plan`, and `terraform apply` are used to manage the infrastructure lifecycle.

---

## 🏗️ Architecture

```text
                         Internet
                            │
                            ▼
                    ┌───────────────┐
                    │   AWS VPC     │
                    │   Default /   │
                    │   Custom VPC  │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │ Security      │
                    │ Group         │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │ EC2 Instance  │
                    │ Amazon Linux  │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │ Apache Web    │
                    │ Server        │
                    └───────┬───────┘
                            │
                            ▼
                       Web Page


             ┌─────────────────────────┐
             │          AWS            │
             │                         │
             │  ┌───────┐   ┌───────┐ │
             │  │  EC2  │   │  S3   │ │
             │  └───────┘   └───────┘ │
             │       │                 │
             │      IAM                │
             └─────────────────────────┘
```

---

## 🎯 Project Objectives

The main objectives of this project are:

1. Learn Infrastructure as Code using Terraform.
2. Automate AWS infrastructure deployment.
3. Provision EC2 and S3 resources using Terraform.
4. Configure IAM permissions.
5. Configure network security using Security Groups.
6. Deploy an Apache web server on EC2.
7. Understand Terraform state management.
8. Practice the Terraform infrastructure lifecycle.
9. Reduce manual AWS Console configuration.
10. Create repeatable and consistent infrastructure.

---

## 🛠️ Technologies Used

| Technology      | Purpose                  |
| --------------- | ------------------------ |
| Terraform       | Infrastructure as Code   |
| AWS             | Cloud platform           |
| EC2             | Virtual server           |
| S3              | Object storage           |
| IAM             | Identity and permissions |
| Security Groups | Network access control   |
| Amazon Linux    | EC2 operating system     |
| Apache          | Web server               |
| Git             | Version control          |
| GitHub          | Project repository       |

---

# 📂 Project Structure

```text
aws-terraform-infrastructure-automation/
│
├── README.md
│
├── provider.tf
├── variables.tf
├── terraform.tfvars
├── ec2.tf
├── s3.tf
├── iam.tf
├── security-group.tf
├── outputs.tf
│
├── scripts/
│   └── install-apache.sh
│
├── screenshots/
│   ├── ec2-instance.png
│   ├── s3-bucket.png
│   ├── terraform-plan.png
│   ├── terraform-apply.png
│   └── website.png
│
└── .gitignore
```

---

# ⚙️ How the Project Works

The workflow is:

```text
Terraform Configuration
          │
          ▼
     terraform init
          │
          ▼
   terraform validate
          │
          ▼
     terraform plan
          │
          ▼
    terraform apply
          │
          ▼
     AWS Resources
          │
     ┌────┴────┐
     ▼         ▼
    EC2       S3
     │
     ▼
  Apache
     │
     ▼
 Website
```

---

# 🚀 Step-by-Step Implementation

## Step 1 — Install Terraform

Install Terraform on your local computer.

Verify the installation:

```bash
terraform -version
```

Expected output:

```text
Terraform v1.x.x
```

---

## Step 2 — Install AWS CLI

Install the AWS CLI and verify:

```bash
aws --version
```

---

## Step 3 — Configure AWS CLI

Run:

```bash
aws configure
```

Enter your AWS credentials and preferred AWS region.

Example:

```text
AWS Access Key ID: ********
AWS Secret Access Key: ********
Default region name: ap-south-1
Default output format: json
```

> ⚠️ Never upload AWS access keys or secret keys to GitHub.

---

# Step 4 — Create the Terraform Project

Create a project directory:

```bash
mkdir aws-terraform-infrastructure-automation
cd aws-terraform-infrastructure-automation
```

Create the Terraform configuration files:

```text
provider.tf
variables.tf
ec2.tf
s3.tf
iam.tf
security-group.tf
outputs.tf
```

---

# Step 5 — Configure the AWS Provider

The AWS provider tells Terraform that AWS will be used as the cloud provider.

Example:

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
}
```

---

# Step 6 — Define Variables

Variables make the Terraform configuration reusable.

Example:

```hcl
variable "aws_region" {
  description = "AWS region"
  type        = string
  default     = "ap-south-1"
}

variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t3.micro"
}
```

---

# Step 7 — Create an S3 Bucket

The S3 configuration defines an AWS S3 bucket.

Example:

```hcl
resource "aws_s3_bucket" "project_bucket" {
  bucket = var.bucket_name

  tags = {
    Name        = "Terraform Project Bucket"
    Environment = "Development"
  }
}
```

Terraform will create the bucket automatically.

---

# Step 8 — Create IAM Role

An IAM role can provide AWS permissions to the EC2 instance without storing access keys on the server.

Example permissions can be attached according to the project's requirements.

This demonstrates an important AWS security principle:

> Use IAM roles instead of storing AWS credentials directly on EC2 whenever possible.

---

# Step 9 — Create Security Group

The Security Group controls traffic to the EC2 instance.

Example rules:

```text
HTTP  → Port 80  → Web traffic
SSH   → Port 22  → Administrative access
```

For production environments, SSH should preferably be restricted to a trusted IP range rather than allowing `0.0.0.0/0`.

---

# Step 10 — Create EC2 Instance

Terraform provisions the EC2 instance.

Example:

```hcl
resource "aws_instance" "web_server" {
  ami           = var.ami_id
  instance_type = var.instance_type

  tags = {
    Name = "Terraform-Web-Server"
  }
}
```

The AMI ID should be appropriate for the selected AWS region.

---

# Step 11 — Install Apache

A startup script can automatically install Apache when the EC2 instance starts.

Example:

```bash
#!/bin/bash

dnf update -y
dnf install -y httpd

systemctl enable httpd
systemctl start httpd

echo "<h1>Terraform AWS Web Server</h1>" > /var/www/html/index.html
```

This removes the need to manually connect to the EC2 instance and install Apache.

---

# Step 12 — Initialize Terraform

Run:

```bash
terraform init
```

Terraform downloads the required provider plugins and prepares the working directory.

---

# Step 13 — Validate Configuration

Run:

```bash
terraform validate
```

This checks whether the Terraform configuration is syntactically valid.

Expected result:

```text
Success! The configuration is valid.
```

---

# Step 14 — Format Terraform Files

Run:

```bash
terraform fmt
```

This automatically formats Terraform configuration files into a standard style.

---

# Step 15 — Create Terraform Plan

Run:

```bash
terraform plan
```

Terraform compares the desired infrastructure with the current state and shows what it intends to create, modify, or delete.

Example:

```text
Plan: 4 to add, 0 to change, 0 to destroy.
```

---

# Step 16 — Deploy Infrastructure

Run:

```bash
terraform apply
```

Terraform will ask for confirmation.

Enter:

```text
yes
```

Terraform will then create the AWS resources.

---

# Step 17 — Verify AWS Resources

After deployment, verify the resources in the AWS Console.

Check:

```text
EC2
S3
IAM
Security Groups
```

The EC2 instance should be running and Apache should be serving the web page.

---

# Step 18 — Test the Website

Find the EC2 instance's public IPv4 address.

Open:

```text
http://EC2-PUBLIC-IP
```

You should see:

```text
Terraform AWS Web Server
```

---

# Step 19 — View Terraform Outputs

Run:

```bash
terraform output
```

Useful outputs can include:

```text
EC2 public IP
EC2 public DNS
S3 bucket name
```

---

# Step 20 — Destroy Resources

When the project is finished, remove the resources:

```bash
terraform destroy
```

Confirm with:

```text
yes
```

Terraform will remove the infrastructure it manages.

> ⚠️ Make sure you really want to delete the resources before running this command.

---

# 🔄 Terraform Lifecycle

The Terraform lifecycle used in this project is:

```text
              Write Code
                  │
                  ▼
          terraform init
                  │
                  ▼
        terraform validate
                  │
                  ▼
           terraform fmt
                  │
                  ▼
           terraform plan
                  │
                  ▼
          terraform apply
                  │
                  ▼
           AWS Resources
                  │
                  ▼
          terraform destroy
```

---

# 🔐 Security Considerations

The following security practices should be followed:

* Never commit AWS access keys to GitHub.
* Never commit secret keys or passwords.
* Use IAM roles where possible.
* Restrict SSH access to trusted IP addresses.
* Use least-privilege IAM permissions.
* Avoid putting passwords directly inside Terraform files.
* Add sensitive files to `.gitignore`.
* Use remote Terraform state with appropriate security controls for production projects.

---

# 📸 Screenshots

Add screenshots of your completed project to the `screenshots` directory.

Recommended screenshots:

```text
screenshots/
│
├── terraform-init.png
├── terraform-plan.png
├── terraform-apply.png
├── ec2-instance.png
├── s3-bucket.png
├── iam-role.png
├── security-group.png
└── website.png
```

These screenshots help recruiters understand that you actually implemented the project.

---

# 📊 Project Results

The project demonstrates:

* Automated AWS infrastructure provisioning.
* Infrastructure as Code using Terraform.
* EC2 deployment.
* S3 provisioning.
* IAM configuration.
* Security Group configuration.
* Automated Apache installation.
* Terraform planning and deployment.
* Infrastructure lifecycle management.
* Reduced manual configuration.

---

# 💼 Interview Explanation

### Tell me about your project.

**Answer:**

> I developed an AWS Infrastructure Automation project using Terraform Infrastructure as Code. The objective was to automate AWS resource provisioning instead of manually creating resources through the AWS Console. I created reusable Terraform configuration files to provision EC2, S3, IAM, and Security Group resources. I used Terraform init to initialize the project, validate to check the configuration, plan to review infrastructure changes, and apply to deploy the resources. I also automated Apache installation on the EC2 instance using a startup script. This project helped me understand Infrastructure as Code, AWS resource automation, Terraform state management, and repeatable infrastructure deployment.

---

# ❓ Interview Questions

## 1. What is Terraform?

Terraform is an Infrastructure as Code tool used to define, provision, and manage infrastructure using configuration files.

---

## 2. Why did you use Terraform?

I used Terraform to automate AWS infrastructure deployment and reduce manual configuration.

---

## 3. What is Infrastructure as Code?

Infrastructure as Code means managing infrastructure using machine-readable configuration files instead of manually configuring resources.

---

## 4. What does `terraform init` do?

It initializes the Terraform working directory and downloads the required provider plugins.

---

## 5. What does `terraform plan` do?

It shows the changes Terraform intends to make without actually applying those changes.

---

## 6. What does `terraform apply` do?

It applies the Terraform configuration and creates or modifies the infrastructure.

---

## 7. What is Terraform state?

Terraform state keeps track of the infrastructure resources managed by Terraform and helps Terraform determine what changes are required.

---

## 8. What does `terraform destroy` do?

It removes resources managed by the Terraform configuration.

---

## 9. What is an AWS provider?

The AWS provider allows Terraform to communicate with AWS and manage AWS resources.

---

## 10. Why use variables in Terraform?

Variables make configurations reusable and allow values such as region, instance type, and resource names to be changed without modifying the main infrastructure code.

---

# 📌 Resume Description

**AWS Infrastructure Automation with Terraform**

* Automated AWS infrastructure deployment using Terraform Infrastructure as Code.
* Provisioned EC2, S3, IAM, and Security Group resources programmatically.
* Created reusable Terraform configuration files using variables and outputs.
* Used Terraform `init`, `validate`, `plan`, `apply`, and `destroy` commands to manage infrastructure.
* Automated Apache web server installation on EC2 using a startup script.
* Improved deployment consistency and reduced manual AWS configuration.

---

# ⭐ Key Skills Demonstrated

```text
AWS
Terraform
Infrastructure as Code
EC2
S3
IAM
Security Groups
Linux
Apache
AWS CLI
Git
GitHub
Cloud Infrastructure Automation
```

---

## 👨‍💻 Author

**Bhuvan Chander**

AWS Cloud / Cloud Infrastructure Enthusiast
