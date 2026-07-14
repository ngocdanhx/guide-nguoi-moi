# 05 — Chuẩn bị nội dung học (đừng để AI chế đáp án)

> Đây là phần **dễ sai nhất** khi làm ứng dụng học tập. Đọc kỹ trước khi đưa dữ liệu vào — sai ở đây có thể làm anh học sai kiến thức, ảnh hưởng đến bệnh nhân sau này.

## Vấn đề: AI hay "chém"

AI ngôn ngữ (Claude, GPT, Gemini, MiniMax) rất giỏi tạo ra **văn bản trông có vẻ đúng**. Nhưng "trông có vẻ đúng" và "đúng thật" là 2 chuyện khác nhau.

Kịch bản tệ:
- Anh nhờ AI "viết 100 câu trắc nghiệm về vảy nến"
- AI viết 100 câu, có đáp án A/B/C/D rõ ràng
- Anh học 100 câu đó
- 30% câu **sai fact** (bịa liều thuốc, sai chẩn đoán phân biệt)
- Anh vào phòng thi, làm sai — hoặc tệ hơn, khám bệnh nhân dùng kiến thức sai

**Đây không phải giả thuyết**. AI "hallucinate" (bịa) là hiện tượng đã được nghiên cứu kỹ. Càng chuyên môn hẹp, AI càng hay bịa.

---

## Nguyên tắc vàng: Correct-by-construction

Nghĩa là: xây dựng dữ liệu theo cách **không thể sai**, vì mọi đáp án đến từ **nguồn có sẵn đáp án chuẩn**, không phải AI sáng tác.

Nói cách khác:
- **KHÔNG** bảo AI "viết câu hỏi mới"
- **CÓ** bảo AI "đọc chương này trong sách, trích ra câu hỏi + đáp án đã có sẵn"

AI chỉ làm việc **đọc và sắp xếp**, không tạo nội dung mới.

---

## Kịch bản giả lập: parse 1 chương sách "Vảy nến"

Đây là quy trình chuẩn cho 1 chương sách bất kỳ:

**Bước 1 — Chuẩn bị nguồn**

- Có file PDF chương "Vảy nến" từ sách CK1
- Xác định phần nào có câu hỏi + đáp án ở cuối chương
- Xác định phần nào có case study có discussion

**Bước 2 — AI đọc + trích xuất (không sáng tác)**

Gõ vào Claude Code:
> "Đọc file `chuong-vay-nen.pdf`, trích xuất **verbatim** các câu hỏi trắc nghiệm và đáp án có trong sách. KHÔNG viết lại, KHÔNG bổ sung câu mới. Sinh file JSON theo mẫu."

AI trả về ví dụ 30 thẻ nháp, mỗi thẻ có:
- Câu hỏi (chép nguyên văn từ sách)
- Các phương án A/B/C/D (chép nguyên văn)
- Đáp án đúng (theo đáp án ở cuối chương)
- Nguồn trích dẫn (tên sách + số trang)

**Bước 3 — Máy tự kiểm tra**

Chạy 4 kiểm tra tự động:

1. **Câu trích dẫn có trong sách không** — nếu thẻ có đoạn "theo sách trang X", đoạn đó phải xuất hiện y nguyên trong sách. Nếu không → cờ đỏ (AI bịa).
2. **Không có ký hiệu rác** — reject thẻ có `[TODO]`, `[GAP]`, `[BLANK_1]`, `xxx`, `???`. Đây là dấu hiệu AI chưa hoàn thành.
3. **Đáp án nằm trong các phương án** — đáp án đúng phải là 1 trong A/B/C/D đưa ra.
4. **Ảnh có tải được không** — nếu thẻ có ảnh, thử tải xem có 200 OK không.

**Bước 4 — Anh duyệt tay 20% mẫu**

Chọn ngẫu nhiên 6 thẻ trong 30 thẻ. Đọc kỹ:
- Đáp án có đúng theo sách không?
- Câu hỏi có rõ nghĩa không?
- Có tag đúng nhóm bệnh không?

Nếu 6/6 đúng → đưa cả 30 thẻ vào kho.
Nếu 4-5/6 đúng → xem lại 24 thẻ còn lại kỹ hơn.
Nếu ≤ 3/6 đúng → bỏ toàn bộ lô, thử lại với AI khác hoặc chỉnh cách hỏi.

**Bước 5 — Đưa vào kho + gắn nhãn**

