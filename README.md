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


# PHẦN 2 - FUNCTION (HÀM XỬ LÝ NGHIỆP VỤ)

**Bản chất chung của Function:** Dùng để đóng gói các công thức tính toán hoặc logic truy xuất dữ liệu để tái sử dụng. 
**Nguyên tắc cốt lõi:** Function **KHÔNG** được phép làm thay đổi trạng thái dữ liệu của hệ thống (Tuyệt đối không sử dụng `INSERT`, `UPDATE`, `DELETE` lên các bảng vật lý bên trong Function).

Trong SQL Server, các hàm này được lưu trữ tại: `Database_Name -> Programmability -> Functions`. Bài tập này triển khai 3 loại Function cơ bản nhằm giải quyết các bài toán thực tế của khách sạn.

## 1. Scalar-valued Function (Hàm vô hướng)

- **Vị trí lưu trong SSMS:** Thư mục `Scalar-valued Functions`.
- **Bản chất:** Nhận vào các tham số và trả về đúng **một giá trị đơn duy nhất** (kiểu INT, MONEY, VARCHAR...). Thân hàm được đặt trong khối `BEGIN...END`.
- **Logic thực tế:** Tính số tiền cuối cùng khách phải trả khi check-out. Logic yêu cầu lấy số ngày ở nhân với giá phòng, sau đó trừ đi khoản tiền khách đã cọc. Nếu khách nhận và trả trong cùng một ngày, hệ thống vẫn phải tính tròn là 1 ngày lưu trú.
- **Nhận xét hiệu năng:** Kém nếu gọi hàm này trong câu lệnh `SELECT` áp dụng cho hàng chục ngàn dòng. Hệ thống sẽ phải gọi hàm này lặp đi lặp lại cho từng dòng (hiện tượng thắt cổ chai RBAR). Chỉ nên dùng cho các truy vấn tính toán nhỏ lẻ.

```sql
CREATE FUNCTION dbo.fn_TinhTienCanThanhToan (@MaDatPhong INT)
RETURNS MONEY
AS
BEGIN
    DECLARE @TongTien MONEY;
    DECLARE @SoNgay INT;

    -- Lấy thông tin số ngày, giá phòng và tiền cọc
    SELECT 
        @SoNgay = DATEDIFF(DAY, dp.NgayNhanPhong, dp.NgayTraPhong),
        @TongTien = (p.GiaPhong * 
                     CASE WHEN DATEDIFF(DAY, dp.NgayNhanPhong, dp.NgayTraPhong) = 0 
                          THEN 1 
                          ELSE DATEDIFF(DAY, dp.NgayNhanPhong, dp.NgayTraPhong) 
                     END) - dp.TienDatCoc
    FROM DatPhong dp
    INNER JOIN Phong p ON dp.MaPhong = p.MaPhong
    WHERE dp.MaDatPhong = @MaDatPhong;

    RETURN ISNULL(@TongTien, 0);
END;
GO

```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/33ccdc5e-1608-4211-8dab-6b598ae29f3f" />

test nhanh:


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/049874c2-1c0a-492d-a305-82f9f10b759b" />


## 2. Inline Table-valued Function (Hàm nội tuyến trả về bảng)
- **Vị trí lưu trong SSMS:** Thư mục Table-valued Functions.

**Bản chất:**  
Trả về một bảng dữ liệu. Điểm đặc trưng là phần thân hàm **KHÔNG có `BEGIN...END`**, nó chỉ chứa duy nhất một câu lệnh `RETURN (SELECT ...)`. Nó hoạt động giống như một `VIEW` nhưng mạnh mẽ hơn vì có thể truyền tham số.

**Logic thực tế:**  
Hỗ trợ nhân viên lễ tân. Khi khách hỏi: *"Còn phòng đôi nào trống không?"*, hàm này sẽ lọc ra các phòng có tình trạng **"Trống"** theo đúng loại phòng khách yêu cầu.

