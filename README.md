# Deploying-a-MERN-Application-on-AWS
Deploying a MERN Application on AWS

Objective:

Gain practical experience in deploying a MERN stack application on AWS using infrastructure automation with Terraform and configuration management with Ansible.


MERN Application to use: https://github.com/UnpredictablePrashant/TravelMemory


Tasks:

Part 1: Infrastructure Setup with Terraform

1. AWS Setup and Terraform Initialization:

   - Configure AWS CLI and authenticate with your AWS account.
        
      - Installed the awscli on Ubuntu
   ```
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   sudo apt install unzip
   unzip awscliv2.zip
   sudo ./aws/install
   ```
   ![image](https://github.com/adm077/Deploying-a-MERN-Application-on-AWS/assets/139608052/76becf9a-8db4-4bb7-ab7d-0450a9a72e31)

   Configure aws using the "aws configure" command
   ```
   aws configure
   ```
   ![image](https://github.com/adm077/Deploying-a-MERN-Application-on-AWS/assets/139608052/59652755-8216-4c6e-a541-2d496befed4d)


   - Initialize a new Terraform project targeting AWS.

      - Install Terraform on Ubuntu using the below command
   ```
   sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
   wget -O- https://apt.releases.hashicorp.com/gpg | \
   gpg --dearmor | \
   echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
   https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
   sudo tee /etc/apt/sources.list.d/hashicorp.list
   sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
   echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
   https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
   sudo tee /etc/apt/sources.list.d/hashicorp.list
   sudo apt update
   sudo apt-get install terraform
   
   ```
   ![image](https://github.com/adm077/Deploying-a-MERN-Application-on-AWS/assets/139608052/afa65648-0d71-4d54-a234-857265cf7e08)

      - Create a new directory for the project
   ```
   mkdir terraform-aws
   cd terraform-aws
   touch main.tf
   ```

      - Initialize Terraform
   ```
   terraform init
   ```
   ![image](https://github.com/adm077/Deploying-a-MERN-Application-on-AWS/assets/139608052/955b982a-30ef-499e-9912-a8ebf0562126)

      - Preview Changes
   ```
   terraform plan
   ```

2. VPC and Network Configuration:

   - Create an AWS VPC with two subnets: one public and one private.

   - Set up an Internet Gateway and a NAT Gateway.

   - Configure route tables for both subnets.

3. EC2 Instance Provisioning:

   - Launch two EC2 instances: one in the public subnet (for the web server) and another in the private subnet (for the database).

   - Ensure both instances are accessible via SSH (public instance only accessible from your IP).

4. Security Groups and IAM Roles:

   - Create necessary security groups for web and database servers.

   - Set up IAM roles for EC2 instances with required permissions.

5. Resource Output:

   - Output the public IP of the web server EC2 instance.


Part 2: Configuration and Deployment with Ansible


1. Ansible Configuration:

   - Configure Ansible to communicate with the AWS EC2 instances.

2. Web Server Setup:

   - Write an Ansible playbook to install Node.js and NPM on the web server.

   - Clone the MERN application repository and install dependencies.

3. Database Server Setup:

   - Install and configure MongoDB on the database server using Ansible.

   - Secure the MongoDB instance and create necessary users and databases.

4. Application Deployment:

   - Configure environment variables and start the Node.js application.

   - Ensure the React frontend communicates with the Express backend.

5. Security Hardening:

   - Harden the security by configuring firewalls and security groups.

   - Implement additional security measures as needed (e.g., SSH key pairs, disabling root login).

Deliverables:

Terraform scripts for AWS infrastructure setup.
Ansible playbooks for configuration and deployment of the MERN application.
A detailed report documenting the implementation process, including how the application components interact with each other and the infrastructure.
Screenshots or a video recording demonstrating the working MERN application.