Mỗi thẻ được gắn:
- Nhóm bệnh (ví dụ "vảy nến")
- Mã bệnh quốc tế (ví dụ L40.0)
- Chương sách (ví dụ "Trần Hậu Khang tập 2 chương 15")
- Độ khó (1-5)
- **Cờ "AI sinh"** — luôn đánh dấu rõ thẻ do AI sinh, khác với thẻ tự viết tay

**Bước 6 — Sao lưu trước khi đưa vào**

Trước khi đưa 30 thẻ mới vào kho, luôn tạo bản sao lưu kho hiện tại (dạng file `.bak`). Nếu 30 thẻ mới có vấn đề, có thể khôi phục.

---

## Bài học từ 1 ứng dụng học tiếng Anh (chuyện có thật)

Có 1 ứng dụng học tiếng Anh từng dính lỗi lớn: 110 file bài đọc kiểu "khoét từ" — script lấy ngẫu nhiên 1 từ trong đoạn văn, sau đó lấy chính từ đó làm đáp án.

Trên giấy tờ có vẻ ổn: có câu hỏi, có đáp án, có nguồn. **Nhưng đề bất khả thi** — vì đoạn văn còn lại không cho đủ thông tin để suy ra từ bị khoét. User chọn đáp án nào cũng sai/đúng như nhau.

Bài học ghi vào bộ nhớ dự án: **"Item tốt = giải được sau khi đọc, không phải khoét chữ ra rồi khớp lại."**

Cách fix: gỡ toàn bộ 110 file rác, thay bằng đề Cambridge thật — có đáp án chuẩn từ sách gốc. Còn lại chỉ 4 file thật dùng được.

**Áp dụng cho da liễu**:
- Không bảo AI viết 100 câu về vảy nến
- Có bảo AI đọc chương "Vảy nến" trong sách CK1, trích 30 câu MCQ đã có sẵn + đáp án ở cuối sách

---

## 4 nguồn có sẵn "correct-by-construction" cho da liễu

### Nguồn 1 — Sách textbook có đáp án

**Sách quốc tế**:
- **Fitzpatrick's Dermatology** — "bible" quốc tế
- **Andrews' Diseases of the Skin** — có case study kèm discussion
- **Bolognia Dermatology** — comprehensive
- **Habif's Clinical Dermatology** — thực dụng

**Sách tiếng Việt**:
- **Bệnh học Da liễu (3 tập) — GS.TS Trần Hậu Khang** — giáo trình chính Đại học Y Hà Nội
- **Da liễu học — PGS.TS Phạm Văn Hiển (Bộ Y tế)** — hệ đại học đa khoa

Sách bản mềm PDF → nhờ AI đọc thành thẻ ôn.

### Nguồn 2 — Website atlas ảnh

- **DermNet NZ** (dermnetnz.org) — atlas 25.000 ảnh, chẩn đoán chuẩn Hội Da liễu New Zealand
- **VisualDx** — atlas có tag mã bệnh (trả phí)
- **DermIS** (dermis.net) — atlas miễn phí tiếng Anh + Đức
- **Medscape Dermatology** — case của tháng có discussion

**Lưu ý bản quyền quan trọng**: DermNet NZ **cấm dùng ảnh để huấn luyện AI** (theo giấy phép CC BY-NC-ND 4.0). Nếu chỉ hiển thị cho anh dùng cá nhân + ghi nguồn đầy đủ → OK. Đọc kỹ giấy phép trước khi đưa vào.

### Nguồn 3 — Guideline chính thức

- **Quyết định 4416/QĐ-BYT (2023)** — Hướng dẫn Chẩn đoán và điều trị các bệnh Da liễu, Bộ Y tế Việt Nam. Đây là **nguồn pháp lý** cho câu hỏi CK1
- **AAD guidelines** — Hội Da liễu Mỹ
- **EDF guidelines** — Hội Da liễu châu Âu

### Nguồn 4 — Đề thi công khai

- Đề CK1 các trường phát (photo tay đồng nghiệp)
- **MRCP UK / RCPCH** — nhiều câu tương đương CK1 Việt Nam
- **USMLE Step 2 CK Dermatology** — kho câu hỏi tiếng Anh

---

## 5 cách xử lý nội dung (không cần thuộc chi tiết)

Khi làm thật, Claude Code sẽ hướng dẫn từng bước. Chỉ cần biết là có 5 cách:

