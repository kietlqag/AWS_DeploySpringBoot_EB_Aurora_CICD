---
title: "Tạo VPC"
date: 2024-12-19
weight: 3
---

## Tổng quan

Trong chương này, chúng ta sẽ tạo một VPC (Virtual Private Cloud) tùy chỉnh thay vì sử dụng VPC mặc định của AWS. Điều này giúp chúng ta có kiểm soát tốt hơn về bảo mật và cấu trúc mạng.

## Tại sao cần VPC tùy chỉnh?

- **Bảo mật cao hơn**: Kiểm soát chính xác traffic vào/ra
- **Cô lập mạng**: Tách biệt các tài nguyên
- **Quản lý IP**: Định nghĩa range IP riêng
- **Kiến trúc rõ ràng**: Public/Private subnets riêng biệt

## Kiến trúc VPC

```
VPC (10.0.0.0/16)
├── Public Subnet 1 - AZ a
├── Public Subnet 2 - AZ b
├── Private Subnet 1 - AZ a
├── Private Subnet 2 - AZ b
├── Internet Gateway
├── NAT Gateway
└── Route Tables
```

## Bước 1: Tạo VPC

### 1.1 Đăng nhập AWS Console
1. Mở AWS Console
2. Đăng nhập với tài khoản có quyền tạo VPC
3. Chọn region: **us-east-1** (Virginia) (hoặc region khác tuỳ chọn)

### 1.2 Tạo VPC mới
1. Tìm và chọn service **VPC**
![VPC Service](/images/003/01.png)
2. Click **Create VPC**
![Create VPC](/images/003/02.png)
3. Chọn **VPC and more** (để tạo tự động các thành phần cơ bản)
![VPC and more](/images/003/03.png)
4. Cấu hình thông tin VPC:

    **Name tag:** `carrentalweb-vpc`  
    **IPv4 CIDR block:** `10.0.0.0/16`  
    **IPv6 CIDR block:** `No IPv6 CIDR block`  
    **Tenancy:** `Default`  
    **Number of Availability Zones:** `2`  
    **Number of public subnets:** `2`  
    **Number of private subnets:** `2`  
    **NAT gateways:** `1 per AZ`  
    **VPC endpoints:** `None`

5. Click **Create VPC**
![Create VPC Button](/images/003/04.png)

### 1.3 Kiểm tra VPC đã tạo
1. Vào **Your VPCs**
2. Xác nhận VPC `carrentalweb-vpc` đã được tạo
3. Ghi lại **VPC ID**
![VPC List](/images/003/05.png)

## Bước 2: Kiểm tra các thành phần

### 2.1 Subnets
1. Vào **Subnets**
2. Xác nhận có 4 subnets:
   - 2 public: `carrentalweb-vpc-public-subnet-1`, `carrentalweb-vpc-public-subnet-2`
   - 2 private: `carrentalweb-vpc-private-subnet-1`, `carrentalweb-vpc-private-subnet-2`
3. Ghi lại **Subnet IDs**
![Subnets List](/images/003/06.png)

### 2.2 Internet Gateway
1. Vào **Internet Gateways**
2. Xác nhận IGW đã attach với VPC `carrentalweb-vpc`
![Internet Gateway](/images/003/07.png)

### 2.3 NAT Gateway
1. Vào **NAT Gateways**
2. Xác nhận trạng thái là **Available**
3. Ghi lại **NAT Gateway ID**
![NAT Gateway](/images/003/08.png)

### 2.4 Route Tables
1. Vào **Route Tables**
2. Xác nhận có 2 route tables:
   - Public: `0.0.0.0/0` → Internet Gateway
   ![Public Route Table](/images/003/09.png)
   - Private: `0.0.0.0/0` → NAT Gateway
   ![Private Route Table](/images/003/10.png)

## Bước 3: Tạo DB Subnet Group

### 3.1 Tạo DB Subnet Group cho RDS
1. Tìm và chọn service **Aurora and RDS**
![Aurora RDS Service](/images/003/11.png)
2. Vào **Subnet groups**
3. Click **Create DB subnet group**
![Create DB Subnet Group](/images/003/12.png)
4. Điền thông tin:

    **Name:** `carrentalweb-db-subnet-group`  
    **Description:** `Subnet group for CarRentalWeb RDS`  
    **VPC:** `carrentalweb-vpc`  
    **Availability Zones:** `us-east-1a, us-east-1b`  
    **Subnets:** chọn 2 private subnets

5. Click **Create**
![Create Button](/images/003/13.png)
6. Xác nhận tạo thành công
![DB Subnet Group Created](/images/003/14.png)

## Lưu ý quan trọng

- **Chi phí**: NAT Gateway có phí theo giờ và data transfer
- **Bảo mật**: Private subnets không thể truy cập trực tiếp từ internet
- **High Availability**: Sử dụng 2 AZ để đảm bảo tính sẵn sàng
- **Backup**: Lưu trữ thông tin VPC để tham khảo sau này

## Bước tiếp theo

Sau khi tạo xong VPC tùy chỉnh, chúng ta sẽ tạo **[Security Groups](../4-Tao-Security-Groups/)** để cấu hình bảo mật mạng cho các service. 
