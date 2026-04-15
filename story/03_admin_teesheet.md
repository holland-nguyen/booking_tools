# Module 03 — Bảng Quản Lý Tee Sheet (Admin Tee Sheet)

> **Phiên bản:** 1.0 | **Ngày lập:** 15/04/2026

---

## 1. Mô tả

Tee Sheet là màn hình trung tâm của cổng quản trị — nơi nhân viên Sale và Admin theo dõi, quản lý toàn bộ lịch đặt theo ngày. Hệ thống hiển thị **hai bảng song song**: **1st Tee** và **10th Tee**, tương ứng với sân golf 18 lỗ theo hình thức split tee.

Giao diện được xây dựng lại từ đầu theo hướng trực quan, thân thiện và cập nhật theo thời gian thực — thay thế quy trình quản lý thủ công hiện tại.

---

## 2. Bố cục giao diện

```
┌──────────────────────────────────────────────────────────────────────┐
│  [◀ Ngày trước]   📅 Thứ 7, 19/04/2026   [Ngày sau ▶]  [Chọn ngày]│
│  [Buổi sáng] [Buổi chiều]      [Tạo booking] [Đặt theo nhóm] [Tìm] │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌─ 1ST TEE ──────────────────────────────────────────────────┐     │
│  │ Giờ   │ Người 1   │ Người 2   │ Người 3   │ Người 4  │ TT │     │
│  │───────────────────────────────────────────────────────     │     │
│  │ 06:00 │ [Bị khóa]                                  │ 🔒  │     │
│  │ 06:10 │ Nguyễn A  │           │           │         │ ✅  │     │
│  │ 06:20 │ [Còn trống]                                │ ⬜  │     │
│  │ 06:30 │ Trần B    │ Lê C      │ Phạm D    │        │ 🟡  │     │
│  └────────────────────────────────────────────────────────────┘     │
│                                                                      │
│  ┌─ 10TH TEE ─────────────────────────────────────────────────┐     │
│  │ Giờ   │ Người 1   │ Người 2   │ Người 3   │ Người 4  │ TT │     │
│  │ 06:00 │ Smith J   │ Jones K   │           │          │ ✅  │     │
│  │ 06:10 │ [Còn trống]                                │ ⬜  │     │
│  └────────────────────────────────────────────────────────────┘     │
└──────────────────────────────────────────────────────────────────────┘
```

---

## 3. Trạng thái booking

| Trạng thái | Ký hiệu | Màu | Mô tả |
|-----------|---------|-----|-------|
| Còn trống | ⬜ | Trắng / xám nhạt | Slot chưa được đặt |
| Chờ xác nhận | 🟡 | Vàng | Khách đặt online, chờ Sale liên hệ xác nhận |
| Đã xác nhận | ✅ | Xanh lá | Booking đã hoàn tất |
| Bị khóa | 🔒 | Xám đậm | Slot không mở đặt lịch |
| Khóa toàn bộ | ⛔ | Đen | Toàn bộ tee bị chặn (xem mục 4.5) |
| Đã hủy | ❌ | Đỏ | Booking đã bị hủy |
| Danh sách chờ | ⏳ | Xanh nhạt | Slot đầy, có khách trong hàng chờ |

---

## 4. Danh sách tính năng

### 4.1 Tạo booking mới (New Booking)
Nhân viên Sale tạo booking trực tiếp cho khách không đặt qua kênh online (gặp trực tiếp, gọi điện):
- Điền thông tin: họ tên khách, số điện thoại, email, số người chơi, ghi chú
- Lựa chọn phương thức thanh toán: thu tại quầy hoặc qua hệ thống
- Booking được tạo với trạng thái **Đã xác nhận** ngay lập tức

### 4.2 Đặt theo nhóm (Group Booking)
Đặt nhiều khung giờ liên tiếp cho một nhóm lớn (giải đấu, sự kiện doanh nghiệp):
- Chọn số tee time liên tiếp
- Điền tên nhóm, số lượng người, thông tin liên hệ đại diện
- Tùy chọn nhập danh sách người chơi (nếu có)

### 4.3 Chỉnh sửa booking (Edit Booking)
Cập nhật thông tin booking đã tồn tại:
- Sửa được: họ tên, số điện thoại, số người, ghi chú
- Không sửa ngày/giờ tại đây — sử dụng tính năng **Di chuyển booking**
- Quyền thực hiện: Sale, Admin

### 4.4 Sao chép booking (Copy Booking)
Tạo bản sao booking sang ngày hoặc giờ khác, giữ nguyên thông tin khách hàng và số người chơi.

### 4.5 Di chuyển booking (Move Booking)
Chuyển booking sang ngày / giờ / tee khác:
- Chọn ngày mới, giờ mới và tee đích
- Hệ thống kiểm tra slot mới còn trống hay không trước khi di chuyển

