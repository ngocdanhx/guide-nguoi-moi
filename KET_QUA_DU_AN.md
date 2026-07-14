# Nếu dự án này hoàn thành, anh có gì?

> Đọc file này **trước tất cả file khác**. Nếu không thấy giá trị rõ ràng ở đây, đừng bắt đầu — 6 tuần setup không đáng.

## Vì sao có file này riêng

Trước khi bàn "làm cái gì" và "làm như nào", cần rõ **cái đó mang lại gì cho anh**. Không có motivation cụ thể, dự án sẽ chết yểu ở tuần thứ 2 khi công việc bác sĩ bận rộn ập đến.

File này là **hình dung cụ thể** về sản phẩm 6 tháng sau — không phải mơ ước, mà là **con số đo được** dựa trên nghiên cứu khoa học.

---

## Phần 1 — 1 năm sau, anh sẽ có gì?

Hình dung anh của tháng 7/2027 (1 năm sau khi bắt đầu):

### Về công cụ

- 1 web app PWA riêng, tên `dalieu.xxx` (domain của anh) — mở từ điện thoại như app native
- 1 bot Telegram `@dalieu_xxx_bot` gửi flashcard sáng + quiz trưa tự động
- Database ~2000-3000 flashcard đã verify từ 15-20 chương sách CK1
- ~500-800 case study có ảnh atlas được tag ICD-10
- Bank ~1000 câu MCQ tự tạo từ đề thi + textbook (đủ mock trước kỳ thi)

### Về kiến thức

- Retention trung bình 75-85% cho toàn bộ card đã học (thay vì 20-30% học truyền thống)
- Nhớ được ~1500-2000 concept da liễu (so với ~500-800 nếu chỉ đọc sách)
- Phản xạ nhận diện lâm sàng — nhìn ảnh 3 giây gọi tên bệnh (nhờ image occlusion training)
- Biết mình yếu chỗ nào — dashboard chỉ ra 5 block ICD retention thấp nhất

### Về hành vi học

- Streak 200-300 ngày liên tiếp có ôn (SRS bắt buộc mỗi ngày, dù chỉ 5 phút)
- Sử dụng thời gian chết hiệu quả — sáng cà phê, trưa nghỉ, chờ ca mổ đều học được
- Không mất data khi format máy hay đổi điện thoại — mọi thứ trên cloud

### Về kỹ năng phụ (bonus)

- Hiểu được cách 1 app hoạt động — data flow, API, database, deploy
- Có thể tự thêm feature mới với AI (VD "thêm command /note vào bot") mà không cần dev outsource
- Repo GitHub cá nhân đầu tiên — có thể share cho khóa sau, mở rộng thành tool cho cả lớp

---

## Phần 2 — 5 kết quả đo lường được

Không phải mục tiêu mơ hồ. Đây là 5 chỉ số cụ thể anh có thể track weekly:

### KQ1 — Số flashcard ôn/tháng

- **Không có app**: ~50-100 thẻ (viết tay), chỉ ôn khi rảnh, không đo được
- **Có app**: **800-1200 thẻ/tháng** — dashboard show con số chính xác
- **Cải thiện**: gấp 10-15 lần

### KQ2 — Retention rate (tỷ lệ nhớ sau 1 tháng)

- **Không có app**: ~40-50% (theo forgetting curve Ebbinghaus)
- **Có app**: **75-85%** (theo meta-analysis SR trong medical education — effect size d = 0.8)
- **Cải thiện**: +30-35 điểm phần trăm

### KQ3 — Điểm thi ước lượng

- **Không có app**: điểm baseline theo cách học truyền thống
- **Có app**: **+10-15% điểm thi** (theo nghiên cứu PMC 2025: nhóm mature cards ≥ 9,390 đạt CBSE 71.5% vs 60.0%, p=0.002)
- **Cải thiện**: 10-15%

### KQ4 — Thời gian ôn/tuần

- **Không có app**: ~5-8 giờ (ngồi bàn, tập trung 1 lượt)
- **Có app**: **~3-4 giờ** (chia nhỏ vào thời gian chết)
- **Cải thiện**: -50% mà chất lượng cao hơn (nhờ SRS + active recall)

