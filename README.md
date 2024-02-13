# Deploying-a-MERN-Application-on-AWS
Deploying a MERN Application on AWS Using Terraform & Ansible

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

      - Crosscheck & Preview Changes
   ```
   terraform validate
   terraform plan
   ```

   ```
   israr@LinuxProj-05:~/terraform-aws$ terraform validate
   Success! The configuration is valid.

   israr@LinuxProj-05:~/terraform-aws$ terraform plan
   data.aws_iam_policy_document.loadbalancer: Reading...
   data.aws_iam_policy_document.loadbalancer: Read complete after 0s [id=2044959750]

   Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
     + create

   Terraform will perform the following actions:

     # aws_iam_policy.loadbalancer_policy will be created
     + resource "aws_iam_policy" "loadbalancer_policy" {
      + arn         = (known after apply)
      + id          = (known after apply)
      + name        = "load_balancer_mern"
      + name_prefix = (known after apply)
      + path        = "/"
      + policy      = jsonencode(
            {
              + Statement = [
                  + {
                      + Action   = [
                          + "elasticloadbalancing:Get*",
                          + "elasticloadbalancing:Describe*",
                        ]
                      + Effect   = "Allow"
                      + Resource = "*"
                      + Sid      = "Statement1"
                    },
                  + {
                      + Action   = [
                          + "ec2:DescribeSecurityGroups",
                          + "ec2:DescribeInstances",
                          + "ec2:DescribeClassicLinkInstances",
                        ]
                      + Effect   = "Allow"
                      + Resource = "*"
                      + Sid      = "Statement2"
                    },
                  + {
                      + Action   = "arc-zonal-shift:GetManagedResource"
                      + Effect   = "Allow"
                      + Resource = "arn:aws:elasticloadbalancing:*:*:loadbalancer/*"
                      + Sid      = "Statement3"
                    },
                  + {
                      + Action   = [
                          + "arc-zonal-shift:ListZonalShifts",
                          + "arc-zonal-shift:ListManagedResources",
                        ]
                      + Effect   = "Allow"
                      + Resource = "*"
                      + Sid      = "Statement4"
                    },
                ]
              + Version   = "2012-10-17"
            }
        )
      + policy_id   = (known after apply)
      + tags_all    = (known after apply)
    }

     # aws_instance.israr_database will be created
     + resource "aws_instance" "israr_database" {
      + ami                                  = "ami-03f4878755434977f"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = (known after apply)
      + availability_zone                    = (known after apply)
      + cpu_core_count                       = (known after apply)
      + cpu_threads_per_core                 = (known after apply)
      + disable_api_stop                     = (known after apply)
      + disable_api_termination              = (known after apply)
      + ebs_optimized                        = (known after apply)
      + get_password_data                    = false
      + host_id                              = (known after apply)
      + host_resource_group_arn              = (known after apply)
      + iam_instance_profile                 = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = (known after apply)
      + instance_lifecycle                   = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t2.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      + key_name                             = "israr"
      + monitoring                           = (known after apply)
      + outpost_arn                          = (known after apply)
      + password_data                        = (known after apply)
      + placement_group                      = (known after apply)
      + placement_partition_number           = (known after apply)
      + primary_network_interface_id         = (known after apply)
      + private_dns                          = (known after apply)
      + private_ip                           = (known after apply)
      + public_dns                           = (known after apply)
      + public_ip                            = (known after apply)
      + secondary_private_ips                = (known after apply)
      + security_groups                      = (known after apply)
      + source_dest_check                    = true
      + spot_instance_request_id             = (known after apply)
      + subnet_id                            = (known after apply)
      + tags                                 = {
          + "Name" = "israr_database_terraform"
        }
      + tags_all                             = {
          + "Name" = "israr_database_terraform"
        }
      + tenancy                              = (known after apply)
      + user_data                            = (known after apply)
      + user_data_base64                     = (known after apply)
      + user_data_replace_on_change          = false
      + vpc_security_group_ids               = (known after apply)
    }

     # aws_instance.israr_terraform will be created
     + resource "aws_instance" "israr_terraform" {
      + ami                                  = "ami-03f4878755434977f"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = (known after apply)
      + availability_zone                    = (known after apply)
      + cpu_core_count                       = (known after apply)
      + cpu_threads_per_core                 = (known after apply)
      + disable_api_stop                     = (known after apply)
      + disable_api_termination              = (known after apply)
      + ebs_optimized                        = (known after apply)
      + get_password_data                    = false
      + host_id                              = (known after apply)
      + host_resource_group_arn              = (known after apply)
      + iam_instance_profile                 = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = (known after apply)
      + instance_lifecycle                   = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t2.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      + key_name                             = "israr"
      + monitoring                           = (known after apply)
      + outpost_arn                          = (known after apply)
      + password_data                        = (known after apply)
      + placement_group                      = (known after apply)
      + placement_partition_number           = (known after apply)
      + primary_network_interface_id         = (known after apply)
      + private_dns                          = (known after apply)
      + private_ip                           = (known after apply)
      + public_dns                           = (known after apply)
      + public_ip                            = (known after apply)
      + secondary_private_ips                = (known after apply)
      + security_groups                      = (known after apply)
      + source_dest_check                    = true
      + spot_instance_request_id             = (known after apply)
      + subnet_id                            = (known after apply)
      + tags                                 = {
          + "Name" = "israr_mern_terraform"
        }
      + tags_all                             = {
          + "Name" = "israr_mern_terraform"
        }
      + tenancy                              = (known after apply)
      + user_data                            = (known after apply)
      + user_data_base64                     = (known after apply)
      + user_data_replace_on_change          = false
      + vpc_security_group_ids               = (known after apply)
    }

     # aws_internet_gateway.gw will be created
     + resource "aws_internet_gateway" "gw" {
      + arn      = (known after apply)
      + id       = (known after apply)
      + owner_id = (known after apply)
      + tags_all = (known after apply)
      + vpc_id   = (known after apply)
    }

     # aws_route_table.private will be created
     + resource "aws_route_table" "private" {
      + arn              = (known after apply)
      + id               = (known after apply)
      + owner_id         = (known after apply)
      + propagating_vgws = (known after apply)
      + route            = (known after apply)
      + tags_all         = (known after apply)
      + vpc_id           = (known after apply)
    }

     # aws_route_table.public will be created
     + resource "aws_route_table" "public" {
      + arn              = (known after apply)
      + id               = (known after apply)
      + owner_id         = (known after apply)
      + propagating_vgws = (known after apply)
      + route            = [
          + {
              + carrier_gateway_id         = ""
              + cidr_block                 = "0.0.0.0/0"
              + core_network_arn           = ""
              + destination_prefix_list_id = ""
              + egress_only_gateway_id     = ""
              + gateway_id                 = (known after apply)
              + ipv6_cidr_block            = ""
              + local_gateway_id           = ""
              + nat_gateway_id             = ""
              + network_interface_id       = ""
              + transit_gateway_id         = ""
              + vpc_endpoint_id            = ""
              + vpc_peering_connection_id  = ""
            },
        ]
      + tags_all         = (known after apply)
      + vpc_id           = (known after apply)
    }

     # aws_route_table_association.private will be created
     + resource "aws_route_table_association" "private" {
      + id             = (known after apply)
      + route_table_id = (known after apply)
      + subnet_id      = (known after apply)
    }

     # aws_route_table_association.public will be created
     + resource "aws_route_table_association" "public" {
      + id             = (known after apply)
      + route_table_id = (known after apply)
      + subnet_id      = (known after apply)
    }

     # aws_security_group.custom_security_group will be created
     + resource "aws_security_group" "custom_security_group" {
      + arn                    = (known after apply)
      + description            = "Security group for israr_terraform instance"
      + egress                 = [
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + description      = ""
              + from_port        = 0
              + ipv6_cidr_blocks = []
              + prefix_list_ids  = []
              + protocol         = "-1"
              + security_groups  = []
              + self             = false
              + to_port          = 0
            },
        ]
      + id                     = (known after apply)
      + ingress                = [
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + description      = ""
              + from_port        = 443
              + ipv6_cidr_blocks = []
              + prefix_list_ids  = []
              + protocol         = "tcp"
              + security_groups  = []
              + self             = false
              + to_port          = 443
            },
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + description      = ""
              + from_port        = 80
              + ipv6_cidr_blocks = []
              + prefix_list_ids  = []
              + protocol         = "tcp"
              + security_groups  = []
              + self             = false
              + to_port          = 80
            },
        ]
      + name                   = "israr_terraform"
      + name_prefix            = (known after apply)
      + owner_id               = (known after apply)
      + revoke_rules_on_delete = false
      + tags_all               = (known after apply)
      + vpc_id                 = (known after apply)
    }

     # aws_security_group.custom_security_group_database will be created
     + resource "aws_security_group" "custom_security_group_database" {
      + arn                    = (known after apply)
      + description            = "Security group for israr_terraform database"
      + egress                 = (known after apply)
      + id                     = (known after apply)
      + ingress                = [
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + description      = ""
              + from_port        = 3306
              + ipv6_cidr_blocks = []
              + prefix_list_ids  = []
              + protocol         = "tcp"
              + security_groups  = []
              + self             = false
              + to_port          = 3306
            },
        ]
      + name                   = "israr_database_terraform"
      + name_prefix            = (known after apply)
      + owner_id               = (known after apply)
      + revoke_rules_on_delete = false
      + tags_all               = (known after apply)
      + vpc_id                 = (known after apply)
    }

     # aws_subnet.private will be created
     + resource "aws_subnet" "private" {
      + arn                                            = (known after apply)
      + assign_ipv6_address_on_creation                = false
      + availability_zone                              = "ap-south-1b"
      + availability_zone_id                           = (known after apply)
      + cidr_block                                     = "172.31.0.0/20"
      + enable_dns64                                   = false
      + enable_resource_name_dns_a_record_on_launch    = false
      + enable_resource_name_dns_aaaa_record_on_launch = false
      + id                                             = (known after apply)
      + ipv6_cidr_block_association_id                 = (known after apply)
      + ipv6_native                                    = false
      + map_public_ip_on_launch                        = false
      + owner_id                                       = (known after apply)
      + private_dns_hostname_type_on_launch            = (known after apply)
      + tags_all                                       = (known after apply)
      + vpc_id                                         = (known after apply)
    }

     # aws_subnet.public will be created
     + resource "aws_subnet" "public" {
      + arn                                            = (known after apply)
      + assign_ipv6_address_on_creation                = false
      + availability_zone                              = "ap-south-1a"
      + availability_zone_id                           = (known after apply)
      + cidr_block                                     = "172.31.32.0/20"
      + enable_dns64                                   = false
      + enable_resource_name_dns_a_record_on_launch    = false
      + enable_resource_name_dns_aaaa_record_on_launch = false
      + id                                             = (known after apply)
      + ipv6_cidr_block_association_id                 = (known after apply)
      + ipv6_native                                    = false
      + map_public_ip_on_launch                        = true
      + owner_id                                       = (known after apply)
      + private_dns_hostname_type_on_launch            = (known after apply)
      + tags_all                                       = (known after apply)
      + vpc_id                                         = (known after apply)
    }

     # aws_vpc.main will be created
     + resource "aws_vpc" "main" {
      + arn                                  = (known after apply)
      + cidr_block                           = "172.31.0.0/16"
      + default_network_acl_id               = (known after apply)
      + default_route_table_id               = (known after apply)
      + default_security_group_id            = (known after apply)
      + dhcp_options_id                      = (known after apply)
      + enable_dns_hostnames                 = true
      + enable_dns_support                   = true
      + enable_network_address_usage_metrics = (known after apply)
      + id                                   = (known after apply)
      + instance_tenancy                     = "default"
      + ipv6_association_id                  = (known after apply)
      + ipv6_cidr_block                      = (known after apply)
      + ipv6_cidr_block_network_border_group = (known after apply)
      + main_route_table_id                  = (known after apply)
      + owner_id                             = (known after apply)
      + tags_all                             = (known after apply)
    }

   Plan: 13 to add, 0 to change, 0 to destroy.

   Changes to Outputs:
     + israr_database_terraform_instance_ip = (known after apply)
     + israr_terraform_instance_ip          = (known after apply)

   ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

   Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.
   israr@LinuxProj-05:~/terraform-aws$
   ```

