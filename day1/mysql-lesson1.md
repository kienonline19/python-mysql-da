# Buổi 1: Giới thiệu và Cơ bản về MySQL
## Thời lượng: 3 giờ

### Phần 1: Giới thiệu (45 phút)

#### 1.1 Tổng quan về Cơ sở dữ liệu (20 phút)
- Định nghĩa cơ sở dữ liệu
- Các loại cơ sở dữ liệu phổ biến
  - Cơ sở dữ liệu quan hệ (RDBMS)
  - Cơ sở dữ liệu NoSQL
- Tại sao chọn MySQL?
  - Ưu điểm và nhược điểm
  - Use cases phổ biến
  - So sánh với các RDBMS khác

#### 1.2 Kiến trúc MySQL (25 phút)
- Mô hình Client-Server
- Các thành phần cơ bản của MySQL
  - MySQL Server
  - MySQL Client
  - Storage Engines
- Các khái niệm cơ bản
  - Database
  - Table
  - Row/Record
  - Column/Field
  - Primary Key
  - Foreign Key

### Phần 2: Cài đặt và Cấu hình (45 phút)

#### 2.1 Cài đặt MySQL (25 phút)
- Cài đặt MySQL Server
  - Windows: MySQL Installer
  - macOS: Homebrew
  - Linux: apt/yum
- Cài đặt MySQL Workbench
- Kiểm tra cài đặt
  - Khởi động/dừng MySQL service
  - Kết nối tới MySQL server

#### 2.2 Cấu hình Cơ bản (20 phút)
- Thiết lập mật khẩu root
- Tạo user mới và phân quyền
- Cấu hình file my.cnf/my.ini
- Kiểm tra trạng thái server

### Phần 3: Thực hành SQL Cơ bản (90 phút)

#### 3.1 Làm việc với Database (20 phút)
```sql
-- Tạo database mới
CREATE DATABASE shop_management;

-- Hiển thị danh sách databases
SHOW DATABASES;

-- Chọn database làm việc
USE shop_management;
```

#### 3.2 Làm việc với Tables (30 phút)
```sql
-- Tạo bảng products
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2),
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tạo bảng categories
CREATE TABLE categories (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    description TEXT
);

-- Hiển thị cấu trúc bảng
DESCRIBE products;
SHOW CREATE TABLE categories;
```

#### 3.3 Thao tác với Dữ liệu (40 phút)
```sql
-- Thêm dữ liệu
INSERT INTO categories (name, description) 
VALUES ('Electronics', 'Electronic devices and accessories');

-- Truy vấn dữ liệu
SELECT * FROM categories;
SELECT name, price FROM products WHERE price > 100;

-- Cập nhật dữ liệu
UPDATE products SET price = price * 1.1 WHERE category_id = 1;

-- Xóa dữ liệu
DELETE FROM products WHERE id = 1;
```

### Bài tập Thực hành
Xây dựng cơ sở dữ liệu cho một cửa hàng nhỏ với các yêu cầu:

1. Tạo các bảng sau:
   - customers (id, name, email, phone, address)
   - products (id, name, price, stock_quantity, category_id)
   - orders (id, customer_id, order_date, total_amount)
   - order_items (order_id, product_id, quantity, unit_price)

2. Thực hiện các thao tác:
   - Thêm ít nhất 5 khách hàng
   - Thêm ít nhất 10 sản phẩm
   - Tạo 3 đơn hàng với các sản phẩm khác nhau

3. Viết các truy vấn:
   - Hiển thị danh sách khách hàng
   - Tìm sản phẩm có giá trên 100
   - Hiển thị đơn hàng của một khách hàng cụ thể

### Tài liệu Tham khảo
1. MySQL Official Documentation
2. W3Schools MySQL Tutorial
3. MySQL for Beginners (PDF đính kèm)

### Yêu cầu Chuẩn bị cho Buổi 2
1. Hoàn thành bài tập thực hành
2. Đọc trước về JOIN trong MySQL
3. Chuẩn bị câu hỏi về những vấn đề gặp phải

### Tiêu chí Đánh giá
- Hiểu được các khái niệm cơ bản về CSDL
- Cài đặt thành công MySQL và công cụ
- Tạo được database và tables
- Thực hiện được các thao tác CRUD cơ bản
- Hoàn thành bài tập thực hành