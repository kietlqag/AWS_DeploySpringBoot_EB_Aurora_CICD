---
title: "Create VPC"
date: 2024-12-19
weight: 3
---

## Overview

In this chapter, we will create a custom VPC (Virtual Private Cloud) instead of using AWS default VPC. This gives us better control over security and network architecture.

## Why Custom VPC?

- **Higher Security**: Precise control over inbound/outbound traffic
- **Network Isolation**: Separate resources
- **IP Management**: Define custom IP ranges
- **Clear Architecture**: Separate public/private subnets

## VPC Architecture

```
VPC (10.0.0.0/16)
├── Public Subnet 1 - AZ a
├── Public Subnet 2 - AZ b
├── Private Subnet 1 - AZ a
├── Private Subnet 2 - AZ b
├── Internet Gateway
├── NAT Gateway
└── Route Tables
```

## Step 1: Create VPC

### 1.1 Login to AWS Console
1. Open AWS Console
2. Login with account that has VPC creation permissions
3. Select region: **us-east-1** (Virginia) (or other region as preferred)

### 1.2 Create New VPC
1. Find and select **VPC** service
![VPC Service](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/003/01.png)
2. Click **Create VPC**
![Create VPC](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/003/02.png)
3. Select **VPC and more** (to automatically create basic components)
![VPC and more](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/003/03.png)
4. Configure VPC information:

    **Name tag:** `carrentalweb-vpc`  
    **IPv4 CIDR block:** `10.0.0.0/16`  
    **IPv6 CIDR block:** `No IPv6 CIDR block`  
    **Tenancy:** `Default`  
    **Number of Availability Zones:** `2`  
    **Number of public subnets:** `2`  
    **Number of private subnets:** `2`  
    **NAT gateways:** `1 per AZ`  
    **VPC endpoints:** `None`

5. Click **Create VPC**
![Create VPC Button](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/003/04.png)

### 1.3 Verify VPC Creation
1. Go to **Your VPCs**
2. Confirm VPC `carrentalweb-vpc` has been created
3. Note down **VPC ID**
![VPC List](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/003/05.png)

## Step 2: Verify Components

### 2.1 Subnets
1. Go to **Subnets**
2. Confirm there are 4 subnets:
   - 2 public: `carrentalweb-vpc-public-subnet-1`, `carrentalweb-vpc-public-subnet-2`
   - 2 private: `carrentalweb-vpc-private-subnet-1`, `carrentalweb-vpc-private-subnet-2`
3. Note down **Subnet IDs**
![Subnets List](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/003/06.png)

### 2.2 Internet Gateway
1. Go to **Internet Gateways**
2. Confirm IGW is attached to VPC `carrentalweb-vpc`
![Internet Gateway](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/003/07.png)

### 2.3 NAT Gateway
1. Go to **NAT Gateways**
2. Confirm status is **Available**
3. Note down **NAT Gateway ID**
![NAT Gateway](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/003/08.png)

### 2.4 Route Tables
1. Go to **Route Tables**
2. Confirm there are 2 route tables:
   - Public: `0.0.0.0/0` → Internet Gateway
   ![Public Route Table](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/003/09.png)
   - Private: `0.0.0.0/0` → NAT Gateway
   ![Private Route Table](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/003/10.png)

## Step 3: Create DB Subnet Group

### 3.1 Create DB Subnet Group for RDS
1. Find and select **Aurora and RDS** service
![Aurora RDS Service](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/003/11.png)
2. Go to **Subnet groups**
3. Click **Create DB subnet group**
![Create DB Subnet Group](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/003/12.png)
4. Fill in information:

    **Name:** `carrentalweb-db-subnet-group`  
    **Description:** `Subnet group for CarRentalWeb RDS`  
    **VPC:** `carrentalweb-vpc`  
    **Availability Zones:** `us-east-1a, us-east-1b`  
    **Subnets:** select 2 private subnets

5. Click **Create**
![Create Button](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/003/13.png)
6. Confirm successful creation
![DB Subnet Group Created](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/003/14.png)

## Important Notes

- **Cost**: NAT Gateway has hourly and data transfer fees
- **Security**: Private subnets cannot be accessed directly from internet
- **High Availability**: Using 2 AZs ensures availability
- **Backup**: Store VPC information for future reference

## Next Steps

After creating the custom VPC, we will create **[Security Groups](../4-Tao-Security-Groups/)** to configure network security for services. 