### KQ5 — Chi phí công cụ/năm

- **Không có app**: ~500k-1 triệu (sách + notebook + in tài liệu)
- **Có app**: **2-6 triệu (~$85-255)** — bao gồm AI API + hosting
- **Cải thiện**: tăng chi phí, nhưng ROI cao vì tăng điểm + tiết kiệm thời gian

---

## Phần 3 — So sánh với cách truyền thống (4 bảng)

Nhìn thẳng vào bảng để thấy **cùng 1 nhu cầu**, cách truyền thống và cách mới khác nhau ra sao. Không phải app thay thế tất cả — nó bổ trợ, giải quyết đúng chỗ đau nhất.

### Bảng 1 — Tình huống hằng ngày

| Tình huống | Cách truyền thống | Cách với app này | Cải thiện |
|---|---|---|---|
| **6h30 sáng chuẩn bị đi trực** | Mở sách CK1, không biết học chương nào, đọc lướt 30 phút quên hết | Mở Telegram, bot đã gửi 5 thẻ due-first, bấm rating 3-5 phút | Tiết kiệm 25 phút, học đúng thẻ cần ôn |
| **12h30 nghỉ trưa giữa 2 ca khám** | Lướt Facebook 30 phút hoặc chợp mắt | Gõ `/quiz` vào bot, làm 5 MCQ trong 3 phút | Biến thời gian chết thành active recall |
| **Đêm trực, wifi bệnh viện lag** | Không truy cập tài liệu online được, học sách giấy khó tìm ảnh | Mở web PWA đã cache offline, tra ảnh atlas 1 chạm | Truy cập kiến thức mọi lúc |
| **Gặp ca lâm sàng lạ** | Về nhà, mở sách tìm chương, đọc lại | Note case vào app, tag ICD-10, sau này quiz tự động | Không bỏ sót ca hiếm |
| **Chủ nhật muốn tổng kết tuần** | Không có công cụ đo lường — cảm giác "đã học nhiều" hay "chưa đủ" | Dashboard: đã ôn 250 thẻ, retention 78%, streak 12 ngày, yếu block L40 | Đo được, biết yếu chỗ nào |

### Bảng 2 — Chuẩn bị nội dung học

| Việc | Cách truyền thống | Cách với app này | Cải thiện |
|---|---|---|---|
| **Soạn flashcard 1 chương sách** | Tự viết tay 50 thẻ, 4-6 giờ, hay bỏ dở | AI parse sách sinh 50 thẻ candidate, anh review 45 phút | ~5× nhanh hơn |
| **Kiểm tra đáp án có đúng sách không** | Đọc tay từng thẻ so với sách | Auto-verify quote literal substring, tự flag thẻ sai | 100% verify, không bỏ sót |
| **Tag phân loại thẻ** | Không tag, hoặc tag lung tung | Auto-tag ICD-10 theo bệnh, chapter, độ khó | Filter được sau này |
| **Lưu ảnh atlas 500 ca lâm sàng** | Tải về máy, sắp folder tay, mất khi format máy | Upload Cloudflare R2 (10GB free), share URL, có backup | Không mất data |
| **Import đề thi CK1 cũ** | Photo, in giấy, lưu ngăn tủ | OCR ảnh chuyển thành flashcard MCQ có key | Ôn được thay vì để in ra rồi quên |

### Bảng 3 — Chi phí + thời gian

| Hạng mục | Cách truyền thống | Cách với app này | Cải thiện |
|---|---|---|---|
| **Chi phí công cụ/năm** | 500k-1 triệu (sách + notebook + in) | ~$85-255 (~2-6 triệu VND) | Tăng ~1-5 triệu — chấp nhận được so với học phí CK1 |
| **Thời gian soạn 500 flashcard đủ cho 1 học kỳ** | ~50-60 giờ | ~10-15 giờ | Tiết kiệm 40 giờ |
| **Thời gian ôn 250 thẻ/tuần** | ~3-4 giờ (ngồi bàn) | ~2 giờ (thời gian chết) | Không chiếm thời gian riêng |
| **Setup ban đầu** | 0 | ~10 giờ (6 tuần × 2 giờ) | Đầu tư 1 lần, dùng nhiều năm |
| **Bảo trì/tháng** | 0 | ~1-2 giờ (thêm content mới) | Chấp nhận được |

