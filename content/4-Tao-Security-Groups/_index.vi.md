---
title: "Tạo Security Groups"
date: 2024-12-19
weight: 4
---

## Tổng quan

Security Groups hoạt động như firewall ảo, kiểm soát traffic vào/ra cho các tài nguyên AWS. Trong chương này, chúng ta sẽ tạo các Security Groups cần thiết cho ứng dụng.

## Các Security Groups cần tạo

1. **Elastic Beanstalk Security Group**: Cho phép HTTP/HTTPS từ internet
2. **RDS Security Group**: Cho phép MySQL từ Elastic Beanstalk

## Bước 1: Tạo Security Group cho Elastic Beanstalk

### 1.1 Tạo Security Group
1. Tìm và chọn service **VPC**
2. Vào **Security Groups**
3. Click **Create security group**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/004/01.png)
4. Điền thông tin:

    **Security group name:** `carrentalweb-eb-sg`  
    **Description:** `Security group for CarRentalWeb Elastic Beanstalk`  
    **VPC:** `carrentalweb-vpc`

### 1.2 Cấu hình Inbound Rules
1. Trong tab **Inbound rules**
2. Click **Add rule**
3. Thêm các rules sau:

| Type | Protocol | Port Range | Source | Description |
|------|----------|------------|--------|-------------|
| HTTP | TCP | 80 | 0.0.0.0/0 | Allow HTTP from internet |
| HTTPS | TCP | 443 | 0.0.0.0/0 | Allow HTTPS from internet |

### 1.3 Cấu hình Outbound Rules
1. Trong tab **Outbound rules**
2. Giữ nguyên rule mặc định.

### 1.4 Tạo Security Group
1. Click **Create security group**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/004/02.png)
2. Ghi lại **Security Group ID**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/004/03.png)

## Bước 2: Tạo Security Group cho RDS (tương tự bước 1)

### 2.1 Tạo Security Group
1. Click **Create security group**
2. Điền thông tin:

    **Security group name:** `carrentalweb-rds-sg`  
    **Description:** `Security group for CarRentalWeb RDS MySQL`  
    **VPC:** `carrentalweb-vpc`

### 2.2 Cấu hình Inbound Rules
1. Trong tab **Inbound rules**
2. Click **Add rule**
3. Thêm rule:

| Type | Protocol | Port Range | Source | Description |
|------|----------|------------|--------|-------------|
| MySQL/Aurora | TCP | 3306 | carrentalweb-eb-sg | Allow MySQL from EB |

### 2.3 Cấu hình Outbound Rules
1. Trong tab **Outbound rules**
2. Giữ nguyên rule mặc định.

### 2.4 Tạo Security Group
1. Click **Create security group**
2. Ghi lại **Security Group ID**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/004/04.png)

## Lưu ý quan trọng

- **Principle of Least Privilege**: Chỉ mở ports cần thiết
- **Security Group References**: Sử dụng Security Group ID thay vì IP
- **Testing**: Test connectivity trước khi deploy production

## Bước tiếp theo

Sau khi tạo xong Security Groups, chúng ta sẽ tạo **[Aurora Serverless MySQL](../5-Tao-Aurora-Database/)** để thiết lập database cho ứng dụng. 