# SQLServer_QuanLyKhachSan
SQL Server  exercise with Database Design, Functions, Stored Procedures, Triggers and Cursor
# SQL Server - Quản Lý Khách Sạn

## 1. Thông tin cá nhân

- **Họ và tên:**  Hoàng Trường Phúc
- **Mã sinh viên:** K235480106055
- **Lớp:**  K59KKMT.K01
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

---

# PHẦN 3 - STORED PROCEDURE (THỦ TỤC LƯU TRỮ)

Mục tiêu của phần này là xây dựng các thủ tục lưu trữ để đóng gói các quy trình nghiệp vụ phức tạp của khách sạn xuống tầng CSDL. Việc này giúp ứng dụng chạy nhanh hơn, bảo mật hơn và dữ liệu luôn đảm bảo tính toàn vẹn (thay vì phải gọi nhiều lệnh `SELECT/INSERT/UPDATE` rời rạc từ phía Backend).

## 1. Tìm hiểu System Procedure

Trong SQL Server, các System Procedure được cung cấp sẵn (thường bắt đầu bằng `sp_`) để quản trị hệ thống. Trong quá trình thiết kế cơ sở dữ liệu này, em đã sử dụng:
* `EXEC sp_help 'KhachHang';`: Để xem nhanh cấu trúc, kiểu dữ liệu và các ràng buộc của bảng.
* `EXEC sp_helptext 'fn_TinhTienCanThanhToan';`: Để xem lại mã nguồn gốc của các Function/Procedure đã tạo.

---

## 2. Các Procedure nghiệp vụ đã xây dựng

### 2.1. Procedure Thêm mới dữ liệu (Insert)
- **Tên SP:** `sp_ThemKhachHang`
- **Mục đích:** Hỗ trợ lễ tân tạo hồ sơ khách hàng mới. Tự động kiểm tra trùng lặp số điện thoại trước khi thêm để tránh tạo ra rác dữ liệu.
**Kỹ thuật sử dụng:**  
- Dùng `IF EXISTS` để kiểm tra dữ liệu trùng  
- Sử dụng `RETURN` để dừng thực thi nếu có lỗi  
- Thực hiện `INSERT` khi dữ liệu hợp lệ
- 
```sql
CREATE PROCEDURE sp_ThemKhachHang
    @HoTen NVARCHAR(100),
    @SoDienThoai VARCHAR(15),
    @Email VARCHAR(100) = NULL,
    @NgaySinh DATE = NULL,
    @DiaChi NVARCHAR(200) = NULL
AS
BEGIN
    -- 1. Kiểm tra logic: Số điện thoại đã tồn tại chưa?
    IF EXISTS (SELECT 1 FROM KhachHang WHERE SoDienThoai = @SoDienThoai)
    BEGIN
        PRINT N'Lỗi: Số điện thoại này đã tồn tại trong hệ thống!';
        RETURN; -- Dừng thực thi ngay lập tức
    END

    -- 2. Thực hiện Insert
    INSERT INTO KhachHang (HoTen, SoDienThoai, Email, NgaySinh, DiaChi)
    VALUES (@HoTen, @SoDienThoai, @Email, @NgaySinh, @DiaChi);

    PRINT N'Thêm khách hàng thành công!';
END;
GO
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0264c105-fc90-45bf-985d-bac340b3e96e" />


-- Lệnh kiểm tra (Test):
```sql
-- EXEC sp_ThemKhachHang N'Nguyễn Văn A', '0912345678', 'a@gmail.com', '1995-01-01', N'Hà Nội';
```



<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ebba3f45-be0f-49bd-a7ce-5f6176c96776" />





## 2.2. Procedure cập nhật dữ liệu (Update) có dùng Transaction

- **Tên Stored Procedure:** `sp_XuLyTraPhong`

**Mục đích:**  
Xử lý nghiệp vụ check-out. Tự động:
- Cập nhật trạng thái hóa đơn thành **"Đã trả phòng"**
- Giải phóng phòng (chuyển tình trạng phòng về **"Trống"**)

**Kỹ thuật sử dụng:**  
Sử dụng `BEGIN TRANSACTION` kết hợp `TRY...CATCH`.  
Nếu cập nhật hóa đơn thành công nhưng cập nhật phòng bị lỗi, hệ thống sẽ **ROLLBACK** để đảm bảo dữ liệu luôn nhất quán.

```sql
CREATE PROCEDURE sp_XuLyTraPhong
    @MaDatPhong INT
