# 02 — Cách "ra chỉ định" cho AI (theo Spec Kit)

> Mấu chốt của làm việc hiệu quả với AI nằm ở **cách anh ra chỉ định**. Chỉ định mơ hồ → AI đoán → sản phẩm sai. Chỉ định rõ → AI làm đúng ngay từ lần đầu.

## Vấn đề: AI không tự hiểu ý anh

Anh nói "khám bệnh nhân giường 5" — một bác sĩ giỏi hiểu ngay: hỏi bệnh sử, khám lâm sàng, ra chỉ định cận lâm sàng. Nhưng nếu người nghe là **sinh viên năm nhất**, họ có thể chỉ đứng đó nhìn.

AI cũng vậy. Model dù mạnh cỡ nào, khi nghe chỉ định mơ hồ, sẽ:
- Đoán một hướng theo cảm tính
- Làm nhanh cái nó nghĩ anh cần
- Kết quả **trông có vẻ đúng** nhưng thiếu chi tiết quan trọng

Bác sĩ hình dung: giống ra y lệnh cho sinh viên trực. Nói "chăm sóc bệnh nhân giường 5" thì kết quả không rõ. Nói "6h sáng lấy máu công thức, 12h đo huyết áp, chiều báo cáo" thì mọi việc chạy đúng.

Giải pháp: dùng **Spec Kit** — bộ công cụ giúp anh viết chỉ định rõ ràng trước khi bảo AI làm.

---

## Spec Kit là gì

Spec Kit là bộ công cụ mã nguồn mở của GitHub. Nó đóng gói quy trình "chỉ định trước — làm sau" thành **7 bước có sẵn**, mỗi bước một lệnh tắt trong Claude Code.

Hình dung như quy trình khám bệnh chuẩn:

| Bước Spec Kit | Tương đương y khoa | Kết quả nhận được |
|---|---|---|
| `/speckit.constitution` | Xây phác đồ chung của khoa (áp dụng mọi ca) | Một tệp "hiến pháp" cho dự án |
| `/speckit.specify` | Ghi bệnh sử + đặt vấn đề | Một tệp mô tả yêu cầu |
| `/speckit.clarify` | Hỏi thêm người nhà cho rõ | Bổ sung mục "làm rõ" vào tệp yêu cầu |
| `/speckit.plan` | Lên phác đồ điều trị + xét nghiệm | Tệp kế hoạch kỹ thuật |
| `/speckit.tasks` | Chia việc cho từng ê-kíp | Danh sách công việc có thứ tự |
| `/speckit.implement` | Thực hiện điều trị | Sản phẩm chạy được |
| `/speckit.converge` | So bệnh án với phác đồ, tìm chỗ thiếu | Báo cáo còn sót gì |

Không cần làm cả 7 bước cho mọi việc. Với dự án cá nhân, tuỳ mức độ:
- **Việc nhỏ** (một chức năng đơn giản): chỉ cần `/speckit.specify` → `/speckit.implement`
- **Việc vừa**: thêm `/speckit.plan` giữa
- **Việc lớn** (nhiều tuần): dùng cả 7 bước

---

## Kịch bản thực tế: thêm chức năng "Bot Telegram gửi thẻ ôn sáng"

Đây là ví dụ luồng chạy với Spec Kit, không phải cách "vibe" (làm liều).

### Kịch bản A — Không dùng Spec Kit

Anh mở Claude Code, gõ:

> "Làm cho tôi bot Telegram gửi thẻ ôn sáng"

Claude làm ngay, ra một đống code. Anh thử:

> "Không, phải 5 thẻ không phải 10"

Claude sửa. Anh tiếp:

> "Ơ, phải chọn thẻ cần ôn nhất, không ngẫu nhiên"

Claude sửa. Anh:

> "Múi giờ Việt Nam"

Claude sửa. Anh:

> "Bấm rating xong phải cập nhật trạng thái ôn"

Claude viết lại lớn.

Sau 2 tiếng, anh có 1 bot chạy được nhưng đã sửa 8 lần, sinh ra 4 tệp rác, chưa kiểm tra kỹ.

### Kịch bản B — Dùng Spec Kit

Anh gõ `/speckit.specify`, viết:

> Chức năng: Bot Telegram gửi thẻ ôn sáng
>
> AI: Bác sĩ đang học chuyên khoa
> LÀM GÌ:
> - 6h30 sáng theo giờ Việt Nam mỗi ngày
> - Chọn 5 thẻ ưu tiên nhất (thẻ cần ôn nhất trước)
> - Mỗi thẻ có câu hỏi + 4 nút: Nhớ / Khó / Quên / Chưa nhớ
> - Bấm nút → cập nhật trạng thái theo thuật toán ôn ngắt quãng
> - Cuối tuần gửi báo cáo tổng kết
> VÌ SAO: Tận dụng thời gian sáng trước khi đi trực
> RÀNG BUỘC: Không dùng thẻ do AI tự sinh; chỉ dùng thẻ đã kiểm tra từ sách

Claude sinh bản mô tả chi tiết + đặt 5 câu hỏi làm rõ:
- "6h30 sáng có gửi cả cuối tuần không?"
- "Nếu ngày đó không có 5 thẻ đến hạn thì lấy sao?"
- "Múi giờ có tính theo Daylight Saving không?"
- ...

Anh trả lời. Claude thêm phần "làm rõ" vào tệp.

Anh gõ `/speckit.plan` → Claude sinh kiến trúc: dùng dịch vụ đám mây chạy lịch tự động, gọi API bot, chia sẻ chung cơ sở dữ liệu với web.

Anh gõ `/speckit.tasks` → Claude chia thành 6 việc nhỏ, mỗi việc 15-30 phút.