2. VPC and Network Configuration:

   - Create an AWS VPC with two subnets: one public and one private.

   ![image](https://github.com/adm077/Deploying-a-MERN-Application-on-AWS/assets/139608052/3616a827-e2d2-4103-83c4-8d3c3abac09f)

   - Set up an Internet Gateway and a NAT Gateway.

   ![image](https://github.com/adm077/Deploying-a-MERN-Application-on-AWS/assets/139608052/eff5bfac-24f1-4d39-a08f-459e40800dec)

   - Configure route tables for both subnets.

   ![image](https://github.com/adm077/Deploying-a-MERN-Application-on-AWS/assets/139608052/baff443e-c37e-44e1-b9a7-31871825b3ab)


3. EC2 Instance Provisioning:

   - Launch two EC2 instances: one in the public subnet (for the web server) and another in the private subnet (for the database).

   - Ensure both instances are accessible via SSH (public instance only accessible from your IP).


   ![image](https://github.com/adm077/Deploying-a-MERN-Application-on-AWS/assets/139608052/927ee700-1713-436c-93de-f8ec814912b8)

   ![image](https://github.com/adm077/Deploying-a-MERN-Application-on-AWS/assets/139608052/95fda945-985c-40f4-961b-e4caffffea67)


4. Security Groups and IAM Roles:

   - Create necessary security groups for web and database servers.

   ![image](https://github.com/adm077/Deploying-a-MERN-Application-on-AWS/assets/139608052/4064c585-ebee-469e-b511-56246b1e8e3c)

   ![image](https://github.com/adm077/Deploying-a-MERN-Application-on-AWS/assets/139608052/23765856-71e4-4215-9071-f69b2e7b2d15)

   - Set up IAM roles for EC2 instances with required permissions.

5. Resource Output:

   - Output the public IP of the web server EC2 instance.

   ![image](https://github.com/adm077/Deploying-a-MERN-Application-on-AWS/assets/139608052/8563a21c-f5d0-4a18-ba0f-e1128af66377)



**Part 2: Configuration and Deployment with Ansible**


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
