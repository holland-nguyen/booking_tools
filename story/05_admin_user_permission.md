# Module 05 — Phân Quyền & Quản Lý Tài Khoản

> **Phiên bản:** 1.0 | **Ngày lập:** 15/04/2026

---

## 1. Mô tả

Hệ thống kiểm soát truy cập dành cho nhân sự vận hành sân golf. Tài khoản không mở đăng ký công khai — chỉ Admin mới có quyền tạo và quản lý tài khoản.

---

## 2. Đăng nhập

- Phương thức: **Email + Mật khẩu**
- Không có đăng ký tự do
- Admin tạo tài khoản và phân quyền cho từng nhân viên

### Quy trình tạo tài khoản mới
```
Admin vào mục Quản lý người dùng
    → Nhấn "Tạo tài khoản"
    → Nhập: Họ tên, Email, Vai trò, Mật khẩu tạm thời
    → Nhân viên đăng nhập lần đầu → đổi mật khẩu
```

### Yêu cầu bảo mật
- Mật khẩu: tối thiểu 8 ký tự, bao gồm chữ hoa, chữ số và ký tự đặc biệt
- Khóa tài khoản tạm thời sau 5 lần đăng nhập sai liên tiếp
- Quên mật khẩu: đặt lại qua email
- Thời gian phiên làm việc: xác nhận với vận hành sân

---

## 3. Vai trò & Quyền hạn

> Ma trận dưới đây là đề xuất ban đầu, sẽ được xác nhận và điều chỉnh cùng khách hàng.

| Tính năng | Admin | Sale | Support |
|-----------|:-----:|:----:|:-------:|
| **Tee Sheet** | | | |
| Xem bảng Tee Sheet | ✅ | ✅ | ✅ |
| Tạo booking thủ công | ✅ | ✅ | ❌ |
| Chỉnh sửa booking | ✅ | ✅ | ❌ |
| Di chuyển booking | ✅ | ✅ | ❌ |
| Sao chép booking | ✅ | ✅ | ❌ |
| Hủy booking | ✅ | ✅ | ❌ |
| Đặt theo nhóm | ✅ | ✅ | ❌ |
| Khóa khung giờ | ✅ | ✅ | ❌ |
| Mở khóa khung giờ | ✅ | ✅ | ❌ |
| Khóa toàn bộ tee | ✅ | ❌ | ❌ |
| Xác nhận booking online | ✅ | ✅ | ❌ |
| Gửi Confirm Letter | ✅ | ✅ | ✅ |
| **Tra cứu** | | | |
| Tìm kiếm booking | ✅ | ✅ | ✅ |
| Xem chi tiết booking | ✅ | ✅ | ✅ |
| **Thanh toán** | | | |
| Xem thông tin thanh toán | ✅ | ✅ | ❌ |
| Thực hiện hoàn tiền | ✅ | ❌ | ❌ |
| **Quản lý tài khoản** | | | |
| Tạo / Sửa / Vô hiệu hóa user | ✅ | ❌ | ❌ |
| Đổi vai trò user | ✅ | ❌ | ❌ |
| **Cấu hình hệ thống** | | | |
| Cấu hình giá / slot / giờ | ✅ | ❌ | ❌ |
| Quản lý ảnh slider | ✅ | ❌ | ❌ |
| **Báo cáo** | | | |
| Xem báo cáo | ✅ | ✅ | ❌ |
| Xuất báo cáo | ✅ | ❌ | ❌ |

---

## 4. Màn hình quản lý người dùng

| Chức năng | Mô tả |
|-----------|-------|
| Danh sách người dùng | Bảng: Họ tên, Email, Vai trò, Trạng thái, Ngày tạo |
| Lọc | Theo vai trò, trạng thái Hoạt động / Vô hiệu hóa |
| Tạo mới | Form tạo tài khoản |
| Chỉnh sửa | Đổi họ tên, vai trò |
| Vô hiệu hóa | Tắt quyền truy cập (không xóa dữ liệu) |
| Đặt lại mật khẩu | Admin đặt lại mật khẩu cho nhân viên |

---

## 5. Giao diện đăng nhập

```
┌──────────────────────────────────┐
│        [Logo Sân Golf]           │
│                                  │
│        Cổng Quản Trị             │
│                                  │
│  Email                           │
│  [______________________________]│
│                                  │
│  Mật khẩu                        │
│  [______________________________]│
│                                  │
│  [          Đăng nhập          ] │
│                                  │
│  Quên mật khẩu?                  │
└──────────────────────────────────┘
```

---

## 6. Nhật ký thao tác (Audit Log) — Tùy chọn

Ghi lại các hành động quan trọng trong hệ thống:

| Thông tin ghi lại | Ví dụ |
|-------------------|-------|
| Người thực hiện | sale01@golf.com |
| Hành động | HỦY booking |
| Đối tượng | Booking #GF-20260419-003 |
| Thời điểm | 19/04/2026 09:45:22 |
| Trạng thái trước/sau | CONFIRMED → CANCELLED |

Tính năng này sẽ được xác nhận mức độ ưu tiên trong quá trình làm rõ yêu cầu.
