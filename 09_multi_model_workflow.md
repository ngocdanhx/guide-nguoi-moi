# 09 — Dùng nhiều AI cùng lúc (rẻ hơn 40-60%)

> Không có 1 AI nào tốt nhất cho mọi việc. Biết chia việc cho đúng AI giúp anh **giảm 40-60% chi phí** mà chất lượng vẫn giữ.

## Vấn đề: Dùng 1 AI xịn cho mọi việc = tốn tiền

**Claude Sonnet** (AI của Anthropic) rất mạnh, hiểu bối cảnh tốt. Nhưng đắt.

Cùng một việc "dịch 200 câu tiếng Anh y khoa sang tiếng Việt":
- Dùng Claude Sonnet: khoảng 25 nghìn đồng
- Dùng MiniMax (AI của Trung Quốc): khoảng 4 nghìn đồng — rẻ hơn 6 lần
- Chất lượng thực tế: gần bằng nhau cho việc dịch đơn giản

Nếu chỉ dùng Claude cho tất cả, sau 2 tháng: khoảng 800 nghìn - 1,2 triệu. Nếu chia đúng: chỉ 400-600 nghìn. **Tiết kiệm 400-600 nghìn cho 2 tháng đầu.**

Chưa kể có việc nhạy cảm dữ liệu (ví dụ ghi chú ca bệnh) — không muốn gửi lên mạng → cần AI chạy trên máy anh.

---

## Nguyên tắc chia việc cho AI

Chia thành 3 nhóm việc, mỗi nhóm 1 loại AI:

### Việc "khó" — dùng Claude

Cần suy nghĩ nhiều bước, có bối cảnh phức tạp:

- Thiết kế cấu trúc dự án (ví dụ "nên tách bảng thành 2 bảng không?")
- Sửa lỗi khó (đã thử 2 lần chưa xong)
- Viết chỉ định lớn, kế hoạch phức tạp
- Xem lại code trước khi hoàn tất

**Chi phí**: 200-500 nghìn/tháng cho dùng cá nhân.

### Việc "vừa" — dùng MiniMax (hoặc Gemini Flash)

Đơn giản hơn, không cần suy nghĩ nhiều bước:

- Nhờ AI đọc PDF, chuyển thành thẻ ôn
- Dịch tiếng Anh y khoa ↔ tiếng Việt
- Định dạng lại text
- Sinh code khung mẫu (biểu mẫu thêm/sửa đơn giản)
- Giải thích code từng dòng
- Gắn nhãn tự động ("câu hỏi này thuộc bệnh nào?")
- Tóm tắt log lỗi

**Chi phí**: 50-150 nghìn/tháng.

### Việc "đơn giản" — dùng AI chạy trên máy (miễn phí)

Rất đơn giản, lặp đi lặp lại, hoặc cần offline:

- Định dạng JSON đơn giản
- Xử lý hàng loạt (batch 1000 tệp)
- Việc nhạy cảm dữ liệu không gửi lên mạng
- Trực đêm không có mạng
- Thử nhanh không muốn tốn tiền

**Chi phí**: 0 (chỉ tốn điện + máy có sẵn của anh).

---

## MiniMax là gì (giới thiệu nhanh)

**MiniMax** là công ty AI Trung Quốc (Thượng Hải). Có nhiều loại AI, trong đó:
- **MiniMax M3** — mạnh nhất, hiểu ngữ cảnh dài, dùng cho việc phức tạp
- **MiniMax M2.7 highspeed** — rẻ nhất, nhanh nhất, dùng cho việc đơn giản

**Điểm hay nhất cho anh**: MiniMax nói chuyện được với Claude Code bằng cùng "ngôn ngữ" như AI thật của Anthropic. Nghĩa là anh có thể dùng Claude Code chạy MiniMax **chỉ bằng cách đổi 1 khoá**, không cần học công cụ mới.

Cụ thể:
- Sáng anh gõ `claude` → dùng AI Claude (cho việc chính)
- Chiều anh gõ `claude-mini` (một cái tên tắt anh đặt) → cùng Claude Code nhưng bên trong là MiniMax
- Trải nghiệm giống hệt nhau, chỉ khác chi phí

---

## Kịch bản chia việc cho từng ngày

### Kịch bản A — Tuần bận (đang trực nhiều)

- **Buổi sáng có thời gian**: dùng `claude` (AI xịn) sửa lỗi trên web
- **Trực đêm bí ý tưởng**: dùng AI trên máy để tra syntax, không mất data lên mạng

