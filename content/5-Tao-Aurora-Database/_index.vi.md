---
title: "Tạo Aurora Serverless MySQL"
date: 2024-12-19
weight: 5
---

## Tổng quan

Amazon Aurora Serverless MySQL là một database serverless tự động scale theo nhu cầu. Trong chương này, chúng ta sẽ tạo Aurora Serverless MySQL cluster để thay thế RDS MySQL thông thường.

## Tại sao chọn Aurora Serverless?

- **Serverless**: Chỉ trả tiền khi sử dụng
- **Auto Scaling**: Tự động scale theo load
- **Data API**: Có thể dùng Query Editor
- **MySQL Compatible**: Tương thích với MySQL
- **High Availability**: Multi-AZ deployment

## Bước 1: Tạo Aurora Serverless Cluster

### 1.1 Đăng nhập RDS Console
1. Tìm và chọn service **Aurora and RDS**
2. Đảm bảo đang ở region **us-east-1** (hoặc region tương ứng ở các bước trước)
3. Click **Create database**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/005/01.png)

### 1.2 Cấu hình Database
1. **Choose a database creation method**: Standard create
2. **Engine type**: Amazon Aurora
3. **Edition**: Aurora (MySQL-Compatible)
4. **Templates**: Dev/Test (cho workshop development)
5. **DB cluster identifier**: `carrentalweb-aurora-cluster`
6. **Master username**: `adminws`
7. **Master password**: Tạo password mạnh (VD: `CarRental2024!`)

### 1.3 Cấu hình Instance
1. **DB instance class**: Chọn **Serverless v2**
2. **Serverless configuration**:
   - **Minimum Aurora capacity unit (ACU)**: 0.5
   - **Maximum Aurora capacity unit (ACU)**: 1
3. **Pause after inactivity**: 5 minutes

### 1.4 Cấu hình Connectivity
1. **Virtual private cloud (VPC)**: `carrentalweb-vpc`
2. **Subnet group**: Tạo mới `carrentalweb-aurora-subnet-group`
3. **Publicly accessible**: Yes
4. **VPC security groups**: `carrentalweb-rds-sg`
5. **Availability Zone**: `us-east-1a`
6. **Database port**: 3306
7. **RDS Data API**: **Enable** (bắt buộc để sử dụng Query Editor)

### 1.5 Cấu hình Database Authentication
1. **IAM database authentication**: **Enable** (để sử dụng Query Editor)
2. **Kerberos authentication**: Không cần

### 1.6 Cấu hình Additional
1. **Initial database name**: `carrentalweb`
2. **Backup retention period**: 7 days
3. **Enable encryption**: Yes
4. **Enable deletion protection**: No (cho development)

### 1.7 Tạo Database
1. Click **Create database**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/005/02.png)
2. Đợi status chuyển thành **Available**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/005/03.png)

## Bước 2: Test Query Editor

### 2.1 Truy cập Query Editor
1. **RDS Console** → **Query Editor**
2. Click **Create query editor**
3. **Database**: Chọn `carrentalweb-aurora-cluster`
4. **Database user**: `adminws`
5. **Database password**: [PASSWORD]
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/005/04.png)
6. Click **Connect to database**
7. Xác nhận kết nối thành công
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/005/05.png)

### 2.2 Test Query
```sql
-- Chọn database
USE carrentalweb;

-- Tạo bảng test
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
- Xác nhận thành công
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/005/06.png)

## Bước 3: Cấu hình Environment Variables

### 3.1 Lưu thông tin quan trọng
```
AURORA_CLUSTER_ENDPOINT: carrentalweb-aurora-cluster.cluster-xxxxx.us-east-1.rds.amazonaws.com
AURORA_WRITER_ENDPOINT: carrentalweb-aurora-cluster.cluster-ro-xxxxx.us-east-1.rds.amazonaws.com
DATABASE_NAME: carrentalweb
MASTER_USERNAME: adminws
MASTER_PASSWORD: [PASSWORD]
```

### 3.2 Cấu hình cho Elastic Beanstalk
Các biến môi trường sẽ được cấu hình trong chương 6:
```
DB_HOST: [AURORA_CLUSTER_ENDPOINT]
DB_NAME: carrentalweb
DB_USERNAME: adminws
DB_PASSWORD: [PASSWORD]
```

## Lưu ý quan trọng

- **Serverless**: Database sẽ pause sau 5 phút không sử dụng
- **Data API**: Cho phép sử dụng Query Editor
- **Auto Scaling**: Tự động scale từ 0.5 đến 1 ACU
- **Cost**: Chỉ trả tiền khi sử dụng
- **Security**: Đảm bảo Security Group chỉ cho phép EB access

## Troubleshooting

### Common Issues
1. **Data API not available**: Đảm bảo đã enable Data API
2. **Query Editor connection failed**: Kiểm tra IAM role permissions
3. **Database pause**: Database sẽ pause sau 5 phút, query đầu tiên sẽ mất thời gian
4. **ACU limits**: Tăng maximum ACU nếu cần performance cao hơn

## Bước tiếp theo

Sau khi tạo xong Aurora Serverless MySQL, chúng ta sẽ **[Deploy Elastic Beanstalk](../6-Deploy-Elastic-Beanstalk/)** để triển khai ứng dụng Spring Boot lên AWS. 