AS
BEGIN
    BEGIN TRY
        BEGIN TRANSACTION;

        -- 1. Cập nhật trạng thái trong bảng DatPhong
        UPDATE DatPhong
        SET TrangThai = N'Đã trả phòng'
        WHERE MaDatPhong = @MaDatPhong;

        -- 2. Lấy MaPhong từ đơn đặt
        DECLARE @MaPhong INT;
        SELECT @MaPhong = MaPhong 
        FROM DatPhong 
        WHERE MaDatPhong = @MaDatPhong;

        -- 3. Cập nhật trạng thái phòng
        UPDATE Phong
        SET TinhTrang = N'Trống'
        WHERE MaPhong = @MaPhong;

        COMMIT TRANSACTION; -- Xác nhận thay đổi
        PRINT N'Xử lý trả phòng thành công! Phòng đã được dọn trống.';
    END TRY
    BEGIN CATCH
        ROLLBACK TRANSACTION; -- Hoàn tác nếu có lỗi
        PRINT N'Có lỗi xảy ra, hệ thống đã hoàn tác thao tác!';
    END CATCH
END;
GO
```

<img width="1920" height="1080" alt="Screenshot 2026-05-01 145245" src="https://github.com/user-attachments/assets/d988cf7b-c2f0-4a1a-95c6-b68962394280" />




-- Lệnh kiểm tra (Test):
```sql
-- EXEC sp_XuLyTraPhong @MaDatPhong = 1;
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/95efb2b9-10f7-41c5-8248-bca8f1330c52" />



## 2.3. Procedure sử dụng Output Parameter

- **Tên Stored Procedure:** `sp_KiemTraPhongNhanh`

**Mục đích:**  
Khi khách hỏi một phòng cụ thể (ví dụ: *"Phòng 101"*), Stored Procedure này nhận `SoPhong` làm **Input** và trả về:
- `TinhTrang` (tình trạng phòng)
- `GiaPhong` (giá phòng)  

👉 Thông tin được trả ra thông qua **Output Parameter** để hệ thống hiển thị trực tiếp.

**Kỹ thuật sử dụng:**  
- Dùng tham số `OUTPUT` để trả dữ liệu ngược ra ngoài  
- Sử dụng `@@ROWCOUNT` để kiểm tra trường hợp không tìm thấy phòng

```sql
CREATE PROCEDURE sp_KiemTraPhongNhanh
    @SoPhong VARCHAR(10),
    @TinhTrang NVARCHAR(30) OUTPUT,
    @GiaPhong MONEY OUTPUT
AS
BEGIN
    SELECT 
        @TinhTrang = TinhTrang,
        @GiaPhong = GiaPhong
    FROM Phong
    WHERE SoPhong = @SoPhong;

    -- Xử lý trường hợp không tìm thấy phòng
    IF @@ROWCOUNT = 0
    BEGIN
        SET @TinhTrang = N'Không tìm thấy phòng';
        SET @GiaPhong = 0;
    END
END;
GO
```


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/57be4c66-f8f2-4c44-bd93-41baf1d2aef1" />

-- Lệnh kiểm tra (Test):
```sql
 DECLARE @TinhTrangTraVe NVARCHAR(30), @GiaTraVe MONEY;
EXEC sp_KiemTraPhongNhanh '101', @TinhTrangTraVe OUTPUT, @GiaTraVe OUTPUT;
SELECT @TinhTrangTraVe AS TinhTrangPhong, @GiaTraVe AS GiaTien;

```


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e1505cf2-732c-4eff-b6b5-602dd984be5c" />




