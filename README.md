# AWS PaaS and SaaS Setup - Automated, Scalable, and Flexible

![image](https://github.com/user-attachments/assets/8cef1182-da27-4e87-9474-3558d4e0dcd8)

## Why bother with PaaS and SaaS?
- **Automated**
- **PayAsYouGo**
- **Flexible**
- **Great for business continuity and boosting agility**
- **Ease of infrastructure management**

## Problems with IaaS:
- Operational overhead
- Struggling with uptime and scaling
- Upfront capital expenditure and regular operational expenditure
- Manual processes/difficult to automate

## AWS Services Involved:
1. **Beanstalk**  
    - VM for Tomcat (app server)
    - Nginx Load Balancer
    - Autoscaling group for VM scaling
    - S3/EFS for storage (can use own S3 bucket)
  
2. **RDS Instance**  
   - For managing Databases
  
3. **Elasticache**  
   - (Instead of memcached)
  
4. **ActiveMQ**  
   - (Instead of RabbitMQ)
  
5. **Route 53**  
   - For DNS management
  
6. **CloudFront**  
   - Content Delivery Network (Great for global audiences)

## Creating Security Group and Key Pairs
Self-referencing Security Group to allow communication within itself.

## Creating RDS Instance
- Look up RDS in the same region and create a DB instance.
- **Parameter Group for RDS:**  
  RDS doesn’t allow SSH login, so any DB parameters must be modified via a parameter group.
  
  **Parameter Group Settings:**
  - Adjust settings as needed for your DB instance.

- **Subnet Groups:**  
  Subnet groups are used for hosting the DB in different subnets for security.

- **Master Username:**  
  `[set master username]`  
  **Password:**  
  `[will have to be either specified or downloaded from the credential notification once the instance is created]`

## Elasticache
- Parameter groups and subnet groups are the same as RDS.

  **Settings:**
  - Port number: `11211`

  After creating the subnet group (selecting pre-created VPC or default), choose **only one** subnet group from all AZs in the region.

## Creating RabbitMQ (ActiveMQ)
Instead of running an EC2 server and running RabbitMQ on top of it, use AWS MQ to consume ActiveMQ and RabbitMQ services.

**Set Credentials for AWS MQ**


Logs are not enabled for this project.

## DB Initialization
- The RDS instance is a private instance, so it can’t be accessed publicly.
- To access the DB, create an EC2 instance within the same VPC.

Steps:
1. **Create EC2 Instance**  
   - Login to the EC2 instance.

2. **Install MySQL Client and Git**  
   ```bash
   apt update && apt install mysql-client git -y

3.**DB Details**

**RDS Endpoint:**  
`copy the RDS endpoint here`

## Allow EC2-SG Communication to RDS-SG:

1. Copy EC2 instance SG ID.
2. Add it as a source in the RDS SG.

---

## Connect to DB via MySQL:

```bash
mysql -h [RDS endpoint] -u [username] -p [password]
```
## Clone parent Repo:
```bash
Copy
Edit
git clone [[repository link](https://github.com/hkhcoder/vprofile-project/tree/awsrefactor)]
```
## Run DB Initialization Command:
```bash
mysql -h [RDS endpoint] -u [username] -p[password] < src/main/resources/db_backup.sql
```
## Run Again to Apply SQL:
```bash
mysql -h [RDS endpoint] -u [username] -p[password] accounts
```
The DB has been initialized! Now, delete the EC2 instance used for initialization.

# Setting Up Elastic Beanstalk

Before diving into Beanstalk, set up necessary IAM roles and permissions.

## IAM Role Creation:
1. Create a role with four policies.
2. Select the role in the next step.

---

## Beanstalk Settings:
- Choose **gp3** for storage (default container is outdated).
- Tomcat runs on port **8080** by default; in Beanstalk, it runs on port **80**.
- Enable **Stickiness** in load balancer settings for session persistence.

---

## Deployment Strategies:
- **Rolling**: Choose the number of instances or percentage of them to upgrade.
---

## Common Issues:

### Health Check Pending:
This issue arises when there’s a misconfiguration with the **Application Load Balancer (ALB)**. Ensure that each subnet in your VPC is in a different Availability Zone (AZ) when attaching the ALB.

## PSA 
This repository focuses on the infrastructure of the vprofile project. For the original code, features, and full details of the website, please check out the official repository here: [hkhcoder](https://github.com/hkhcoder/vprofile-project/tree/awsliftandshift)

