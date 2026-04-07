# SQL Assignments - Multi-Database Setup

Dự án này cung cấp một environment hoàn chỉnh để chạy **4 database** (PostgreSQL, MySQL, MongoDB, và Microsoft SQL Server) cùng một lúc sử dụng Docker Compose.

## 📋 Yêu Cầu

- **Docker**: [Cài đặt Docker](https://www.docker.com/products/docker-desktop)
- **Docker Compose**: Thường được cài đặt cùng với Docker Desktop

## 🚀 Khởi Chạy

### 1. Khởi chạy tất cả các database

```bash
docker-compose up -d
```

Lệnh `-d` (detached mode) sẽ chạy các container ở chế độ nền.

### 2. Dừng tất cả các database

```bash
docker-compose down
```

### 3. Xóa tất cả các container và dữ liệu

```bash
docker-compose down -v
```

## 📊 Thông Tin Kết Nối

### PostgreSQL
- **Host**: `localhost` hoặc `127.0.0.1`
- **Port**: `5432`
- **Database**: `postgres_db`
- **Username**: `postgres`
- **Password**: `postgres`

### MySQL
- **Host**: `localhost` hoặc `127.0.0.1`
- **Port**: `3306`
- **Database**: `mysql_db`
- **Username**: `mysql`
- **Password**: `mysql`
- **Root Password**: `root`

### MongoDB
- **Host**: `localhost` hoặc `127.0.0.1`
- **Port**: `27017`
- **Username**: `mongo`
- **Password**: `mongo`
- **Connection String**: `mongodb://mongo:mongo@localhost:27017/`

### Microsoft SQL Server
- **Host**: `localhost` hoặc `127.0.0.1`
- **Port**: `1433`
- **Username**: `SA`
- **Password**: `mysqlserver`
- **Edition**: Express

## 🛠️ Công Cụ Quản Lý

### Adminer (Quản lý PostgreSQL, MySQL, MSSQL)
- **URL**: http://localhost:8080
- Hỗ trợ quản lý PostgreSQL, MySQL, và Microsoft SQL Server

### Mongo Express (Quản lý MongoDB)
- **URL**: http://localhost:8081
- **Username**: `mongo`
- **Password**: `mongo`

## 📝 Các Lệnh Hữu Ích

### Kiểm tra trạng thái các container

```bash
docker-compose ps
```

### Xem log của một container cụ thể

```bash
docker-compose logs postgres   # PostgreSQL
docker-compose logs mysql      # MySQL
docker-compose logs mongodb    # MongoDB
docker-compose logs mssql      # SQL Server
```

### Xem real-time logs

```bash
docker-compose logs -f
```

### Truy cập vào container

```bash
docker-compose exec postgres psql -U postgres
docker-compose exec mysql mysql -u mysql -p
docker-compose exec mongodb mongosh
```

## 📁 Cấu Trúc Thư Mục

```
sql_assignments/
├── docker-compose.yml    # File cấu hình Docker Compose
├── README.md            # Tài liệu này
```

## 🔗 Network

Tất cả các container đều được kết nối qua một custom network `db_network` (driver: bridge), cho phép chúng giao tiếp với nhau bằng tên container.

**Ví dụ**: Từ ứng dụng Node.js bên ngoài:
- PostgreSQL: `postgresql://postgres:postgres@localhost:5432/postgres_db`
- MySQL: `mysql://mysql:mysql@localhost:3306/mysql_db`
- MongoDB: `mongodb://mongo:mongo@localhost:27017/`
- MSSQL: `Server=localhost,1433;User Id=SA;Password=mysqlserver;`

## 💾 Persistence

Dữ liệu của tất cả các database được lưu trữ trong các Docker volumes:
- `postgres_data`: Dữ liệu PostgreSQL
- `mysql_data`: Dữ liệu MySQL
- `mongodb_data`: Dữ liệu MongoDB
- `mssql_data`: Dữ liệu MSSQL

Dữ liệu sẽ được giữ lại ngay cả khi container bị dừng (trừ khi bạn chạy `docker-compose down -v`).

## ⚠️ Lưu Ý Bảo Mật

**Cảnh báo**: Các thông tin đăng nhập được thiết lập trong tệp này là mặc định cho mục đích phát triển. **KHÔNG sử dụng cho môi trường production!**

Để bảo mật, bạn nên:
- Sử dụng biến môi trường từ file `.env`
- Thay đổi các mật khẩu mạnh trước khi deploy
- Sử dụng Docker secrets cho environment production

## 🐛 Troubleshooting

### Port đã được sử dụng

Nếu nhận lỗi "Port already in use", hãy:

```bash
# Kiểm tra process đang sử dụng port
netstat -ano | findstr :5432  # Windows

# Hoặc thay đổi port trong docker-compose.yml
# Ví dụ: "5433:5432" (external:internal)
```

### Container không khởi động

```bash
# Kiểm tra logs
docker-compose logs

# Restart container
docker-compose restart
```

### Reset hoàn toàn

```bash
# Dừng và xóa tất cả
docker-compose down -v

# Rebuild
docker-compose up -d --build
```

## 📚 Tài Liệu Hữu Ích

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Guide](https://docs.docker.com/compose/)
- [PostgreSQL Docs](https://www.postgresql.org/docs/)
- [MySQL Docs](https://dev.mysql.com/doc/)
- [MongoDB Docs](https://docs.mongodb.com/)
- [MSSQL Docs](https://learn.microsoft.com/en-us/sql/)

---

**Tác giả**: SQL Assignments  
**Ngày cập nhật**: 2026