## 2.4. Procedure trả về Result Set (Kết hợp Function)

- **Tên Stored Procedure:** `sp_TraCuuLichSuDatPhong`

**Mục đích:**  
Hiển thị danh sách chi tiết các lần lưu trú của một khách hàng.  
Điểm đặc biệt: Stored Procedure này **gọi lại Function** `fn_TinhTienCanThanhToan` để tự động tính tổng tiền cho từng đơn đặt phòng.

**Kỹ thuật sử dụng:**  
- Trả về **Result Set** (bảng dữ liệu) thông qua câu lệnh `SELECT`  
- Kết hợp với **Scalar Function** để tái sử dụng logic tính toán  
- Sử dụng `JOIN` để liên kết nhiều bảng

```sql
CREATE PROCEDURE sp_TraCuuLichSuDatPhong
    @MaKhachHang INT
AS
BEGIN
    SELECT 
        dp.MaDatPhong,
        p.SoPhong,
        p.LoaiPhong,
        dp.NgayNhanPhong,
        dp.NgayTraPhong,
        dp.TrangThai,
        -- Gọi lại Function để tính tổng tiền
        dbo.fn_TinhTienCanThanhToan(dp.MaDatPhong) AS TongTien
    FROM DatPhong dp
    INNER JOIN Phong p ON dp.MaPhong = p.MaPhong
    WHERE dp.MaKhachHang = @MaKhachHang
    ORDER BY dp.NgayNhanPhong DESC; -- Ngày gần nhất trước
END;
GO
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9b1efbc0-8ec5-4121-bacc-b40eea7529bf" />



Lệnh kiểm tra (Test):
```sql
EXEC sp_TraCuuLichSuDatPhong @MaKhachHang = 1;
```


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e9d46648-3004-413b-bcc3-0ead80123297" />


---

# PHẦN 4 - TRIGGER (BẪY LỖI VÀ TỰ ĐỘNG HÓA)

Mục tiêu của phần này là sử dụng Trigger để tự động hóa các quy trình thay đổi trạng thái dữ liệu ngầm bên dưới hệ thống, giúp đảm bảo tính nhất quán của cơ sở dữ liệu mà ứng dụng không cần phải gọi thêm bất kỳ câu lệnh nào.

Khác với Stored Procedure phải được gọi thủ công (`EXEC`), Trigger sẽ tự động được kích hoạt khi có sự kiện `INSERT`, `UPDATE`, hoặc `DELETE` xảy ra trên một bảng.

## 1. Chuẩn bị: Tạo bảng Nhật Ký (Log)

Để quan sát rõ hiện tượng **Trigger lồng nhau (Nested Triggers)**, em thiết kế thêm một bảng `NhatKyPhong` để hệ thống tự động ghi lại lịch sử mỗi khi một phòng thay đổi tình trạng.
```sql
CREATE TABLE [NhatKyPhong] (
    [MaLog] INT IDENTITY(1,1) PRIMARY KEY,
    [MaPhong] INT NOT NULL,
    [HanhDong] NVARCHAR(255) NOT NULL,
    [ThoiGian] DATETIME DEFAULT GETDATE()
);
GO
```

## 2. Các Trigger đã xây dựng

### 2.1. Trigger tự động cập nhật dữ liệu giữa các bảng

- **Tên Trigger:** `trg_DatPhong_CapNhatTrangThai`  
- **Gắn trên bảng:** `DatPhong`

**Mục đích:**  
Khi một bản ghi trong `DatPhong` được:
- Thêm mới (khách đặt phòng)
- Cập nhật (khách hủy / trả phòng)

👉 Hệ thống sẽ tự động cập nhật bảng `Phong` để thay đổi **Tình trạng phòng** tương ứng (*Đã đặt / Trống*).

**Kỹ thuật sử dụng:**  
- Sử dụng bảng ảo `inserted` để lấy dữ liệu vừa được thêm/cập nhật  
- Dùng `JOIN` để xử lý đúng trong trường hợp **nhiều dòng bị ảnh hưởng cùng lúc**  
- Sử dụng `SET NOCOUNT ON` để tối ưu hiệu năng

```sql
CREATE TRIGGER trg_DatPhong_CapNhatTrangThai
ON DatPhong
AFTER INSERT, UPDATE
AS
BEGIN
    SET NOCOUNT ON; -- Giảm thông báo số dòng bị ảnh hưởng

    -- 1. Khách đặt phòng
    UPDATE p
    SET p.TinhTrang = N'Đã đặt'
    FROM Phong p
    INNER JOIN inserted i ON p.MaPhong = i.MaPhong
    WHERE i.TrangThai = N'Đã đặt';

    -- 2. Khách trả phòng hoặc hủy phòng
    UPDATE p
    SET p.TinhTrang = N'Trống'
    FROM Phong p
    INNER JOIN inserted i ON p.MaPhong = i.MaPhong
    WHERE i.TrangThai IN (N'Đã trả phòng', N'Đã hủy');
