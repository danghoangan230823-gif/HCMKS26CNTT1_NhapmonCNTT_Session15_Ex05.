# Bài 5: Giao tiếp bất đồng trong doanh nghiệp 
## 1.Bối cảnh múi giờ
| Thành viên | Múi giờ | Giờ làm việc | Quy đổi sang giờ Hà Nội |
|---|---|---|---|
| 3 thành viên ở Hà Nội | GMT+7 | 09:00 - 18:00 | 09:00 - 18:00 |
| 1 Thành viên ở Nhật Bản | GMT+9 (chênh +2 giờ) | 09:00 - 18:00 | 07:00 - 16:00 |
| 1 bạn ở Mỹ (giả định bờ Đông, EST/GMT-5) | GMT-5 (chênh -12 giờ) | 09:00 - 18:00 | 21:00 - 06:00 hôm sau |

Giờ làm việc của Hà Nội, Nhật và Mỹ gần như không trùng nhau, nên chat đòi trả lời ngay sẽ làm dự án đình trệ. Lưu ý: nước Mỹ đổi giờ mùa hè/mùa đông, chênh lệch có thể là 11 hoặc 12 giờ, cần kiểm tra lại khi lên lịch.

## Phần 2: Tin nhắn mẫu nhờ fix bug (Asynchronous)

Đăng trong kênh `#ky-thuat`, tag đích danh người cần hỗ trợ:

> **[Cần hỗ trợ] Lỗi API đăng nhập trả về 500 khi token hết hạn** @Minh (Mỹ)
>
> **Bối cảnh:** Mình đang làm module đăng nhập (nhánh `feature/login`). Khi token hết hạn, API `/auth/refresh` trả về lỗi 500 thay vì 401.
>
> **Cách tái hiện:**
> 1. Đăng nhập, đợi token hết hạn (hoặc chỉnh thời hạn còn 1 phút).
> 2. Gọi `/auth/refresh` bằng token cũ.
> 3. Kết quả: 500. Mong đợi: 401.
>
> **Đã thử:** kiểm tra log (lỗi `NullPointerException` ở `TokenService.java`, dòng 48), thử bắt lỗi nhưng vẫn lỗi.
>
> **Cần bạn:** xem giúp đoạn xử lý token hết hạn và gợi ý cách sửa.
>
> **Hạn:** cần trước **17:00 thứ Sáu giờ Hà Nội (GMT+7)** để kịp demo. Bạn trả lời trong thread này khi bắt đầu ngày làm việc, không cần trả lời ngay.
>
> **Tài liệu:** log đính kèm, link PR: `(dán link)`.

Tin nhắn có đủ: bối cảnh, bước tái hiện, kết quả mong đợi, việc đã thử, yêu cầu cụ thể, hạn kèm múi giờ. Người đọc có thể bắt tay làm ngay mà không phải hỏi lại, nên tránh được lỗi No-Hello.

## Phần 3: Lịch trình gửi tin (Schedule Send)

Cách dùng: Lark hoặc Slack, bấm mũi tên cạnh nút Gửi, chọn **Schedule send**, đặt ngày giờ.

Giả sử mình soạn tin lúc **14:00 giờ Hà Nội**. Mình hẹn gửi để tin đến đúng lúc mỗi người bắt đầu ngày làm:

| Người nhận | Giờ gửi (giờ Hà Nội) | Giờ địa phương người nhận | Lý do |
|---|---|---|---|
| Bạn ở Mỹ | 21:00 | 09:00 sáng | Tin đến đầu ngày làm việc, không đánh thức giữa đêm |
| Bạn ở Nhật | 07:00 hôm sau | 09:00 sáng | Tránh gửi lúc 14:00 vì đã gần tối, dễ bị bỏ sót |
| 3 bạn Hà Nội | 08:30 hôm sau | 08:30 sáng | Đầu ngày làm việc, mọi người chưa bị ngắt quãng |

**Quy tắc kèm theo:**
- Không gửi tin trong khung 22:00 - 07:00 giờ địa phương của người nhận.
- Luôn ghi hạn kèm múi giờ (ví dụ "17:00 GMT+7") để không hiểu nhầm.
- Chỉ việc thật sự khẩn (hệ thống sập) mới gọi trực tiếp, không dùng chat thường.