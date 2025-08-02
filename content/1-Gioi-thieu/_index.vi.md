---
title: "Giới thiệu"
date: 2024-12-19
weight: 1
---

# Triển khai Spring Boot lên AWS với Elastic Beanstalk, Aurora Serverless MySQL và CI/CD

## Tổng quan

Workshop này sẽ hướng dẫn bạn triển khai một ứng dụng Spring Boot lên AWS sử dụng Elastic Beanstalk, kết nối với Aurora Serverless MySQL và thiết lập CI/CD pipeline với GitHub Actions.

## Các bước thực hiện

{{% children description="true" /%}}

1. **[Chuẩn bị môi trường](../2-Chuan-bi-moi-truong/)** - Cài đặt công cụ và cấu hình AWS
2. **[Tạo VPC tùy chỉnh](../3-Tao-VPC-tuy-chinh/)** - Xây dựng network infrastructure
3. **[Tạo Security Groups](../4-Tao-Security-Groups/)** - Cấu hình bảo mật mạng
4. **[Tạo Aurora Serverless MySQL](../5-Tao-Aurora-Database/)** - Thiết lập database Aurora
5. **[Deploy Elastic Beanstalk](../6-Deploy-Elastic-Beanstalk/)** - Triển khai ứng dụng Spring Boot
6. **[Test ứng dụng](../7-Test-ung-dung/)** - Kiểm tra ứng dụng với database
7. **[Cấu hình CI/CD](../8-Cau-hinh-CI-CD/)** - Tự động hóa deployment với GitHub Actions
8. **[Dọn dẹp tài nguyên](../9-Don-dep-tai-nguyen/)** - Xóa các tài nguyên AWS

## Kết quả mong đợi

Sau khi hoàn thành workshop, chúng ta sẽ có:

- Ứng dụng Spring Boot chạy trên AWS Elastic Beanstalk
- Database Aurora Serverless MySQL với Data API
- VPC tùy chỉnh với bảo mật cao
- CI/CD pipeline tự động với GitHub Actions
- Query Editor để quản lý database

## Yêu cầu trước khi bắt đầu

- Tài khoản AWS
- Tài khoản GitHub (để CI/CD)
- Kiến thức cơ bản về Spring Boot
- Hiểu biết cơ bản về AWS services 