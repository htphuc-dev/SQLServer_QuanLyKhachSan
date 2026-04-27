# SQLServer_QuanLyKhachSan
SQL Server  exercise with Database Design, Functions, Stored Procedures, Triggers and Cursor
# SQL Server - Quản Lý Khách Sạn

## 1. Thông tin cá nhân

- **Họ và tên:**  
- **Mã sinh viên:** 
- **Lớp:**  
- **Môn học:** SQL Server / Hệ Quản Trị Cơ Sở Dữ Liệu  




---

## 2. Giới thiệu đề tài

Đề tài được lựa chọn là **Hệ thống Quản lý Khách sạn**.

Mục tiêu của bài tập là xây dựng một cơ sở dữ liệu hoàn chỉnh bằng **Microsoft SQL Server**, sử dụng ngôn ngữ **T-SQL** để giải quyết các yêu cầu thực tế trong quản lý khách sạn như:

- Quản lý khách hàng
- Quản lý phòng
- Quản lý đặt phòng
- Tính toán dữ liệu
- Tự động hóa nghiệp vụ
- Xử lý dữ liệu nâng cao

---

## 3. Yêu cầu đầu bài

Bài tập gồm 5 phần:

### Phần 1: Thiết kế và khởi tạo cấu trúc dữ liệu

- Tạo database mới có mã sinh viên
- Tạo ít nhất 3 bảng có quan hệ
- Sử dụng nhiều kiểu dữ liệu
- Có PK, FK, CHECK CONSTRAINT

### Phần 2: Function

- Tìm hiểu các hàm build-in
- Viết:
  - Scalar Function
  - Inline Table-Valued Function
  - Multi-statement Table-Valued Function

### Phần 3: Stored Procedure

- Tìm hiểu system procedure
- Viết procedure:
  - Insert / Update
  - Output parameter
  - Trả về result set

### Phần 4: Trigger

- Tự động cập nhật dữ liệu giữa các bảng
- Quan sát trigger lồng nhau

### Phần 5: Cursor

- Duyệt từng dòng dữ liệu
- So sánh với cách không dùng cursor

---

## 4. Giới thiệu cách làm

Để hoàn thành bài tập, em chọn mô hình **Quản lý Khách sạn** vì đây là bài toán gần thực tế, dễ mở rộng và phù hợp để triển khai đầy đủ các kiến thức SQL Server.

Hệ thống gồm 3 bảng chính:

- `KhachHang`
- `Phong`
- `DatPhong`

Sau đó phát triển tiếp:

- Function xử lý tính toán
- Procedure xử lý thêm/sửa dữ liệu
- Trigger xử lý tự động
- Cursor xử lý từng bản ghi

---

# PHẦN 1 - THIẾT KẾ VÀ KHỞI TẠO CSDL

---

## 1. Tạo Database

