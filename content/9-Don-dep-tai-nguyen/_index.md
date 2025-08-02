---
title: "Clean up Resources"
date: 2024-12-19
weight: 9
---

## Overview

After completing the workshop and testing the application, you should clean up AWS resources to avoid unnecessary costs. This chapter will guide you through deleting all created resources in a safe order.

## Why do we need to clean up?

- **Cost Savings**: Avoid unnecessary costs
- **Security**: Delete resources to avoid security risks
- **Management**: Keep AWS account clean
- **Best Practice**: Good habit when working with cloud

## Step 1: Delete Elastic Beanstalk Environment

### 1.1 Delete Environment
1. Go to **Elastic Beanstalk Console** → **Environments**
2. Select environment `carrentalweb-prod`
3. Click **Actions** → **Terminate environment**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/01.png)
4. Enter environment name and select **Terminate**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/02.png)
5. Confirm successful deletion (When "terminated" appears after the environment name)
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/03.png)

### 1.2 Delete Application
1. After the environment is deleted
2. Select application `carrentalweb-app`
3. Click **Actions** → **Delete application**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/04.png)
4. Enter application name to confirm and select **Delete**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/05.png)
5. Application name will disappear after successful deletion

## Step 2: Delete Aurora Serverless Cluster

### 2.1 Delete Database Cluster
1. Go to **RDS Console** → **Databases**
2. Select cluster `carrentalweb-aurora-cluster-instance-1`
3. Click **Actions** → **Delete**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/06.png)
4. Enter `delete me` and select **Delete**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/07.png)
5. Select cluster `carrentalweb-aurora-cluster`
6. Click **Actions** → **Delete**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/08.png)
7. Select as shown and choose **Delete DB Cluster**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/09.png)
8. Confirm successful deletion
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/10.png)

### 2.2 Delete Subnet Group
1. Go to **RDS Console** → **Subnet groups**
2. Select `carrentalweb-aurora-subnet-group`
3. Select **Delete**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/11.png)
5. Click **Delete** again. When successfully deleted, the subnet group name will disappear

## Step 3: Delete Security Groups

### 3.1 Delete RDS Security Group
1. Find `carrentalweb-rds-sg`
2. Select → **Actions** → **Delete security group**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/13.png)
3. Click **Delete**

### 3.2 Delete EB Security Group (similar to 3.1)
1. Go to **VPC Console** → **Security Groups**
2. Find `carrentalweb-eb-sg`
3. Select → **Actions** → **Delete security group**
4. Click **Delete**

## Step 4: Delete VPC

### 4.1 Delete NAT Gateway
1. Go to **VPC Console** → **NAT gateways**
2. Select `carrentalweb-nat-public1-us-east-1a`
3. Click **Actions** → **Delete NAT gateway**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/16.png)
4. Enter `delete` and Click **Delete**
5. Perform the same for the remaining NAT Gateway

### 4.2 Release Elastic IP
1. Go to **VPC Console** → **Elastic IPs**
2. Select the EIP being used
3. Click **Actions** → **Release Elastic IPs**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/15.png)
4. Click **Release**

### 4.3 Delete Internet Gateway
1. **Detach Internet Gateway:**
   - Go to **VPC Console** → **Internet gateways**
   - Select `carrentalweb-igw`
   - Click **Actions** → **Detach from VPC**
   ![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/14.png)
   - Click **Detach internet gateway**

2. **Delete Internet Gateway:**
   - Click **Actions** → **Delete internet gateway**
   ![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/17.png)
   - Enter `delete` and Click **Delete**

### 4.4 Delete VPC
1. **VPC Console** → **Your VPCs**
2. Select `carrentalweb-vpc`
3. Click **Actions** → **Delete VPC**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/12.png)
4. Enter `delete` and Click **Delete**
5. VPC and related configurations like Subnet, Route table will also be deleted
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/18.png)

## Step 5: Delete IAM Roles
1. Go to **IAM Console** → **Users**
2. Find `carrentalweb-cicd-user`
3. Select → **Delete**
4. Select **Deactivate access key**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/009/19.png)
4. Then enter `confirm` and Click **Delete user**

## Important Notes

### Important deletion order:
1. **Environment/Application** first
2. **Database** after
3. **Security Groups** after no dependencies
4. **VPC** last

### Backup before deletion:
- **Database snapshots**: Create snapshot if you need to backup data
- **Application code**: Ensure code has been committed to Git
- **Configuration**: Save important configurations

### Costs after deletion:
- **Aurora Serverless**: No more costs
- **EC2 instances**: No more costs
- **Load Balancer**: No more costs
- **Data transfer**: May still have some

## Troubleshooting

### Common Issues
1. **Cannot delete VPC**: Check if there are any resources left in VPC
2. **Cannot delete Security Group**: Check if any instances are using SG
3. **Cannot delete Subnet**: Check if any resources are in subnet
4. **Cannot delete IAM Role**: Check if any services are using the role

🎉 **Congratulations!** You have completed the workshop on deploying Spring Boot to AWS with Elastic Beanstalk, Aurora Serverless MySQL, and CI/CD.

### What you have learned:
- Create custom VPC with high security
- Set up Aurora Serverless MySQL with Data API
- Deploy Spring Boot application to Elastic Beanstalk
- Configure CI/CD pipeline with GitHub Actions
- Manage and clean up AWS resources

### Explore more:
- [AWS Elastic Beanstalk Documentation](https://docs.aws.amazon.com/elasticbeanstalk/)
- [Amazon Aurora Documentation](https://docs.aws.amazon.com/aurora/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions) 
