# Module 02 — Trang Thanh Toán (Payment Page)

> **Phiên bản:** 1.0 | **Ngày lập:** 15/04/2026

---

## 1. Mô tả

Sau khi khách hàng hoàn tất lựa chọn ngày/giờ/số người trên widget, hệ thống chuyển đến trang thanh toán độc lập (full-page). Đây là bước cuối trong luồng đặt lịch trực tuyến, nơi khách hàng xác nhận thông tin, cung cấp dữ liệu liên hệ và thực hiện thanh toán.

---

## 2. Bố cục giao diện

```
┌──────────────────────────────────────────────────────────────────┐
│                   [Logo sân golf + Ảnh banner]                    │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│   ┌──────────────────────────┐  ┌──────────────────────────┐    │
│   │     THÔNG TIN ĐẶT LỊCH   │  │   THÔNG TIN LIÊN HỆ      │    │
│   │                          │  │                          │    │
│   │  📅 Thứ 7, 19/04/2026   │  │  Họ và tên *             │    │
│   │  🕐 07:00               │  │  [___________________]   │    │
│   │  👥 3 người chơi        │  │                          │    │
│   │                          │  │  Email *                 │    │
│   │  ─────────────────────  │  │  [___________________]   │    │
│   │  Đơn giá:    XXX.000đ   │  │                          │    │
│   │  Số người:       × 3    │  │  Số điện thoại *         │    │
│   │  ─────────────────────  │  │  [___________________]   │    │
│   │  TỔNG:    XXX.000đ      │  │                          │    │
│   │                          │  │  Ghi chú                 │    │
│   │  [Ảnh thương hiệu sân]   │  │  [___________________]   │    │
│   │                          │  │                          │    │
│   └──────────────────────────┘  │  ── THANH TOÁN ──────── │    │
│                                  │  [Airwallex Card Form]   │    │
│                                  │                          │    │
│                                  │  [  XÁC NHẬN ĐẶT SÂN  ]│    │
│                                  └──────────────────────────┘    │
└──────────────────────────────────────────────────────────────────┘
```

---

## 3. Thông tin tóm tắt booking

Dữ liệu được truyền từ widget sang trang thanh toán:

| Thông tin | Ví dụ |
|-----------|-------|
| Ngày | Thứ Bảy, 19/04/2026 |
| Khung giờ | 07:00 |
| Điểm xuất phát (Tee) | Xác nhận sau |
| Số người chơi | 3 |
| Đơn giá / người | Theo bảng giá |
| Tổng tiền | Tự động tính |

---

## 4. Form thông tin liên hệ

> Thông tin này được chuyển đến bộ phận Sale để liên hệ xác nhận booking.

| Trường | Bắt buộc | Điều kiện |
|--------|----------|-----------|
| Họ và tên | ✅ | Tối thiểu 2 ký tự |
| Email | ✅ | Định dạng email hợp lệ |
| Số điện thoại | ✅ | Số Việt Nam hoặc quốc tế |
| Ghi chú | ❌ | Tối đa 500 ký tự |

---

## 5. Tích hợp thanh toán Airwallex

### 5.1 Phương thức thanh toán được hỗ trợ

| Loại thẻ | Hỗ trợ |
|----------|--------|
| Visa | ✅ |
| Mastercard | ✅ |
| American Express (Amex) | ✅ |
| JCB | ✅ |
| Discover | ✅ |
| Diners Club | ✅ |

### 5.2 Phương thức tích hợp

Sử dụng **Airwallex Embedded Elements** — biểu mẫu thẻ được nhúng trực tiếp vào trang, không điều hướng sang cổng ngoài:
- Dữ liệu thẻ không đi qua máy chủ của hệ thống
- Tuân thủ tiêu chuẩn PCI DSS
- Trải nghiệm mượt mà, không gián đoạn

### 5.3 Quy trình thanh toán

```
Khách hàng nhập thông tin thẻ (Airwallex Embedded Form)
    → Nhấn "Xác nhận đặt sân"
    → Backend khởi tạo Payment Intent qua Airwallex API
    → Frontend xác thực thanh toán với client_secret
    → Airwallex xử lý → Gửi webhook xác nhận về backend
    → Backend cập nhật trạng thái Booking → CHỜ XÁC NHẬN
    → Gửi sự kiện WebSocket đến Admin Panel
    → Điều hướng khách hàng đến trang xác nhận đặt lịch
```

### 5.4 Trạng thái thanh toán

| Trạng thái | Mô tả | Hành động hệ thống |
|-----------|-------|-------------------|
| `AUTHORIZED` | Tiền được giữ tạm thời | Chờ Sale xác nhận |
| `CAPTURED` | Tiền đã được thu | Booking chuyển HOÀN THÀNH |
| `FAILED` | Thanh toán thất bại | Hiển thị thông báo lỗi, cho phép thử lại |
| `CANCELLED` | Đã hủy | Thực hiện hoàn tiền nếu đã capture |

---

## 6. Thương hiệu (Branding)

- Header: Logo và tên sân golf
- Ảnh banner / hình ảnh đặc trưng của sân
- Màu sắc, font chữ theo bộ nhận diện thương hiệu của sân

---

## 7. Trang xác nhận đặt lịch thành công

```
┌────────────────────────────────────┐
│  ✅  ĐẶT SÂN THÀNH CÔNG           │
│                                    │
│  Mã booking: #GF-20260419-001     │
│  Ngày:  19/04/2026  —  07:00      │
│  Số người: 3 người chơi           │
│                                    │
│  Bộ phận Sale sẽ liên hệ xác nhận │
│  qua số điện thoại đã đăng ký     │
│  trong thời gian sớm nhất.        │
│                                    │
│  [Quay về trang chủ]              │
└────────────────────────────────────┘
```

---

## 8. Yêu cầu bảo mật

- Bắt buộc kết nối HTTPS
- Không lưu trữ số thẻ trên hệ thống
- Phiên làm việc hết hạn sau 15 phút không có thao tác
- Giao diện tương thích với thiết bị di động
