---
title: "Configure CI/CD"
date: 2024-12-19
weight: 8
---

## Overview

CI/CD (Continuous Integration/Continuous Deployment) helps automate the process of building, testing, and deploying applications. In this chapter, we will configure GitHub Actions to automatically deploy the Spring Boot application to Elastic Beanstalk.

## Why do we need CI/CD?

- **Automation**: Minimize manual errors
- **Speed**: Fast and safe deployment
- **Consistency**: Ensure consistent environment
- **Rollback**: Easily rollback to previous version
- **Monitoring**: Track deployment process

## Step 1: Create IAM User for CI/CD

### 1.1 Create IAM User
1. Find and select **IAM** service
![](/images/008/01.png)
2. Select **Users** and choose **Create user**
![](/images/008/02.png)
3. **User name**: `carrentalweb-cicd-user`
4. Click **Next**

### 1.2 Assign Permissions
1. Select **Attach policies directly**
2. Find and select:
   - `AdministratorAccess-AWSElasticBeanstalk`
   - `AmazonS3FullAccess`
   - `AmazonEC2FullAccess`
3. Click **Next**
![](/images/008/03.png)
5. Click **Create user**
![](/images/008/04.png)
6. Click on the created user name, select **Create access key**
![](/images/008/05.png)
7. Click select **Other**, then **Next**
![](/images/008/06.png)
8. Click select **Create access key**
![](/images/008/07.png)
9. Confirm successful creation
![](/images/008/08.png)

### 1.3 Save Important Information
```
Access Key: AKIA...
Secret Access Key: [SECRET_KEY]
```

## Step 2: Configure GitHub Secrets

### 2.1 Access Repository Settings
1. Go to GitHub repository
2. **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
![](/images/008/09.png)

### 2.2 Add Secrets
```
AWS_ACCESS_KEY_ID: [ACCESS_KEY_ID]
AWS_SECRET_ACCESS_KEY: [SECRET_ACCESS_KEY]
AWS_REGION: us-east-1
EB_APPLICATION_NAME: carrentalweb-app
EB_ENVIRONMENT_NAME: carrentalweb-prod
```
![](/images/008/10.png)

## Step 3: Create GitHub Actions Workflow

### 3.1 Create deploy.yml file
Create file `.github/workflows/deploy.yml`:

```yaml
name: Deploy to AWS Elastic Beanstalk

on:
  push:
    branches: [ main, master ]
  pull_request:
    branches: [ main, master ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main' || github.ref == 'refs/heads/master'
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Set up JDK 21
      uses: actions/setup-java@v4
      with:
        java-version: '21'
        distribution: 'temurin'
        
    - name: Cache Maven packages
      uses: actions/cache@v3
      with:
        path: ~/.m2
        key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
        restore-keys: ${{ runner.os }}-m2
        
    - name: Build with Maven
      run: mvn clean package -DskipTests
      
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v4
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ secrets.AWS_REGION }}
        
    - name: Show deployment info
      run: |
        echo "Deploying to AWS Elastic Beanstalk..."
        echo "Application: ${{ secrets.EB_APPLICATION_NAME }}"
        echo "Environment: ${{ secrets.EB_ENVIRONMENT_NAME }}"
        echo "Region: ${{ secrets.AWS_REGION }}"
        
    - name: Deploy to EB
      uses: einaregilsson/beanstalk-deploy@v21
      with:
        aws_access_key: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws_secret_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        application_name: ${{ secrets.EB_APPLICATION_NAME }}
        environment_name: ${{ secrets.EB_ENVIRONMENT_NAME }}
        region: ${{ secrets.AWS_REGION }}
        version_label: ${{ github.sha }}
        deployment_package: target/CarRentalWeb-1.0.0.jar
        wait_for_deployment: true
```

## Step 4: Test CI/CD Pipeline
1. Make a small change in the code (e.g., Change "Hi!" on the login page to "Quoc Kiet")
- Edit line 19 of file /resource/templates/login.html: change "Hi!" to "Quoc Kiet"
- Check the interface before pushing new code
![](/images/008/11.png)
2. Commit and push to GitHub:
```bash
git add .
git commit -m "Test CI/CD Deployment"
git push origin main
```
3. Go to **GitHub Repository** → **Actions** tab, monitor GitHub Actions:
![](/images/008/12.png)
4. Wait for status to turn green
![](/images/008/13.png)

### 4.3 Check deployment
1. Go to **Elastic Beanstalk Console**
2. Check environment `carrentalweb-prod`
3. **Events** tab → View deployment events
![](/images/008/14.png)
4. Access the application URL again to ensure changes have been deployed
![](/images/008/15.png)


### 4.4 Test rollback (if needed)
1. If deployment has issues:
   - Go to **Elastic Beanstalk Console**
   - **Application versions** tab
   - Select old version → **Deploy**
2. Or use GitHub Actions:
   - Go to **Actions** → **Deploy to AWS Elastic Beanstalk**
   - Click **Re-run jobs** with old version


## Important Notes

- **Security**: Don't commit AWS credentials to code
- **Branch Protection**: Protect main branch
- **Rollback**: Can rollback to previous version
- **Monitoring**: Monitor deployment logs
- **Cost**: CI/CD doesn't cost significantly

## Next Step

After configuring CI/CD, we will **[Clean up resources](../9-Don-dep-tai-nguyen/)** to delete AWS resources and avoid unnecessary costs. 