**Cách 1 — Đọc PDF chữ (dễ nhất)**: PDF có sẵn chữ (không phải ảnh scan) → công cụ đọc nhanh + AI chuyển thành thẻ. Cực nhanh, cực chính xác.

**Cách 2 — Đọc PDF ảnh scan (khó hơn)**: sách y khoa Việt Nam hay scan → cần render page thành ảnh chất lượng cao → AI đọc bằng khả năng nhìn.

*Bài học*: chất lượng ảnh phải cao (DPI ≥ 220-300) mới đọc đúng đáp án. DPI thấp dễ đọc sai.

**Cách 3 — Lấy từ trang web thường**: có sẵn công cụ để lấy nội dung HTML.

**Cách 4 — Lấy từ trang web có Cloudflare chặn**: có công cụ đặc biệt giả trình duyệt Chrome để qua được.

**Cách 5 — Trình duyệt tự động**: khi trang có JS challenge (thử tương tác) — dùng trình duyệt thật ẩn. Chậm hơn nhưng qua được mọi bảo vệ.

---

## Cấu trúc thẻ đề xuất cho da liễu

Không cần nhớ chi tiết. Chỉ cần hiểu **mỗi thẻ nên có những thông tin gì**:

**Nội dung**:
- Loại thẻ (câu hỏi ngắn / trắc nghiệm / chẩn đoán ảnh / case study)
- Câu hỏi
- Đáp án
- Giải thích chi tiết
- Ảnh (nếu có)

**Phân loại**:
- Bệnh (ví dụ "vảy nến")
- Mã bệnh quốc tế ICD-10 (ví dụ "L40.0")
- Chương sách
- Độ khó (1-5)

**Nguồn**:
- Loại nguồn ("sách", "đề thi CK1", "DermNet")
- Trích dẫn cụ thể ("Fitzpatrick 9e tr. 234")
- URL (nếu online)

**Quan trọng nhất**:
- `nguoi_kiem_tra` — "tự", "đồng nghiệp", "chuyên gia"
- `ai_sinh` — TRUE/FALSE — **luôn đánh dấu rõ thẻ do AI sinh vs thẻ tự viết**. Sau này lọc `WHERE ai_sinh = FALSE` để duyệt lại trước kỳ thi

**Trạng thái ôn**:
- Thời điểm cần ôn lại
- Số ngày giãn cách hiện tại
- Số lần đã ôn
- Số lần quên

---

## Nguyên tắc "không xoá bao giờ"

**Rule tuyệt đối**: **KHÔNG BAO GIỜ XOÁ thẻ**. Kể cả thẻ sai, thẻ trùng, thẻ "hết dùng".

**Vì sao?** Trạng thái ôn tập phụ thuộc lịch sử. Xoá 1 thẻ thì:
- Bảng thống kê gãy (số lần ôn không khớp)
- Nếu 3 tháng sau anh muốn tối ưu thuật toán — thiếu dữ liệu
- Không khôi phục được

**Thay vì xoá, dùng "cờ ẩn"**: thêm 1 trường `dang_dung_o_deck` — mảng các deck thẻ đó thuộc về. Khi muốn "xoá" thẻ khỏi deck "da-lieu-ck1" → chỉ bỏ tên deck khỏi mảng. Thẻ vẫn ở kho, chỉ ẩn khỏi deck cụ thể.

---

## Quy trình 6 bước chuẩn (không cần thuộc)

Khi làm thật Claude Code sẽ setup cho anh. Chỉ cần biết có 6 bước:

1. **Sinh** — AI đọc nguồn → sinh file thẻ nháp
2. **So sánh** — so với kho hiện tại, chia thành 4 nhóm: thêm mới / cập nhật / xoá mềm / cần duyệt tay
3. **Áp dụng** — đưa vào kho, tự tạo file sao lưu trước
4. **Xử lý ảnh** — tải ảnh từ URL, đưa lên kho ảnh
5. **Đóng gói** — nhóm thẻ thành các deck theo chương/bệnh
6. **Đồng bộ** — đưa lên kho hồ sơ chính thức

Mỗi bước **có thể chạy lại nhiều lần** cho cùng kết quả. Có sao lưu trước khi thay đổi lớn — an toàn.

---

## 4 kiểm tra bắt buộc trước khi đưa vào kho chính

Trước khi đưa 100 thẻ mới vào kho, phải chạy 4 kiểm tra tự động:

**Kiểm tra 1 — Câu trích dẫn có trong sách không**
Nếu thẻ có "theo sách trang X: ...", đoạn `...` phải xuất hiện **y nguyên** trong sách. Không phải AI diễn giải lại.

