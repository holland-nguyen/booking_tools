# Module 01 — Widget Đặt Lịch (iFrame Booking Widget)

> **Phiên bản:** 1.0 | **Ngày lập:** 15/04/2026

---

## 1. Mô tả

Widget đặt lịch là điểm tiếp xúc trực tuyến đầu tiên giữa khách hàng và hệ thống. Widget được tích hợp vào website chính của sân golf thông qua thẻ HTML `<iframe>`, không yêu cầu khách hàng tạo tài khoản hay rời khỏi trang chủ sân.

---

## 2. Bố cục giao diện

```
┌─────────────────────────────────────────────────────────────┐
│                       Widget Đặt Lịch                        │
│                                                              │
│  ┌─────────────────────┐  ┌──────────────────────────────┐  │
│  │                     │  │       ĐẶTLỊCH TEE TIME       │  │
│  │   Slider ảnh sân    │  │                              │  │
│  │                     │  │  📅 Chọn ngày               │  │
│  │  [◀]  [Ảnh]  [▶]   │  │  🕐 Chọn khung giờ         │  │
│  │                     │  │  👥 Số người chơi (1–4)     │  │
│  │    • • • •          │  │                              │  │
│  │                     │  │  ┌──────────────────────┐   │  │
│  └─────────────────────┘  │  │  XEM GIÁ & ĐẶT SÂN   │   │  │
│                            │  └──────────────────────┘   │  │
│                            └──────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
```

### 2.1 Khu vực trái — Slider ảnh sân golf
- Hiển thị hình ảnh sân golf dạng trình chiếu (auto-play + điều hướng thủ công)
- Hỗ trợ 5–10 ảnh định dạng JPG / WebP
- Có chỉ báo vị trí (pagination dots) phía dưới
- Tùy chọn: hiển thị logo sân golf dạng overlay

### 2.2 Khu vực phải — Form đặt lịch

| Trường | Kiểu | Điều kiện |
|--------|------|-----------|
| Ngày | Bộ chọn ngày (Date Picker) | Không chọn ngày đã qua; chỉ hiển thị ngày có slot trống |
| Khung giờ | Danh sách slot / lưới giờ | Chỉ hiển thị slot còn chỗ |
| Số người chơi | Bộ đếm (1–4) | Tối thiểu 1, tối đa 4 |

---

## 3. Trạng thái slot thời gian

| Trạng thái | Hiển thị | Tương tác |
|-----------|----------|-----------|
| Còn trống | ✅ Màu xanh | Khách hàng có thể chọn |
| Đã đủ người (4/4) | 🔴 Màu đỏ, vô hiệu hóa | Không thể chọn |
| Bị khóa | ⚫ Màu xám | Không thể chọn |
| Đang chọn | 🔵 Xanh đậm, highlight | Đã được chọn |

---

## 4. Luồng người dùng

```
Chọn ngày
    → Tải danh sách khung giờ khả dụng
    → Chọn khung giờ
    → Chọn số người chơi
    → Nút "Xem giá & Đặt sân" kích hoạt
    → Nhấn → Chuyển sang Trang Thanh Toán (Module 02)
```

---

## 5. Hiển thị thông tin giá

Trước khi chuyển sang thanh toán, widget hiển thị tóm tắt:

| Thông tin | Ví dụ |
|-----------|-------|
| Ngày đặt | Thứ Bảy, 19/04/2026 |
| Khung giờ | 07:00 |
| Số người | 3 người chơi |
| Tổng tiền | Tính theo bảng giá vận hành |

---

## 6. Hiển thị đa thiết bị (Responsive)

| Thiết bị | Bố cục |
|---------|--------|
| Máy tính để bàn | 2 cột: slider (trái) + form (phải) |
| Máy tính bảng / Điện thoại | 1 cột: slider (trên), form (dưới) |

---

## 7. Tham số cấu hình (từ Admin Panel)

| Tham số | Mô tả |
|---------|-------|
| `tee_interval_minutes` | Khoảng cách giữa các slot (ví dụ: 10 phút) |
| `operating_hours` | Giờ mở/đóng cửa |
| `max_players_per_slot` | Số người tối đa / slot (mặc định: 4) |
| `price_per_player` | Bảng giá theo phân loại |
| `slider_images` | Danh sách ảnh cho slider |
| `blocked_dates` | Ngày đóng cửa hoặc bảo trì |

---

## 8. Yêu cầu phi chức năng

- Thời gian tải trang dưới 2 giây
- Không điều hướng người dùng ra ngoài domain khi tra cứu lịch
- Tương thích: Chrome, Safari, Firefox, Edge (2 phiên bản mới nhất)
