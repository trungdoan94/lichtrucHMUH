# 📋 Lịch Trực → Google Calendar

Ứng dụng web đơn trang giúp trích xuất lịch trực từ file Excel và xuất ra file CSV để nhập vào **Google Calendar**. Toàn bộ xử lý diễn ra ngay trên trình duyệt — không gửi dữ liệu lên bất kỳ server nào.

---

## 🚀 Cách dùng

### Bước 1 — Upload file Excel
- Kéo thả hoặc nhấn **Chọn file Excel** để tải lên file `.xlsx` / `.xls`
- App tự động chọn sheet có tên chứa "lịch" hoặc "trực"

### Bước 2 — Xác nhận cột
- App đọc **2 hàng tiêu đề** của Excel:
  - **Hàng 1**: tên địa điểm (`Hoàng Mai`, `Tôn Thất Tùng`) — được forward-fill cho các cột phía dưới
  - **Hàng 2**: tên ca trực (`Nội trú`, `Đi sớm A2`, `Trực trưa`...)
- Kiểm tra cột **Ngày** và **Thứ** đã được nhận diện đúng chưa
- Tick/bỏ tick các cột ca trực muốn đưa vào lịch
- Nhấn **Phân tích lịch trực**

### Bước 3 — Chọn nhân viên
- Nhấn vào tên để xem toàn bộ ca trực trong tháng
- Bảng hiển thị đầy đủ: ngày, thứ, địa điểm, ca trực, giờ bắt đầu, giờ kết thúc

### Bước 4 — Xuất CSV
- Nhấn **Xuất CSV** để tải file về
- Tên file: `lich_truc_<Tên>_T<tháng>_<năm>.csv`

### Nhập vào Google Calendar
1. Vào [calendar.google.com](https://calendar.google.com)
2. Cài đặt ⚙️ → **Nhập & xuất** → **Nhập**
3. Chọn file CSV vừa tải → chọn lịch muốn thêm → **Nhập**

---

## 📐 Cấu trúc file Excel hỗ trợ

```
Hàng 0:  LỊCH TRỰC THÁNG 06/2026
Hàng 1:  Ngày | Thứ | Hoàng Mai        |              | Tôn Thất Tùng  | ...
Hàng 2:       |     | Nội trú | Ra trực | Đi sớm | ... | Nội trú | Đi sớm A2 | ...
Hàng 3+: dữ liệu (1/6 | Thứ Hai | Cao | Bằng | ...)
```

- Địa điểm ở hàng 1 được **tự động forward-fill** sang các cột bên phải cho đến khi gặp địa điểm mới
- Các cột ghi chú (`pk Cầu Giấy`, `PHỤ TRÁCH KHU VỰC`...) được tự động bỏ qua
- Cột **Ra trực** không được tích mặc định (không cần thêm vào Google Calendar)

---

## ⏰ Quy tắc giờ ca trực

### Ca thông thường

| Ca trực | Giờ bắt đầu | Giờ kết thúc |
|---|---|---|
| Đi sớm (tất cả biến thể) | 06:15 | 07:00 |
| Đi sớm phần mềm 6h30 | 06:30 | 07:00 |
| Trực trưa | 12:00 | 13:30 |

### Ca Nội trú — theo loại ngày

| Loại ngày | Giờ bắt đầu | Giờ kết thúc |
|---|---|---|
| Ngày thường (Thứ 2 – Thứ 6) | 16:30 | 07:00 hôm sau |
| Thứ 7 — ca Sáng | 12:00 | 17:00 cùng ngày |
| Thứ 7 — ca Tối | 17:00 | 08:00 hôm sau |
| Chủ nhật — ca Sáng | 08:00 | 17:00 cùng ngày |
| Chủ nhật — ca Tối | 17:00 | 07:00 hôm sau |
| Ngày lễ — ca Sáng | 08:00 | 17:00 cùng ngày |
| Ngày lễ — ca Tối | 17:00 | 07:00 hôm sau |
| Ngày lễ Tối → hôm sau cũng nghỉ lễ | 17:00 | **08:00** hôm sau |

> **Ngày lễ** được nhận diện tự động: ngày nào chỉ có ca Nội trú, không có ca Đi sớm hay Trực trưa thì được coi là ngày lễ/nghỉ bù.

---

## 🔧 Kỹ thuật

| Thành phần | Chi tiết |
|---|---|
| Ngôn ngữ | HTML + CSS + JavaScript thuần (không framework) |
| Đọc Excel | [SheetJS (xlsx) v0.18.5](https://sheetjs.com/) — chạy hoàn toàn trên trình duyệt |
| Font | Be Vietnam Pro, JetBrains Mono (Google Fonts) |
| Định dạng CSV | UTF-8 BOM — tương thích Google Calendar import |
| Bảo mật | Không có backend, không gửi dữ liệu ra ngoài |

---

## 📄 Định dạng CSV xuất ra

```
Subject,Start Date,Start Time,End Date,End Time,All Day Event,Description,Location
"Nội trú - Hoàng Mai",05/06/2026,16:30,06/06/2026,07:00,FALSE,"Cao - Nội trú tại Hoàng Mai (Thứ Sáu)","Hoàng Mai"
"Đi sớm A2 - Tôn Thất Tùng",05/06/2026,06:15,05/06/2026,07:00,FALSE,"Trung - Đi sớm A2 tại Tôn Thất Tùng (Thứ Sáu)","Tôn Thất Tùng"
```
