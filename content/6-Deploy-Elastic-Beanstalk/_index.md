---
title: "Deploy Elastic Beanstalk"
date: 2024-12-19
weight: 6
---

## Overview

AWS Elastic Beanstalk is a Platform as a Service (PaaS) that helps deploy web applications easily. In this chapter, we will deploy the Spring Boot application to Elastic Beanstalk.

## Why Choose Elastic Beanstalk?

- **Managed Platform**: AWS manages the infrastructure
- **Auto Scaling**: Automatically scales based on load
- **Load Balancing**: Health checks and traffic distribution
- **Easy Deployment**: Deploy from source code or JAR file
- **Monitoring**: CloudWatch integration

## Step 1: Prepare the Application

### 1.1 Build the application
```bash
# Clean and build
mvn clean package -DskipTests

# Check JAR file
ls -la target/CarRentalWeb-1.0.0.jar
```

## Step 2: Create Elastic Beanstalk Environment

### 2.1 Access EB Console
1. Find and select **Elastic Beanstalk** service
![](/images/006/01.png)
2. Ensure you're in **us-east-1** region (or the region used in previous steps)
3. Click **Create application**
![](/images/006/02.png)

### 2.2 Configure Application and Environment
1. In **Step 1: Configure environment**:
   - **Environment tier**: Select **Web server environment**
   - **Application name**: `carrentalweb-app`
   - **Environment name**: `carrentalweb-prod`
   - **Domain**: `carrentalweb-prod` (or leave empty)
   - **Environment description**: `Production environment for CarRentalWeb`
   - **Platform**: Java
   - **Platform branch**: Java 21 (Corretto)
   - **Platform version**: 4.6.2 (Recommended)
   - **Application code**: Select **Upload your code**
   - **Version label**: `1.0.0`
   - Select **Local file**
   - Upload JAR file: `CarRentalWeb-1.0.0.jar`
![](/images/006/03.png)
![](/images/006/04.png)

### 2.3 Configure Service Access
1. In **Step 2: Configure service access**:
   - **Service role**: 
     - If exists: `aws-elasticbeanstalk-service-role`
     - If not: Click **Create and use new service role** → Auto-create
   - **EC2 instance profile**: 
     - If exists: `aws-elasticbeanstalk-ec2-role`
     - If not: Click **Create and use new instance profile** → Auto-create
   - **EC2 key pair**: None (for development)
2. Click **Next**
![](/images/006/05.png)

### 2.4 Configure Networking
1. In **Step 3: Set up networking, database, and tags**:
   - **VPC**: `carrentalweb-vpc`
   - **Public IP**: Enabled
   - **Instance subnets**: `carrentalweb-vpc-public-subnet-1`, `carrentalweb-vpc-public-subnet-2`
   - **Load balancer subnets**: `carrentalweb-vpc-public-subnet-1`, `carrentalweb-vpc-public-subnet-2`
   - **Security groups**: `carrentalweb-eb-sg`
2. Click **Next**
![](/images/006/06.png)

### 2.5 Configure Environment Variables
1. In **Step 4: Configure updates, monitoring, and logging**:
   - **Environment properties**: Add variables with keys saved from previous steps
     ```
     DB_HOST: carrentalweb-aurora-cluster.cluster-xxxxxxxxx.us-east-1.rds.amazonaws.com
     DB_PORT: 3306
     DB_NAME: carrentalweb
     DB_USERNAME: adminws
     DB_PASSWORD: [AURORA_PASSWORD]
     MAIL_HOST: smtp.gmail.com
     MAIL_PORT: 587
     MAIL_USERNAME: [EMAIL]
     MAIL_PASSWORD: [EMAIL_PASSWORD]
     DDL_AUTO: update
     ```
2. Click **Next**
![](/images/006/07.png)

### 2.6 Deploy Application
1. After completing all Steps, click **Create**
![](/images/006/08.png)
2. Monitor deployment process
![](/images/006/09.png)
3. Wait for Health to turn **OK**
![](/images/006/10.png)

## Step 3: Test the Application

### 3.1 Test Application
1. Open browser
2. Visit: `http://[EB_URL]`. If you see the interface below, the project has been deployed successfully:
![](/images/006/001.png)
3. Check application functionality
- Add `/login, /register, /home` to the URL to test interfaces
![](/images/006/002.png)
![](/images/006/003.png)
![](/images/006/004.png)

**💡 Common Issue**: Elastic Beanstalk automatically detects wrong port 8080 instead of 5000

## Important Notes

- **Health Checks**: Ensure `/health` endpoint works
- **Database Connection**: Test database connectivity
- **Environment Variables**: Don't commit sensitive data

## Next Steps

After successfully deploying Elastic Beanstalk, we will **[Test the Application](../7-Test-ung-dung/)** to ensure everything works properly. 