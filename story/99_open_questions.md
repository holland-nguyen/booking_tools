# 99 — Open Questions & Clarifications

> **Mục đích:** Danh sách tất cả câu hỏi cần hỏi client để làm rõ requirements
> **Last Updated:** 2026-04-15
> **Status:** 🔴 Chưa có câu trả lời

---

## Hướng dẫn sử dụng

Khi đã có câu trả lời, cập nhật cột **Trả lời** và đổi status ✅.
Ưu tiên hỏi theo nhóm **[P0]** trước (block development).

---

## 🔴 Nhóm A — Nghiệp vụ Booking Cốt lõi [P0 — Block dev]

| # | Câu hỏi | Lý do cần biết | Trả lời |
|---|---------|---------------|---------|
| A1 | **Interval giữa các tee time là bao nhiêu phút?** (7 phút? 8 phút? 10 phút? Có thể khác nhau giữa 1st và 10th tee không?) | Ảnh hưởng toàn bộ data model và UI tee sheet | — |
| A2 | **Giờ mở cửa và đóng cửa hàng ngày?** Có khác nhau giữa các ngày trong tuần không? (ví dụ: CN mở sớm hơn) | Config slot time | — |
| A3 | **Tee nào khách được phép chọn khi đặt online?** Chỉ 1st Tee? Hay cả 2? Hay hệ thống tự phân? | iFrame design | — |
| A4 | **Cách tính giá?** Có phân loại không:<br>- Weekday vs Weekend<br>- Peak hours vs Off-peak<br>- Hội viên vs Khách lẻ<br>- Quốc tịch (VN vs nước ngoài)<br>- Theo mùa? | Payment page, price display | — |
| A5 | **Thanh toán online: Authorize trước hay Capture ngay?**<br>- Authorize: giữ tiền, chờ Sale xác nhận rồi mới thu<br>- Capture ngay: thu tiền luôn khi đặt | Ảnh hưởng logic payment và refund | — |
| A6 | **Chính sách hoàn tiền khi cancel?**<br>- Hoàn 100%?<br>- Hoàn theo thời gian (cancel trước 24h: 100%, trước 2h: 50%...)<br>- Không hoàn? | Refund flow | — |
| A7 | **Tên người chơi trong tee sheet lấy từ đâu?**<br>- Flow online chỉ thu email, SĐT, số lượng người<br>- Tên từng người chơi: Sale nhập thủ công sau khi gọi điện?<br>- Hay cần thêm form thu tên vào payment page? | Data model, tee sheet display | — |
| A8 | **SLA xác nhận booking?** Sau khi khách đặt, Sale có bao nhiêu thời gian để gọi confirm? Nếu quá thời gian thì auto-cancel hay auto-confirm? | Booking lifecycle | — |

---

## 🟠 Nhóm B — Tính năng Admin cần làm rõ [P1 — High priority]

| # | Câu hỏi | Lý do cần biết | Trả lời |
|---|---------|---------------|---------|
| B1 | **"Register" trong tee sheet cũ làm gì?**<br>Các giả thuyết:<br>(A) Đăng ký thêm dịch vụ (caddy, cart, locker)<br>(B) Đăng ký hội viên<br>(C) Đăng ký nhân sự nội bộ<br>(D) Khác | Feature scope | — |
| B2 | **"Allocate Info" làm gì?**<br>Có phải là gán thông tin bổ sung vào booking sau xác nhận (caddy số mấy, cart số mấy...)? | Feature scope | — |
| B3 | **"Extra Item" là gì?** Danh sách các extra items hiện tại của sân? (Caddy fee, Golf cart, Club rental, Locker...) | Feature scope, pricing | — |
| B4 | **"Squeeze" / "Unsqueeze" là gì?**<br>Giả thuyết: Thêm 1 booking vào giữa 2 slot đang đặt (compressed interval). Đúng không? Khi nào dùng? | Feature scope | — |
| B5 | **"Full Block" / "Unblock" khác gì với "Block Tee Time"?**<br>Giả thuyết: Block toàn bộ 1 tee (1st hoặc 10th) trong 1 khoảng thời gian — dùng khi có giải đấu | Feature scope | — |
| B6 | **"Move Registra" là gì?** Liên quan đến Register ở B1 | Feature scope | — |
| B7 | **"Time Sheet" là gì?** Khác với Tee Sheet không? Có phải là báo cáo/tổng hợp theo giờ? | Feature scope | — |
| B8 | **"Confirm Letter" gửi qua đâu?** Email PDF? Download PDF? In trực tiếp? | Feature design | — |
| B9 | **"Quit" trong hệ thống cũ làm gì?** Đóng form? Log out? Thoát khỏi action? | Feature scope | — |
| B10 | **Waiting List hoạt động thế nào?**<br>- Khi slot full, khách đặt online có thể vào waiting list không?<br>- Khi có người cancel, Sale thông báo thủ công hay tự động?<br>- Thứ tự ưu tiên trong waiting list? | Feature design | — |
| B11 | **Group Booking có giá đặc biệt không?** Cần duyệt từ ai? | Pricing, workflow | — |

---

## 🟡 Nhóm C — Phân quyền & User Management [P1]

