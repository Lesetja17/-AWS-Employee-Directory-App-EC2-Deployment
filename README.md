# AWS-Employee-Directory-App-EC2-Deployment
This project demonstrates the deployment of a simple **Employee Directory web application** on **Amazon EC2** using the AWS Free Tier. The goal is to build foundational AWS skills around compute, networking, identity, and automation.
This is my first hands-on AWS project as part of my cloud learning journey 🚀

🎯 Learning Objectives

By completing this project, I learned and practiced:

AWS EC2 instance provisioning

Default VPC and subnet usage

IAM role assignment

Security Group configuration (HTTP / HTTPS)

Automated instance setup using User Data

Understanding basic AWS networking & compute concepts

🛠️ Tech Stack & AWS Services
Category	Tools
Compute	Amazon EC2 (t2.micro — Free Tier)
Network	AWS Default VPC, Subnet, Security Group
Identity	IAM Role for instance access
Automation	User-data bootstrap script
OS	Amazon Linux AMI
📐 Architecture
┌──────────────────────────────┐
|         AWS Cloud            |
|  ┌────────────────────────┐  |
|  |     Default VPC        |  |
|  | ┌────────────────────┐ |  |
|  | |  Public Subnet     | |  |
|  | | ┌───────────────┐  | |  |
|  | | |  EC2 Instance |  | |  |
|  | | | (t2.micro)    |  | |  |
|  | | | Linux AMI     |  | |  |
|  | | | IAM Role       → S3* |
|  | | | User Data Script     |
|  | | └───────────────┘  | |  |
|  | └────────────────────┘ |  |
|  └────────────────────────┘  |
|      Security Group          |
|  (HTTP & HTTPS inbound)      |
└──────────────────────────────┘

*Database integration to be added later

🚀 Deployment Steps

1️⃣ Log in to AWS Console
2️⃣ Navigate to EC2 → Launch Instance
3️⃣ Choose Amazon Linux AMI
4️⃣ Select t2.micro (Free-tier)
5️⃣ Use Default VPC & Subnet
6️⃣ Create Security Group:
Allow HTTP (80)
Allow HTTPS (443)
7️⃣ Attach IAM Role
8️⃣ Add User Data Script

🧾 User-Data Script Used
#!/bin/bash -ex
wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/FlaskApp.zip
unzip FlaskApp.zip
cd FlaskApp/
yum -y install python3-pip
pip install -r requirements.txt
yum -y install stress
export PHOTOS_BUCKET=${SUB_PHOTOS_BUCKET}
export AWS_DEFAULT_REGION=eu-north-1
export DYNAMO_MODE=on
FLASK_APP=application.py /usr/local/bin/flask run --host=0.0.0.0 --port=80 
Purpose: Automates installation & app launch at boot — no SSH needed.

✅ Result
Once the instance initialized, I accessed the application using the EC2 public IP in a browser — and the app loaded successfully 🎉
