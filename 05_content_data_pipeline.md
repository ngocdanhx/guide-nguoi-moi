# 05 — Nguyên tắc seed nội dung (đừng để AI chế đáp án)

> Đây là phần **dễ sai nhất** khi làm app học tập. Đọc kỹ trước khi seed dữ liệu vào database — sai ở đây có thể làm anh học sai kiến thức, ảnh hưởng đến bệnh nhân sau này.

## Vấn đề: AI hay "chém"

AI ngôn ngữ (Claude, GPT, Gemini) rất giỏi tạo ra **văn bản trông có vẻ đúng**. Nhưng "trông có vẻ đúng" và "đúng thật" là 2 chuyện khác nhau.

Kịch bản tệ:
- Anh bảo Claude "viết 100 câu MCQ về vảy nến"
- Claude viết 100 câu, có đáp án A/B/C/D rõ ràng
- Anh học 100 câu đó
- 30% câu **sai fact** (bịa liều thuốc, sai chẩn đoán phân biệt)
- Anh vào phòng thi, làm sai — hoặc tệ hơn, khám bệnh nhân dùng kiến thức sai

**Đây không phải giả thuyết**. AI "hallucinate" (bịa) là hiện tượng đã được nghiên cứu kỹ. Càng chuyên môn hẹp, AI càng hay bịa.

---

## Nguyên tắc vàng: Correct-by-construction

**Correct-by-construction** = xây dựng dữ liệu theo cách **không thể sai**, vì mọi đáp án đến từ **nguồn gốc có sẵn key thật**, không phải AI sáng tác.

Nói cách khác:
- **KHÔNG** bảo AI "viết câu hỏi mới"
- **CÓ** bảo AI "đọc chương này trong sách, trích xuất câu hỏi + đáp án đã có sẵn"

AI chỉ làm **ETL** (extract – transform – load), không làm **generate**.

---

## Bài học đau từ 1 app học tiếng Anh

Có 1 app học tiếng Anh từng dính bug lớn: 110 file bài đọc kiểu "self-cloze" — script khoét bừa 1 từ trong đoạn văn, lấy chính từ đó làm đáp án.

Trên giấy tờ có vẻ ổn: có câu hỏi, có đáp án, có source đầy đủ. **Nhưng đề bất khả thi** — vì đoạn văn còn lại không cho đủ thông tin để suy ra từ bị khoét. User chọn đáp án nào cũng sai/đúng như nhau.

Bài học ghi vào project memory: **"Item tốt = giải được sau khi đọc, không phải khoét chữ ra rồi khớp lại."**

Cách fix: gỡ toàn bộ 110 file rác, thay bằng đề Cambridge thật — có đáp án chuẩn từ sách gốc, có examiner comment, 0% AI-chế. Còn lại chỉ 4 file thật dùng được.

**Áp dụng cho da liễu**: 
- Không bảo AI viết 100 câu về vảy nến
- Có bảo AI đọc chương "Vảy nến" trong sách CK1, trích 30 câu MCQ đã có sẵn + key ở cuối sách

---

## 4 nguồn "correct-by-construction" cho da liễu

### Nguồn 1 — Sách textbook có key

- **Fitzpatrick's Dermatology 9e** — "bible" quốc tế
- **Andrews' Diseases of the Skin 13e** — có case study kèm discussion
- **Bolognia Dermatology 5e** — 2717 trang, comprehensive
- **Habif's Clinical Dermatology 7e** — practical
- **Bệnh học Da liễu (3 tập) — GS.TS Trần Hậu Khang** — giáo trình chính HMU
- **Da liễu học — PGS.TS Phạm Văn Hiển (Bộ Y tế)** — hệ đại học đa khoa

Sách bản mềm PDF → dùng AI parse thành flashcard.

### Nguồn 2 — Website medical education

- **DermNet NZ** (dermnetnz.org) — atlas ảnh 25000+, chẩn đoán chuẩn của Hội Da liễu New Zealand
- **VisualDx** — atlas ảnh có tag ICD (thương mại, cần subscription)
- **DermIS** (dermis.net) — atlas free tiếng Anh + Đức
- **Medscape Dermatology** — case của tháng có discussion

**Lưu ý bản quyền quan trọng**: DermNet NZ **cấm dùng ảnh để train AI** (theo license CC BY-NC-ND 4.0). Nếu chỉ hiển thị cho user cá nhân + credit đầy đủ → OK. Đọc kỹ license trước khi seed.

### Nguồn 3 — Guideline chính thức