END;
GO
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4d85fa40-8e1e-4b3b-881d-eed95e344dfb" />


### 2.2. Trigger ghi log (Quan sát Trigger lồng nhau)

- **Tên Trigger:** `trg_Phong_GhiLogThayDoi`  
- **Gắn trên bảng:** `Phong`

**Mục đích:**  
Giám sát bảng `Phong`.  
Mỗi khi cột **TinhTrang** bị thay đổi, Trigger sẽ tự động ghi lại lịch sử vào bảng `NhatKyPhong`.

**Kỹ thuật sử dụng:**  
- Sử dụng đồng thời:
  - `inserted` → chứa **giá trị mới**
  - `deleted` → chứa **giá trị cũ**  
- Dùng `IF UPDATE(TinhTrang)` để chỉ chạy khi đúng cột bị thay đổi  
- So sánh dữ liệu cũ và mới để tránh ghi log dư thừa

```sql
CREATE TRIGGER trg_Phong_GhiLogThayDoi
ON Phong
AFTER UPDATE
AS
BEGIN
    SET NOCOUNT ON;

    -- Chỉ xử lý khi cột TinhTrang bị cập nhật
    IF UPDATE(TinhTrang)
    BEGIN
        INSERT INTO NhatKyPhong (MaPhong, HanhDong)
        SELECT 
            i.MaPhong,
            N'Tình trạng phòng thay đổi từ [' 
            + d.TinhTrang 
            + N'] sang [' 
            + i.TinhTrang 
            + N']'
        FROM inserted i
        INNER JOIN deleted d 
            ON i.MaPhong = d.MaPhong
        -- Chỉ ghi log khi có thay đổi thực sự
        WHERE i.TinhTrang <> d.TinhTrang;
    END
END;
GO
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1867f485-5d24-4593-86eb-4b4b1428f868" />

## . Kiểm tra (Test) hoạt động của Trigger và hiệu ứng lồng nhau

Để chứng minh hệ thống Trigger hoạt động chính xác theo chuỗi (*domino*), thực hiện kịch bản test sau:  

👉 **Giả lập trường hợp một khách hàng gọi điện yêu cầu hủy đơn đặt phòng số 1 (`MaDatPhong = 1`).**

---

### Bước 1: Xem trạng thái trước khi test

Trước khi thao tác, kiểm tra trạng thái hiện tại của:
- Đơn đặt phòng  
- Phòng  
- Bảng nhật ký  

-Đưa đơn số 1 về trạng thái Đã đặt
```sql
UPDATE DatPhong SET TrangThai = N'Đã đặt' WHERE MaDatPhong = 1;
```
-- Xóa dữ liệu cũ trong bảng Log (nếu có)
```sql
TRUNCATE TABLE NhatKyPhong;
```
```sql
-- Xem trạng thái đơn đặt
SELECT MaDatPhong, TrangThai 
FROM DatPhong 
WHERE MaDatPhong = 1;
```
-- Giả sử: TrangThai = 'Đã đặt'

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d3ac7970-86fb-4925-a20e-196a8256abf3" />


-- Xem trạng thái phòng tương ứng
```sql
SELECT MaPhong, SoPhong, TinhTrang 
FROM Phong 
WHERE MaPhong = 1;
-- Giả sử: TinhTrang = 'Đã đặt'
```
-- Xem bảng log
```sql
SELECT * 
FROM NhatKyPhong;
-- Kết quả: Bảng đang trống (0 rows)
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6af0f322-6c5b-40dd-b28a-af1f948a1b71" />


