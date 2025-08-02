---
title: "Dọn dẹp tài nguyên"
date: 2024-12-19
weight: 9
---

## Tổng quan

Sau khi hoàn thành workshop và test ứng dụng, bạn nên dọn dẹp các tài nguyên AWS để tránh phát sinh chi phí không cần thiết. Chương này sẽ hướng dẫn bạn xóa tất cả tài nguyên đã tạo theo thứ tự an toàn.

## Tại sao cần dọn dẹp?

- **Tiết kiệm chi phí**: Tránh phát sinh chi phí không cần thiết
- **Bảo mật**: Xóa tài nguyên để tránh rủi ro bảo mật
- **Quản lý**: Giữ AWS account sạch sẽ
- **Best Practice**: Thói quen tốt khi làm việc với cloud

## Bước 1: Xóa Elastic Beanstalk Environment

### 1.1 Xóa Environment
1. Vào **Elastic Beanstalk Console** → **Environments**
2. Chọn environment `carrentalweb-prod`
3. Click **Actions** → **Terminate environment**
![](/images/009/01.png)
4. Nhập tên environment và chọn **Terminate**
![](/images/009/02.png)
5. Xác nhận xoá thành công (Khi phía sau tên environment có chữ **terminated**)
![](/images/009/03.png)

### 1.2 Xóa Application
1. Sau khi environment bị xóa
2. Chọn application `carrentalweb-app`
3. Click **Actions** → **Delete application**
![](/images/009/04.png)
4. Nhập tên application để xác nhận và chọn **Delete**
![](/images/009/05.png)
5. Tên application sẽ biến mất sau khi xoá thành công

## Bước 2: Xóa Aurora Serverless Cluster

### 2.1 Xóa Database Cluster
1. Vào **RDS Console** → **Databases**
2. Chọn cluster `carrentalweb-aurora-cluster-instance-1`
3. Click **Actions** → **Delete**
![](/images/009/06.png)
4. Nhập `delete me` và chọn **Delete**
![](/images/009/07.png)
5. Chọn tiếp cluster `carrentalweb-aurora-cluster`
6. Click **Actions** → **Delete**
![](/images/009/08.png)
7. Chọn như hình và chọn **Delete DB Cluster**
![](/images/009/09.png)
8. Xác nhận xoá thành công
![](/images/009/10.png)

### 2.2 Xóa Subnet Group
1. Vào **RDS Console** → **Subnet groups**
2. Chọn `carrentalweb-aurora-subnet-group`
3. Chọn **Delete**
![](/images/009/11.png)
5. Click **Delete** lần nữa. Khi xoá thành công, tên của subnet group sẽ biến mất

## Bước 3: Xóa Security Groups

### 3.1 Xóa RDS Security Group
1. Tìm `carrentalweb-rds-sg`
2. Chọn → **Actions** → **Delete security group**
![](/images/009/13.png)
3. Click **Delete**

### 3.2 Xóa EB Security Group (tương tự 3.1)
1. Vào **VPC Console** → **Security Groups**
2. Tìm `carrentalweb-eb-sg`
3. Chọn → **Actions** → **Delete security group**
4. Click **Delete**

## Bước 4: Xóa VPC

### 4.1 Xóa NAT Gateway
1. Vào **VPC Console** → **NAT gateways**
2. Chọn `carrentalweb-nat-public1-us-east-1a`
3. Click **Actions** → **Delete NAT gateway**
![](/images/009/16.png)
4. Nhập `delete` và Click **Delete**
5. Thực hiện tương tự với NAT Gateway còn lại

### 4.2 Release Elastic IP
1. Vào **VPC Console** → **Elastic IPs**
2. Chọn EIP đang được sử dụng
3. Click **Actions** → **Release Elastic IPs**
![](/images/009/15.png)
4. Click **Release**

### 4.3 Xóa Internet Gateway
1. **Detach Internet Gateway:**
   - Vào **VPC Console** → **Internet gateways**
   - Chọn `carrentalweb-igw`
   - Click **Actions** → **Detach from VPC**
   ![](/images/009/14.png)
   - Click **Detach internet gateway**

2. **Xóa Internet Gateway:**
   - Click **Actions** → **Delete internet gateway**
   ![](/images/009/17.png)
   - Nhập `delete` và Click **Delete**

### 4.4 Xóa VPC
1. **VPC Console** → **Your VPCs**
2. Chọn `carrentalweb-vpc`
3. Click **Actions** → **Delete VPC**
![](/images/009/12.png)
4. Nhập `delete` và Click **Delete**
5. VPC và những cấu hình liên quan như Subnet, Route table cũng sẽ được xoá theo
![](/images/009/18.png)

## Bước 5: Xóa IAM Roles
1. Vào **IAM Console** → **Users**
2. Tìm `carrentalweb-cicd-user`
3. Chọn → **Delete**
4. Chọn **Deactivate access key**
![](/images/009/19.png)
4. Sau đó nhập `confirm` và Click **Delete user**

## Lưu ý quan trọng

### Thứ tự xóa quan trọng:
1. **Environment/Application** trước
2. **Database** sau
3. **Security Groups** sau khi không còn dependencies
4. **VPC** cuối cùng

### Backup trước khi xóa:
- **Database snapshots**: Tạo snapshot nếu cần backup data
- **Application code**: Đảm bảo code đã được commit lên Git
- **Configuration**: Lưu lại cấu hình quan trọng

### Chi phí sau khi xóa:
- **Aurora Serverless**: Không còn chi phí
- **EC2 instances**: Không còn chi phí
- **Load Balancer**: Không còn chi phí
- **Data transfer**: Có thể còn một ít

## Troubleshooting

### Common Issues
1. **Cannot delete VPC**: Kiểm tra còn tài nguyên nào trong VPC không
2. **Cannot delete Security Group**: Kiểm tra còn instance nào dùng SG không
3. **Cannot delete Subnet**: Kiểm tra còn resource nào trong subnet không
4. **Cannot delete IAM Role**: Kiểm tra còn service nào dùng role không

🎉 **Chúc mừng!** Bạn đã hoàn thành workshop triển khai Spring Boot lên AWS với Elastic Beanstalk, Aurora Serverless MySQL và CI/CD.

### Những gì bạn đã học được:
- Tạo VPC tùy chỉnh với bảo mật cao
- Thiết lập Aurora Serverless MySQL với Data API
- Deploy ứng dụng Spring Boot lên Elastic Beanstalk
- Cấu hình CI/CD pipeline với GitHub Actions
- Quản lý và dọn dẹp tài nguyên AWS

### Khám phá thêm:
- [AWS Elastic Beanstalk Documentation](https://docs.aws.amazon.com/elasticbeanstalk/)
- [Amazon Aurora Documentation](https://docs.aws.amazon.com/aurora/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions) 