Anh gõ `/speckit.implement` — Claude làm từng việc. Sau mỗi việc dừng để anh xem.

Xong sau 90 phút. **Không lần nào phải làm lại**. Sản phẩm sạch, có tài liệu đầy đủ để 3 tháng sau đọc lại vẫn hiểu.

---

## Ba mức dùng Spec Kit (tuỳ mức độ phức tạp)

### Mức 1 — Việc nhỏ (fix chỗ chữ, đổi màu nút)

Không cần Spec Kit. Cứ nói thẳng với Claude Code:

> "Đổi màu nút Nhớ từ xanh sang tím"

Claude làm ngay, xong sau 1 phút.

### Mức 2 — Việc vừa (thêm chức năng đơn lẻ)

Dùng 2 lệnh:

1. `/speckit.specify` — viết chỉ định
2. `/speckit.implement` — làm luôn

Ví dụ: thêm nút "Bỏ qua" cho phép tạm dừng thẻ 1 tuần.

### Mức 3 — Việc lớn (chức năng mới nhiều tuần)

Dùng đủ 4-7 lệnh:

1. `/speckit.specify` — chỉ định
2. `/speckit.clarify` — làm rõ chỗ mơ hồ
3. `/speckit.plan` — kiến trúc
4. `/speckit.tasks` — chia nhỏ
5. `/speckit.implement` — làm
6. `/speckit.converge` — kiểm tra sót gì

Ví dụ: xây web app từ đầu, hoặc thêm hệ thống chia sẻ deck cho đồng nghiệp.

---

## Kịch bản 2: Xây dựng chức năng chấm bài tự luận

Giả sử anh muốn: **user nhập câu trả lời tự luận, AI so với đáp án chuẩn, cho điểm 0-10**.

**Với Spec Kit**:

1. `/speckit.specify`:
   > Chức năng: Chấm bài tự luận bằng AI
   > 
   > NGƯỜI DÙNG: Anh, đang tự học chuyên khoa
   > LÀM GÌ:
   > - Chọn một thẻ dạng tự luận
   > - Nhập câu trả lời (100-300 từ)
   > - Nhấn "Chấm"
   > - AI so với đáp án chuẩn từ sách
   > - Cho điểm 0-10 + giải thích chỗ đúng chỗ sai
   > VÌ SAO: Luyện viết đáp án theo chuẩn kỳ thi
   > RÀNG BUỘC: KHÔNG dùng AI để tự sinh đáp án chuẩn (đáp án đã có sẵn từ sách), chỉ dùng AI để so sánh

2. `/speckit.clarify` — Claude hỏi:
   - "Nếu câu trả lời hoàn toàn khác đáp án, cho 0 hay -1 để phân biệt?"
   - "Có ghi lịch sử chấm để anh xem lại không?"
   - "AI dùng model nào?"

3. `/speckit.plan` — Claude ra kế hoạch: dùng dịch vụ AI gọi qua mạng, lưu kết quả vào cơ sở dữ liệu, hiển thị trong giao diện web.

4. `/speckit.implement` — làm.

Kết quả: chức năng chạy được sau ~2-3 giờ, có tài liệu, có test.

---

## Kỷ luật cần đi kèm — Superpowers

Spec Kit là **quy trình chỉ định**. Nhưng cần thêm **kỷ luật bắt buộc** khi AI thực thi. Đó là vai trò của **Superpowers** (một bộ cài thêm cho Claude Code).

Superpowers là 5 "quy tắc thép" mà AI **bắt buộc** phải tuân:

- **Trước khi làm việc lớn, phải viết kế hoạch** — không code luôn
- **Có bug phải tìm nguyên nhân gốc** — không được vá triệu chứng
- **Chưa chạy kiểm tra thì không được nói "xong"** — bắt buộc có bằng chứng
- **Trước khi commit phải rà secret** — chặn cứng nếu có mật khẩu / khoá bí mật trong tệp
- **Neo đầu buổi** — nhắc AI rằng nếu có quy tắc match, phải dùng

Cài Superpowers là **miễn phí** (một lệnh cài trong Claude Code). Chất lượng làm việc của AI tăng ngay 30-40%.

---

## Bảng cheat sheet trước khi bắt đầu

Trước bất cứ việc gì lớn hơn 30 phút, tự hỏi 5 câu:

1. Tôi biết mình muốn gì chưa? (LÀM GÌ + VÌ SAO)
2. Có chỗ nào mơ hồ chưa làm rõ?
3. Có kế hoạch kỹ thuật chưa?
4. Có bảng chia việc nhỏ 15-30 phút chưa?
5. Có cách kiểm tra "đã xong đúng" chưa?

Nếu 5 câu đều "có" → an tâm bảo AI làm.

Nếu ≥ 2 câu "chưa" → dừng, dùng Spec Kit làm rõ trước.

---

## Chi phí

- **Spec Kit**: miễn phí (mã nguồn mở của GitHub)
- **Superpowers**: miễn phí (bộ cài thêm)
- Duy chỉ Claude Code có tính phí theo lượng dùng (chi tiết ở file [03](03_setup_may.md))

---

## Nếu anh chỉ nhớ được 3 điều

1. **AI cần chỉ định rõ ràng** — dùng Spec Kit là cách chuẩn để viết chỉ định
2. **7 lệnh Spec Kit**, không cần dùng hết — chọn 2-3 lệnh phù hợp mức phức tạp
3. **Cài Superpowers miễn phí** — 1 lệnh là chất lượng AI tăng ngay 30-40%

## Đọc tiếp

- [03_setup_may.md](03_setup_may.md) — Cài Claude Code, Spec Kit, Superpowers
- [07_du_an_da_lieu_kickoff.md](07_du_an_da_lieu_kickoff.md) — Áp dụng vào dự án cụ thể
