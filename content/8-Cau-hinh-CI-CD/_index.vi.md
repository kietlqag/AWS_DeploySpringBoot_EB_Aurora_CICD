---
title: "Cấu hình CI/CD"
date: 2024-12-19
weight: 8
---

## Tổng quan

CI/CD (Continuous Integration/Continuous Deployment) giúp tự động hóa quá trình build, test và deploy ứng dụng. Trong chương này, chúng ta sẽ cấu hình GitHub Actions để tự động deploy ứng dụng Spring Boot lên Elastic Beanstalk.

## Tại sao cần CI/CD?

- **Tự động hóa**: Giảm thiểu lỗi manual
- **Nhanh chóng**: Deploy nhanh và an toàn
- **Consistency**: Đảm bảo môi trường đồng nhất
- **Rollback**: Dễ dàng rollback về version cũ
- **Monitoring**: Theo dõi quá trình deploy

## Bước 1: Tạo IAM User cho CI/CD

### 1.1 Tạo IAM User
1. Tìm và chọn dịch vụ **IAM** 
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/01.png)
2. Chọn **Users** và chọn **Create user**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/02.png)
3. **User name**: `carrentalweb-cicd-user`
4. Click **Next**

### 1.2 Gán Permissions
1. Chọn **Attach policies directly**
2. Tìm và chọn:
   - `AdministratorAccess-AWSElasticBeanstalk`
   - `AmazonS3FullAccess`
   - `AmazonEC2FullAccess`
3. Click **Next**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/03.png)
5. Click **Create user**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/04.png)
6. Click vào tên user vừa tạo, chọn **Create access key**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/05.png)
7. Click chọn **Other**, sau đó **Next**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/06.png)
8. Click chọn **Creat access key**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/07.png)
9. Xác nhận tạo thành công
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/08.png)

### 1.3 Lưu thông tin quan trọng
```
Access Key: AKIA...
Secret Access Key: [SECRET_KEY]
```

## Bước 2: Cấu hình GitHub Secrets

### 2.1 Truy cập Repository Settings
1. Vào GitHub repository
2. **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/09.png)

### 2.2 Thêm các Secrets
```
AWS_ACCESS_KEY_ID: [ACCESS_KEY_ID]
AWS_SECRET_ACCESS_KEY: [SECRET_ACCESS_KEY]
AWS_REGION: us-east-1
EB_APPLICATION_NAME: carrentalweb-app
EB_ENVIRONMENT_NAME: carrentalweb-prod
```
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/10.png)

## Bước 3: Tạo GitHub Actions Workflow

### 3.1 Tạo file deploy.yml
Tạo file `.github/workflows/deploy.yml`:

```yaml
name: Deploy to AWS Elastic Beanstalk

on:
  push:
    branches: [ main, master ]
  pull_request:
    branches: [ main, master ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main' || github.ref == 'refs/heads/master'
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Set up JDK 21
      uses: actions/setup-java@v4
      with:
        java-version: '21'
        distribution: 'temurin'
        
    - name: Cache Maven packages
      uses: actions/cache@v3
      with:
        path: ~/.m2
        key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
        restore-keys: ${{ runner.os }}-m2
        
    - name: Build with Maven
      run: mvn clean package -DskipTests
      
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v4
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ secrets.AWS_REGION }}
        
    - name: Show deployment info
      run: |
        echo "Deploying to AWS Elastic Beanstalk..."
        echo "Application: ${{ secrets.EB_APPLICATION_NAME }}"
        echo "Environment: ${{ secrets.EB_ENVIRONMENT_NAME }}"
        echo "Region: ${{ secrets.AWS_REGION }}"
        
    - name: Deploy to EB
      uses: einaregilsson/beanstalk-deploy@v21
      with:
        aws_access_key: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws_secret_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        application_name: ${{ secrets.EB_APPLICATION_NAME }}
        environment_name: ${{ secrets.EB_ENVIRONMENT_NAME }}
        region: ${{ secrets.AWS_REGION }}
        version_label: ${{ github.sha }}
        deployment_package: target/CarRentalWeb-1.0.0.jar
        wait_for_deployment: true
```

## Bước 4: Test CI/CD Pipeline
1. Tạo một thay đổi nhỏ trong code (VD: Thay chữ "Hi!" ở trang đăng nhập thành "Quốc Kiệt")
- Sửa dòng 19 file /resource/templates/login.htmt: thay chữ Hi! sang Quốc Kiệt
- Kiểm tra giao diện trước khi push code mới
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/11.png)
2. Commit và push lên GitHub:
```bash
git add .
git commit -m "Test CI/CD Deployment"
git push origin main
```
3. Vào **GitHub Repository** → **Actions** tab, theo dõi GitHub Actions:
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/12.png)
4. Chờ trạng thái chuyển sang màu xanh lá
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/13.png)

### 4.3 Kiểm tra deployment
1. Vào **Elastic Beanstalk Console**
2. Kiểm tra environment `carrentalweb-prod`
3. Tab **Events** → Xem deployment events
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/14.png)
4. Truy cập lại URL ứng dụng để đảm bảo thay đổi đã được deploy
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/008/15.png)


### 4.4 Test rollback (nếu cần)
1. Nếu deployment có vấn đề:
   - Vào **Elastic Beanstalk Console**
   - Tab **Application versions**
   - Chọn version cũ → **Deploy**
2. Hoặc sử dụng GitHub Actions:
   - Vào **Actions** → **Deploy to AWS Elastic Beanstalk**
   - Click **Re-run jobs** với version cũ


## Lưu ý quan trọng

- **Security**: Không commit AWS credentials vào code
- **Branch Protection**: Bảo vệ main branch
- **Rollback**: Có thể rollback về version cũ
- **Monitoring**: Theo dõi deployment logs
- **Cost**: CI/CD không tốn chi phí đáng kể

## Bước tiếp theo

Sau khi cấu hình xong CI/CD, chúng ta sẽ **[Dọn dẹp tài nguyên](../9-Don-dep-tai-nguyen/)** để xóa các tài nguyên AWS và tránh phát sinh chi phí không cần thiết. 