- **Quyết định 4416/QĐ-BYT (06/12/2023)** — Hướng dẫn Chẩn đoán và điều trị các bệnh Da liễu, Bộ Y tế Việt Nam. Là source pháp lý cho câu hỏi CK1
- **AAD guidelines** — Hội Da liễu Mỹ
- **EDF guidelines** — Hội Da liễu châu Âu

### Nguồn 4 — Đề thi công khai

- Đề CK1 các trường phát (photo tay đồng nghiệp)
- **MRCP UK / RCPCH** — nhiều câu tương đương CK1 Việt Nam
- **USMLE Step 2 CK Dermatology** — bank câu hỏi tiếng Anh

---

## 5 pattern xử lý content (đã kiểm nghiệm)

Anh không cần nhớ chi tiết — khi làm, Claude Code hướng dẫn từng bước. Chỉ cần biết là có 5 cách:

**Pattern 1 — Parse PDF text (dễ nhất)**
PDF chữ (không phải ảnh) → dùng tool `pdftotext` extract → AI parse thành flashcard JSON. Cực nhanh, cực chính xác.

**Pattern 2 — Parse PDF scan (khó hơn)**
PDF ảnh (sách y khoa Việt Nam hay scan) → render page thành PNG DPI cao → Claude vision đọc.

**Bài học thực tế**: DPI ≥ 220-300 mới đọc chính xác answer key trong grid nhỏ. DPI thấp hơn dễ đọc sai đáp án.

**Pattern 3 — Scrape web (nếu site không có robot chặn)**
Dùng Python + BeautifulSoup extract HTML.

**Pattern 4 — Scrape site có Cloudflare**
Dùng `curl_cffi` giả TLS Chrome, qua được passive check.

**Pattern 5 — Playwright headless**
Khi Cloudflare có JS challenge (Turnstile) — dùng browser thật headless. Chậm hơn nhưng qua được mọi bảo vệ.

---

## Schema flashcard đề xuất cho da liễu

Không cần nhớ SQL — chỉ cần hiểu **mỗi flashcard nên có những trường thông tin gì**:

**Nội dung**:
- Loại thẻ (basic / MCQ / image diagnosis / case)
- Câu hỏi
- Đáp án
- Giải thích chi tiết
- Ảnh (nếu có)

**Tag phân loại**:
- Bệnh (VD "vay-nen", "viem-da-co-dia")
- ICD-10 (VD "L40.0" cho vảy nến)
- Chương sách (VD "fitzpatrick_ch15")
- Độ khó (1-5)

**Nguồn**:
- Loại nguồn ("sach", "de-thi-ck1", "dermnet")
- Reference cụ thể ("Fitzpatrick 9e p.234")
- URL (nếu online)

**Meta quan trọng nhất**:
- `verified_by` — "self" / "peer" / "expert"
- `ai_generated` — TRUE/FALSE — **luôn flag rõ card do AI sinh vs card từ nguồn thật**. Sau này lọc `WHERE ai_generated = FALSE` để review trước kỳ thi

**SRS state** (để chạy thuật toán Anki):
- Thời điểm cần ôn tiếp
- Interval hiện tại (số ngày)
- Ease factor
- Số lần đã ôn
- Số lần quên

---

## Invariant "không xoá bao giờ"

**Rule tuyệt đối**: **KHÔNG BAO GIỜ DELETE flashcard**. Kể cả thẻ sai, thẻ trùng, thẻ "hết dùng".

**Vì sao?** SRS state (Anki-like) phụ thuộc lịch sử. Xoá 1 card thì:
- Analytics gãy (số lần ôn không match)
- Nếu 3 tháng sau anh muốn train model FSRS tối ưu — thiếu data
- Không phục hồi được

**Thay vì xoá, dùng soft-flag**: thêm 1 field `active_in_profiles` — mảng các "profile" thẻ đó thuộc về. Khi muốn "xoá" thẻ khỏi deck "da-lieu-ck1" → chỉ remove tên deck khỏi mảng. Thẻ vẫn ở DB, chỉ ẩn khỏi deck cụ thể.

---

## Workflow build seed data (6 bước tiêu chuẩn)

Không cần thuộc — khi làm Claude sẽ setup cho anh. Chỉ cần biết luồng:

1. **`build`** — parse nguồn (PDF/HTML) → sinh file JSON candidate
2. **`diff`** — so với DB hiện tại, tạo 4 queue: cái nào thêm mới / cái nào cập nhật / cái nào xoá (soft-flag) / cái nào cần review tay
3. **`apply`** — apply diff, tự tạo file `.bak` backup DB trước
4. **`images`** — download ảnh từ URL, upload lên storage
5. **`decks`** — group flashcard thành các "deck" theo chương/bệnh
6. **`sync`** — push lên Supabase production

