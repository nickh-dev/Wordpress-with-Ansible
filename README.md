# Wordpress deployment with Ansible and AWS:
This Ansible playbook is designed to automate the deployment of a WordPress application on an Amazon Web Services (AWS) infrastructure. The playbook consists of two plays: the first play sets up the necessary AWS resources such as security groups, EC2 instances, and RDS databases, while the second play configures the web server and installs the required software for WordPress.

Here's a breakdown of the playbook's structure and tasks:

# Play 1: Setting up AWS Resources
1. Prompts the user to enter various parameters required for the AWS infrastructure setup, such as the database name, master username and password, AWS access key and secret key, region, VPC ID, and key name.
2. Ensuring that the required Python packages, boto3 and botocore, are installed using the pip module.
3. Creating a security group using the amazon.aws.ec2_security_group module, allowing inbound traffic on ports 22 (SSH), 80 (HTTP), and 3306 (MySQL) from any IP address.
4. Creating an EC2 instance using the ec2_instance module, specifying the region, AWS access key, secret key, image ID, instance type, key name, security group ID, and other parameters. It also registers the output of this task in the ec2 variable for later use.
5. Coping the EC2 instance's public IP address and SSH key information to a hosts file (./myinventory/hosts.txt) using the copy module.
6. Creating an AWS RDS MySQL database instance using the amazon.aws.rds_instance module, specifying the necessary parameters like AWS access key, secret key, database engine, security group ID, database name, instance type, master user details, and other settings. The output endpoint address of the RDS instance is registered in the rds_instance_output variable for later use.
7. It uses the meta: refresh_inventory module to refresh the Ansible inventory after the EC2 and RDS instances are created.
8. It prints the RDS instance's endpoint address using the debug module.
9. It pauses the playbook execution for 60 seconds using the pause module to allow time for the instances to initialize.

# Play 2: Configuring Web Server and Installing WordPress

1. Seting up the web server by installing Apache HTTP Server using the package module.
2. Installing software-properties-common using the apt module.
3. Adding the Ondrej PHP repository using the apt_repository module.
4. Updating the apt cache using the apt module.
5. Installing PHP 7.4 using the apt module.
6. Installing the prerequisites for WordPress, such as PHP extensions and packages, using the apt module.
7. It enables the PHP extension by restarting the Apache service using the service module.
8. Downloading the latest WordPress tar file from the official website using the ansible.builtin.unarchive module and extracts it to the /var/www/html directory.
9. It sets the ownership of the WordPress files to the www-data user and group using the file module.
10. Starting the Apache service and enables it to start automatically using the service module.

The playbook is designed to be run on the localhost inventory, indicating that the deployment is performed from the control machine itself. Before running the playbook, ensure that you have Ansible installed and configured properly, and make any necessary adjustments to the playbook variables, such as the image ID and PHP versions, according to your specific requirements.

Once executed, this playbook will automate the setup of AWS resources and the installation and configuration of the WordPress application, providing a streamlined deployment process for WordPress on AWS infrastructure.

# Prerequisites:
1. AWS account with free tier
2. AWS Credentials (AWS Home page -> Click on your nickname on the right upper corner -> Security Credentials -> Look for Access Key section -> Create your credentials)
3. Preinstalled software:
   - Python3 and pip3
   - Boto and Boto3
   - Ansible and Ansible-galaxy
   - amazon.aws collection (version 6.10)
4. Add your private key in the main directory with your project.

# To run the playbook you need to type:
sudo ansible-playbook <nameOfYourPlaybook> -i <nameOfYourHostDirectory/hosts>
