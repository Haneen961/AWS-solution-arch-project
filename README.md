# AWS-solution-arch-project

link : https://lucid.app/lucidchart/992fdc15-955f-4ec3-9b26-e06cfe1a582e/edit?viewport_loc=429%2C218%2C2466%2C1118%2C0_0&invitationId=inv_c3a0b878-4fec-4806-9ae1-ac99e90b2771 

# Project 1: Scalable Web Application with ALB and Auto Scaling

## Table of Contents
- [1- proposed model](#1- proposed model)
- [Architecture](#architecture)
- [Installation](#installation)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)
  
## 1- proposed model  
The proposed model is a highly available (using 2 avalability zones) and scalable 3-tier web application hosted on AWS. 
![AWS solution arch project](https://github.com/user-attachments/assets/1ee08746-d908-494f-9a32-97eb10daa82d)

2- Main AWS Services to build the system :

Amazon EC2: Hosts web and application servers in Auto Scaling groups.

Application Load Balancer (ALB): Distributes traffic across multiple EC2 instances in the Web and App tiers.

Amazon RDS: Provides a managed relational database with high availability using Multi-AZ deployment.

Amazon VPC: Creates isolated network environments.

Amazon CloudWatch: Monitors resources and applications.

Amazon SNS: Sends notifications in case of events or failures.

NAT Gateway: Allows outbound internet access from private subnets.

Internet Gateway: Enables communication between instances in the public subnet and the internet.

3- security best practices (IAM roles and security groups)

IAM Roles: Applied to EC2 instances and services to grant least-privilege access to AWS resources (e.g., RDS, S3).

Security Groups:

Web Tier SG: Allows HTTP/HTTPS traffic from the internet, and only allows traffic to the App tier.

App Tier SG: Accepts traffic from the Web tier only.

DB Tier SG: Restricts access to the App tier and allows only necessary ports (e.g., 3306 for MySQL).

Private Subnets: Application and database servers are hosted in private subnets to prevent direct internet access.

4- Project architecture : 

The system is deployed in two Availability Zones to ensure high availability. Each AZ contains:

One public subnet for the Web tier (EC2 instances behind an ALB).

One public subnet for NAT Gateway.

One private subnet for the App tier (EC2 instances behind a second ALB).

One private subnet for the DB tier (Amazon RDS Multi-AZ deployment).

Architecture is organized into:

NAT Gateway : to allow private subnets (App & DB tiers) to access the internet (for updates, patches, etc.)

Web Tier: Handles client requests.

App Tier: Business logic and processing.

DB Tier: Persistent data storage using Amazon RDS.

5- How the system operate : 

  1- Users access the application through the Application Load Balancer in the Web tier.

  2- Traffic is routed to EC2 instances in Auto Scaling Groups deployed across public subnets in different AZs.

  3- The Web tier forwards requests to the App tier through a second ALB, which balances the load among backend EC2 instances.

  4- The App tier communicates with the Amazon RDS database hosted in the DB tier (private subnet).

  5- Amazon CloudWatch monitors system health and performance.

  6- If anomalies or failures occur, Amazon SNS sends alerts to administrators.

  7- Auto Scaling Groups ensure that instances are scaled in or out depending on traffic load, maintaining availability and cost efficiency.