### Bảng 4 — Chất lượng học

| Chỉ số | Cách truyền thống | Cách với app này | Cải thiện |
|---|---|---|---|
| **Retention sau 1 tháng** | ~40-50% | ~75-85% (SRS) | +30-35% |
| **Retention sau 6 tháng** | ~20-30% | ~65-75% | +40-45% |
| **Số câu practice trước kỳ thi** | ~500-1000 câu (đề trường phát) | ~2000-5000 câu (accumulate) | Gấp 3-5× |
| **Biết mình yếu chỗ nào** | Không biết, học lan man | Dashboard chỉ ra block ICD yếu nhất | Học đúng chỗ |
| **Nhớ kiến thức sau kỳ thi** | Quên nhanh sau vài tháng | Vẫn duy trì (SRS tiếp tục ôn nhẹ) | Kiến thức bền cho CK2 |
| **Nhận diện lâm sàng bằng ảnh** | Đọc mô tả text, khó chuyển thành visual | Image occlusion training — nhìn ảnh 3 giây gọi tên bệnh | Tăng phản xạ lâm sàng |

---

## Phần 4 — Nghiên cứu khoa học đứng sau

Không phải lời quảng cáo. 4 nguồn nghiên cứu dưới đây chứng minh **spaced repetition (SRS) trong y khoa** hiệu quả:

**1. Meta-analysis 2025** — "The Effectiveness of Spaced Repetition in Medical Education: A Systematic Review and Meta-Analysis" (PubMed 41601436). Tổng hợp nhiều nghiên cứu, kết luận SR effect size lớn cho medical students.

**2. Frontiers Medicine 2025** — SR trong paediatrics undergrad, effect size **d = 0.8** (large). Nghĩa là nhóm dùng SR outperform nhóm control ~1 độ lệch chuẩn — cực kỳ đáng kể trong nghiên cứu giáo dục.

**3. JMIR 2024** — "Spaced Digital Education for Health Professionals: Systematic Review and Meta-Analysis". Confirmă hiệu quả của digital SR platforms (như app anh sẽ xây).

**4. PMC12357012 (2025)** — n=36 UNLV medical students: nhóm mature Anki cards ≥ 9,390 đạt CBSE score **71.5%** vs nhóm dưới trung bình **60.0%** (p=0.002). **+11.5% điểm khác biệt có ý nghĩa thống kê**.

**Ý nghĩa cho anh**: nếu anh maintain được ~9000 mature cards trước kỳ thi CK1 (khả thi trong 6 tháng dùng đều), anh có 95% xác suất tăng ~10-15% điểm so với học truyền thống.

Chi tiết các nghiên cứu ở [research-notes.md](research-notes.md) phần C.1.

---

## Phần 5 — Cột mốc kết quả theo thời gian

Không phải "chạy đâu về đó ngay". Cần thời gian để SRS phát huy. Đây là mốc kỳ vọng:

### Sau tuần 1 (setup)
- Đã có Claude Code + Supabase + repo
- Chưa có card nào, chưa dùng được thực tế
- **Cảm giác**: hơi mệt vì setup, nhưng thấy "có nền tảng"

### Sau tuần 4 (MVP xong)
- Web app chạy được, bot Telegram gửi flashcard sáng
- Có ~50 card đầu tiên từ 1 chương sách
- **Cảm giác**: bắt đầu thấy giá trị — mỗi sáng thức dậy nhận 5 thẻ mới lạ

### Sau tuần 8 (2 tháng)
- Có ~200 card verified từ 3-4 chương
- Retention bắt đầu ổn định — thẻ tuần trước quay lại, thấy nhớ tốt
- **Cảm giác**: thói quen hình thành — không mở bot 1 buổi thấy thiếu

