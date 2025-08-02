---
title: "Test ứng dụng"
date: 2024-12-19
weight: 7
---

## Tổng quan

Sau khi deploy thành công ứng dụng Spring Boot lên Elastic Beanstalk và kết nối với Aurora database, chúng ta cần test để đảm bảo mọi thứ hoạt động bình thường.

## Mục tiêu test

- **Kết nối database**: Đảm bảo ứng dụng kết nối được với Aurora
- **Chức năng cơ bản**: Test các tính năng chính của ứng dụng
- **Performance**: Kiểm tra tốc độ phản hồi
- **Data persistence**: Đảm bảo dữ liệu được lưu trữ đúng

## Bước 1: Test kết nối database
1. Mở browser
2. Truy cập: `http://[EB_URL]/health`
3. Kiểm tra response:
```json
{
  "status": "UP",
  "application": "CarRentalWeb",
  "timestamp": 1234567890
}
```

## Bước 2: Test chức năng đăng ký

### 2.1 Test form đăng ký
1. Truy cập: `http://[EB_URL]/register`
2. Điền thông tin test:
   - **Tên hiển thị**: `testuser`
   - **Email**: [Email của bạn]
   - **Password**: `123`
3. Click **Register**
![](/images/007/01.png)
4. Kiểm tra email nhập mã xác thực
![](/images/007/02.png)
![](/images/007/03.png)
5. Xác nhận đăng ký thành công
![](/images/007/04.png)

### 2.2 Kiểm tra dữ liệu trong database
1. Vào **RDS Console** → **Query Editor**
2. Kết nối với `carrentalweb-aurora-cluster`
3. Chạy query:
```sql
USE carrentalweb;
SELECT * FROM accounts WHERE full_name = 'testuser';
```
4. Xác nhận user đã được tạo
![](/images/007/05.png)

## Bước 3: Test chức năng đăng nhập

### 3.1 Test đăng nhập
1. Truy cập: `http://[EB_URL]/login`
2. Đăng nhập với thông tin vừa tạo:
   - **Email**: [Email đã đăng ký ở bước 1]
   - **Password**: `123`
![](/images/007/06.png)
3. Kiểm tra:
   - Đăng nhập thành công
   - Redirect về dashboard
   - Session được tạo
![](/images/007/07.png)

## Bước 4: Tạo dữ liệu xe

### 4.1 Tạo bảng cars
1. Vào **RDS Console** → **Query Editor**
2. Kết nối với `carrentalweb-aurora-cluster`
3. Chạy câu lệnh tạo bảng:

```sql
USE carrentalweb;

CREATE TABLE cars (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    brand VARCHAR(100) NOT NULL,
    price INT NOT NULL,
    status VARCHAR(50) DEFAULT 'Sẵn sàng',
    engine VARCHAR(100),
    seat INT,
    model VARCHAR(50),
    bodystyle VARCHAR(50),
    image VARCHAR(255)
);
```

### 4.2 Thêm dữ liệu xe
1. Chạy câu lệnh INSERT để thêm xe:

```sql
INSERT INTO cars (name, brand, price, status, engine, seat, model, bodystyle, image) VALUES
('Toyota Camry 2.5G', 'Toyota', 1200000, 'Sẵn sàng', '2.5L 4-cylinder', 5, '2023', 'Sedan', 'https://carshop.vn/wp-content/uploads/2022/07/hinh-nen-xe-oto-dep-7.jpg'),
('Honda City G', 'Honda', 800000, 'Sẵn sàng', '1.5L i-VTEC', 5, '2023', 'Sedan', 'honda-city.jpg'),
('Ford Ranger XLT', 'Ford', 1500000, 'Sẵn sàng', '2.0L EcoBlue', 5, '2023', 'Pickup', 'ford-ranger.jpg');
```

2. Xác nhận xe đã được thêm:
```sql
SELECT * FROM cars;
```

## Bước 5: Test chức năng đặt xe

### 5.1 Test đặt xe
1. Đăng nhập thành công
2. Truy cập trang đặt xe
![](/images/007/08.png)
3. Chọn **Xem chi tiết** trên thẻ xe
4. Chọn nhận, ngày trả, dịch vụ (nếu có)
5. Click **Chọn đặt**
![](/images/007/09.png)
6. Nhập thông tin còn thiếu và chọn **Đặt xe**
![](/images/007/10.png)
7. Xác nhận đặt thành công
![](/images/007/11.png)

### 5.2 Kiểm tra dữ liệu đặt xe
1. Vào **RDS Console** → **Query Editor**
2. Chạy query kiểm tra booking:
```sql
USE carrentalweb;
SELECT * FROM orders WHERE accountemail = [Email của bạn];
```
3. Xác nhận booking đã được tạo
![](/images/007/12.png)

## Bước tiếp theo

Sau khi test thành công ứng dụng, chúng ta sẽ **[Cấu hình CI/CD](../8-Cau-hinh-CI-CD/)** để tự động hóa quá trình deployment với GitHub Actions. 