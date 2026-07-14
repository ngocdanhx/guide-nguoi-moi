# 04 — Bộ đồ nghề (mỗi món làm gì, mất phí ở đâu)

> File này **không nói tên công nghệ cụ thể**. Chỉ giới thiệu **các món đồ nghề theo vai trò** và **chỗ nào miễn phí, chỗ nào trả tiền**. Khi làm thật, Claude Code sẽ chọn công nghệ phù hợp cho anh.

## Bức tranh tổng thể

Hình dung ứng dụng học tập của anh giống một bệnh viện:

- **Người dùng** = bệnh nhân (chính là anh khi dùng ứng dụng)
- **Trang web** = phòng khám chính — vào đó làm việc, xem hồ sơ
- **Bot chat** = trợ lý gọi điện — nhắc lịch, hỏi han giờ nghỉ
- **Kho hồ sơ** = nơi lưu mọi dữ liệu (thẻ ôn, câu hỏi, tiến độ)
- **Kho ảnh** = nơi lưu ảnh atlas
- **Phòng xét nghiệm tự động** = xử lý các việc phức tạp (gọi AI chấm bài, gửi thông báo)
- **Y tá tự động** = chạy công việc định kỳ (6h30 sáng gửi thẻ)

Anh không cần cài hết ngay. Bắt đầu từ 2 món (trang web + kho hồ sơ) là đã chạy được.

---

## Món 1 — Kho hồ sơ (backend)

**Vai trò**: Lưu mọi dữ liệu (thẻ ôn, câu hỏi, tiến độ ôn, lịch sử). Cung cấp đăng nhập. Chạy công việc định kỳ.

**Vì sao cần**: không có kho hồ sơ, dữ liệu chỉ nằm trên một máy — đổi máy là mất.

**Chỗ nào miễn phí, chỗ nào trả tiền**:

- **Miễn phí** cho quy mô cá nhân: lưu được khoảng 500MB dữ liệu, 1GB ảnh, hàng chục nghìn lượt gọi mỗi tháng
- **Trả tiền** khi vượt ngưỡng: khoảng 500-700 nghìn đồng/tháng, nhưng dự án cá nhân **rất khó vượt**

Với 2 tháng đầu, gần như chắc chắn ở mức miễn phí.

---

## Món 2 — Trang web (frontend)

**Vai trò**: Giao diện chính anh làm việc. Đăng nhập, xem thẻ, soạn thẻ, ôn tập, xem thống kê.

**Đặc điểm quan trọng**:
- Chạy trong trình duyệt (không cần tải app từ App Store)
- Có thể **cài vào điện thoại** như một app thật — bấm icon trên màn hình chính là mở
- **Hoạt động khi mất mạng** — trực đêm mạng chập chờn vẫn xem được nội dung đã tải

**Chỗ miễn phí, chỗ trả tiền**:

- **Miễn phí** để đưa lên mạng cho quy mô cá nhân: khoảng 100GB dữ liệu chạy qua mỗi tháng — rất thoải mái
- **Miễn phí** để dùng như app trên điện thoại — không tốn phí App Store
- **Trả tiền** chỉ khi có hàng nghìn user cùng lúc, không phải trường hợp của anh

---

## Món 3 — Bot chat (Telegram)

**Vai trò**: Gửi thông báo chủ động cho anh — thẻ ôn sáng, quiz trưa, báo cáo cuối tuần.

**Vì sao có trang web rồi vẫn cần bot?**

Trang web tuyệt vời khi anh **chủ động mở**. Nhưng anh sẽ không chủ động mở nếu quên. Bot làm ngược — **nó chủ động nhắn cho anh**.

**Kịch bản dùng bot** (5 tình huống):

