---
title: "Environment Preparation"
date: 2024-12-19
weight: 2
---

## Overview

Prepare environment and source code before deploying to AWS.

## Requirements

- **AWS Account** - Access to EC2, RDS, Elastic Beanstalk, VPC, IAM
- **GitHub Account** - For source code storage and CI/CD
- **Git** - Source code management
- **Java 21** - Runtime for Spring Boot
- **Maven** - Build tool

## Download and Configure Source Code

### 1. Download Project
```bash
# Clone from GitHub
git clone https://github.com/kietlqag/AWS_Workshop.git

# Or download ZIP: 
https://github.com/kietlqag/AWS_Workshop/archive/main.zip
```

### 2. Verify Project
```bash
# Build project
mvn clean package -DskipTests

# Check JAR file
ls -la target/CarRentalWeb-1.0.0.jar
```

### 3. Important Project Structure
```
CarRentalWeb/
├── pom.xml                    # Maven configuration
├── Procfile                   # Elastic Beanstalk startup
├── src/main/
│   ├── java/hcmute/edu/vn/CarRentalWeb/
│   │   ├── CarRentalWebApplication.java  # Main class
│   │   ├── SecurityConfig.java           # Security configuration
│   │   ├── controller/                   # REST Controllers
│   │   ├── service/                      # Business Logic
│   │   ├── repository/                   # Data Access
│   │   ├── entity/                       # JPA Entities
│   │   ├── dto/                          # Data Transfer Objects
│   │   ├── strategy/                     # Design Patterns
│   │   ├── observer/                     # Design Patterns
│   │   ├── singleton/                    # Design Patterns
│   │   └── decorator/                    # Design Patterns
│   └── resources/
│       ├── application.properties        # DB & Email config
│       ├── static/                       # CSS, JS, Images
│       └── templates/                    # Thymeleaf templates
├── .ebextensions/                        # Elastic Beanstalk config
│   ├── 01_environment.config             # Environment variables
│   ├── 02_vpc.config                     # VPC configuration
│   ├── 03_port.config                    # Port settings
│   ├── 04_nginx.config                   # Nginx configuration
│   ├── 05_force_port.config              # Force port 5000
│   ├── 06_selinux.config                 # SELinux settings
│   └── nginx/conf.d/
│       └── proxy.conf                    # Custom proxy config
└── .github/workflows/                    # CI/CD workflows
    └── deploy.yml                        # GitHub Actions
```

## GitHub Configuration

### 1. Create Repository
1. Create new repository
2. Push source code to GitHub

### 2. Verify
```bash
git --version
java --version  # Java 21
mvn --version
```

## Notes

- **Java 21**: Ensure correct version
- **Maven**: Required for project build
- **Git**: For source code management and CI/CD

## Next Steps

When environment and source code are ready, we will start creating **[Custom VPC](../3-Tao-VPC-tuy-chinh/)** to build network infrastructure for the application. 