### 4.6 Hủy booking (Cancel Booking)
Hủy một booking đang tồn tại:
- Ghi lý do hủy
- Nếu đã thanh toán trực tuyến: kích hoạt quy trình hoàn tiền qua Airwallex
- Booking chuyển trạng thái **Đã hủy**

### 4.7 Đặt theo nhóm lớn (Group Booking) — xem 4.2

### 4.8 Tìm kiếm booking (Search Booking)
Tra cứu booking theo các tiêu chí:
- Họ tên khách hàng
- Số điện thoại hoặc email
- Mã booking
- Ngày đặt
- Kết quả hiển thị dạng danh sách; nhấp vào để xem chi tiết và highlight vị trí trên tee sheet

### 4.9 Khóa khung giờ (Block Tee Time)
Vô hiệu hóa một slot cụ thể để không nhận đặt lịch:
- Ví dụ: bảo trì, ưu tiên VIP, sự kiện đặc biệt
- Bắt buộc ghi chú lý do
- Slot bị khóa không hiển thị trên widget đặt lịch của khách

### 4.10 Mở khóa khung giờ (Unblock Tee Time)
Mở lại slot đã bị khóa để tiếp tục nhận đặt lịch.

### 4.11 Khóa toàn bộ tee (Full Block / Unblock)
Chặn hoặc mở toàn bộ một tee (1st hoặc 10th) trong một khoảng thời gian xác định. Thường dùng khi tổ chức giải đấu chiếm toàn bộ một tee trong một buổi.

### 4.12 Chuyển ngày xem (Change Date)
Xem lịch tee sheet của bất kỳ ngày nào:
- Điều hướng nhanh: nút **Ngày trước / Ngày sau**
- Chọn ngày cụ thể: bộ chọn ngày trên thanh tiêu đề

### 4.13 Lọc buổi sáng / chiều (Morning / Afternoon)
Hiển thị rút gọn theo buổi để dễ quan sát:
- **Buổi sáng**: từ giờ mở cửa đến 12:00
- **Buổi chiều**: từ 12:00 đến giờ đóng cửa
- Chế độ mặc định: xem toàn bộ ngày (có thanh cuộn)

### 4.14 Danh sách chờ (Waiting List)
Khi slot đã đầy (4/4 người), khách hàng có thể được thêm vào danh sách chờ:
- Hiển thị số lượng người đang chờ tại mỗi slot
- Khi có người hủy, nhân viên Sale nhận thông báo và liên hệ người đầu danh sách

### 4.15 Gửi thư xác nhận (Confirm Letter)
Gửi thông báo xác nhận booking đến khách hàng:
- Nội dung: mã booking, ngày giờ, số người, hướng dẫn đến sân
- Kênh gửi và template: xác nhận với khách hàng

### 4.16 Register *(cần làm rõ)*
Tính năng Register trong hệ thống hiện tại — chi tiết được xác nhận trong tài liệu **Danh sách câu hỏi làm rõ**.

### 4.17 Allocate Info *(cần làm rõ)*
Chức năng gán thông tin bổ sung vào booking — chi tiết cần xác nhận.

### 4.18 Extra Item *(cần làm rõ)*
Quản lý các dịch vụ đi kèm (ví dụ: caddy, xe điện, thuê gậy...) — danh sách dịch vụ cần được cung cấp bởi sân golf.

### 4.19 Squeeze / Unsqueeze *(cần làm rõ)*
Thêm hoặc rút bỏ một slot trung gian giữa các tee time — chi tiết và điều kiện áp dụng cần được xác nhận.

### 4.20 Move Registra *(cần làm rõ)*
Di chuyển một "registration" — liên quan đến tính năng Register (4.16), sẽ được làm rõ cùng.

### 4.21 Time Sheet *(cần làm rõ)*
Màn hình hoặc báo cáo tổng hợp theo thời gian — phân biệt với Tee Sheet ở điểm nào sẽ được xác nhận.

---

## 5. Cập nhật thời gian thực

Bảng Tee Sheet tự động cập nhật khi có thay đổi:
- Booking mới từ kênh online (trạng thái Chờ xác nhận xuất hiện ngay)
- Booking được xác nhận / hủy / di chuyển bởi tab hoặc thiết bị khác
- Slot bị khóa hoặc mở khóa

Thực hiện qua **WebSocket** — nhân viên không cần tải lại trang thủ công.

---

## 6. Lưu ý về dữ liệu người chơi

Hệ thống cũ hiển thị tên từng người chơi trong mỗi slot. Hiện tại, luồng đặt lịch trực tuyến chỉ thu thập email và số điện thoại của người đặt. Vấn đề này đang được làm rõ trong tài liệu câu hỏi — kết quả xác nhận sẽ ảnh hưởng đến thiết kế form thu thập dữ liệu.