| # | Câu hỏi | Lý do cần biết | Trả lời |
|---|---------|---------------|---------|
| C1 | **Role Support có quyền gì cụ thể?** Chỉ xem? Hay có thể làm gì thêm? | Permission matrix | — |
| C2 | **Sale có quyền refund không?** Hay phải Admin duyệt? | Permission matrix | — |
| C3 | **Cần audit log không?** Ghi lại ai làm gì, lúc nào? | Scope & priority | — |
| C4 | **Có cần role "Manager" / "Supervisor" không?** Giữa Admin và Sale | Permission design | — |
| C5 | **Session timeout bao lâu?** (8 giờ? 24 giờ? Không timeout?) | Security config | — |
| C6 | **Nhân viên Sale có thể tự confirm booking của khách online không?** Hay chỉ Admin? | Workflow, permission | — |

---

## 🟡 Nhóm D — Thanh toán & Airwallex [P1]

| # | Câu hỏi | Lý do cần biết | Trả lời |
|---|---------|---------------|---------|
| D1 | **Airwallex account đã có chưa?** (Merchant account, API keys) | Integration timeline | — |
| D2 | **Currency?** VND? USD? Hay cả hai? | Payment config | — |
| D3 | **Sale tự đặt thủ công có cần thanh toán online không?** Hay thu tiền mặt, chuyển khoản riêng? | Booking flow | — |
| D4 | **Có cần lưu thẻ (saved card) cho khách không?** | Payment UX | — |
| D5 | **Invoice / Receipt:** Có cần xuất hóa đơn điện tử không? Tích hợp gì? (VAT invoice?) | Scope | — |

---

## 🟢 Nhóm E — UX & Technical [P2]

| # | Câu hỏi | Lý do cần biết | Trả lời |
|---|---------|---------------|---------|
| E1 | **Ngôn ngữ của hệ thống?** Tiếng Việt? Tiếng Anh? Đa ngôn ngữ? | i18n scope | — |
| E2 | **Ảnh slider sân golf:** Có sẵn assets chưa? Bao nhiêu ảnh? | iFrame design | — |
| E3 | **Brand guideline:** Màu sắc, font, logo của sân? | Design | — |
| E4 | **Domain và hosting:** Hệ thống deploy ở đâu? Cloud provider? | Infrastructure | — |
| E5 | **Thông báo cho khách hàng:** Có cần gửi gì cho khách sau khi đặt không?<br>(SMS? Zalo? Email? Hay chỉ Sale gọi điện?) | Notification scope | — |
| E6 | **Số lượng booking trung bình / ngày?** Ước tính concurrent users? | Performance sizing | — |
| E7 | **Sân golf có hệ thống nào khác không?** (POS, handicap tracker, member management) Cần tích hợp không? | Integration scope | — |
| E8 | **Dữ liệu cũ (historical bookings):** Có cần migrate không? | Data migration | — |
| E9 | **Khoảng thời gian "Morning" / "Afternoon" là bao nhiêu?** Phân chia ở giờ nào? | Tee sheet filter | — |
| E10 | **Có cần báo cáo / analytics không?** Thống kê doanh thu, occupancy rate...? | Reporting scope | — |

---

## 📋 Template Email hỏi client

> Dùng template sau khi gặp trực tiếp hoặc qua email/Zalo

---
**Subject:** [Golf Booking Project] Câu hỏi làm rõ yêu cầu - Cần phản hồi

Chào anh/chị [Tên client],

Để tiến hành thiết kế và phát triển hệ thống booking tee time, chúng tôi cần làm rõ một số điểm sau. Anh/chị vui lòng phản hồi để chúng ta có thể bắt đầu nhanh nhất:

**🔴 Ưu tiên cao (cần sớm nhất):**
1. [A1] Interval giữa các tee time là bao nhiêu phút?
2. [A3] Khi đặt online, khách chọn được tee nào (1st, 10th, hay hệ thống tự phân)?
3. [A4] Cách tính giá — phân loại theo gì?
4. [A5] Thu tiền ngay khi đặt (capture) hay chỉ giữ (authorize)?
5. [A7] Tên người chơi trong tee sheet lấy từ đâu?

**🟠 Làm rõ tính năng cũ:**
6. [B1] "Register" trong hệ thống cũ làm gì?
7. [B3] "Extra Item" là những mục nào?
8. [B4] "Squeeze / Unsqueeze" hoạt động thế nào?

Ngoài ra, nếu có thể, anh/chị cho chúng tôi xem **demo / video màn hình** của hệ thống cũ để hiểu rõ hơn luồng nghiệp vụ hiện tại.

Trân trọng cảm ơn!

---

## 📌 Ghi chú thêm

### Observations từ cuộc trò chuyện ban đầu
- Hệ thống cũ dùng email → Sale check email → confirm: **chi phí cao, chậm** → cần thay bằng real-time
- Tee sheet cũ dạng Excel: **không có real-time, khó dùng** → cần UI hiện đại hơn
- Một số tính năng (squeeze, full block, allocate) có thể là terminology của phần mềm golf cụ thể → cần xác nhận xem sân đang dùng phần mềm gì trước đây
- "Morning / Afternoon" trong hệ thống cũ có thể là do UI không scroll được → **có thể giải quyết bằng UX tốt hơn** thay vì 2 mode riêng
- "Change Date" có thể là workaround do UI cứng → **nên làm date picker linh hoạt ngay từ đầu**
