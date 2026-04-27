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
Mục tiêu của phần 1 là:

- Tạo một database mới theo đúng yêu cầu đề bài.
- Thiết kế ít nhất 3 bảng có quan hệ với nhau.
- Sử dụng nhiều kiểu dữ liệu khác nhau.
- Áp dụng khóa chính, khóa ngoại và các ràng buộc dữ liệu.
- Viết script T-SQL để khởi tạo database và bảng.
---

## 1. Tạo Database

Cơ sở dữ liệu gồm 3 bảng chính:
Cơ sở dữ liệu gồm 3 bảng chính:

| Tên bảng | Chức năng |
| :--- | :--- |
| `KhachHang` | Lưu thông tin khách hàng |
| `Phong` | Lưu thông tin phòng khách sạn |
| `DatPhong` | Lưu thông tin đặt phòng |

### Mối quan hệ
Mối quan hệ giữa các bảng: **KhachHang (1) ----- (n) DatPhong (n) ----- (1) Phong**

* Một khách hàng có thể có nhiều lần đặt phòng.
* Một phòng có thể được đặt nhiều lần ở các thời điểm khác nhau.
* Bảng `DatPhong` đóng vai trò liên kết giữa khách hàng và phòng.

---

## 2. Bảng [KhachHang]

### 2.1. Mục đích
Dùng để lưu thông tin cá nhân của khách hàng khi họ đặt phòng tại khách sạn.
Bảng [KhachHang] dùng để lưu thông tin cá nhân của khách hàng khi họ đặt phòng tại khách sạn.

**Các thông tin gồm:**
* Mã khách hàng
* Họ tên
* Số điện thoại
* Email
* Ngày sinh
* Địa chỉ

<img width="1920" height="1032" alt="Screenshot 2026-04-27 154551" src="https://github.com/user-attachments/assets/56e11e7b-f30e-4155-9ed5-05c5694c8650" />


 

### 2.2. Giải thích thiết kế bảng [KhachHang]

| Tên trường | Kiểu dữ liệu | Ý nghĩa | Ghi chú |
| :--- | :--- | :--- | :--- |
| `[MaKhachHang]` | INT IDENTITY(1,1) | Mã khách hàng | Tự động tăng |
| `[HoTen]` | NVARCHAR(100) | Họ tên khách hàng | Bắt buộc nhập |
| `[SoDienThoai]` | VARCHAR(15) | Số điện thoại | Bắt buộc, không trùng |
| `[Email]` | VARCHAR(100) | Email khách hàng | Có thể để trống |
| `[NgaySinh]` | DATE | Ngày sinh | Có thể để trống |
| `[DiaChi]` | NVARCHAR(200) | Địa chỉ | Có thể để trống |

### 2.3. Giải thích ràng buộc

#### Khóa chính (Primary Key)
```sql
CONSTRAINT [PK_KhachHang] PRIMARY KEY ([MaKhachHang])
```
[MaKhachHang] là khóa chính của bảng.

Khóa chính dùng để định danh duy nhất mỗi khách hàng. Mỗi khách hàng sẽ có một mã riêng và không bị trùng.

Ràng buộc UNIQUE
```SQL
CONSTRAINT [UQ_KhachHang_SoDienThoai] UNIQUE ([SoDienThoai])
```
Số điện thoại của khách hàng không được trùng nhau.

Lý do: Trong thực tế, số điện thoại thường được dùng để tìm kiếm hoặc xác nhận thông tin khách hàng.


## 3. Bảng [Phong]
Bảng [Phong] dùng để lưu thông tin các phòng trong khách sạn.

Các thông tin gồm:

Mã phòng
Số phòng
Loại phòng
Giá phòng
Diện tích
Tình trạng phòng
```sql
 CREATE TABLE [Phong]
(
    [MaPhong] INT IDENTITY(1,1),
    [SoPhong] VARCHAR(10) NOT NULL,
    [LoaiPhong] NVARCHAR(50) NOT NULL,
    [GiaPhong] MONEY NOT NULL,
    [DienTich] FLOAT NULL,
    [TinhTrang] NVARCHAR(30) NOT NULL,

    CONSTRAINT [PK_Phong] 
    PRIMARY KEY ([MaPhong]),

    CONSTRAINT [UQ_Phong_SoPhong] 
    UNIQUE ([SoPhong]),

    CONSTRAINT [CK_Phong_GiaPhong] 
    CHECK ([GiaPhong] > 0),

    CONSTRAINT [CK_Phong_TinhTrang] 
    CHECK ([TinhTrang] IN (N'Trống', N'Đã đặt', N'Đang sửa'))
);
GO
```

| Tên trường | Kiểu dữ liệu | Ý nghĩa | Ghi chú |
| :--- | :--- | :--- | :--- |
| `[MaPhong]` | INT IDENTITY(1,1) | Mã phòng | Tự động tăng |
| `[SoPhong]` | VARCHAR(10) | Số phòng | Không được trùng |
| `[LoaiPhong]` | NVARCHAR(50) | Loại phòng | Ví dụ: Phòng đơn, Phòng đôi |
| `[GiaPhong]` | MONEY | Giá thuê phòng | Phải lớn hơn 0 |
| `[DienTich]` | FLOAT | Diện tích phòng | Kiểu số thực |
| `[TinhTrang]` | NVARCHAR(30) | Tình trạng phòng | Chỉ nhận giá trị hợp lệ |

---

#### 3.2. Giải thích ràng buộc

