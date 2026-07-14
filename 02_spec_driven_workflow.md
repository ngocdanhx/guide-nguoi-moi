# 02 — Cách "ra chỉ định" cho AI: Spec-driven vs Vibe coding

> Mấu chốt của làm việc với AI hiệu quả nằm ở **cách anh ra chỉ định**. Sai chỗ này thì AI làm ra sản phẩm tồi, dù model đắt tiền cỡ nào.

## Vấn đề: AI không tự hiểu ý anh

Anh nói "khám bệnh nhân giường 5" — 1 bác sĩ giỏi hiểu ngay: hỏi bệnh sử, khám lâm sàng, ra chỉ định cận lâm sàng. Nhưng nếu người nghe là **sinh viên năm nhất**, họ có thể chỉ đứng đó nhìn.

AI cũng vậy. Model dù mạnh cỡ nào, khi nghe câu chỉ định mơ hồ, nó sẽ:
- Đoán 1 hướng theo cảm tính
- Làm nhanh cái nó nghĩ anh cần
- Sản phẩm ra thường **trông có vẻ đúng** nhưng thiếu chi tiết quan trọng

Cách xử lý: **viết chỉ định rõ ràng, đầy đủ trước khi bảo AI làm**. Cái đó gọi là **spec-driven** — dùng "đặc tả" làm nguồn sự thật.

---

## Hai trường phái đang cạnh tranh

### Trường phái 1 — Vibe coding

**Andrej Karpathy** (co-founder OpenAI) đặt tên "vibe coding" đầu năm 2025. Ông mô tả: "cứ nhìn, cứ nói, cứ chạy, cứ copy-paste — hầu hết là chạy được."

Nghĩa là: cảm thấy cần gì thì bảo AI làm ngay, không plan, không đọc kỹ code, thấy chạy được là thôi. Nhanh, đã tay, hưng phấn.

Thuật ngữ này thắng **Word of the Year 2025** của Collins Dictionary — vì cả thế giới đang làm theo cách này.

**Nhược điểm** (đã được đo bằng số liệu):
- Code có AI viết ra có **~1.7× lỗi lớn** và **~2.74× lỗ hổng bảo mật** so với code người viết (nghiên cứu CodeRabbit 12/2025)
- Nhiều dự án crash sau 3 tháng vì "technical debt" (nợ kỹ thuật) chồng chất
- Anh sửa cùng 1 chỗ 3-4 lần vì mỗi lần AI hiểu khác nhau

Với dự án cá nhân nhỏ (viết script 1 lần dùng, prototype 1 ngày), vibe coding OK. Với dự án 6 tháng học tập CK1 thì không.

### Trường phái 2 — Spec-driven development (SDD)

Chính GitHub gọi vibe coding là "vấn đề cần giải". Họ ra bộ công cụ **Spec Kit** với triết lý:

> **Ý định là nguồn sự thật, không phải code.**

Nghĩa là: anh viết cái anh muốn (spec), AI dịch spec thành code. Nếu code sai, sửa spec chứ không sửa code.

**Ưu điểm**:
- Giảm 70-80% số lần rewrite
- Dễ maintain vì có tài liệu tự nhiên
- Đồng nghiệp đọc spec là hiểu, không cần đọc code
- Sau 6 tháng quay lại vẫn hiểu dự án làm gì

**Nhược điểm**:
- Chậm hơn 20-30% ở tuần đầu (phải viết spec)
- Có cảm giác "phiền" — muốn làm ngay lại phải ngồi viết

**Recommend cho bác sĩ**: bắt đầu bằng SDD. Sau 1 tháng đã quen thì kết hợp cả 2 — SDD cho việc lớn, vibe cho việc nhỏ.

---

## Spec Kit là gì (nói bằng ngôn ngữ y khoa)

Spec Kit là bộ công cụ của GitHub, đóng gói quy trình **spec-driven** thành 7 bước có sẵn — mỗi bước 1 slash command.

Hình dung như quy trình khám bệnh chuẩn:

