# AWS Static Application Deployment using 3-Tier Architecture

This project aims to deploy a static application on AWS using a 3-tier architecture. The architecture includes a Virtual Private Cloud (VPC), Subnets, NAT Gateway, Internet Gateway, Route Table, Security Groups, Amazon Route 53 for DNS management, AWS Certificate Manager for SSL/TLS certificate, Application Load Balancer (ALB), EC2 instances, Auto Scaling Group, and Launch Template.

## Prerequisites

Before you begin, ensure you have the following:

1. An AWS account with appropriate permissions to create the required resources.
2. AWS CLI installed and configured on your local machine.
3. Basic knowledge of AWS services and command-line interface.

## Architecture Overview

The architecture follows a classic 3-tier setup, separating the application into three layers:

1. **Presentation Tier**: This layer handles user interactions and serves as the entry point for the application. It consists of an Application Load Balancer (ALB) that distributes incoming traffic across multiple web server instances.

2. **Application Tier**: This layer hosts the web servers responsible for serving the static application. EC2 instances are used to deploy the web servers. An Auto Scaling Group ensures high availability and scalability of the application.

3. **Data Tier**: As this project involves a static application, there is no dedicated data tier. However, if required, you can integrate a database or use AWS S3 for storing static files.

## Deployment Steps

Follow these steps to deploy the static application using the provided Bash script:

1. **Create the VPC, Subnets, Internet Gateway, and NAT Gateway**:
   - Use AWS Management Console or AWS CLI to create a VPC with public and private subnets.
   - Attach an Internet Gateway to the VPC to allow communication with the internet.
   - Set up a NAT Gateway in the public subnet to enable internet access for instances in the private subnet.

2. **Create Security Groups**:
   - Set up Security Groups for the ALB, EC2 instances, and any other necessary resources. For example:
     - ALB Security Group (Allow inbound HTTP/HTTPS from 0.0.0.0/0)
     - Web Server Security Group (Allow inbound HTTP/HTTPS from ALB Security Group)

3. **Configure Route 53 and Obtain SSL Certificate**:
   - Create a hosted zone on Route 53 to manage the DNS for your application.
   - Obtain an SSL/TLS certificate using AWS Certificate Manager for your domain/subdomain to enable HTTPS traffic.

4. **Create the Application Load Balancer (ALB)**:
   - Set up the Application Load Balancer (ALB) and associate it with the ALB Security Group and the public subnets.

5. **Create Launch Template**:
   - Create a Launch Template with the required specifications for your EC2 instances.

6. **Launch EC2 Instances using Auto Scaling Group**:
   - Use the Launch Template to create an Auto Scaling Group (ASG) that launches EC2 instances in the private subnet.
   - Configure the ASG to use the ALB as the target group for distributing traffic.

7. **Run the Bash Script on EC2 Instances**:
   - Connect to the EC2 instances launched by the Auto Scaling Group via SSH.
   - Copy the provided Bash script to the instances and execute it to install and configure the web servers.

8. **Configure ALB Listener**:
   - Set up an ALB Listener to listen on port 80 (HTTP) and port 443 (HTTPS) and forward traffic to the EC2 instances.

9. **Point DNS to ALB**:
   - Update the DNS records on Route 53 to point to the ALB's DNS name.

10. **Testing**:
   - Once the DNS records have propagated, access your application using the domain name (HTTPS) to ensure the deployment is successful.

## Additional Considerations

1. **Security**:
   - Implement secure practices, such as regularly updating packages and using secure passwords/SSH keys.
   - Restrict access to critical resources by modifying security group rules and network ACLs.

2. **Monitoring and Logging**:
   - Set up CloudWatch Alarms to monitor the health of your EC2 instances and ALB.
   - Configure centralized logging to track application and server logs.

3. **Scaling**:
   - Monitor your application's traffic and performance to determine if you need to adjust the Auto Scaling settings for your instances.

4. **Backup and Recovery**:
   - Consider implementing backup strategies for your static files, especially if your application allows user-generated content.

## Conclusion

Congratulations! You have successfully deployed your static application on AWS using a 3-tier architecture. Feel free to enhance the project further by integrating a database, CDN, or additional AWS services to meet your specific requirements. Always remember to monitor your resources and follow best practices for security and scalability. Happy coding!