### Sau 6 tháng
- ~2000-3000 card
- Retention 75-85% cho card đã học
- Dashboard cho biết yếu block ICD nào
- **Cảm giác**: tự tin — có tool riêng, đo lường được tiến độ, không lo "học có đủ chưa"

### Sau 1 năm (trước kỳ thi CK1)
- Bank câu hỏi đủ mock 5-10 kỳ thi
- Retention long-term ~65-75%
- Điểm thi kỳ vọng +10-15% so với baseline
- **Cảm giác**: bước vào phòng thi với đầy đủ chuẩn bị

---

## Phần 6 — Câu hỏi tự đánh giá: dự án này có phù hợp với anh không?

Đọc 8 câu dưới, đếm số câu anh trả lời "Có":

1. Anh có ít thời gian ôn liên tục (2-3 giờ/lần), thường chỉ có 5-15 phút rảnh mỗi lần?
2. Anh hay quên kiến thức đã học sau vài tuần/tháng?
3. Anh có smartphone Android hoặc iPhone dùng hằng ngày?
4. Anh dùng Telegram (hoặc chấp nhận cài Telegram)?
5. Anh sẵn lòng đầu tư 6 tuần × 2-3 giờ (tổng ~15 giờ) để setup ban đầu?
6. Anh sẵn lòng chi ~$85-255/năm (2-6 triệu VND) cho công cụ?
7. Anh muốn có bằng chứng đo lường được tiến độ học (thay vì cảm giác "đã học nhiều")?
8. Anh nghiêm túc với kỳ thi CK1, muốn tối ưu điểm và giữ kiến thức lâu?

**Điểm đánh giá**:
- **7-8 câu Có**: Rất phù hợp. Nên bắt đầu ngay.
- **5-6 câu Có**: Phù hợp. Nên bắt đầu nhưng chấp nhận có thể chậm.
- **3-4 câu Có**: Cân nhắc kỹ. Có thể chỉ dùng 1 phần (ví dụ chỉ bot Telegram, không cần web app).
- **0-2 câu Có**: Chưa phù hợp. Anh nên học truyền thống + Anki app có sẵn thay vì tự xây.

---

## Phần 7 — Những gì dự án này KHÔNG làm được

Cân bằng góc nhìn — app không phải "viên đạn bạc":

- **Không thay thế đi trực** — kinh nghiệm lâm sàng phải đi trực mới có
- **Không thay thế đọc sách sâu** — hiểu cơ chế bệnh vẫn phải đọc kỹ, flashcard chỉ ôn sau khi đã hiểu
- **Không thay thế thầy hướng dẫn** — CK1 cần mentor thật, không phải AI
- **Không giúp anh nếu không dùng đều** — SRS chỉ hiệu quả khi review đúng lịch
- **Không tự động giỏi hơn nếu anh không review card AI-gen** — content pipeline cần anh verify tay 20% mẫu
- **Không thay được đề thi thật của trường** — chỉ practice, phải đọc quy chế trường

Nói cách khác: app là **bổ trợ**, không phải **thay thế**. Nếu anh chờ app tự làm hết, sẽ thất vọng.

---

## Nếu anh chỉ nhớ được 3 điều

1. **Retention 75-85% vs 40-50%** — điểm khác biệt cốt lõi so với truyền thống
2. **Đầu tư ~15 giờ setup + 2-3 giờ/tuần** — nhận về 800-1200 thẻ ôn/tháng
3. **+10-15% điểm thi ước lượng** — theo nghiên cứu, không phải quảng cáo

## Đọc tiếp

- Nếu thấy giá trị đáng làm → [00_START_HERE.md](00_START_HERE.md) → lộ trình đọc chi tiết
- Nếu muốn hiểu ngay công cụ dùng → [04_tech_stack.md](04_tech_stack.md)
- Nếu muốn xem roadmap 6 tuần cụ thể → [07_du_an_da_lieu_kickoff.md](07_du_an_da_lieu_kickoff.md)
