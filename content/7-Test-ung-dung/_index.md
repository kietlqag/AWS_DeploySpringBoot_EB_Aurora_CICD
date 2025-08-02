---
title: "Test Application"
date: 2024-12-19
weight: 7
---

## Overview

After successfully deploying the Spring Boot application to Elastic Beanstalk and connecting to the Aurora database, we need to test to ensure everything works properly.

## Test Objectives

- **Database Connection**: Ensure the application can connect to Aurora
- **Basic Functions**: Test the main features of the application
- **Performance**: Check response speed
- **Data Persistence**: Ensure data is stored correctly

## Step 1: Test Database Connection
1. Open browser
2. Access: `http://[EB_URL]/health`
3. Check response:
```json
{
  "status": "UP",
  "application": "CarRentalWeb",
  "timestamp": 1234567890
}
```

## Step 2: Test Registration Function

### 2.1 Test Registration Form
1. Access: `http://[EB_URL]/register`
2. Fill in test information:
   - **Display Name**: `testuser`
   - **Email**: [Your Email]
   - **Password**: `123`
3. Click **Register**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/007/01.png)
4. Check email for verification code
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/007/02.png)
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/007/03.png)
5. Confirm successful registration
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/007/04.png)

### 2.2 Check Data in Database
1. Go to **RDS Console** → **Query Editor**
2. Connect to `carrentalweb-aurora-cluster`
3. Run query:
```sql
USE carrentalweb;
SELECT * FROM accounts WHERE full_name = 'testuser';
```
4. Confirm user has been created
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/007/05.png)

## Step 3: Test Login Function

### 3.1 Test Login
1. Access: `http://[EB_URL]/login`
2. Login with information just created:
   - **Email**: [Email registered in step 1]
   - **Password**: `123`
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/007/06.png)
3. Check:
   - Login successful
   - Redirect to dashboard
   - Session created
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/007/07.png)

## Step 4: Create Car Data

### 4.1 Create Cars Table
1. Go to **RDS Console** → **Query Editor**
2. Connect to `carrentalweb-aurora-cluster`
3. Run command to create table:

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

### 4.2 Add Car Data
1. Run INSERT command to add cars:

```sql
INSERT INTO cars (name, brand, price, status, engine, seat, model, bodystyle, image) VALUES
('Toyota Camry 2.5G', 'Toyota', 1200000, 'Sẵn sàng', '2.5L 4-cylinder', 5, '2023', 'Sedan', 'https://carshop.vn/wp-content/uploads/2022/07/hinh-nen-xe-oto-dep-7.jpg'),
('Honda City G', 'Honda', 800000, 'Sẵn sàng', '1.5L i-VTEC', 5, '2023', 'Sedan', 'honda-city.jpg'),
('Ford Ranger XLT', 'Ford', 1500000, 'Sẵn sàng', '2.0L EcoBlue', 5, '2023', 'Pickup', 'ford-ranger.jpg');
```

2. Confirm cars have been added:
```sql
SELECT * FROM cars;
```

## Step 5: Test Car Booking Function

### 5.1 Test Car Booking
1. Login successfully
2. Access car booking page
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/007/08.png)
3. Select **View Details** on car card
4. Choose pickup date, return date, services (if any)
5. Click **Choose Booking**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/007/09.png)
6. Enter missing information and select **Book Car**
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/007/10.png)
7. Confirm successful booking
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/007/11.png)

### 5.2 Check Booking Data
1. Go to **RDS Console** → **Query Editor**
2. Run query to check booking:
```sql
USE carrentalweb;
SELECT * FROM orders WHERE accountemail = [Your Email];
```
3. Confirm booking has been created
![](https://kietlqag.github.io/AWS_DeploySpringBoot_EB_Aurora_CICD/images/007/12.png)

## Next Step

After successfully testing the application, we will **[Configure CI/CD](../8-Cau-hinh-CI-CD/)** to automate the deployment process with GitHub Actions. 
