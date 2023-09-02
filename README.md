## Deploying an AWS Static Website Using a 3 Tier-Architecture.

This project is geared towards the deployment of a simple static web application on Amazon Web Services (AWS) utilizing a comprehensive 3-tier architectural framework. This architecture is made up of a Virtual Private Cloud (VPC), strategically configured Subnets, a NAT Gateway, an Internet Gateway for seamless connectivity, a meticulously designed Route Table, Security Groups, the integration of Amazon Route 53 for robust DNS management, AWS Certificate Manager for the establishment of secure SSL/TLS certification, the implementation of an Application Load Balancer (ALB) for optimized traffic distribution, the utilization of EC2 instances for efficient computation, the orchestration of an Auto Scaling Group for dynamic resource management, and lastly, the creation of a Launch Template to streamline instance launches.

## **Prerequisites**

  - Grant all the necessary permissions from the root user account.
  - Basic to intermidiate skill in Github, AWS management console and command line interface.
  - Ability to read and decipher AWS Well-Architected framework diagrams.

## **Architecture Overview**

  - Presentation Layer: This layer serves as the interface between users and the application, functioning as the primary entry point for user interactions. To enhance reliability and performance, it's supported by an Application Load Balancer (ALB). The ALB efficiently distributes incoming traffic across multiple web server instances, ensuring consistent access to the application.

  - Application Layer: This layer plays a role in hosting the web servers responsible for delivering the static components of the application. Using EC2 instances, I deploy these web servers for optimal performance and scalability. To safeguard against unexpected traffic spikes or server failures, I employ an Auto Scaling Group, which dynamically adjusts the number of EC2 instances based on demand.

  - While the current project focuses on a static application, future flexibility is maintained in the architecture. If the need arises for dynamic data storage or retrieval, we have the capability to integrate a database seamlessly. Alternatively, we can employ Amazon Simple Storage Service (S3) to store static files securely. This adaptability ensures that the application can evolve to meet changing requirements without major architectural modifications.

## **Architecture**

<p align="center">
<img src="https://i.imgur.com/Z1o1RnL.png" height="50%" width="75%" alt="RDP event fail logs to iP Geographic information"/>
</p>

## **Replicating the Architecture: Deployment Steps**

1. Infrastructure Setup:

    - VPC Creation: Create a Virtual Private Cloud (VPC) with public and private subnets using AWS Management Console or AWS CLI. Ensure that you carefully plan your IP addressing scheme and route tables.
  
    - Internet Connectivity: Attach an Internet Gateway to the VPC to enable communication with the internet.
  
    - NAT Gateway: Set up a NAT Gateway in the public subnet to allow instances in the private subnet to access the internet securely.
      
  
2. Security Group Configuration:

    - ALB Security Group: Create a Security Group for the Application Load Balancer (ALB) to allow inbound HTTP and HTTPS traffic from 0.0.0.0/0.
  
    - Web Server Security Group: Configure a Security Group for your EC2 instances to allow inbound HTTP and HTTPS traffic from the ALB Security Group as welL SSH from "My IP".
  
3. DNS and SSL/TLS Certificate Setup:

    - Route 53 Configuration: Create a hosted zone on Route 53 to manage the DNS records for your application. Configure DNS records like A and CNAME records as needed.

    - SSL/TLS Certificate: Obtain an SSL/TLS certificate using AWS Certificate Manager for your domain or subdomain to enable secure HTTPS traffic. Ensure the certificate is associated with the ALB.
  
4. Application Load Balancer (ALB):

    - ALB Setup: Configure the Application Load Balancer (ALB) and associate it with the ALB Security Group. Ensure it's connected to the appropriate public subnets.

    - Listeners: Set up ALB listeners for HTTP (port 80) and HTTPS (port 443) to forward traffic to backend EC2 instances. Configure appropriate SSL policies for HTTPS.
  
5. Launch Template Creation:

    - Launch Template: Create a Launch Template specifying the required instance specifications for your EC2 instances. Include user data or user scripts for configuring instances upon launch.

      ```
      #!/bin/bash
      sudo su
      yum update -y
      yum install -y httpd
      cd /var/www/html
      wget https://github.com/Ahmednas211/jupiter-zip-file/raw/main/jupiter-main.zip
      unzip jupiter-main.zip
      cp -r jupiter-main/* /var/www/html
      rm -rf jupiter-main jupiter-main.zip
      systemctl start httpd
      systemctl enable httpd
      
      ```
  
6. Auto Scaling Group (ASG):

    - ASG Configuration: Create an Auto Scaling Group (ASG) using the Launch Template. Configure scaling policies, instance count, and health checks.
      
    - Target Group: Set up a Target Group and configure it as the target for the ASG. This ensures that traffic is distributed correctly among instances.
  
7. ALB Listener Configuration:

    - Listener Rules: Define rules in the ALB listener to route traffic based on hostnames, paths, or other criteria. Ensure that traffic is appropriately directed to backend instances.
  
8. DNS Configuration:

    - Route 53 Updates: Update DNS records in Route 53 to point to the ALB's DNS name. Implement health checks in Route 53 to monitor the availability of your application.

9. Testing and Monitoring:

    - Testing: After DNS records have propagated, thoroughly test your application using the domain name (HTTPS) to verify the deployment's success.
      
    - Monitoring: Implement cloud monitoring and logging solutions (e.g., AWS CloudWatch) to monitor the health and performance of your infrastructure and applications.
  
10. Backup and Disaster Recovery:

    - Implement backup and disaster recovery mechanisms to ensure data and application availability in case of failures.
   
11. Security and Compliance:

    - Follow AWS security best practices, implement IAM roles and policies, and consider compliance requirements specific to your application.
   
12. Documentation:

    - Maintain detailed documentation of the deployment process, infrastructure architecture, and any changes made over time.

## **A Screenshot of Landing Page Static Website**

<p align="center">
<img src="https://i.imgur.com/eUP7W7a.png" height="75%" width="95%" alt="RDP event fail logs to iP Geographic information"/>
</p>

<p align="center">
<img src="https://i.imgur.com/ZHRd67Q.png" height="75%" width="95%" alt="RDP event fail logs to iP Geographic information"/>
</p>

**Congratulations! You've Successfuly Hosted a Static Jupiter Website :)**