Bước 2: Thực thi lệnh UPDATE (kích hoạt Trigger)

Nhân viên lễ tân cập nhật trạng thái đơn đặt phòng thành "Đã hủy".
👉 Chỉ cần chạy duy nhất 1 câu lệnh:
```sql
UPDATE DatPhong
SET TrangThai = N'Đã hủy'
WHERE MaDatPhong = 1;
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/15c072e5-2310-4bf2-9df9-f246d1c5e301" />

Bước 3: Kiểm tra kết quả sau khi Trigger chạy

Sau khi thực hiện lệnh UPDATE, hệ thống không chỉ thay đổi bảng DatPhong mà các Trigger đã tự động xử lý dây chuyền.

```sql
SELECT MaPhong, SoPhong, TinhTrang 
FROM Phong 
WHERE MaPhong = 1;
-- KẾT QUẢ: TinhTrang = 'Trống' (Trigger 2.1 hoạt động)

SELECT * 
FROM NhatKyPhong;
-- KẾT QUẢ: Xuất hiện 1 dòng log:
-- "Tình trạng phòng thay đổi từ [Đã đặt] sang [Trống]" (Trigger 2.2 hoạt động)

```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1576aeed-720e-44e8-bb03-01d7122176d5" />


Chuỗi xử lý tự động diễn ra như sau:

UPDATE DatPhong
➝ Trigger trg_DatPhong_CapNhatTrangThai chạy
➝ Cập nhật bảng Phong
➝ Trigger trg_Phong_GhiLogThayDoi chạy
➝ Ghi log vào NhatKyPhong

👉 Chỉ 1 câu lệnh nhưng kích hoạt nhiều hành động liên tiếp (Trigger lồng nhau).

---

# PHẦN 5 - CURSOR (CON TRỎ DỮ LIỆU)

Mục tiêu của phần này là áp dụng kỹ thuật Cursor để xử lý dữ liệu theo từng dòng (Row-by-row) thay vì xử lý theo tập hợp (Set-based) như các câu lệnh SQL thông thường. 

Mặc dù Cursor tốn tài nguyên hệ thống hơn, nhưng nó rất hữu ích trong các nghiệp vụ yêu cầu tính toán phức tạp phụ thuộc vào từng cá nhân, hoặc khi cần mô phỏng các tác vụ hệ thống như gửi Email, in sao kê tuần tự.

## 1. Xây dựng kịch bản dùng Cursor

**Nghiệp vụ:** Chạy chiến dịch tri ân khách hàng cuối năm. Hệ thống sẽ duyệt qua danh sách từng khách hàng, tính toán tổng số tiền họ đã chi tiêu tại khách sạn (gọi lại Function đã tạo ở Phần 2). Nếu khách có chi tiêu, hệ thống sẽ tự động in ra một thông báo mô phỏng việc gửi Email tặng mã giảm giá.


### Mã nguồn thực thi Cursor :
```sql
-- Chuẩn bị biến lưu trữ dữ liệu cho từng dòng
DECLARE @MaKhachHang INT;
DECLARE @HoTen NVARCHAR(100);
DECLARE @Email VARCHAR(100);
DECLARE @TongChiTieu MONEY;

