# 🚀 **Full DevOps Project: Dynamic Web App Deployment on AWS with Docker, GitHub, and Flyway**

This project demonstrates the complete lifecycle of deploying a **3-tier web application** on **AWS** with Docker containers, automated CI/CD pipelines, and infrastructure-as-code techniques.

---

## 📋 **Project Overview**
You will:
- Set up a **3-tier AWS Network VPC** with public and private subnets.
- Use **GitHub** for version control and **Docker Hub** to manage container images.
- Deploy infrastructure using **Amazon ECS** and set up secure access through **IAM roles**, **Bastion Hosts**, and **Application Load Balancer (ALB)**.
- Use **Flyway** to automate database migrations.
- Enable **HTTPS** using certificates from **AWS Certificate Manager**.

---

## 🛠 **Prerequisites**

| **Tool**              | **Purpose**                      | **Installation Link**                            |
|-----------------------|-----------------------------------|-------------------------------------------------|
| GitHub Account        | Code repository                  | [Sign up for GitHub](https://github.com/)       |
| Docker Hub Account    | Manage container images          | [Docker Hub](https://hub.docker.com/)           |
| Git & VSCode          | Code editor & version control    | [Git](https://git-scm.com/downloads) / [VSCode](https://code.visualstudio.com/) |
| AWS CLI               | Interact with AWS from terminal  | [AWS CLI](https://aws.amazon.com/cli/)          |
| Flyway                | Manage database migrations       | [Flyway](https://flywaydb.org/)                 |

---

## 🛠️ **Step-by-Step Setup Guide**

### 1️⃣ **GitHub Setup**
1. **Create an SSH Key Pair**:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
Upload your public key to GitHub:
Settings → SSH and GPG keys → New SSH key

**Create a GitHub Repository:**

bash
Copy code
git init
git remote add origin git@github.com:your-username/your-repo.git
git add .
git commit -m "Initial commit"
git push origin main

**2️⃣ AWS VPC Setup**
Create a VPC with public and private subnets across 2 Availability Zones.

NAT Gateway Setup for internet access to private subnets.

Security Groups:

EC2: Allow SSH (port 22) and HTTP (port 80/443).
RDS: Allow only internal traffic from private subnets.
Create an RDS Database in a private subnet.

**3️⃣ Docker Configuration**
Create a Dockerfile for your app:

dockerfile
Copy code
FROM node:14
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]
EXPOSE 3000
Add a .gitignore file:

plaintext
Copy code
node_modules/
.env
Build the Docker Image:

bash
Copy code
docker build -t your-image-name .
Push the Image to Docker Hub:

bash
Copy code
docker tag your-image-name your-dockerhub-username/your-image-name
docker push your-dockerhub-username/your-image-name

**4️⃣ AWS CLI & ECR Setup**
Configure AWS CLI:

bash
Copy code
aws configure
Create a Repository in Amazon ECR:

bash
Copy code
aws ecr create-repository --repository-name your-ecr-repo
Push Docker Image to ECR:

bash
Copy code
aws ecr get-login-password | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
docker tag your-image-name <account-id>.dkr.ecr.<region>.amazonaws.com/your-ecr-repo
docker push <account-id>.dkr.ecr.<region>.amazonaws.com/your-ecr-repo

**5️⃣ Bastion Host Setup**
Create an EC2 Instance (Bastion Host) in the public subnet.
SSH into the Bastion Host:
bash
Copy code
ssh -i your-key.pem ec2-user@<bastion-host-public-ip>

**6️⃣ Flyway Setup**
Install Flyway on your local machine.
Update flyway.conf to connect to the RDS database.
Organize SQL scripts in a /sql directory.
Run Flyway Migrations:
bash
Copy code
ssh -i your-key.pem -L 5432:your-rds-endpoint:5432 ec2-user@<bastion-host-public-ip>
flyway migrate

**7️⃣ Deploy ECS Cluster**
Create IAM Roles:

ECS Task Execution Role
EC2 Container Service Role
Set Up an ECS Cluster and Task Definition using the Docker image from ECR.

Deploy the ECS Service:

Link it to the Application Load Balancer (ALB).
Configure HTTPS using ACM Certificates.

**8️⃣ Route 53 & DNS Configuration**
Register a Domain using Route 53.
Create an A Record to point the domain to the ALB.
🧑‍💻 Build and Run Scripts
Build Script (build.sh)
bash
Copy code
#!/bin/bash
docker build -t your-image-name .
Run Script (run.sh)
bash
Copy code
#!/bin/bash
docker run -d -p 3000:3000 your-image-name
Make the scripts executable:

bash
chmod +x build.sh run.sh

**✅ Testing the Deployment**
Access your web application using your domain name or ALB DNS name.
Verify the application’s health status via the ECS Service Console.
Test HTTPS functionality to ensure the certificate is valid.

**📦 Project Directory Structure**
arduino
Copy code
/your-repo
│
├── Dockerfile
├── .gitignore
├── build.sh
├── run.sh
├── sql/
│   └── migrations.sql
├── src/
│   └── app.js
└── README.md

**🎉 Conclusion**
By following these steps, you've successfully built and deployed a 3-tier web app on AWS using Docker, ECS, and Flyway. The application is now live and secured with HTTPS, ready to serve users across the globe.

**Enjoy your journey in DevOps! 🚀**








