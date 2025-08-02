---
title: "Deploy Elastic Beanstalk"
date: 2024-12-19
weight: 6
---

## Tổng quan

AWS Elastic Beanstalk là Platform as a Service (PaaS) giúp triển khai ứng dụng web một cách dễ dàng. Trong chương này, chúng ta sẽ deploy ứng dụng Spring Boot lên Elastic Beanstalk.

## Tại sao chọn Elastic Beanstalk?

- **Managed Platform**: AWS quản lý infrastructure
- **Auto Scaling**: Tự động scale theo load
- **Load Balancing**: Health checks và traffic distribution
- **Easy Deployment**: Deploy từ source code hoặc JAR file
- **Monitoring**: CloudWatch integration

## Bước 1: Chuẩn bị ứng dụng

### 1.1 Build ứng dụng
```bash
# Clean và build
mvn clean package -DskipTests

# Kiểm tra JAR file
ls -la target/CarRentalWeb-1.0.0.jar
```

## Bước 2: Tạo Elastic Beanstalk Environment

### 2.1 Đăng nhập EB Console
1. Tìm và chọn service **Elastic Beanstalk**
![](/images/006/01.png)
2. Đảm bảo đang ở region **us-east-1** (hoặc region tương ứng ở các bước trước)
3. Click **Create application**
![](/images/006/02.png)

### 2.2 Cấu hình Application và Environment
1. Trong **Step 1: Configure environment**:
   - **Environment tier**: Chọn **Web server environment**
   - **Application name**: `carrentalweb-app`
   - **Environment name**: `carrentalweb-prod`
   - **Domain**: `carrentalweb-prod` (hoặc để trống)
   - **Environment description**: `Production environment for CarRentalWeb`
   - **Platform**: Java
   - **Platform branch**: Java 21 (Corretto)
   - **Platform version**: 4.6.2 (Recommended)
   - **Application code**: Chọn **Upload your code**
   - **Version label**: `1.0.0`
   - Chọn **Local file**
   - Upload file JAR: `CarRentalWeb-1.0.0.jar`
![](/images/006/03.png)
![](/images/006/04.png)

### 2.3 Cấu hình Service Access
1. Trong **Step 2: Configure service access**:
   - **Service role**: 
     - Nếu có: `aws-elasticbeanstalk-service-role`
     - Nếu không có: Click **Create and use new service role** → Tự động tạo
   - **EC2 instance profile**: 
     - Nếu có: `aws-elasticbeanstalk-ec2-role`
     - Nếu không có: Click **Create and use new instance profile** → Tự động tạo
   - **EC2 key pair**: None (cho development)
2. Click **Next**
![](/images/006/05.png)

### 2.4 Cấu hình Networking
1. Trong **Step 3: Set up networking, database, and tags**:
   - **VPC**: `carrentalweb-vpc`
   - **Public IP**: Enabled
   - **Instance subnets**: `carrentalweb-vpc-public-subnet-1`, `carrentalweb-vpc-public-subnet-2`
   - **Load balancer subnets**: `carrentalweb-vpc-public-subnet-1`, `carrentalweb-vpc-public-subnet-2`
   - **Security groups**: `carrentalweb-eb-sg`
2. Click **Next**
![](/images/006/06.png)

### 2.5 Cấu hình Environment Variables
1. Trong **Step 4: Configure updates, monitoring, and logging**:
   - **Environment properties**: Thêm các biến với key là các giá trị đã lưu ở các bước trước
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
1. Sau khi hoàn thành tất cả các Step, click **Create**
![](/images/006/08.png)
2. Theo dõi quá trình deployment
![](/images/006/09.png)
3. Đợi Health chuyển thành **OK**
![](/images/006/10.png)

## Bước 3: Kiểm tra ứng dụng

### 3.1 Test Application
1. Mở browser
2. Truy cập: `http://[EB_URL]`. Nếu hiện lên giao diện như sau thì đã deploy dự án thành công:
![](/images/006/001.png)
3. Kiểm tra ứng dụng hoạt động
- Thêm `/login, /register, /home` vào sau đường dẫn để test giao diện
![](/images/006/002.png)
![](/images/006/003.png)
![](/images/006/004.png)

**💡 Lỗi thường gặp**: Elastic Beanstalk tự động detect sai port 8080 thay vì 5000

## Lưu ý quan trọng

- **Health Checks**: Đảm bảo `/health` endpoint hoạt động
- **Database Connection**: Test kết nối database
- **Environment Variables**: Không commit sensitive data

## Bước tiếp theo

Sau khi deploy thành công Elastic Beanstalk, chúng ta sẽ **[Test ứng dụng](../7-Test-ung-dung/)** để đảm bảo mọi thứ hoạt động bình thường. 
