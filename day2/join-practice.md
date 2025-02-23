# Buổi 2: MySQL Nâng cao - Phần Thực Hành
## Thời lượng: 90 phút

### A. CHUẨN BỊ DATABASE MẪU

```sql
-- Tạo database và tables
CREATE DATABASE shop_management;
USE shop_management;

-- Tạo các bảng cần thiết
CREATE TABLE customers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    phone VARCHAR(20)
);

CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2),
    stock INT DEFAULT 0
);

CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT,
    order_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10,2),
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);

CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    unit_price DECIMAL(10,2),
    PRIMARY KEY (order_id, product_id),
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);

-- Thêm dữ liệu vào bảng customers
INSERT INTO customers (name, email, phone) VALUES
('Nguyễn Văn A', 'vana@email.com', '0901234567'),
('Trần Thị B', 'thib@email.com', '0912345678'),
('Lê Văn C', 'vanc@email.com', '0923456789'),
('Phạm Thị D', 'thid@email.com', '0934567890'),
('Hoàng Văn E', 'vane@email.com', '0945678901');

-- Thêm dữ liệu vào bảng products
INSERT INTO products (name, price, stock) VALUES
('iPhone 13', 799.99, 50),
('Samsung Galaxy S21', 699.99, 45),
('MacBook Pro', 1299.99, 30),
('Dell XPS 13', 999.99, 25),
('AirPods Pro', 249.99, 100),
('Galaxy Buds', 149.99, 80),
('iPad Air', 599.99, 40),
('Lenovo ThinkPad', 899.99, 20),
('Apple Watch', 399.99, 60),
('Xiaomi Mi 11', 499.99, 55);

-- Thêm dữ liệu vào bảng orders
INSERT INTO orders (customer_id, total_amount) VALUES
(1, 1049.98),  -- Khách hàng 1 mua iPhone 13 và AirPods Pro
(2, 1449.98),  -- Khách hàng 2 mua MacBook Pro và Galaxy Buds
(3, 949.98),   -- Khách hàng 3 mua Samsung Galaxy S21 và Apple Watch
(1, 599.99),   -- Khách hàng 1 mua iPad Air
(4, 1299.99);  -- Khách hàng 4 mua MacBook Pro

-- Thêm dữ liệu vào bảng order_items
INSERT INTO order_items (order_id, product_id, quantity, unit_price) VALUES
-- Đơn hàng 1
(1, 1, 1, 799.99),   -- iPhone 13
(1, 5, 1, 249.99),   -- AirPods Pro

-- Đơn hàng 2
(2, 3, 1, 1299.99),  -- MacBook Pro
(2, 6, 1, 149.99),   -- Galaxy Buds

-- Đơn hàng 3
(3, 2, 1, 699.99),   -- Samsung Galaxy S21
(3, 9, 1, 249.99),   -- Apple Watch

-- Đơn hàng 4
(4, 7, 1, 599.99),   -- iPad Air

-- Đơn hàng 5
(5, 3, 1, 1299.99);  -- MacBook Pro
```

### B. BÀI TẬP THỰC HÀNH

#### 1. Thực hành JOIN (30 phút)

##### Bài 1.1: Hiển thị chi tiết đơn hàng
```sql
-- Yêu cầu: Hiển thị thông tin đơn hàng kèm tên khách hàng và sản phẩm
SELECT 
    o.id as order_id,
    c.name as customer_name,
    p.name as product_name,
    oi.quantity,
    oi.unit_price,
    (oi.quantity * oi.unit_price) as total
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id
INNER JOIN order_items oi ON o.id = oi.order_id
INNER JOIN products p ON oi.product_id = p.id;
```

##### Bài 1.2: Thống kê khách hàng và đơn hàng
```sql
-- Yêu cầu: Hiển thị tất cả khách hàng và số đơn hàng của họ
SELECT 
    c.name,
    COUNT(o.id) as total_orders,
    COALESCE(SUM(o.total_amount), 0) as total_spent
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id
GROUP BY c.id, c.name;
```

#### 2. Thực hành Subqueries và CTE (30 phút)

##### Bài 2.1: Phân tích đơn hàng
```sql
-- Yêu cầu: Tìm các đơn hàng có giá trị lớn hơn trung bình
WITH avg_order AS (
    SELECT AVG(total_amount) as avg_amount
    FROM orders
)
SELECT 
    o.id,
    c.name as customer_name,
    o.total_amount
FROM orders o
JOIN customers c ON o.customer_id = c.id
WHERE o.total_amount > (SELECT avg_amount FROM avg_order);
```

##### Bài 2.2: Phân tích sản phẩm
```sql
-- Yêu cầu: Tìm top 3 sản phẩm bán chạy nhất
SELECT 
    p.name,
    SUM(oi.quantity) as total_sold
FROM products p
LEFT JOIN order_items oi ON p.id = oi.product_id
GROUP BY p.id, p.name
ORDER BY total_sold DESC
LIMIT 3;
```

#### 3. Thực hành Index và Optimization (30 phút)

##### Bài 3.1: Tạo và kiểm tra index
```sql
-- Tạo các index phù hợp
CREATE INDEX idx_customer_email ON customers(email);
CREATE INDEX idx_order_date ON orders(order_date);
CREATE INDEX idx_product_price ON products(price);

-- Kiểm tra hiệu quả
EXPLAIN SELECT * FROM customers WHERE email = 'example@email.com';
EXPLAIN SELECT * FROM orders WHERE order_date > '2024-01-01';
```

##### Bài 3.2: Tối ưu truy vấn
```sql
-- Truy vấn ban đầu
SELECT *
FROM orders o
JOIN customers c ON o.customer_id = c.id
WHERE o.order_date > '2024-01-01';

-- Truy vấn tối ưu
SELECT 
    o.id,
    o.order_date,
    o.total_amount,
    c.name,
    c.email
FROM orders o
JOIN customers c ON o.customer_id = c.id
WHERE o.order_date > '2024-01-01';
```

### C. BÀI TẬP VỀ NHÀ

#### 1. Phân tích Doanh số
- Tính doanh số theo tháng
- Phân tích xu hướng mua hàng
- Top khách hàng theo doanh số

#### 2. Tối ưu Database
- Thêm các index cần thiết
- Tối ưu các truy vấn phức tạp
- Viết báo cáo phân tích hiệu suất

### D. TIÊU CHÍ ĐÁNH GIÁ
1. Hoàn thành các bài tập thực hành: 50%
2. Tối ưu được truy vấn: 25%
3. Hiểu và áp dụng được index: 25%