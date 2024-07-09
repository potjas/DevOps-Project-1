# DevOps-Project-1
Project Overview
Goal
The primary objective of this project is to deploy a scalable, highly available, and secure Java application using a 3-tier architecture. The application will be accessible to end-users via the public internet.

Pre-Requisites
AWS Free Tier Account: Sign up for an Amazon Web Services (AWS) Free Tier account to use various AWS services for deployment.
GitHub Account and Repository: Create a GitHub account and a repository to host your Java source code. Migrate the provided Java source code from the Java-Login-App repository to your own GitHub repository.
SonarCloud Account: Create an account on SonarCloud for static code analysis and quality checks.
JFrog Cloud Account: Create an account on JFrog Cloud to manage your artifacts.
Pre-Deployment Steps
Create Global AMI (Amazon Machine Image):

Install AWS CLI.
Install CloudWatch Agent.
Install AWS Systems Manager (SSM) Agent.
Create Golden AMIs:

For Nginx:
Install Nginx.
Configure custom memory metrics for CloudWatch.
For Apache Tomcat:
Install Apache Tomcat.
Configure Tomcat as a systemd service.
Install JDK 11.
Configure custom memory metrics for CloudWatch.
For Apache Maven Build Tool:
Install Apache Maven.
Install Git.
Install JDK 11.
Update the Maven Home in the system PATH environment variable.
VPC Deployment
Deploy AWS infrastructure resources as per the architecture diagram.

VPC (Network Setup):

Create VPC networks:
192.168.0.0/16 for Bastion Host.
172.32.0.0/16 for application servers.
Set up NAT Gateway in the public subnet and update the route table for the private subnet.
Create a Transit Gateway and associate both VPCs for private communication.
Create Internet Gateways for each VPC and update route tables for inbound/outbound internet access.
Bastion Host:

Deploy a Bastion Host in the public subnet with an Elastic IP (EIP).
Create a security group allowing port 22 (SSH) from the public internet.
Maven (Build)
Launch an EC2 instance using the Maven Golden AMI.
Clone the GitHub repository to VSCode, update pom.xml with SonarCloud and JFrog deployment details, and add settings.xml with JFrog credentials.
Update application.properties with JDBC connection string.
Push code changes to a feature branch, create a pull request, and merge changes to the master branch.
Clone the repository on the EC2 instance and build the source code using Maven with -s settings.xml.
Integrate the Maven build with SonarCloud and generate an analysis dashboard using the default quality gate profile.
3-Tier Architecture
Database (RDS):

Deploy a Multi-AZ MySQL RDS instance in private subnets.
Create a security group allowing port 3306 from application instances and Bastion Host.
Tomcat (Backend):

Create a private-facing Network Load Balancer and Target Group.
Create a Launch Configuration:
Use Tomcat Golden AMI.
Deploy .war artifacts from JFrog to the webapps folder.
Configure security groups to allow port 22 from Bastion Host and port 8080 from the private NLB.
Set up an Auto Scaling Group.
Nginx (Frontend):

Create a public-facing Network Load Balancer and Target Group.
Create a Launch Configuration:
Use Nginx Golden AMI.
Update proxy_pass rules in nginx.conf and reload the Nginx service.
Configure security groups to allow port 22 from Bastion Host and port 80 from the public NLB.
Set up an Auto Scaling Group.
Application Deployment
The deployment of artifacts is handled by user data scripts during the launch of EC2 instances in the application tier.
Log into the MySQL database from the application server using MySQL CLI and create the database and table schema to store user login data (as described in the README.md file in the GitHub repo).
Post-Deployment
Configure a Cron job to push Tomcat application logs to an S3 bucket and rotate logs by deleting them from the server after upload.
Set up CloudWatch alarms to send email notifications if database connections exceed 100.
Validation
Ensure administrators can log into EC2 instances via the session manager and Bastion Host.
Verify that end-users can access the application from a public internet browser.
Final Note
If you find this repository useful for learning, please give it a star on GitHub. Thank you!