| Bước Spec Kit | Tương đương y khoa | Sản phẩm |
|---|---|---|
| `/speckit.constitution` | Xây phác đồ chung của khoa (áp dụng cho mọi ca) | 1 file "hiến pháp" dự án |
| `/speckit.specify` | Ghi bệnh sử + đặt vấn đề | 1 file mô tả yêu cầu |
| `/speckit.clarify` | Hỏi thêm bệnh nhân/người nhà làm rõ | Bổ sung vào file spec |
| `/speckit.plan` | Ra kế hoạch điều trị + xét nghiệm | File kiến trúc kỹ thuật |
| `/speckit.tasks` | Chia nhỏ task cho từng ê-kíp | Danh sách task có thứ tự |
| `/speckit.implement` | Thực hiện điều trị | Code chạy được |
| `/speckit.converge` | So bệnh án với kế hoạch, phát hiện thiếu sót | Báo cáo gap |

Không cần chạy hết 7 bước cho mọi feature. Với dự án cá nhân, thường:
- Feature nhỏ (1 tính năng đơn giản): `/speckit.specify` → `/speckit.implement`
- Feature vừa: thêm `/speckit.plan` giữa 2 bước trên
- Feature lớn (nhiều tuần): dùng cả 7 bước

---

## Ví dụ luồng làm 1 tính năng thật

Giả sử anh muốn thêm tính năng: **"Bot Telegram gửi 5 flashcard mỗi sáng 6h30"**.

### Cách vibe coding (KHÔNG NÊN)

Anh: "Làm cho tôi bot Telegram gửi flashcard mỗi sáng."

Claude: [code 200 dòng, deploy]

Anh test → phát hiện: "À, phải là 5 thẻ, không phải 10"
Claude: [sửa]

Anh: "Ơ, phải chọn thẻ due-first, không ngẫu nhiên"
Claude: [sửa]

Anh: "Timezone phải là Việt Nam"
Claude: [sửa]

Anh: "Bấm rating xong phải cập nhật SRS state"
Claude: [rewrite lớn]

...

Sau 2 tiếng, anh có 1 bot chạy được, nhưng đã sửa 8 lần, đã sinh ra 4 file rác, chưa test kỹ.

### Cách spec-driven (NÊN)

Anh gõ `/speckit.specify`, viết:

> Feature: Bot Telegram gửi flashcard sáng
>
> WHO: Bác sĩ đang học CK1, đã đăng ký bot
> WHAT:
> - 6h30 giờ VN mỗi ngày
> - Chọn 5 thẻ theo thứ tự due-first (thẻ cần ôn nhất trước)
> - Mỗi thẻ gồm câu hỏi + 4 nút inline: Nhớ / Khó / Quên / Chưa nhớ
> - User bấm → cập nhật SRS state theo SM-2
> - Cuối tuần gửi báo cáo tổng kết
> WHY: Tận dụng thời gian sáng sớm trước khi đi trực
> CONSTRAINT: Không dùng thẻ AI chế; chỉ deck đã seed từ sách CK1

Claude sinh spec chi tiết + đặt 5 câu hỏi làm rõ. Anh trả lời.

Anh gõ `/speckit.plan` → Claude sinh kiến trúc: dùng Supabase pg_cron trigger Edge Function, share DB với web app.

Anh gõ `/speckit.tasks` → Claude chia thành 6 task nhỏ, mỗi task 15-30 phút.

Anh gõ `/speckit.implement` — Claude làm từng task. Sau mỗi task dừng để anh review.

Xong sau 90 phút. **Không lần nào rewrite**. Code sạch. Có tài liệu đầy đủ cho lần sau đọc lại.

---

## Superpowers — framework kỷ luật cho AI

**Superpowers** (do Jesse Vincent tạo) là 1 bộ skill cài đặt vào Claude Code. Nó không phải quy trình như Spec Kit, mà là **kỷ luật bắt buộc**.

Triết lý: "Nếu có 1 skill để làm việc X, thì **bắt buộc** phải dùng skill đó khi làm X. Không có ngoại lệ."

Ví dụ 5 skill "phải có":

- **using-superpowers** — skill neo. Kích hoạt đầu conversation. Nhắc AI rằng: nếu có skill match tình huống, phải dùng.
- **writing-plans** — task dài > 30 phút phải viết plan trước, không code luôn
- **systematic-debugging** — có bug phải tìm root cause, không được vá triệu chứng
- **verification-before-completion** — chưa chạy verify command thì không được nói "xong"
- **no-secrets-in-git** — chặn commit `.env` hoặc file có API key