-- BƯỚC 1: DECLARE - Khai báo Cursor lấy danh sách khách hàng
DECLARE cur_ChienDichTriAn CURSOR FOR
SELECT MaKhachHang, HoTen, Email
FROM KhachHang;

-- BƯỚC 2: OPEN - Mở Cursor để nạp dữ liệu vào bộ nhớ
OPEN cur_ChienDichTriAn;

-- BƯỚC 3: FETCH NEXT - Đọc dòng dữ liệu đầu tiên
FETCH NEXT FROM cur_ChienDichTriAn INTO @MaKhachHang, @HoTen, @Email;

-- Vòng lặp duyệt qua từng dòng cho đến khi hết tập dữ liệu (@@FETCH_STATUS = 0)
WHILE @@FETCH_STATUS = 0
BEGIN
    -- Tính tổng chi tiêu của khách hàng này bằng cách gọi Function ở Phần 2
    SELECT @TongChiTieu = SUM(dbo.fn_TinhTienCanThanhToan(MaDatPhong))
    FROM DatPhong
    WHERE MaKhachHang = @MaKhachHang AND TrangThai = N'Đã trả phòng';

    SET @TongChiTieu = ISNULL(@TongChiTieu, 0);

    -- Xử lý logic theo từng dòng: Chỉ in thư tri ân cho khách có chi tiêu
    IF @TongChiTieu > 0
    BEGIN
        PRINT N'Đang gửi email tới: ' + ISNULL(@Email, N'Chưa cập nhật') + N' | Khách hàng: ' + @HoTen;
        PRINT N'Tổng chi tiêu: ' + CAST(@TongChiTieu AS NVARCHAR) + N' VNĐ';
        PRINT N'-> Nội dung: Cảm ơn quý khách đã tin tưởng. Tặng mã giảm giá 10% cho lần đặt tiếp theo!';
        PRINT N'------------------------------------------------------------';
    END

    -- Tiếp tục lấy dòng tiếp theo
    FETCH NEXT FROM cur_ChienDichTriAn INTO @MaKhachHang, @HoTen, @Email;
END

-- BƯỚC 4: CLOSE - Đóng Cursor
CLOSE cur_ChienDichTriAn;

-- BƯỚC 5: DEALLOCATE - Giải phóng hoàn toàn Cursor khỏi bộ nhớ RAM
DEALLOCATE cur_ChienDichTriAn;
GO
```
## . Kiểm tra (Test) hoạt động của Cursor

Để chứng minh Cursor hoạt động đúng logic (duyệt tuần tự từng khách hàng và chỉ gửi thông báo cho những ai có tổng chi tiêu > 0), thiết lập kịch bản test bằng cách nạp dữ liệu giả (Mock Data) như sau:

### Bước 1: Chuẩn bị dữ liệu mẫu (Setup Data)
Tạo 2 khách hàng: một người có lịch sử "Đã trả phòng" (có phát sinh chi tiêu) và một người chỉ mới đăng ký thành viên nhưng chưa từng đặt phòng (chi tiêu = 0).
```sql
-- 1. Thêm 2 khách hàng mẫu
INSERT INTO KhachHang (HoTen, SoDienThoai, Email) 
VALUES (N'Hoàng Trường Phúc', '0999888777', 'conganh_tnut@gmail.com'),
       (N'Nguyễn Hoàng Long', '0111222333', 'noni_test@gmail.com');

-- Lấy mã khách hàng vừa tạo để làm dữ liệu đặt phòng
DECLARE @MaKHTieuTien INT = (SELECT MaKhachHang FROM KhachHang WHERE SoDienThoai = '0999888777');