### Kịch bản B — Cuối tuần soạn nội dung hàng loạt

- **Sáng thứ 7**: dùng `claude-mini` (rẻ) đọc 3 chương sách → 150 thẻ nháp
- **Trưa thứ 7**: dùng `claude` (xịn) duyệt 20 thẻ ngẫu nhiên, kiểm tra chất lượng
- Nếu tỷ lệ sai > 10% → chuyển sang `claude` cho phần còn lại
- **Chiều thứ 7**: duyệt thẻ đã kiểm tra, gắn nhãn, lưu

### Kịch bản C — Sửa lỗi phức tạp

- **Bước 1**: dùng `claude` với quy tắc "tìm nguyên nhân gốc" — hiểu vấn đề
- **Bước 2**: hiểu rồi thì dùng `claude-mini` viết sửa (việc đơn giản)

### Kịch bản D — Trước ngày thi 1 tuần

- Dùng MiniMax hoặc Gemini Flash chấm 500 câu trả lời tự luận
- Chi phí thấp — không cần suy nghĩ phức tạp

---

## Bảng giá tham khảo (đơn vị: đồng cho 1 triệu chữ)

Không cần nhớ chính xác. Chỉ cần biết **thứ tự đắt-rẻ**:

| AI | Đầu vào | Đầu ra | Dùng khi nào |
|---|---|---|---|
| Claude Opus | ~350k | ~1,7 triệu | Suy nghĩ cực sâu (hiếm dùng) |
| Claude Sonnet | ~70k | ~350k | Việc chính, cần chất lượng |
| Claude Haiku | ~18k | ~90k | Việc nhanh, đơn giản |
| Gemini Pro | ~28k | ~230k | Nhiều ngữ cảnh |
| Gemini Flash | ~7k | ~55k | Rẻ + đủ dùng |
| MiniMax M3 | ~9k | ~35k | Nhiều ngôn ngữ |
| MiniMax M2.7 highspeed | ~5k | ~18k | Rẻ nhất, đơn giản |
| AI trên máy (Ollama) | 0 | 0 | Miễn phí, offline |

**Ước lượng dự án cá nhân 2 tháng**:
- Việc lớn (thiết kế, xem code): ~100 việc x 30k chữ = 3 triệu chữ
- Việc vừa (đọc sách, dịch, gắn nhãn): ~200 việc x 5k chữ = 1 triệu chữ
- Chỉ Claude: khoảng 800 nghìn - 1,2 triệu
- Chia đúng (Claude + MiniMax): khoảng 400-600 nghìn

**Tiết kiệm khoảng nửa.**

---

## Cách setup (không đi vào chi tiết command)

Không nói câu lệnh cụ thể. Khi anh cần cài, gõ vào Claude Code: **"hướng dẫn tôi setup MiniMax để chạy song song với Claude"** — Claude sẽ chỉ từng bước.

Ý chính:
1. Đăng ký tài khoản MiniMax (miễn phí), tạo khoá dùng API
2. Nạp trước 100-200 nghìn để chạy thử
3. Cấu hình 2 tên tắt: `claude` (thật) và `claude-mini` (MiniMax)
4. Từ đó anh tự chọn tên nào cho việc gì

Cài xong mất 10-15 phút.

---

## Công cụ hỗ trợ đa AI

Ngoài Claude Code, có 2 công cụ hữu ích cài thêm trong VS Code:

### Cline (miễn phí)

Một bộ cài thêm cho VS Code, hỗ trợ nhiều loại AI (Claude, MiniMax, Gemini, GPT). Có nút dropdown chọn AI nào cho việc nào.

**Khi nào dùng Cline thay Claude Code**:
- Muốn so sánh nhanh cùng 1 câu hỏi với 3 AI khác nhau
- Việc đơn lẻ, không cần workflow phức tạp

### Continue.dev (miễn phí)

Tương tự Cline nhưng mạnh về **AI chạy trên máy**. Có gợi ý tự động khi anh gõ code (như Copilot).

**Khi nào dùng**:
- Muốn có gợi ý tự động khi gõ
- Chủ yếu dùng AI trên máy để tiết kiệm

**Có thể cài cả 2** — không xung đột. Em khuyên cài **Cline** trước.

---

## Ollama — AI chạy trên máy miễn phí

**Ollama** là công cụ cho phép chạy AI trên máy anh, không cần mạng, không tốn tiền.