*Bài học thực tế*: trong 1 lần điền 146 thẻ, 144/146 câu trích dẫn là chuỗi y nguyên từ nguồn, 2/146 dùng `…` rút gọn — nhưng đầu và cuối đều có thật trong nguồn (0 chế). Nhờ kiểm tra này mà bắt được 100% câu AI bịa.

**Kiểm tra 2 — Không có ký hiệu rác**
Reject thẻ có `[TODO]`, `[GAP]`, `[BLANK_1]`, `xxx`, `???`, "điền vào chỗ trống". Đây là dấu hiệu AI để trống chưa hoàn thành.

**Kiểm tra 3 — Đáp án nằm trong các phương án (trắc nghiệm)**
Đáp án đúng phải là 1 trong các phương án đưa ra. Có thẻ AI sinh đáp án là "E" trong khi phương án chỉ có A/B/C/D — sai cơ bản.

**Kiểm tra 4 — Ảnh tải được**
Với thẻ có ảnh, thử tải xem có OK không. Có thẻ ghi URL local, khi đưa lên mạng thì lỗi 404.

Wire 4 kiểm tra này thành script tự động, chạy trước mỗi lần đồng bộ.

---

## AI dùng đúng cách trong quy trình nội dung

AI vẫn hữu ích, chỉ ở **2 vai trò cụ thể**:

### Vai trò 1 — Đọc + trích xuất từ nguồn có sẵn (không sáng tác)

Gõ vào Claude: "Đọc file `page-042.png`, **trích xuất verbatim** câu hỏi và đáp án đã có trong đó. **KHÔNG** viết lại, **KHÔNG** bổ sung."

→ AI làm OCR + sắp xếp có cấu trúc, không tạo nội dung mới.

### Vai trò 2 — Chấm câu trả lời của học viên (so sánh)

Gõ vào Claude: "So sánh câu trả lời học viên với đáp án chuẩn. Cho điểm 0-10, giải thích chỗ đúng chỗ sai."

→ AI so sánh, không phán quyết đúng-sai tuyệt đối.

### KHÔNG BAO GIỜ:

- "Viết 100 câu hỏi da liễu về vảy nến" — AI sẽ chế, sai fact
- "Chẩn đoán bệnh này" — AI không được cấp phép hành nghề y
- "Kê đơn thuốc" — nghiêm cấm tuyệt đối

---

## 3 sai lầm gặp nhất

**Sai lầm 1 — Khoét từ trong đoạn văn**: script khoét từ rồi lấy chính từ đó làm đáp án. Không có đủ thông tin để suy ra. Đề bất khả thi.

**Sai lầm 2 — AI sinh trắc nghiệm không nguồn**: nghe hay, đọc trôi chảy, nhưng sai fact khi chuyên gia review. Với y khoa, sai fact = nguy hiểm.

**Sai lầm 3 — Ảnh không public**: dùng URL local `file://` → khi đưa lên mạng hiển thị 404. Luôn tải ảnh lên kho mạng trước.

---

## Bảng kiểm tra "nội dung sẵn sàng đưa vào kho chính"

Trước khi đồng bộ 100 thẻ mới lên kho chính:

- [ ] Tất cả thẻ có trích dẫn nguồn cụ thể (sách + trang hoặc URL)
- [ ] Tất cả thẻ đánh dấu `ai_sinh = FALSE` (hoặc đã duyệt tay bởi anh)
- [ ] Chạy 4 kiểm tra (câu trích dẫn / không rác / đáp án đúng / ảnh OK)
- [ ] Có sao lưu kho trước khi áp dụng
- [ ] Test giao diện tải được deck (không chỉ kho hồ sơ chạy được)
- [ ] Duyệt tay ≥ 20% mẫu (đọc và OK)

---

## Nếu anh chỉ nhớ được 3 điều

1. **AI không viết câu hỏi mới** — chỉ đọc + trích từ nguồn có sẵn đáp án
2. **Trường `ai_sinh` phải có** — để lọc duyệt lại trước kỳ thi
3. **Không xoá thẻ bao giờ** — chỉ ẩn khỏi deck bằng cờ

## Đọc tiếp

- [06_bao_mat_git.md](06_bao_mat_git.md) — Không lộ khoá khi commit
- [07_du_an_da_lieu_kickoff.md](07_du_an_da_lieu_kickoff.md) — Áp dụng vào lộ trình 2 tháng