Chỉ cần cài 5 skill trên là chất lượng AI của anh tăng 30-40%.

---

## Chọn giữa Spec Kit và Superpowers thế nào?

Nếu chọn 1:

| Anh muốn | Chọn |
|---|---|
| Feature lớn, muốn spec + plan + task rõ ràng | **Spec Kit** |
| Muốn AI đi đúng process, không tự "vibe" | **Superpowers** |
| Chỉ 1-2 tuần đầu, cần đơn giản | **Superpowers** |

**Recommend cho bác sĩ**: cài **Superpowers** trước (1 lệnh cài xong, dùng ngay). Sau 2-3 tuần khi làm feature lớn hơn thì thử thêm Spec Kit.

Không xung đột nhau — cài cả 2 cùng được.

---

## Karpathy Rules — 4 nguyên tắc để AI không "over-engineer"

AI có xu hướng làm phức tạp hoá vấn đề. Cần 4 rule kỷ luật ghi vào CLAUDE.md:

1. **Think Before Coding** — trước khi code, phải nói rõ giả định. Nếu yêu cầu mơ hồ, hỏi lại 2-3 hướng để user chọn.
2. **Simplicity First** — code tối thiểu để giải bài toán. Không thêm feature "nghĩ sau sẽ cần". Nếu 200 dòng có thể viết bằng 50 dòng, rewrite.
3. **Surgical Changes** — chỉ sửa cái được yêu cầu. Không "tiện tay" refactor code lân cận.
4. **Goal-Driven Execution** — task mơ hồ → biến thành mục tiêu đo được. Chỉ dừng khi verify PASS.

Copy 4 rule này vào CLAUDE.md là AI đỡ "vẽ" rất nhiều.

---

## Bảng cheat sheet trước khi bắt đầu task lớn

Task > 30 phút, tự hỏi 5 câu:

1. Tôi biết mình muốn cái gì chưa? (WHAT + WHY) — Nếu chưa → viết spec trước
2. Có chỗ nào mơ hồ chưa clarify? — Có → hỏi Claude clarify trước
3. Có plan kỹ thuật chưa? — Chưa → dùng `/speckit.plan` hoặc bàn bạc trước
4. Có bảng task nhỏ (2-30 phút) chưa? — Chưa → dùng `/speckit.tasks`
5. Có verify command sẵn để check "done" chưa? — Chưa → viết trước

Nếu 5 câu đều "có" → an tâm gõ `/speckit.implement`.

Nếu ≥ 2 câu "chưa" → dừng, đừng gõ implement.

---

## 3 pattern áp dụng thực tế

Không có pattern nào đúng tuyệt đối. Chọn cái phù hợp giai đoạn:

**Pattern A — Slash command tự viết**
Tự viết 5-7 lệnh tắt của riêng anh: `/start`, `/specify`, `/implement`, `/review`, `/commit`. Đơn giản, control 100%. Phù hợp khi anh đã hiểu workflow.

**Pattern B — Rule dày trong CLAUDE.md + skill gating**
Viết Karpathy Rules chi tiết trong CLAUDE.md + tự viết skill "gating protocol" (hỏi user trước khi apply skill). Phù hợp khi dự án đã lớn, có nhiều convention riêng.

**Pattern C — Cài Superpowers vanilla**
Cài Superpowers plugin, dùng skill có sẵn, không customize. Phù hợp khi mới bắt đầu, muốn nhanh.

**Recommend cho bác sĩ**: **Pattern C** trong 2 tháng đầu. Sau đó mix thêm Pattern A khi có nhu cầu đặc thù (VD `/import-sach-ck1`).

---

## Nếu anh chỉ nhớ được 3 điều

1. **AI cần chỉ định rõ ràng** — spec-driven > vibe coding cho dự án nghiêm túc
2. **Spec Kit có 7 slash command chuẩn** — không cần dùng hết, cứ chọn 2-3 cái phù hợp
3. **Cài Superpowers plugin** là quyết định hời nhất — 1 lệnh, chất lượng AI tăng ngay

## Đọc tiếp

- [03_setup_may.md](03_setup_may.md) — Cài VS Code + Claude Code + Git thực tế
- [09_multi_model_workflow.md](09_multi_model_workflow.md) — Dùng nhiều AI cùng lúc (Claude + MiniMax)
