---
title: "Chuẩn bị môi trường"
date: 2024-12-19
weight: 2
---

## Tổng quan

Chuẩn bị môi trường và source code trước khi triển khai lên AWS.

## Yêu cầu

- **AWS Account** - Có quyền truy cập EC2, RDS, Elastic Beanstalk, VPC, IAM
- **GitHub Account** - Để lưu trữ source code và CI/CD
- **Git** - Quản lý source code
- **Java 21** - Runtime cho Spring Boot
- **Maven** - Build tool

## Tải và cấu hình Source Code

### 1. Tải dự án
```bash
# Clone từ GitHub
git clone https://github.com/kietlqag/AWS_Workshop.git

# Hoặc tải ZIP: 
https://github.com/kietlqag/AWS_Workshop/archive/main.zip
```

### 2. Kiểm tra dự án
```bash
# Build project
mvn clean package -DskipTests

# Kiểm tra JAR file
ls -la target/CarRentalWeb-1.0.0.jar
```

### 3. Cấu trúc dự án quan trọng
```
CarRentalWeb/
├── pom.xml                    # Cấu hình Maven
├── Procfile                   # Khởi động Elastic Beanstalk
├── src/main/
│   ├── java/hcmute/edu/vn/CarRentalWeb/
│   │   ├── CarRentalWebApplication.java  # Lớp chính
│   │   ├── SecurityConfig.java           # Cấu hình bảo mật
│   │   ├── controller/                   # REST Controllers
│   │   ├── service/                      # Logic nghiệp vụ
│   │   ├── repository/                   # Truy cập dữ liệu
│   │   ├── entity/                       # JPA Entities
│   │   ├── dto/                          # Data Transfer Objects
│   │   ├── strategy/                     # Design Patterns
│   │   ├── observer/                     # Design Patterns
│   │   ├── singleton/                    # Design Patterns
│   │   └── decorator/                    # Design Patterns
│   └── resources/
│       ├── application.properties        # Cấu hình DB & Email
│       ├── static/                       # CSS, JS, Hình ảnh
│       └── templates/                    # Thymeleaf templates
├── .ebextensions/                        # Cấu hình Elastic Beanstalk
│   ├── 01_environment.config             # Biến môi trường
│   ├── 02_vpc.config                     # Cấu hình VPC
│   ├── 03_port.config                    # Cài đặt port
│   ├── 04_nginx.config                   # Cấu hình Nginx
│   ├── 05_force_port.config              # Ép port 5000
│   ├── 06_selinux.config                 # Cài đặt SELinux
│   └── nginx/conf.d/
│       └── proxy.conf                    # Cấu hình proxy tùy chỉnh
└── .github/workflows/                    # CI/CD workflows
    └── deploy.yml                        # GitHub Actions
```

## Cấu hình GitHub

### 1. Tạo Repository
1. Tạo repository mới
2. Push source code lên GitHub

### 2. Kiểm tra
```bash
git --version
java --version  # Java 21
mvn --version
```

## Lưu ý

- **Java 21**: Đảm bảo sử dụng đúng version
- **Maven**: Cần để build project
- **Git**: Để quản lý source code và CI/CD

## Bước tiếp theo

Khi đã chuẩn bị xong môi trường và source code, chúng ta sẽ bắt đầu tạo **[VPC tùy chỉnh](../3-Tao-VPC-tuy-chinh/)** để xây dựng network infrastructure cho ứng dụng. 