DevOps Project: Deploy Docker on EC2 with Terraform

Overview
This DevOps project automates the deployment of Docker on an EC2 instance using Terraform. The project leverages AWS infrastructure and includes scripts for installing Docker, Docker Compose, Git, and Wget on the EC2 instance. The main objective is to provide a seamless and efficient method for setting up a Docker environment on AWS.

Project Structure
The project is organized into several key files and directories:

- main.tf: Contains the Terraform configuration for creating the AWS resources, including the EC2 instance.
- install.sh: A shell script that installs Docker, Docker Compose, Git, and Wget on the EC2 instance, and configures the environment.
- .terraform.lock.hcl: Manages the versions of Terraform providers.
- terraform.tfstate & terraform.tfstate.backup: Terraform state files that keep track of the deployed infrastructure.
- .gitignore: Specifies files and directories to be ignored by Git.
- docker-keypair.pem: SSH key for accessing the EC2 instance (ensure this is kept secure).

Prerequisites
Before running this project, ensure you have the following prerequisites:

AWS account with sufficient permissions to create EC2 instances and other resources.
Terraform installed on your local machine.
AWS CLI configured with your AWS credentials.
Git installed on your local machine.

Setup Instructions
1. Clone the Repository:
git clone https://github.com/tyleboyd/Devops_Project.git
cd Devops_Project

2. Initialize Terraform:
terraform init

3. Apply Terraform Configuration:
terraform apply

4. Access the EC2 Instance:
Use the docker-keypair.pem file to SSH into the instance:
ssh -i "docker-keypair.pem" ec2-user@<EC2-Instance-Public-IP>

5. Verify Docker Installation:
Once logged into the EC2 instance, run the following command to verify Docker is installed and running:
docker --version
docker-compose --version

Scripts
install.sh
This script performs the following actions:

Updates the package list and installs Docker.
Adds the ec2-user to the Docker group.
Starts the Docker service and enables it to start on boot.
Changes the terminal prompt color for the ec2-user.
Installs Git and Wget.
Installs Docker Compose.

#!/bin/bash
# docker install
sudo yum update -y
sudo yum install docker -y
sudo usermod -aG docker ec2-user
sudo service docker start
sudo systemctl enable docker 
# Change terminal color for user ec2-user
echo "PS1='\e[1;32m\u@\h \w$ \e[m'" >> /home/ec2-user/.bash_profile
# install git
sudo yum install git -y
# install wget
sudo yum install wget -y
# install docker compose
sudo curl -L https://github.com/docker/compose/releases/download/1.20.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

Cleanup
To destroy the infrastructure created by this project, run the following command:
terraform destroy
Review the plan and type yes to confirm the destruction of the resources.