- **6h30 sáng**: bot gửi 5 thẻ, anh vừa uống cà phê vừa bấm rating trên điện thoại
- **12h trưa nghỉ giữa 2 ca**: gõ `/quiz` vào bot, làm 5 câu trắc nghiệm 3 phút
- **Đêm trực có case lạ**: gõ `/case`, bot gửi 1 case study ngẫu nhiên có ảnh
- **Đi ăn cưới xa nhà**: nhắn `/tam-nghi` bot dừng gửi 3 ngày
- **Cuối tuần**: bot tự gửi báo cáo — tuần này ôn 250 thẻ, nhớ 78%, chuỗi 12 ngày

**Chỗ miễn phí, chỗ trả tiền**:

- **Bot chạy trên Telegram: hoàn toàn miễn phí** — không giới hạn số tin nhắn cho dùng cá nhân
- Chạy bot ở đâu:
  - Trên máy laptop cá nhân + hẹn giờ tự động: **miễn phí** (nhưng phải bật máy đúng giờ)
  - Trên máy chủ đám mây: **miễn phí** với dịch vụ có sẵn ở Món 1 (5-10 phút chạy mỗi ngày, dư thoải mái trong ngưỡng free)

---

## Món 4 — Kho ảnh (khi vượt 1GB)

**Vai trò**: Lưu ảnh atlas ca lâm sàng, ảnh sách quét.

**Vì sao tách riêng**:

Kho hồ sơ ở Món 1 chỉ cho lưu 1GB ảnh miễn phí. Một ảnh ca lâm sàng khoảng 500KB — được khoảng 2000 ảnh. Nếu anh tích luỹ nhiều hơn, cần một kho ảnh riêng.

**Chỗ miễn phí, chỗ trả tiền**:

- **Miễn phí** lưu đến 10GB — được khoảng 20.000 ảnh
- **Miễn phí** cho mọi lượt tải xuống (bất cứ ai xem ảnh cũng không tốn phí)
- Với 2 tháng đầu, hoàn toàn nằm trong ngưỡng miễn phí

---

## Món 5 — Phòng xét nghiệm tự động (edge functions)

**Vai trò**: Xử lý các việc phức tạp không tiện làm trên trang web:
- Nhận webhook từ Telegram khi anh bấm nút
- Gọi API AI để chấm bài tự luận
- Sinh báo cáo tự động cuối tuần

**Vì sao cần**: một số việc cần chạy trên máy chủ (không phải máy anh), để đảm bảo bảo mật và có thể chạy 24/7.

**Chỗ miễn phí, chỗ trả tiền**:

- **Miễn phí** 500.000 lượt chạy mỗi tháng — dự án cá nhân dùng khoảng 3.000-5.000 lượt, dư thoải mái
- **Trả tiền** khi vượt: khoảng 50 nghìn đồng/1 triệu lượt thêm

---

## Món 6 — AI dev (Claude Code + MiniMax)

**Vai trò**: AI viết code cho anh, hướng dẫn từng bước setup, chấm bài trong ứng dụng.

**Chỗ miễn phí, chỗ trả tiền**:

- **Trả tiền** theo lượng dùng thực tế
- **Claude Code** (AI xịn): khoảng 200-500 nghìn đồng/tháng cho dùng cá nhân
- **MiniMax** (AI rẻ): khoảng 50-150 nghìn đồng/tháng, dùng cho việc đơn giản (dịch, sắp xếp lại chữ)
- Chi tiết cách chọn AI nào lúc nào: xem file [09_multi_model_workflow.md](09_multi_model_workflow.md)

---

## Món 7 — AI chạy trên máy (miễn phí, khi cần offline)

**Vai trò**: Chạy AI đơn giản trên máy anh, không cần mạng, không tốn tiền.

**Khi nào cần**:
- Đêm trực mạng chập chờn
- Dữ liệu nhạy cảm không muốn gửi lên mạng
- Việc lặp đi lặp lại đơn giản (định dạng chữ, dịch câu ngắn)

**Yêu cầu**: máy phải có RAM ≥ 16GB cho model mạnh, RAM 8GB dùng model nhẹ hơn.

**Chỗ miễn phí, chỗ trả tiền**:

- **Hoàn toàn miễn phí** — chỉ tốn điện + phần cứng anh đã có

---

## Bảng chọn món theo nhu cầu

| Anh muốn làm gì | Cần món nào | Miễn phí không? |
|---|---|---|
| Lưu 500-800 thẻ ôn | Món 1 (kho hồ sơ) | Miễn phí (thoải mái) |
| Đăng nhập trên nhiều máy | Món 1 (kho hồ sơ) | Miễn phí |
| Nhận thẻ ôn sáng qua Telegram | Món 3 (bot) + Món 1 (kho) | Miễn phí |
| Xem ảnh atlas offline khi trực | Món 2 (web) + Món 4 (kho ảnh) | Miễn phí |
| Chấm bài tự luận bằng AI | Món 5 (phòng xét nghiệm) + Món 6 (AI) | AI tính tiền |
| Lưu progress SRS theo Anki | Món 1 (kho) + Món 5 (xử lý logic) | Miễn phí |
| Chia sẻ deck cho đồng nghiệp | Món 1 (kho) + quy tắc phân quyền | Miễn phí |
| Dùng offline hoàn toàn | Món 2 (web PWA) + Món 7 (AI local) | Miễn phí |

---

## Luồng chạy thực tế (kịch bản "sáng 6h30 nhận thẻ")

1. **Y tá tự động** (trong Món 1) đúng 6h30 giờ Việt Nam → gọi phòng xét nghiệm (Món 5)
2. **Phòng xét nghiệm** truy vấn kho hồ sơ → lấy 5 thẻ cần ôn nhất cho anh
3. Phòng xét nghiệm gọi API bot Telegram → gửi 5 tin nhắn kèm nút "Nhớ/Khó/Quên"
4. Anh nhận thông báo trên điện thoại, mở Telegram, bấm rating
5. Telegram gọi phòng xét nghiệm khác → cập nhật trạng thái ôn vào kho hồ sơ
6. Anh mở trang web trên máy tính → thấy tiến độ đã cập nhật (vì cùng chung kho hồ sơ)

Xong một luồng, khoảng 5 giây. Không cần máy anh chạy gì — mọi thứ trên đám mây.

---

## Chi phí toàn bộ trong 2 tháng đầu

| Món | Chi phí 2 tháng |
|---|---|
| Kho hồ sơ (Món 1) | 0 |
| Trang web (Món 2) | 0 |
| Bot Telegram (Món 3) | 0 |
| Kho ảnh (Món 4) | 0 |
| Phòng xét nghiệm (Món 5) | 0 |
| AI dev — Claude Code | 300-600 nghìn (dùng nhiều) |
| AI dev — MiniMax | 100-200 nghìn |
| AI chạy trên máy (Món 7) | 0 |
| Tên miền tuỳ chọn | 200-300 nghìn/năm (không bắt buộc) |
| **Tổng 2 tháng đầu** | **400-800 nghìn** |

So sánh: học phí chuyên khoa 1 khoảng 20-40 triệu/năm. Đầu tư 400-800 nghìn cho ứng dụng học tập trong 2 tháng — nếu giúp anh tăng 8-12% điểm thi thì rất đáng.

---

## Nếu anh chỉ nhớ được 3 điều

1. **Hầu hết mọi thứ miễn phí** cho dự án cá nhân — chỉ có AI dev tính tiền theo lượng dùng
2. **Món 1 (kho hồ sơ) là trung tâm** — mọi món khác đều nói chuyện qua nó
3. **Chi phí 2 tháng đầu chỉ 400-800 nghìn** — rẻ hơn 1 ngày trực

## Đọc tiếp

- [05_content_data_pipeline.md](05_content_data_pipeline.md) — Nguyên tắc chuẩn bị nội dung học không sai
- [07_du_an_da_lieu_kickoff.md](07_du_an_da_lieu_kickoff.md) — Áp dụng vào lộ trình 2 tháng
