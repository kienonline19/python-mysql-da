# Buổi 2: MySQL Nâng cao - Phần Lý Thuyết
## Thời lượng: 90 phút

### I. JOINS TRONG MySQL (40 phút)

#### 1. Khái niệm JOIN
- JOIN là phương thức kết hợp dữ liệu từ nhiều bảng dựa trên mối quan hệ giữa chúng
- Sử dụng các khóa (key) để thiết lập mối quan hệ
- Cho phép truy vấn dữ liệu một cách linh hoạt và hiệu quả

#### 2. Các loại JOIN

##### a. INNER JOIN
- Chỉ trả về các dòng có giá trị khớp ở cả hai bảng
- Cú pháp:
```sql
SELECT columns
FROM table1
INNER JOIN table2 ON table1.column = table2.column;
```
- Ví dụ:
```sql
SELECT orders.id, customers.name
FROM orders
INNER JOIN customers ON orders.customer_id = customers.id;
```

##### b. LEFT JOIN (LEFT OUTER JOIN)
- Trả về tất cả dòng từ bảng bên trái và các dòng khớp từ bảng bên phải
- Nếu không có dữ liệu khớp, kết quả sẽ là NULL
- Cú pháp:
```sql
SELECT columns
FROM table1
LEFT JOIN table2 ON table1.column = table2.column;
```

##### c. RIGHT JOIN (RIGHT OUTER JOIN)
- Ngược lại với LEFT JOIN
- Trả về tất cả dòng từ bảng bên phải
- Cú pháp tương tự LEFT JOIN

##### d. FULL OUTER JOIN
- MySQL không hỗ trợ trực tiếp
- Có thể mô phỏng bằng UNION của LEFT và RIGHT JOIN

### II. SUBQUERIES VÀ CTE (30 phút)

#### 1. Subqueries (Truy vấn con)
- Là truy vấn nằm trong truy vấn khác
- Có thể sử dụng trong WHERE, FROM, SELECT

##### a. Subquery trong WHERE
```sql
SELECT *
FROM products
WHERE price > (SELECT AVG(price) FROM products);
```

##### b. Subquery trong FROM
```sql
SELECT *
FROM (
    SELECT customer_id, COUNT(*) as order_count
    FROM orders
    GROUP BY customer_id
) as customer_orders;
```

#### 2. Common Table Expressions (CTE)
- Tạo truy vấn tạm thời có thể tái sử dụng
- Cú pháp rõ ràng và dễ đọc hơn subquery
```sql
WITH customer_summary AS (
    SELECT customer_id, COUNT(*) as order_count
    FROM orders
    GROUP BY customer_id
)
SELECT * FROM customer_summary;
```

### III. INDEXING VÀ TỐI ƯU HÓA (20 phút)

#### 1. Index trong MySQL
- Cải thiện tốc độ truy vấn
- Các loại index phổ biến:
  - Primary Key
  - Unique Index
  - Normal Index
  - Composite Index

#### 2. Tạo và Quản lý Index
```sql
-- Tạo index đơn
CREATE INDEX index_name ON table(column);

-- Tạo composite index
CREATE INDEX index_name ON table(column1, column2);

-- Xem danh sách index
SHOW INDEX FROM table_name;
```

#### 3. Query Optimization
- Sử dụng EXPLAIN để phân tích query
- Best practices:
  - Sử dụng index phù hợp
  - Tránh SELECT *
  - Tối ưu điều kiện WHERE
  - Sử dụng LIMIT trong queries lớn

### TÀI LIỆU THAM KHẢO
1. MySQL Documentation
2. MySQL Performance Guide
3. Database Indexing Techniques