-- 2. Thêm 1 phòng VIP
INSERT INTO Phong (SoPhong, LoaiPhong, GiaPhong, TinhTrang)
VALUES ('VIP-99', N'Phòng VIP', 1500000, N'Trống');

DECLARE @MaPhongVIP INT = (SELECT MaPhong FROM Phong WHERE SoPhong = 'VIP-99');

-- 3. Tạo 1 đơn đặt phòng đã hoàn tất cho khách 'Hoàng Trường Phúc' (Ở 2 ngày, cọc 500k)
INSERT INTO DatPhong (MaKhachHang, MaPhong, NgayNhanPhong, NgayTraPhong, TienDatCoc, TrangThai)
VALUES (@MaKHTieuTien, @MaPhongVIP, GETDATE()-2, GETDATE(), 500000, N'Đã trả phòng');
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fa37739a-d6dd-4b59-b4d5-6ed7c01b08a6" />

Bước 2: Thực thi Cursor và quan sát kết quả
Sau khi nạp dữ liệu thành công, em tiến hành chạy khối lệnh Cursor ở mục 1. Thay vì nhìn vào bảng kết quả (Results) thông thường, chuyển sang tab Messages trong SSMS để xem quá trình Cursor in tuần tự từng dòng.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/051f295a-d96c-468e-8f9c-6e32dffbb93c" />


## 2. So sánh với cách không dùng Cursor (Set-based)

Nếu chỉ xét trên khía cạnh **"Lấy danh sách khách hàng và tổng chi tiêu"** mà không cần hiệu ứng xử lý tuần tự từng dòng (ví dụ: gửi email), thì việc sử dụng các lệnh SQL truyền thống (`SELECT` kết hợp `JOIN`, `GROUP BY`) sẽ cho kết quả tương tự nhưng **ngắn gọn và tối ưu hơn rất nhiều**.

---

### Cách viết không dùng Cursor (Set-based)

```sql
SELECT 
    kh.MaKhachHang,
    kh.HoTen,
    kh.Email,
    ISNULL(SUM(dbo.fn_TinhTienCanThanhToan(dp.MaDatPhong)), 0) AS TongChiTieu
FROM KhachHang kh
LEFT JOIN DatPhong dp 
    ON kh.MaKhachHang = dp.MaKhachHang 
    AND dp.TrangThai = N'Đã trả phòng'
GROUP BY kh.MaKhachHang, kh.HoTen, kh.Email
HAVING ISNULL(SUM(dbo.fn_TinhTienCanThanhToan(dp.MaDatPhong)), 0) > 0;
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9ddf7972-2ec7-4388-a1eb-5c03c439b9be" />




### Bảng đánh giá và so sánh

| Tiêu chí | Dùng Cursor (Row-by-row) | Không dùng Cursor (Set-based) |
| :--- | :--- | :--- |
| **Bản chất xử lý** | Duyệt và tính toán từng dòng một | Xử lý toàn bộ tập dữ liệu cùng lúc |
| **Hiệu năng & tốc độ** | Chậm, tốn RAM & CPU (RBAR) | Rất nhanh, được SQL Server tối ưu |
| **Độ dài mã nguồn** | Dài, phải theo quy trình nhiều bước | Ngắn gọn, dễ đọc, dễ bảo trì |
| **Ứng dụng thực tế** | Khi cần xử lý tuần tự: gửi email, gọi API, logic phức tạp từng dòng | Dùng cho truy vấn CRUD, báo cáo, thống kê |

---

### ✅ Kết luận

* 👉 Luôn ưu tiên phương pháp **Set-based** thay vì **Cursor** trong hầu hết các trường hợp.
* 👉 Chỉ sử dụng **Cursor** khi:
  * Cần xử lý tuần tự từng dòng.
  * Có logic phụ thuộc giữa các dòng.
  * Hoặc phải gọi tác vụ bên ngoài (như gọi API, gửi email, logging phức tạp).