**Nhận xét hiệu năng:**  
Cực kỳ tối ưu. SQL Server Query Optimizer có thể đọc *"xuyên"* qua cấu trúc hàm này để tối ưu Execution Plan và sử dụng Index hiệu quả.  
👉 **Luôn ưu tiên dùng loại này thay vì Multi-statement TVF nếu có thể.**

```sql
CREATE FUNCTION dbo.fn_TimPhongTrong (@LoaiPhong NVARCHAR(50))
RETURNS TABLE
AS
RETURN (
    SELECT 
        SoPhong, 
        GiaPhong, 
        DienTich
    FROM Phong
    WHERE TinhTrang = N'Trống' 
      AND LoaiPhong = @LoaiPhong
);
GO
```


<img width="1920" height="1080" alt="Screenshot 2026-04-30 165817" src="https://github.com/user-attachments/assets/42996642-288f-4b54-9797-2914bcab43c6" />

test nhanh:
```sql
SELECT * FROM dbo.fn_TimPhongTrong(N'Phòng đôi');
```

<img width="1920" height="1080" alt="Screenshot 2026-04-30 165853" src="https://github.com/user-attachments/assets/6f2a4ebd-c0d1-42ba-8f59-a4ced3a1b4ba" />


## 3. Multi-statement Table-valued Function (Hàm đa lệnh trả về bảng)

- **Vị trí lưu trong SSMS:** Thư mục Table-valued Functions.

**Bản chất:**  
Trả về một bảng dữ liệu, nhưng cấu trúc của bảng trả về phải được định nghĩa rõ ràng ngay từ đầu. Phần thân hàm có `BEGIN...END`, cho phép sử dụng các cấu trúc rẽ nhánh `IF...ELSE`, khai báo biến, và bắt buộc dùng lệnh `INSERT` để đẩy dữ liệu vào bảng tạm trước khi gọi `RETURN`.

**Logic thực tế:**  
Đánh giá phân hạng khách hàng. Hệ thống cần đếm số lần đặt phòng của từng khách. Nếu số lượt đặt >= 3 thì gán mác **"Khách VIP"**, ngược lại là **"Khách Thường"**. Logic rẽ nhánh này rất khó viết gọn trong một câu lệnh `SELECT`.

**Nhận xét hiệu năng:**  
Trung bình đến thấp. Do SQL Server phải tạo một biến bảng (Table Variable) trong bộ nhớ, nó không dự đoán được chính xác số lượng dòng trả về. Điều này dễ dẫn đến việc Query Optimizer chọn kế hoạch thực thi không tối ưu khi `JOIN` với các bảng lớn khác.

```sql
CREATE FUNCTION dbo.fn_PhanLoaiKhachHang ()
RETURNS @BangKetQua TABLE 
(
    MaKhachHang INT,
    HoTen NVARCHAR(100),
    SoLuotDat INT,
    PhanHang NVARCHAR(50)
)
AS
BEGIN
    -- 1. Insert dữ liệu thô và đếm số lượt đặt vào bảng tạm
    INSERT INTO @BangKetQua (MaKhachHang, HoTen, SoLuotDat)
    SELECT 
        kh.MaKhachHang, 
        kh.HoTen, 
        COUNT(dp.MaDatPhong)
    FROM KhachHang kh
    LEFT JOIN DatPhong dp ON kh.MaKhachHang = dp.MaKhachHang
    GROUP BY kh.MaKhachHang, kh.HoTen;

    -- 2. Cập nhật logic phân hạng
    UPDATE @BangKetQua
    SET PhanHang = N'Khách VIP'
    WHERE SoLuotDat >= 3;

    UPDATE @BangKetQua
    SET PhanHang = N'Khách Thường'
    WHERE SoLuotDat < 3;

    RETURN;
END;

```
-- Lệnh kiểm tra (Test):
```sql
SELECT * FROM dbo.fn_PhanLoaiKhachHang() ORDER BY SoLuotDat DESC;
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/796bdcf6-0e63-4d55-8ec8-fa9eebb16b12" />