Mỗi bước **idempotent** (chạy nhiều lần cho cùng kết quả). Có backup trước khi apply — an toàn.

---

## 4 quy tắc verify trước khi seed production

Trước khi push 100 card mới lên DB, phải chạy 4 check tự động:

**Check 1 — Đáp án literal substring**
Nếu explanation có trích dẫn passage, quote phải là **substring literal** của passage. Nghĩa là quote phải xuất hiện y nguyên trong passage, không phải AI paraphrase.

**Bài học thực tế**: trong 1 pass backfill 146 câu, 144/146 quote là substring literal của passage, 2/146 dùng `…` rút gọn — head + tail đều có thật trong passage (0 chế). Nhờ verify literal substring mà bắt được 100% quote AI bịa.

**Check 2 — Không có placeholder rác**
Reject card có text `[TODO]`, `[GAP]`, `[BLANK_1]`, `xxx`, `???`, `Fill in`. Đây là dấu hiệu AI để trống chưa hoàn thành.

**Check 3 — Đáp án nằm trong options (MCQ)**
Đáp án đúng phải là 1 trong các option đưa ra. Có card AI sinh đáp án là "E" trong khi options chỉ có A/B/C/D — sai basic.

**Check 4 — Ảnh load được**
Với card có ảnh, thử HTTP HEAD request đến URL. Status 200 mới OK. Có card ghi URL `file://...` local, deploy production 404.

Wire 4 check này thành script tự động, chạy trước mỗi lần sync.

---

## AI dùng đúng cách trong content pipeline

AI vẫn hữu ích, chỉ ở **2 vai trò cụ thể**:

**Vai trò 1 — Extract từ nguồn có sẵn (không sáng tác)**

Yêu cầu Claude: "Đọc file `page-042.png`, **trích xuất verbatim** câu hỏi và đáp án đã có trong đó. **KHÔNG** viết lại, **KHÔNG** bổ sung."

→ AI làm OCR + parse có structure, không tạo content mới.

**Vai trò 2 — Chấm câu trả lời của học viên (comparative)**

Yêu cầu Claude: "So sánh câu trả lời học viên với đáp án chuẩn. Cho điểm 0-10, giải thích chỗ đúng/sai."

→ AI so sánh, không phán quyết đúng-sai tuyệt đối.

**KHÔNG BAO GIỜ**:
- "Viết 100 câu hỏi da liễu về vảy nến" — AI sẽ chế, sai fact
- "Chẩn đoán bệnh này" — AI không được cấp phép hành nghề y
- "Kê đơn thuốc" — nghiêm cấm tuyệt đối

---

## 3 anti-pattern gặp nhất

**Anti-pattern 1 — Self-cloze**
Khoét từ trong passage rồi lấy chính từ đó làm đáp án. Text informativity = 0. Đề bất khả thi.

**Anti-pattern 2 — AI-generated MCQ không nguồn**
Nghe hay, đọc trôi chảy, nhưng sai fact khi chuyên gia review. Với y khoa, sai fact = nguy hiểm.

**Anti-pattern 3 — Ảnh không public**
Dùng URL local `file://` hoặc localhost → app deploy production hiển thị 404. Luôn upload ảnh lên storage cloud (R2/Supabase) trước.

---

## Checklist "content sẵn sàng seed"

Trước khi sync 100 card mới lên production:

- [ ] Tất cả card có `source_ref` cụ thể (sách + page hoặc URL)
- [ ] Tất cả card `ai_generated = FALSE` (hoặc đã review tay bởi anh)
- [ ] Chạy 4 check verify (quote literal, no placeholder, correct in options, image OK)
- [ ] Có `.bak` backup DB trước khi apply
- [ ] Test frontend load được deck (không chỉ SQL query success)
- [ ] Peer review ≥ 20% mẫu (đồng nghiệp đọc và OK)

---

## Nếu anh chỉ nhớ được 3 điều

1. **AI không viết câu hỏi mới** — chỉ extract từ nguồn có key
2. **Field `ai_generated` phải có** — để lọc review trước kỳ thi
3. **Không xoá thẻ bao giờ** — soft-flag qua `active_in_profiles`

## Đọc tiếp

- [06_bao_mat_git.md](06_bao_mat_git.md) — Không leak secret khi commit content pipeline
- [07_du_an_da_lieu_kickoff.md](07_du_an_da_lieu_kickoff.md) — Áp dụng vào roadmap 6 tuần