**Yêu cầu máy**:
- RAM ≥ 16GB → chạy được AI 7 tỷ tham số (mạnh nhất trong tầm này)
- RAM 8GB → chỉ chạy được AI 3 tỷ nhẹ hơn
- Không cần card đồ hoạ (chạy CPU cũng được, chỉ chậm hơn)

**Chất lượng so với Claude**:
- Yếu hơn Claude Sonnet khoảng 50-70%
- Không dùng cho việc quan trọng
- Chỉ dùng cho: định dạng, dịch câu ngắn, xử lý hàng loạt, khi offline

**Chi phí**: 0.

---

## Kịch bản cụ thể để tiết kiệm

Áp dụng cho tuần soạn nội dung điển hình:

**Việc**: Đọc 5 chương sách CK1, sinh 300 thẻ ôn.

**Cách đắt** (chỉ Claude Sonnet):
- 5 chương x ~10 nghìn chữ đầu vào = 50 nghìn chữ
- Sinh 300 thẻ x ~200 chữ đầu ra = 60 nghìn chữ
- Chi phí: 50k × 70k/triệu + 60k × 350k/triệu ≈ **~25 nghìn**
- Cộng thêm duyệt thẻ (dùng Claude luôn): ~15 nghìn nữa
- **Tổng: ~40 nghìn**

**Cách rẻ** (MiniMax M2.7 + Claude Sonnet review):
- MiniMax đọc + sinh 300 thẻ: 110k chữ × 15k/triệu = **~2 nghìn**
- Claude Sonnet review 30 thẻ ngẫu nhiên (10%): ~5 nghìn
- **Tổng: ~7 nghìn**
- **Tiết kiệm ~33 nghìn cho 1 tuần**

Nhân với 8 tuần trong 2 tháng: **tiết kiệm ~260 nghìn**. Đủ tiền mua 3-4 cuốn sách chuyên khoa.

---

## Điều gì cần cẩn thận

### 1. Đừng dùng Claude Opus cho mọi việc

Opus đắt gấp 5-20 lần các AI khác. Chỉ dùng khi thật sự cần suy nghĩ sâu (hội chẩn ca khó, không phải khám thường).

### 2. Đừng dùng MiniMax cho việc cần chính xác cao

MiniMax rất nhanh nhưng đôi khi sai đáp án tinh vi. Việc "chấm câu trả lời y khoa" nên dùng Claude Sonnet trở lên — kẻo học viên bị chấm sai.

### 3. Đừng đổi AI giữa chừng 1 buổi làm việc dài

Bối cảnh bị reset khi đổi. Nên xong 1 việc, xoá session, rồi mới đổi AI.

### 4. Theo dõi chi phí hàng tuần

Vào bảng điều khiển tài khoản Anthropic + MiniMax mỗi tuần xem đã tiêu bao nhiêu. Nếu tăng bất thường → có thể do khoá bị lộ (kẻ khác dùng).

### 5. Nhiều khoá = nhiều rủi ro lộ

Càng nhiều tài khoản AI càng phải cẩn thận. Rotate mỗi 3-6 tháng.

---

## Bảng kiểm tra "đa AI đã setup xong"

- [ ] Có khoá Claude (nạp 200-300 nghìn)
- [ ] Có khoá MiniMax (nạp 50-100 nghìn)
- [ ] Cả 2 khoá đều nằm trong tệp `.env` gitignored
- [ ] 2 tên tắt `claude` và `claude-mini` chạy được trong terminal
- [ ] Đã thử 1 câu hỏi đơn giản với cả 2 AI, thấy có kết quả
- [ ] (Tuỳ chọn) Cài Cline hoặc Continue.dev trong VS Code
- [ ] (Tuỳ chọn) Cài Ollama nếu máy có RAM ≥ 16GB

---

## Nếu anh chỉ nhớ được 3 điều

1. **Claude cho việc khó, MiniMax cho việc vừa, AI trên máy cho việc đơn giản** — nguyên tắc chia việc cơ bản
2. **Tiết kiệm 40-60% chi phí** với chất lượng tương đương
3. **Cài xong 15 phút**, dùng cả đời — đầu tư một lần

## Đọc tiếp

- [03_setup_may.md](03_setup_may.md) — Cài Claude Code (làm trước)
- [06_bao_mat_git.md](06_bao_mat_git.md) — Nhiều khoá hơn thì càng phải giữ chặt
- [07_du_an_da_lieu_kickoff.md](07_du_an_da_lieu_kickoff.md) — Áp dụng vào lộ trình 2 tháng
