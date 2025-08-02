---
title: "Create Security Groups"
date: 2024-12-19
weight: 4
---

## Overview

Security Groups act as virtual firewalls, controlling inbound/outbound traffic for AWS resources. In this chapter, we will create the necessary Security Groups for the application.

## Security Groups to Create

1. **Elastic Beanstalk Security Group**: Allow HTTP/HTTPS from internet
2. **RDS Security Group**: Allow MySQL from Elastic Beanstalk

## Step 1: Create Security Group for Elastic Beanstalk

### 1.1 Create Security Group
1. Find and select **VPC** service
2. Go to **Security Groups**
3. Click **Create security group**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/004/01.png)
4. Fill in information:

    **Security group name:** `carrentalweb-eb-sg`  
    **Description:** `Security group for CarRentalWeb Elastic Beanstalk`  
    **VPC:** `carrentalweb-vpc`

### 1.2 Configure Inbound Rules
1. In **Inbound rules** tab
2. Click **Add rule**
3. Add the following rules:

| Type | Protocol | Port Range | Source | Description |
|------|----------|------------|--------|-------------|
| HTTP | TCP | 80 | 0.0.0.0/0 | Allow HTTP from internet |
| HTTPS | TCP | 443 | 0.0.0.0/0 | Allow HTTPS from internet |

### 1.3 Configure Outbound Rules
1. In **Outbound rules** tab
2. Keep default rule.

### 1.4 Create Security Group
1. Click **Create security group**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/004/02.png)
2. Note down **Security Group ID**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/004/03.png)

## Step 2: Create Security Group for RDS (similar to step 1)

### 2.1 Create Security Group
1. Click **Create security group**
2. Fill in information:

    **Security group name:** `carrentalweb-rds-sg`  
    **Description:** `Security group for CarRentalWeb RDS MySQL`  
    **VPC:** `carrentalweb-vpc`

### 2.2 Configure Inbound Rules
1. In **Inbound rules** tab
2. Click **Add rule**
3. Add rule:

| Type | Protocol | Port Range | Source | Description |
|------|----------|------------|--------|-------------|
| MySQL/Aurora | TCP | 3306 | carrentalweb-eb-sg | Allow MySQL from EB |

### 2.3 Configure Outbound Rules
1. In **Outbound rules** tab
2. Keep default rule.

### 2.4 Create Security Group
1. Click **Create security group**
2. Note down **Security Group ID**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/004/04.png)

## Important Notes

- **Principle of Least Privilege**: Only open necessary ports
- **Security Group References**: Use Security Group ID instead of IP
- **Testing**: Test connectivity before production deployment

## Next Steps

After creating Security Groups, we will create **[Aurora Serverless MySQL](../5-Tao-Aurora-Database/)** to set up the database for the application. 