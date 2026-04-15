# Hệ thống Đặt Lịch Tee Time — Golf Course Booking Platform

> **Phiên bản:** 1.0
> **Ngày lập:** 15/04/2026
> **Đơn vị thực hiện:** [Tên công ty]
> **Khách hàng:** [Tên sân golf]

---

## 1. Tổng quan dự án

Dự án xây dựng nền tảng đặt lịch tee time trực tuyến dành cho sân golf 18 lỗ, bao gồm ba thành phần chính:

1. **Widget đặt lịch (iFrame)** — Tích hợp trực tiếp vào website hiện có của sân golf, cho phép khách hàng tra cứu và đặt lịch tee time mà không cần rời khỏi trang chủ.
2. **Cổng thanh toán** — Trang thanh toán bảo mật, tích hợp giải pháp Airwallex để chấp nhận các loại thẻ quốc tế phổ biến.
3. **Cổng quản trị (Admin Panel)** — Giao diện quản lý tập trung dành cho nhân viên vận hành, thay thế quy trình xử lý thủ công hiện tại.

---

## 2. Phạm vi thực hiện

### Bao gồm trong phạm vi
- Widget đặt lịch nhúng (iFrame) — dành cho khách hàng
- Trang thanh toán tích hợp Airwallex (Visa, Mastercard, Amex, JCB, Discover, Diners Club)
- Cổng quản trị: bảng tee sheet (1st Tee & 10th Tee), quản lý booking toàn diện
- Hệ thống phân quyền: Admin / Sale / Support
- Thông báo nội bộ thời gian thực qua WebSocket & Webhook

### Không bao gồm *(xác nhận thêm nếu cần)*
- Ứng dụng di động (mobile app)
- Tích hợp hệ thống POS tại quầy lễ tân
- Quản lý điểm thưởng / chương trình hội viên
- Báo cáo tài chính nâng cao

---

## 3. Kiến trúc hệ thống

```
┌─────────────────────────────────────────────────────────┐
│                   PHÍA KHÁCH HÀNG                        │
│                                                          │
│   ┌──────────────────────────────────────────────────┐  │
│   │           Widget Đặt Lịch (iFrame)               │  │
│   │    [Slider ảnh sân]  |  [Form đặt lịch]         │  │
│   └──────────────────────────────────────────────────┘  │
└──────────────────────────┬──────────────────────────────┘
                           │ HTTPS
┌──────────────────────────▼──────────────────────────────┐
│                      API Gateway                         │
│           REST API + WebSocket + Webhook handler         │
└──────┬──────────────────┬──────────────────┬────────────┘
       │                  │                  │
  ┌────▼────┐       ┌─────▼─────┐    ┌──────▼──────┐
  │   CSDL  │       │ Airwallex │    │  WebSocket  │
  │Booking  │       │  Payment  │    │   Server    │
  └─────────┘       └───────────┘    └─────────────┘
                                            │
                              ┌─────────────▼──────────┐
                              │       Admin Panel        │
                              │  (Sale / Admin / Supp.)  │
                              └─────────────────────────┘
```

---

## 4. Danh sách tài liệu đặc tả

| Tài liệu | Nội dung |
|----------|----------|
| `01_iframe_booking_widget.md` | Widget đặt lịch nhúng — giao diện, luồng người dùng |
| `02_payment_page.md` | Trang thanh toán — form liên hệ, tích hợp Airwallex |
| `03_admin_teesheet.md` | Bảng Tee Sheet — 1st Tee & 10th Tee, tính năng quản lý |
| `05_admin_user_permission.md` | Xác thực, phân quyền, quản lý tài khoản nhân sự |
| `06_realtime_notification.md` | Thông báo thời gian thực — WebSocket, Webhook |

---

## 5. Phân loại người dùng

| Vai trò | Mô tả |
|---------|-------|
| **Admin** | Toàn quyền trên hệ thống; tạo và quản lý tài khoản nhân sự; cấu hình vận hành |
| **Sale** | Quản lý booking, xác nhận lịch đặt, tự tạo booking cho khách |
| **Support** | Tra cứu thông tin, hỗ trợ giải quyết thắc mắc của khách hàng |
| **Khách hàng** | Đặt lịch qua widget — không yêu cầu tài khoản |

---

## 6. Cấu hình sân (sơ bộ)

| Thông số | Giá trị |
|----------|---------|
| Số lỗ | 18 |
| Điểm xuất phát | 1st Tee và 10th Tee (split tee) |
| Số người tối đa / slot | 4 người |
| Khoảng cách tee time | Theo cấu hình vận hành |
| Giờ hoạt động | Theo cấu hình vận hành |

---

## 7. Luồng nghiệp vụ tổng quát

### Luồng đặt lịch trực tuyến (Online Booking)
```
Khách hàng mở widget trên website
    → Chọn ngày / khung giờ / số người chơi
    → Chuyển sang trang thanh toán
    → Điền thông tin liên hệ (email, số điện thoại)
    → Thanh toán qua Airwallex
    → Hệ thống tạo booking — trạng thái: CHỜ XÁC NHẬN
    → Nhân viên Sale nhận thông báo tức thời (WebSocket)
    → Sale liên hệ xác nhận → Booking HOÀN THÀNH hoặc HỦY
```

### Luồng tạo booking thủ công (Walk-in / Điện thoại)
```
Nhân viên Sale đăng nhập Admin Panel
    → Vào bảng Tee Sheet
    → Chọn slot trống → Tạo booking mới
    → Điền thông tin khách hàng
    → Booking được tạo — trạng thái: ĐÃ XÁC NHẬN
    → Gửi Confirm Letter cho khách (nếu cần)
```

---

## 8. Công nghệ dự kiến

| Thành phần | Công nghệ |
|-----------|-----------|
| Widget Frontend | HTML / CSS / JavaScript |
| Admin Frontend | React / Next.js |
| Backend API | Node.js (NestJS) |
| Cơ sở dữ liệu | PostgreSQL |
| Real-time | WebSocket (Socket.IO) |
| Thanh toán | Airwallex Embedded Elements |
| Hosting | Xác nhận sau |
