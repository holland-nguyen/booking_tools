# Module 06 — Thông Báo Thời Gian Thực (Real-time Notification)

> **Phiên bản:** 1.0 | **Ngày lập:** 15/04/2026

---

## 1. Mục tiêu

Thay thế quy trình thông báo qua email hiện tại bằng giải pháp thời gian thực, giúp nhân viên vận hành nhận và phản hồi thông tin booking ngay lập tức mà không phụ thuộc vào hộp thư.

Giải pháp gồm hai cơ chế:
- **Webhook**: Tiếp nhận sự kiện thanh toán từ Airwallex
- **WebSocket**: Đẩy thông báo từ backend đến giao diện quản trị theo thời gian thực

---

## 2. Kiến trúc thông báo

```
┌─────────────────┐    Webhook     ┌──────────────────┐
│   Airwallex     │ ──────────────▶│   Backend API    │
│   (Thanh toán)  │                │                  │
└─────────────────┘                │  • Xác thực       │
                                    │  • Cập nhật CSDL  │
┌─────────────────┐    REST API    │  • Phát sự kiện   │
│  Widget         │ ──────────────▶│    WebSocket      │
│  (Đặt lịch)     │                └────────┬─────────┘
└─────────────────┘                         │
                                            │ WebSocket
                               ┌────────────▼───────────┐
                               │      Admin Panel        │
                               │                         │
                               │  🔔 Booking mới!       │
                               │  Toast / Thông báo      │
                               │  Tee sheet tự cập nhật  │
                               └─────────────────────────┘
```

---

## 3. Danh sách sự kiện WebSocket

### Server → Admin Client

| Sự kiện | Dữ liệu | Khi nào kích hoạt |
|---------|---------|-------------------|
| `booking.created` | ID booking, ngày, giờ, tee, số người, trạng thái | Booking mới được tạo (online hoặc thủ công) |
| `booking.confirmed` | ID booking, người xác nhận | Sale xác nhận booking |
| `booking.cancelled` | ID booking, lý do | Booking bị hủy |
| `booking.moved` | ID booking, vị trí cũ, vị trí mới | Booking được di chuyển |
| `booking.updated` | ID booking, nội dung thay đổi | Thông tin booking được sửa |
| `payment.received` | ID booking, số tiền, phương thức | Thanh toán thành công |
| `payment.refunded` | ID booking, số tiền hoàn | Hoàn tiền hoàn tất |
| `tee.blocked` | Tee, ngày, khoảng giờ, lý do | Khung giờ bị khóa |
| `tee.unblocked` | Tee, ngày, khoảng giờ | Khung giờ được mở khóa |
| `waitinglist.updated` | Slot, số người đang chờ | Danh sách chờ thay đổi |

---

## 4. Webhook từ Airwallex

### Các sự kiện cần xử lý

| Sự kiện Airwallex | Hành động hệ thống |
|-------------------|-------------------|
| `payment_intent.succeeded` | Cập nhật booking → Chờ xác nhận; thông báo WebSocket đến Admin |
| `payment_intent.payment_failed` | Cập nhật booking → Thất bại; ghi log |
| `refund.succeeded` | Cập nhật booking → Đã hủy (đã hoàn tiền) |
| `refund.failed` | Cảnh báo Admin xử lý thủ công |

### Bảo mật Webhook
- Xác thực chữ ký `X-Airwallex-Signature` trong mỗi request
- Xử lý idempotency: kiểm tra `event_id` để tránh xử lý trùng lặp
- Phản hồi `200 OK` trong vòng 15 giây; Airwallex sẽ thử lại nếu không nhận được

---

## 5. Giao diện thông báo trong Admin Panel

### Chuông thông báo (Header)
- Hiển thị số lượng thông báo chưa đọc
- Mở danh sách thông báo khi nhấp vào

### Thông báo nổi (Toast)
- Xuất hiện góc dưới bên phải khi có sự kiện mới
- Tự động biến mất sau 5 giây
- Nhấp vào để xem chi tiết booking tương ứng

### Bảng Tee Sheet
- Slot tự động thay đổi màu và trạng thái khi có sự kiện mới
- Có hiệu ứng chuyển tiếp nhẹ để nhân viên dễ nhận biết thay đổi

---

## 6. Xử lý mất kết nối

Khi kết nối WebSocket bị gián đoạn:
- Hệ thống tự động kết nối lại (theo chiến lược exponential backoff)
- Hiển thị cảnh báo "Đang mất kết nối" trên giao diện
- Fallback: gọi API polling mỗi 30 giây nếu không kết nối lại được sau 60 giây

---

## 7. Thông báo cho khách hàng *(Xem xét cho giai đoạn tiếp theo)*

Phạm vi hiện tại chỉ bao gồm thông báo nội bộ cho nhân viên. Các phương án thông báo đến khách hàng (SMS, Zalo OA, Email...) sẽ được xem xét trong giai đoạn phát triển tiếp theo, sau khi xác nhận yêu cầu với khách hàng.
