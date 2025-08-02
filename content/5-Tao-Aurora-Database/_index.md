---
title: "Create Aurora Serverless MySQL"
date: 2024-12-19
weight: 5
---

## Overview

Amazon Aurora Serverless MySQL is a serverless database that automatically scales based on demand. In this chapter, we will create an Aurora Serverless MySQL cluster to replace the standard RDS MySQL.

## Why Choose Aurora Serverless?

- **Serverless**: Pay only when using
- **Auto Scaling**: Automatically scales based on load
- **Data API**: Can use Query Editor
- **MySQL Compatible**: Compatible with MySQL
- **High Availability**: Multi-AZ deployment

## Step 1: Create Aurora Serverless Cluster

### 1.1 Access RDS Console
1. Find and select **Aurora and RDS** service
2. Ensure you're in region **us-east-1** (or corresponding region from previous steps)
3. Click **Create database**
![](/images/005/01.png)

### 1.2 Configure Database
1. **Choose a database creation method**: Standard create
2. **Engine type**: Amazon Aurora
3. **Edition**: Aurora (MySQL-Compatible)
4. **Templates**: Dev/Test (for workshop development)
5. **DB cluster identifier**: `carrentalweb-aurora-cluster`
6. **Master username**: `adminws`
7. **Master password**: Create a strong password (e.g., `CarRental2024!`)

### 1.3 Configure Instance
1. **DB instance class**: Select **Serverless v2**
2. **Serverless configuration**:
   - **Minimum Aurora capacity unit (ACU)**: 0.5
   - **Maximum Aurora capacity unit (ACU)**: 1
3. **Pause after inactivity**: 5 minutes

### 1.4 Configure Connectivity
1. **Virtual private cloud (VPC)**: `carrentalweb-vpc`
2. **Subnet group**: Create new `carrentalweb-aurora-subnet-group`
3. **Publicly accessible**: Yes
4. **VPC security groups**: `carrentalweb-rds-sg`
5. **Availability Zone**: `us-east-1a`
6. **Database port**: 3306
7. **RDS Data API**: **Enable** (required to use Query Editor)

### 1.5 Configure Database Authentication
1. **IAM database authentication**: **Enable** (to use Query Editor)
2. **Kerberos authentication**: Not needed

### 1.6 Configure Additional
1. **Initial database name**: `carrentalweb`
2. **Backup retention period**: 7 days
3. **Enable encryption**: Yes
4. **Enable deletion protection**: No (for development)

### 1.7 Create Database
1. Click **Create database**
![](/images/005/02.png)
2. Wait for status to change to **Available**
![](/images/005/03.png)

## Step 2: Test Query Editor

### 2.1 Access Query Editor
1. **RDS Console** → **Query Editor**
2. Click **Create query editor**
3. **Database**: Select `carrentalweb-aurora-cluster`
4. **Database user**: `adminws`
5. **Database password**: [PASSWORD]
![](/images/005/04.png)
6. Click **Connect to database**
7. Confirm successful connection
![](/images/005/05.png)

### 2.2 Test Query
```sql
-- Select database
USE carrentalweb;

-- Create test table
CREATE TABLE test_table (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insert data
INSERT INTO test_table (name) VALUES ('Test Data');

-- Query data
SELECT * FROM test_table;
```
- Confirm success
![](/images/005/06.png)

## Step 3: Configure Environment Variables

### 3.1 Save Important Information
```
AURORA_CLUSTER_ENDPOINT: carrentalweb-aurora-cluster.cluster-xxxxx.us-east-1.rds.amazonaws.com
AURORA_WRITER_ENDPOINT: carrentalweb-aurora-cluster.cluster-ro-xxxxx.us-east-1.rds.amazonaws.com
DATABASE_NAME: carrentalweb
MASTER_USERNAME: adminws
MASTER_PASSWORD: [PASSWORD]
```

### 3.2 Configure for Elastic Beanstalk
Environment variables will be configured in chapter 6:
```
DB_HOST: [AURORA_CLUSTER_ENDPOINT]
DB_NAME: carrentalweb
DB_USERNAME: adminws
DB_PASSWORD: [PASSWORD]
```

## Important Notes

- **Serverless**: Database will pause after 5 minutes of inactivity
- **Data API**: Allows use of Query Editor
- **Auto Scaling**: Automatically scales from 0.5 to 1 ACU
- **Cost**: Pay only when using
- **Security**: Ensure Security Group only allows EB access

## Troubleshooting

### Common Issues
1. **Data API not available**: Ensure Data API is enabled
2. **Query Editor connection failed**: Check IAM role permissions
3. **Database pause**: Database will pause after 5 minutes, first query will take time
4. **ACU limits**: Increase maximum ACU if higher performance is needed

## Next Step

After creating Aurora Serverless MySQL, we will **[Deploy Elastic Beanstalk](../6-Deploy-Elastic-Beanstalk/)** to deploy the Spring Boot application to AWS. 
