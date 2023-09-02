# Deploying a Static and Secured Website Using a 3 Tier-Architecture
This project is geared towards the deployment of a simple static web application on Amazon Web Services (AWS) utilizing a comprehensive 3-tier architectural framework. This architecture is made up of a Virtual Private Cloud (VPC), strategically configured Subnets, a NAT Gateway, an Internet Gateway for seamless connectivity, a meticulously designed Route Table, Security Groups, the integration of Amazon Route 53 for robust DNS management, AWS Certificate Manager for the establishment of secure SSL/TLS certification, the implementation of an Application Load Balancer (ALB) for optimized traffic distribution, the utilization of EC2 instances for efficient computation, the orchestration of an Auto Scaling Group for dynamic resource management, and lastly, the creation of a Launch Template to streamline instance launches.

## Prerequisites
1- Clone my github repo in the following link OR download the zipped file here [jupiter-zip-file](https://github.com/Ahmednas211/jupiter-zip-file)

2- Allow all the necessary permissions from root account.

3- Basic knowledge of AWS Command Line Interface and Github.

## Architecture Overview
Tier 1: The first tier is expected to accommodate our Applciation Load Balancer, sitting between two Availability Zones (AZ-1: us-east-1a & AZ-2: us-east-2b).

Tier 2: The second tier is our private Application Webserver (resposnsible for serving the staic website). This layer also accommodates our Auto-Scaling Group to ensure high avalability and scalability of our EC2 intances accross the two AZs indicated above. 

Tier 3: This tier is responsible for storing and managing data. However, in a static website, there may not be a need for a database because static sites don't typically store dynamic data that needs to be retrieved or modified frequently. Instead, you might use simpler storage solutions like S3 or Content Delivery Networks (CDNs) to serve static assets.

## Sample Architecture

![Static Web Application Architecture](https://i.imgur.com/Z1o1RnL.png)

## Deployment Steps

1. **Infrastructure Setup:**

    - VPC Creation: Create a Virtual Private Cloud (VPC) with public and private subnets using AWS Management Console or AWS CLI. Ensure that you carefully plan your IP addressing scheme and route tables (Check provided architecture for proper reference).

    - Internet Connectivity: Attach an Internet Gateway to the VPC to enable communication with the internet.

    - NAT Gateway: Set up a NAT Gateway in the public subnet to allow instances in the private subnet to access the internet securely.

2. **Security Group Configuration:**

    - ALB Security Group: Create a Security Group for the Application Load Balancer (ALB) to allow inbound HTTP and HTTPS traffic from Anywhere (0.0.0.0/0).

    - Web Server Security Group: Configure a Security Group for your EC2 instances to allow inbound HTTP and HTTPS traffic from the ALB Security Group. Then SSH from "MY IP".

3. **Application Load Balancer (ALB):**

    - ALB Setup: Configure the Application Load Balancer (ALB) and associate it with the ALB Security Group. Ensure it's connected to the appropriate public subnets.

    - Listeners: Set up ALB listeners for HTTP (port 80) and HTTPS (port 443) to forward traffic to backend EC2 instances. Configure appropriate SSL policies for HTTPS.

4. **DNS and SSL/TLS Certificate Setup:**

    - Route 53 Configuration: Create a hosted zone on Route 53 to manage the DNS records for your application. Configure DNS records like A and CNAME records as needed.

    - SSL/TLS Certificate: Obtain an SSL/TLS certificate using AWS Certificate Manager for your domain or subdomain to enable secure HTTPS traffic. Ensure the certificate is associated with the ALB.

5. **Auto Scaling Group (ASG):**

    - ASG Configuration: Create an Auto Scaling Group (ASG) using the Launch Template. Configure scaling policies, instance count, and health checks.

    - Target Group: Set up a Target Group and configure it as the target for the ASG. This ensures that traffic is distributed correctly among instances.

6. **Launch EC2 & Bash Script Execution:**

    - Instance Access: Connect securely to the EC2 instances launched by the ASG using SSH. Ensure that SSH keys and access credentials are managed securely.

    - Bash Script: Copy the provided Bash script to the instances and execute it. Include error handling, logging, and validation steps in the script to ensure the web servers are installed and configured correctly.

    ```
    #!/bin/bash
    sudo su
    yum update -y 
    yum install httpd -y
    cd /var/www/html
    wget https://github.com/Ahmednas211/jupiter-zip-file/raw/main/jupiter-main.zip
    unzip jupiter-main.zip
    cp -r jupiter-main/* /var/www/html
    rm -rf jupiter-main jupiter-main.zip
    systemctl start httpd
    systemctl enable httpd

    ```

7. **ALB Listener Configuration:**

    - Listener Rules: Define rules in the ALB listener to route traffic based on hostnames, paths, or other criteria. Ensure that traffic is appropriately directed to backend instances.

8. **DNS Configuration:**

    - Route 53 Updates: Update DNS records in Route 53 to point to the ALB's DNS name. Implement health checks in Route 53 to monitor the availability of your application.

9. **Testing and Monitoring:**

    - Testing: After DNS records have propagated, thoroughly test your application using the domain name (HTTPS) to verify the deployment's success.
  
    - Monitoring: Implement cloud monitoring and logging solutions (e.g., AWS CloudWatch) to monitor the health and performance of your infrastructure and applications.

10. **Backup and Disaster Recovery:**

    - Implement backup and disaster recovery mechanisms to ensure data and application availability in case of failures.

11. **Security and Compliance:**

    - Follow AWS security best practices, implement IAM roles and policies, and consider compliance requirements specific to your application.

12. **Documentation:**

    - Maintain detailed documentation of the deployment process, infrastructure architecture, and any changes made over time.
   

