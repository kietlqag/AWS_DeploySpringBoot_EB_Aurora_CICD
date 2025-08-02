---
title: "Introduction"
date: 2024-12-19
weight: 1
---

# Deploy Spring Boot to AWS with Elastic Beanstalk, Aurora Serverless MySQL and CI/CD

## Overview

This workshop will guide you through deploying a Spring Boot application to AWS using Elastic Beanstalk, connecting to Aurora Serverless MySQL, and setting up CI/CD pipeline with GitHub Actions.

## Implementation Steps

{{% children description="true" /%}}

1. **[Environment Preparation](../2-Chuan-bi-moi-truong/)** - Install tools and configure AWS
2. **[Create Custom VPC](../3-Tao-VPC-tuy-chinh/)** - Build network infrastructure
3. **[Create Security Groups](../4-Tao-Security-Groups/)** - Configure network security
4. **[Create Aurora Serverless MySQL](../5-Tao-Aurora-Database/)** - Set up Aurora database
5. **[Deploy Elastic Beanstalk](../6-Deploy-Elastic-Beanstalk/)** - Deploy Spring Boot application
6. **[Configure CI/CD](../7-Cau-hinh-CI-CD/)** - Automate deployment with GitHub Actions
7. **[Clean Up Resources](../8-Don-dep-tai-nguyen/)** - Delete AWS resources

## Expected Results

After completing the workshop, we will have:

- Spring Boot application running on AWS Elastic Beanstalk
- Aurora Serverless MySQL database with Data API
- Custom VPC with high security
- Automated CI/CD pipeline with GitHub Actions
- Query Editor for database management

## Prerequisites

- AWS Account
- GitHub Account (for CI/CD)
- Basic knowledge of Spring Boot
- Basic understanding of AWS services 