##### Khóa chính (Primary Key)
```sql
CONSTRAINT [PK_Phong] PRIMARY KEY ([MaPhong])
```
[MaPhong] là khóa chính của bảng [Phong].

Mỗi phòng sẽ có một mã riêng biệt để hệ thống dễ dàng quản lý.

Ràng buộc UNIQUE
```SQL
CONSTRAINT [UQ_Phong_SoPhong] UNIQUE ([SoPhong])
```
Đảm bảo [SoPhong] không được trùng lặp.

Ví dụ: Khách sạn không thể có hai phòng cùng mang số 101.

Ràng buộc CHECK giá phòng
```SQL
CONSTRAINT [CK_Phong_GiaPhong] CHECK ([GiaPhong] > 0)
```
Đảm bảo giá thuê phòng luôn phải lớn hơn 0.

Ví dụ hợp lệ: 500000

Ví dụ không hợp lệ: -100000

Ràng buộc CHECK tình trạng phòng
```SQL
CONSTRAINT [CK_Phong_TinhTrang] CHECK ([TinhTrang] IN (N'Trống', N'Đã đặt', N'Đang sửa'))
```
Giới hạn [TinhTrang] chỉ được nhận một trong ba giá trị sau:
Trống
Đã đặt
Đang sửa
Lợi ích: Giúp dữ liệu thống nhất, tránh việc nhập liệu sai lệch (ví dụ: "trong", "da dat", "đặt rồi" hoặc "available").

## 4. Bảng [DatPhong]

Bảng [DatPhong] dùng để lưu thông tin đặt phòng của khách hàng.

Đây là bảng quan trọng nhất vì nó thể hiện mối quan hệ giữa khách hàng và phòng.

Giải thích thiết kế bảng [DatPhong]

| Tên trường | Kiểu dữ liệu | Ý nghĩa | Ghi chú |
| :--- | :--- | :--- | :--- |
| `[MaDatPhong]` | INT IDENTITY(1,1) | Mã đặt phòng | Tự động tăng |
| `[MaKhachHang]` | INT | Mã khách hàng | Khóa ngoại |
| `[MaPhong]` | INT | Mã phòng | Khóa ngoại |
| `[NgayDat]` | DATE | Ngày khách đặt phòng | Bắt buộc nhập |
| `[NgayNhanPhong]` | DATE | Ngày nhận phòng | Bắt buộc nhập |
| `[NgayTraPhong]` | DATE | Ngày trả phòng | Phải sau ngày nhận |
| `[TienDatCoc]` | MONEY | Tiền đặt cọc | Không được âm |
| `[TrangThai]` | NVARCHAR(30) | Trạng thái đặt phòng | Chỉ nhận giá trị hợp lệ |

<img width="1920" height="1032" alt="Screenshot 2026-04-27 154741" src="https://github.com/user-attachments/assets/215638f7-402b-44bf-9153-135943dbd2ce" />








Giải thích ràng buộc
Khóa chính
CONSTRAINT [PK_DatPhong] 
PRIMARY KEY ([MaDatPhong])

[MaDatPhong] là khóa chính của bảng [DatPhong].

Mỗi lần đặt phòng sẽ có một mã đặt phòng riêng.
Khóa ngoại liên kết với bảng [KhachHang]
```sql
CONSTRAINT [FK_DatPhong_KhachHang] 
FOREIGN KEY ([MaKhachHang]) 
REFERENCES [KhachHang]([MaKhachHang])
```
[MaKhachHang] trong bảng [DatPhong] là khóa ngoại.

Nó tham chiếu đến [MaKhachHang] trong bảng [KhachHang].

Ý nghĩa: một lượt đặt phòng phải thuộc về một khách hàng có tồn tại trong hệ thống.

Khóa ngoại liên kết với bảng [Phong]
```sql
CONSTRAINT [FK_DatPhong_Phong] 
FOREIGN KEY ([MaPhong]) 
REFERENCES [Phong]([MaPhong])
```
[MaPhong] trong bảng [DatPhong] là khóa ngoại.

Nó tham chiếu đến [MaPhong] trong bảng [Phong].

Ý nghĩa: một lượt đặt phòng phải gắn với một phòng có tồn tại trong hệ thống.

Ràng buộc CHECK ngày: 
```sql
CONSTRAINT [CK_DatPhong_Ngay] 
CHECK ([NgayTraPhong] > [NgayNhanPhong])
```
Ràng buộc này đảm bảo ngày trả phòng phải sau ngày nhận phòng.

Ví dụ hợp lệ:

Ngày nhận phòng: 2026-04-25
Ngày trả phòng: 2026-04-27

Ví dụ không hợp lệ:

Ngày nhận phòng: 2026-04-27
Ngày trả phòng: 2026-04-25
Ràng buộc CHECK tiền đặt cọc
```sql
CONSTRAINT [CK_DatPhong_TienDatCoc] 
CHECK ([TienDatCoc] >= 0)
```
Tiền đặt cọc không được âm.

Ví dụ hợp lệ:

TienDatCoc = 200000

Ví dụ không hợp lệ:

TienDatCoc = -50000
Ràng buộc CHECK trạng thái đặt phòng
```sql
CONSTRAINT [CK_DatPhong_TrangThai] 
CHECK ([TrangThai] IN (N'Đã đặt', N'Đã nhận phòng', N'Đã trả phòng', N'Đã hủy'))
```
Trạng thái đặt phòng chỉ được nhận một trong bốn giá trị:

Đã đặt
Đã nhận phòng
Đã trả phòng
Đã hủy

Việc này giúp dữ liệu không bị nhập sai hoặc thiếu thống nhất.



