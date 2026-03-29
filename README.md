# 🚀 Project 2: Terraform AWS Infrastructure (Real DevOps Project)

## 📌 Overview

This project demonstrates how to automate AWS infrastructure using Terraform.

👉 You will build:

VPC → Subnet → Security Group → EC2 Instance

✅ Fully automated using Infrastructure as Code (IaC)

---

## 🎯 What You Will Learn

- Terraform basics  
- AWS infrastructure provisioning  
- Infrastructure as Code (IaC)  
- Real-world DevOps workflow  

---

## ⚙️ Prerequisites

Make sure you have:

- AWS Account  
- AWS CLI installed  
- Terraform installed  

---

## 🧱 STEP 1: Install Terraform

Check if Terraform is installed:

terraform -v

If not installed:

sudo apt update
sudo apt install unzip -y
wget https://releases.hashicorp.com/terraform/1.6.6/terraform_1.6.6_linux_amd64.zip
unzip terraform_1.6.6_linux_amd64.zip
sudo mv terraform /usr/local/bin/

Verify installation:

terraform -v

---

## 🔐 STEP 2: Configure AWS CLI

aws configure

Enter:

AWS Access Key ID  
AWS Secret Access Key  
Region (example: ap-south-1)  
Press Enter for default output format  

---

## 📁 STEP 3: Create Project Structure

mkdir terraform-project
cd terraform-project
touch main.tf variables.tf outputs.tf

Project structure:

terraform-project/
 ├── main.tf
 ├── variables.tf
 ├── outputs.tf

---

## ⚙️ STEP 4: Write Terraform Code

### main.tf

provider "aws" {
  region = "ap-south-1"
}

resource "aws_vpc" "main_vpc" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "main_subnet" {
  vpc_id     = aws_vpc.main_vpc.id
  cidr_block = "10.0.1.0/24"
}

resource "aws_security_group" "allow_ssh" {
  vpc_id = aws_vpc.main_vpc.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "my_ec2" {
  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.main_subnet.id

  vpc_security_group_ids = [aws_security_group.allow_ssh.id]

  tags = {
    Name = "Terraform-EC2"
  }
}

---

### outputs.tf

output "instance_ip" {
  value = aws_instance.my_ec2.public_ip
}

---

## 🚀 STEP 5: Run Terraform

Initialize Terraform:

terraform init

Preview infrastructure:

terraform plan

Apply configuration:

terraform apply

Type:

yes

---

## 🎉 RESULT

Terraform will automatically:

- Create VPC  
- Create Subnet  
- Create Security Group  
- Launch EC2 Instance  

---

## 🌐 Get EC2 Public IP

terraform output instance_ip

---

## 🔑 Connect to EC2

ssh -i your-key.pem ubuntu@<public-ip>

---

## 🧹 STEP 6: Destroy Infrastructure (IMPORTANT)

To avoid AWS charges:

terraform destroy

Type:

yes

---

## ⚠️ Common Issues

### Invalid AMI Error

Error:
InvalidAMIID.NotFound

Fix:
- AMI is region-specific  
- Use correct AMI for your AWS region  

---

## 🔥 Upgrade This Project (Resume Boost)

You can improve this project by adding:

- Remote backend (S3)  
- Variables file  
- Reusable modules  
- Provisioners (auto-install Docker)  

---

## 🧠 How to Explain in Interview

I used Terraform to automate AWS infrastructure provisioning including VPC, subnet, security groups, and EC2 instance. This reduced manual setup and ensured reproducibility using Infrastructure as Code.

---

## 💡 Resume Line

Automated AWS infrastructure provisioning using Terraform with modular architecture and remote state management, improving deployment consistency and reducing manual errors.

---

## 👨‍💻 Author

Ajay Nikam
