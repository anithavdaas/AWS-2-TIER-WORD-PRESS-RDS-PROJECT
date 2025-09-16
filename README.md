# AWS-2-TIER-WORD-PRESS-RDS-PROJECT
DEPLOYING WORD PRESS ON AWS EC2 WITH RDS

# AWS 2-Tier Architecture Project – WordPress & MySQL

## Overview
This project demonstrates deploying a WordPress website using a 2-tier architecture on AWS, separating the web and database layers for better security and scalability.

## Architecture
- *VPC* with public and private subnets
- *EC2* instance in public subnet running WordPress, Apache, PHP
- *RDS MySQL* database in private subnet
- *Security Groups* for controlled access
- *DB Subnet Group* to place database across multiple AZs

## Steps Implemented
1. Created VPC, subnets, Internet Gateway, and route tables.
2. Configured security groups (HTTP/HTTPS/SSH for EC2, MySQL only from EC2 SG).
3. Created DB subnet group and launched RDS instance (private, non-public).
4. Deployed EC2 instance and installed Apache, PHP, and WordPress.
5. Configured WordPress to connect to RDS.
6. Verified setup by accessing WordPress via EC2 public IP.

## Skills Gained
- AWS Networking (VPC, Subnets, Route Tables)
- Compute & Database (EC2, RDS)
- Security (Security Groups, Private/Public Subnets)
- Linux administration & WordPress setup

# 1. Update server and install Apache, PHP, and MariaDB client
sudo dnf update -y
sudo dnf install -y httpd php php-mysqlnd
sudo dnf install -y mariadb105

# 2. Start and enable Apache
sudo systemctl start httpd
sudo systemctl enable httpd

# 3. Verify PHP installation
php -v

# 4. Navigate to Apache web root
cd /var/www/html

# 5. Download and extract WordPress
sudo curl -O https://wordpress.org/latest.tar.gz
sudo tar -xzf latest.tar.gz
sudo cp -r wordpress/* /var/www/html/

# 6. Set ownership and permissions
sudo chown -R apache:apache /var/www/html/
sudo find /var/www/html -type d -exec chmod 755 {} \;
sudo find /var/www/html -type f -exec chmod 644 {} \;

# 7. Verify permissions
ls -l /var/www/html | head

# 8. Configure WordPress to connect to RDS
sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php
# (Edit DB_NAME, DB_USER, DB_PASSWORD, DB_HOST with your RDS info)

## 2-Tier AWS Architecture

            ┌────────────┐
            │   Users    │
            └─────┬──────┘
                  │ HTTP/HTTPS
                  ▼
            ┌────────────┐
            │   EC2      │
            │ Apache+PHP │
            │ WordPress  │
            └─────┬──────┘
                  │ MySQL Queries
                  ▼
            ┌────────────┐
            │   RDS      │
            │  MySQL DB  │
            └────────────┘

VPC:
- Public Subnet → EC2
- Private Subnet → RDS

Security Groups:
- EC2 SG → HTTP/SSH
- RDS SG → MySQL only from EC2 SG
