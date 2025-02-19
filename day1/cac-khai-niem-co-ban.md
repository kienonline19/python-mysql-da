### 1. Database (Cơ sở dữ liệu)
- Là tập hợp các dữ liệu có tổ chức
- Chứa nhiều tables có liên quan với nhau
- Ví dụ: Database quản lý trường học
```sql
CREATE DATABASE school_management;
USE school_management;
```

### 2. Table (Bảng)
- Là nơi lưu trữ dữ liệu theo cấu trúc cột và hàng
- Mỗi table đại diện cho một đối tượng/thực thể
- Ví dụ: Bảng students lưu thông tin sinh viên
```sql
CREATE TABLE students (
    id INT,
    name VARCHAR(100),
    email VARCHAR(100),
    birth_date DATE
);
```

### 3. Row/Record (Hàng/Bản ghi)
- Một bản ghi cụ thể trong table
- Chứa thông tin về một thực thể
- Ví dụ: Thông tin của một sinh viên
```sql
INSERT INTO students VALUES (1, 'Nguyen Van A', 'a@gmail.com', '2000-01-01');
```
```
| id | name         | email        | birth_date |
|----|--------------|--------------|------------|
| 1  | Nguyen Van A | a@gmail.com  | 2000-01-01 |
```

### 4. Column/Field (Cột/Trường)
- Định nghĩa một thuộc tính của thực thể
- Có kiểu dữ liệu cụ thể (INT, VARCHAR, DATE...)
- Ví dụ: Cột name lưu tên sinh viên
```sql
-- Cấu trúc các cột trong bảng students
DESCRIBE students;
```
```
| Field      | Type         | Null | Key |
|------------|--------------|------|-----|
| id         | INT          | NO   | PRI |
| name       | VARCHAR(100) | NO   |     |
| email      | VARCHAR(100) | YES  |     |
| birth_date | DATE         | YES  |     |
```

### 5. Primary Key (Khóa chính)
- Định danh duy nhất cho mỗi record
- Không được trùng lặp và không được NULL
- Thường dùng: id, mã số,...
```sql
CREATE TABLE students (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE
);
```

### 6. Foreign Key (Khóa ngoại)
- Tạo liên kết giữa hai bảng
- Là Primary Key của bảng khác
- Đảm bảo tính toàn vẹn dữ liệu

Ví dụ về mối quan hệ giữa bảng classes và students:
```sql
-- Bảng lớp học
CREATE TABLE classes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    class_name VARCHAR(50) NOT NULL,
    teacher VARCHAR(100)
);

-- Bảng sinh viên có khóa ngoại tham chiếu đến lớp
CREATE TABLE students (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    class_id INT,
    FOREIGN KEY (class_id) REFERENCES classes(id)
);
```

Ví dụ về cách dữ liệu liên kết:
```
classes:
| id | class_name | teacher     |
|----|------------|-------------|
| 1  | 10A1       | Ms. Huong   |
| 2  | 10A2       | Mr. Tuan    |

students:
| id | name        | class_id |
|----|-------------|----------|
| 1  | Nguyen Van A| 1        |
| 2  | Tran Thi B  | 1        |
| 3  | Le Van C    | 2        |
```

### Mối quan hệ giữa các khái niệm:
1. Database chứa nhiều Tables
2. Table chứa nhiều Rows và Columns
3. Primary Key định danh cho mỗi Row
4. Foreign Key tạo liên kết giữa các Tables

### Ứng dụng thực tế:
```sql
-- Tạo database quản lý trường học
CREATE DATABASE school_management;
USE school_management;

-- Tạo bảng khoa
CREATE TABLE departments (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    head_teacher VARCHAR(100)
);

-- Tạo bảng lớp học
CREATE TABLE classes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    class_name VARCHAR(50) NOT NULL,
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(id)
);

-- Tạo bảng sinh viên
CREATE TABLE students (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    class_id INT,
    FOREIGN KEY (class_id) REFERENCES classes(id)
);
```