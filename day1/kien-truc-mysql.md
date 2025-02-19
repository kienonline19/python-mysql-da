### 1. Mô hình Client-Server

1. Nguyên lý hoạt động:
- MySQL Server: chạy liên tục (daemon process) và lắng nghe các kết nối
- MySQL Client: kết nối đến server để thực hiện truy vấn
- Communication: sử dụng TCP/IP protocol

2. Luồng xử lý:
```
Client -> TCP/IP Connection -> MySQL Server
                           -> Authentication
                           -> Authorization
                           -> Query Processing
                           -> Result Return
```

### 2. Các thành phần của MySQL Server

1. Connection Handling:
- Thread management
- Authentication
- Security

2. Query Processing:
- Parser: Phân tích cú pháp SQL
- Optimizer: Tối ưu hóa truy vấn
- Cache: Lưu trữ kết quả truy vấn
- Buffer Pool: Cache data và index

3. Storage Engine Layer:
- Quản lý lưu trữ và truy xuất dữ liệu
- Tương tác với file system

### 3. MySQL Client

1. Các loại client phổ biến:
```
- mysql Command Line Tool
- MySQL Workbench
- phpMyAdmin
- Application Connectors (JDBC, ODBC)
```

2. Chức năng chính:
- Kết nối tới server
- Gửi queries
- Nhận và hiển thị results
- Quản lý database

### 4. Storage Engines

1. InnoDB (Default):
```plaintext
Ưu điểm:
- ACID compliance
- Transaction support
- Foreign key support
- Row-level locking
- Crash recovery

Use cases:
- Web applications
- Transaction processing
- Data integrity quan trọng
```

2. MyISAM:
```plaintext
Ưu điểm:
- Read performance tốt
- Compression support
- Full-text indexing

Use cases:
- Read-heavy workloads
- Data warehousing
- Logging
```

3. MEMORY (HEAP):
```plaintext
Ưu điểm:
- Extremely fast
- In-memory storage

Use cases:
- Temporary tables
- Quick lookups
- Session management
```

### 5. Ví dụ thực tế

1. Kiểm tra Storage Engine:
```sql
-- Xem storage engine mặc định
SHOW ENGINES;

-- Xem storage engine của table
SHOW TABLE STATUS WHERE Name = 'table_name';

-- Tạo table với storage engine cụ thể
CREATE TABLE test (
    id INT PRIMARY KEY
) ENGINE = InnoDB;
```

2. Quản lý kết nối:
```sql
-- Xem các process đang chạy
SHOW PROCESSLIST;

-- Kiểm tra số kết nối
SHOW STATUS WHERE variable_name = 'Threads_connected';

-- Kiểm tra max connections
SHOW VARIABLES LIKE 'max_connections';
```

3. Buffer Pool Management:
```sql
-- Xem kích thước buffer pool
SHOW VARIABLES LIKE 'innodb_buffer_pool_size';

-- Kiểm tra buffer pool usage
SHOW ENGINE INNODB STATUS;
```

### 6. Best Practices

1. Chọn Storage Engine:
- InnoDB cho ứng dụng cần transaction
- MyISAM cho read-only data
- MEMORY cho temporary tables

2. Configuration:
```ini
[mysqld]
# Buffer Pool Size (50-75% của RAM)
innodb_buffer_pool_size=1G

# Thread Cache
thread_cache_size=8

# Query Cache (từ MySQL 8.0 đã deprecated)
query_cache_type=0
```

3. Monitoring:
- Theo dõi connection count
- Kiểm tra buffer pool utilization
- Monitor query performance