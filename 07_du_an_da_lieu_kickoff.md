# 07 — Kickoff dự án Da liễu CK1

> File này là **bản đồ khởi động** dự án cụ thể. Không hướng dẫn từng dòng code — chỉ giúp anh hiểu **luồng tổng thể + tuần nào làm gì**.

## Bước 1 — Nếu dự án thành công, anh có gì?

Trước khi bàn "làm cái gì", cần rõ **cái đó mang lại gì cho anh**. Nếu không thấy rõ giá trị, sẽ không đủ động lực đi qua 6 tuần setup.

### 5 kết quả cụ thể sau 6 tháng dùng đều

1. **Ôn được 800-1200 flashcard/tháng** trên các quãng thời gian chết (sáng cà phê, trưa nghỉ, giờ chờ, chỗ trực đêm) — thời gian trước đây lướt Facebook
2. **Tăng retention kiến thức** từ ~40-50% (học 1 lần rồi quên) lên **75-85%** (thuật toán SRS chống forgetting curve)
3. **Biết mình yếu ở đâu** — dashboard chỉ ra block ICD nào retention thấp, tập trung ôn đúng chỗ trước kỳ thi
4. **Xem ảnh atlas mọi lúc** — kể cả trực đêm không wifi (PWA offline cache), 1 chạm mở app từ home điện thoại
5. **Có bank flashcard cá nhân** của riêng anh, verify từ sách gốc — dùng lại được cho CK2 sau này, share cho khóa sau nếu muốn

### Ảnh hưởng đến kỳ thi CK1

Không cam kết cứng "sẽ +2 điểm" — vì phụ thuộc vào việc anh có dùng đều không. Nhưng theo nghiên cứu về SRS trong y khoa (xem [research-notes.md](research-notes.md) mục C.1):

- Meta-analysis 2025 trên med student: nhóm dùng SR đạt **effect size d = 0.8** (large) so với nhóm học truyền thống
- Nghiên cứu PMC 2025 (n=36 UNLV medical students): nhóm mature cards ≥ 9,390 đạt CBSE **71.5%** vs nhóm dưới trung bình **60.0%** (p=0.002)
- Chuyển sang đơn vị điểm: **~+10-15% điểm thi** so với học truyền thống, với điều kiện dùng đều

Nếu học phí CK1 là 30 triệu VND/năm, đầu tư $85-255/năm cho app (~2-6 triệu) mà tăng 10-15% điểm → ROI rất cao. Chưa kể kiến thức lâu bền hơn.

---

## Bước 2 — So sánh: học truyền thống vs học với app

Nhìn thẳng vào bảng dưới để thấy **cùng 1 nhu cầu**, cách truyền thống và cách mới khác nhau ra sao. Không phải app thay thế tất cả — nó bổ trợ, giải quyết đúng chỗ đau nhất.

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

### Bảng 3 — Chi phí + thời gian

| Hạng mục | Cách truyền thống | Cách với app này | Cải thiện |
|---|---|---|---|
| **Chi phí công cụ/năm** | 0 (chỉ sách + notebook) | ~$85-255 (~2-6 triệu VND) | Tăng ~2-6 triệu — chấp nhận được |
| **Thời gian soạn 500 flashcard đủ cho 1 học kỳ** | ~50-60 giờ | ~10-15 giờ | Tiết kiệm 40 giờ |
| **Thời gian ôn 250 thẻ/tuần** | ~3-4 giờ (ngồi bàn) | ~2 giờ (thời gian chết) | Không chiếm thời gian riêng |
| **Setup ban đầu** | 0 | ~10 giờ (6 tuần × 2 giờ) | Đầu tư 1 lần, dùng nhiều năm |

### Bảng 4 — Chất lượng học

| Chỉ số | Cách truyền thống | Cách với app này | Cải thiện |
|---|---|---|---|
| **Retention sau 1 tháng** | ~40-50% | ~75-85% (SRS) | +30-35% |
| **Retention sau 6 tháng** | ~20-30% | ~65-75% | +40-45% |
| **Số câu practice trước kỳ thi** | ~500-1000 câu (đề trường phát) | ~2000-5000 câu (accumulate qua nhiều tháng) | Gấp 3-5× |
| **Biết mình yếu chỗ nào** | Không biết, học lan man | Dashboard chỉ ra block ICD yếu nhất | Học đúng chỗ |
| **Nhớ kiến thức sau kỳ thi** | Quên nhanh sau vài tháng | Vẫn duy trì (SRS tiếp tục ôn nhẹ) | Kiến thức bền |

### Kết luận từ bảng

**Không phải cái gì cũng thay thế được**. Sách + đọc kỹ + hiểu cơ chế bệnh → vẫn phải làm. Đi trực, khám thật → không app nào thay được.

**App giải quyết đúng 3 chỗ đau**:
1. Retention (chống quên) — SRS + Anki algorithm
2. Truy cập kiến thức mọi lúc — PWA offline
3. Đo lường tiến độ — dashboard analytics

Nếu anh nhìn 3 bảng trên và thấy giá trị hợp lý → làm. Nếu chỉ thấy "hay hay" mà không rõ mình cần → khoan, đọc kỹ lại xem có đau ở đâu chưa.

---

## Bước 3 — Vấn đề cụ thể anh chọn giải

Học CK1 Da liễu ở Hà Nội có 3 khó khăn thực tế:

1. **Không có thời gian ôn** — trực nhiều, khám nhiều
2. **Khó nhớ hình ảnh** — 100 bệnh da lâm sàng nhìn giống nhau
3. **Không biết đề thi hỏi gì** — không có bank câu hỏi công khai chuẩn

Tuần đầu cần chọn **1-2 khó khăn** để giải trước — đừng cố giải tất cả.

**Recommend**: chọn **khó khăn 1 + 2** (thời gian + hình ảnh). Xây bot Telegram push flashcard sáng + web PWA xem ảnh atlas offline. Khó khăn 3 (bank câu hỏi) đến sau khi có đủ 200 thẻ verify.

---

## Bước 4 — Bức tranh sản phẩm cuối cùng

Sau 6 tuần, anh sẽ có 1 hệ thống chạy tự động:

**Sáng 6h30**:
- Chuông điện thoại reo, mở Telegram
- Bot đã gửi 5 flashcard mới (chọn thuật toán due-first — thẻ cần ôn nhất trước)
- Anh vừa uống cà phê vừa bấm rating: "Nhớ / Khó / Quên / Chưa nhớ"
- Bot tự cập nhật SRS state trong database

**Trưa 12h30 nghỉ giữa 2 ca**:
- Gõ `/quiz` vào bot
- Bot gửi 5 câu MCQ ngẫu nhiên từ deck tuần trước (interleaving)
- 3 phút xong, tăng retention

**Tối 20h vào laptop soạn thẻ mới**:
- Mở web PWA (đã cài trên bookmark)
- Đăng nhập, vào trang "Soạn thẻ"
- Parse thêm 1 chương sách CK1 bằng AI → sinh 20 flashcard candidate
- Review từng thẻ, verify đáp án khớp sách, tag ICD-10, submit
- Sáng mai 6h30 bot sẽ dùng thẻ mới này

**Trực đêm bệnh viện, wifi lag**:
- Mở web PWA đã cache offline
- Xem lại ảnh atlas ca lâm sàng gặp trong đêm
- Note vào 1 case study mới

**Cuối tuần Chủ nhật**:
- Bot gửi báo cáo tuần: đã ôn 250 thẻ, retention 78%, streak 12 ngày
- Mở web xem dashboard chi tiết theo tag ICD-10 — biết mình yếu block L40 (vảy nến)

Đây là mục tiêu cuối. Không phải làm ngay tuần 1.

---

## Bước 5 — Luồng chạy dưới capô (không cần code, chỉ cần hiểu)

Kịch bản "sáng 6h30 nhận flashcard":

1. **pg_cron** (báo thức trong Supabase) đúng 6h30 giờ Việt Nam → trigger Edge Function tên `send-morning-flashcards`
2. **Edge Function** truy vấn database — lấy 5 thẻ due nhất cho user anh, cùng các nút inline button "Nhớ / Khó / Quên"
3. Edge Function gọi **Telegram Bot API** gửi 5 tin nhắn đến chat_id của anh
4. Anh nhận notification trên điện thoại
5. Bấm nút "Khó" → Telegram gọi **webhook** (URL callback) về Edge Function khác tên `bot-callback`
6. `bot-callback` chạy thuật toán **SM-2 (Anki)** cập nhật SRS state: interval mới, ease factor mới, due date mới
7. Ghi log vào bảng `review_log` để sau train FSRS optimizer

Xong 1 luồng, ~5 giây. Không cần máy anh chạy gì — mọi thứ trên Supabase cloud.

Cùng data đó khi anh mở web app: đăng nhập → thấy progress đã cập nhật vì cùng chung 1 database.

---

## Bước 6 — Tech stack (tóm tắt vai trò)

| Vai trò | Chọn |
|---|---|
| Web frontend | Vite + React 19 + TanStack Router + shadcn/ui + Tailwind + vite-plugin-pwa |
| Database + Auth | Supabase (free tier) |
| Storage ảnh atlas | Cloudflare R2 (free 10GB) |
| Bot backend | Supabase Edge Function (TypeScript) + pg_cron |
| Bot alternative | Python + python-telegram-bot chạy laptop (khi cần scrape site trường) |
| Deploy web | Vercel (free Hobby) |
| AI dev chính | Claude Code (task lớn) |
| AI dev rẻ | MiniMax API (extract sách, dịch, format) |
| IDE | VS Code + extension Claude Code + Cline |

Chi tiết mỗi món: file [04_tech_stack.md](04_tech_stack.md).

---

## Bước 7 — Roadmap 6 tuần (mỗi tuần 2-3 giờ)

### Tuần 1 — Setup nền + học lý thuyết

**Mục tiêu**: máy sẵn sàng, hiểu quy trình, có 1 repo khởi tạo.

- Đọc 9 file trong `guide-nguoi-moi/` (không cần thuộc, đọc để quen)
- Cài VS Code + Claude Code + Git + Supabase account + Telegram bot token
- Cài Superpowers plugin + Cline (multi-model)
- Đăng ký MiniMax API key
- Tạo folder `da-lieu-hoc-tap`, chạy `git init`
- Gõ `/init` trong Claude Code để nó sinh CLAUDE.md mẫu
- Sửa CLAUDE.md ngắn gọn: mục tiêu, tech stack, secret pattern

**Kết quả cuối tuần**: repo có cấu trúc cơ bản, chạy `claude` mở được, sẵn sàng làm task đầu tiên.

### Tuần 2 — Web PWA phase 1 (skeleton + auth + list)

**Mục tiêu**: web hiển thị được, đăng nhập được, có 1 trang list card (dù chưa có card).

Task chi tiết Claude sẽ giúp anh chia:

- Sinh schema database — bảng flashcard, review_log, user_settings + RLS policy
- Setup Vite project với React + shadcn/ui + Tailwind
- Trang login (email + magic link)
- Trang "Deck" list flashcard
- Deploy Vercel → có URL `dalieu.vercel.app`
- Cài vite-plugin-pwa → cài được vào home điện thoại

**Kết quả cuối tuần**: web deploy được, đăng nhập từ điện thoại được, có 1 trang trống chờ có card.

### Tuần 3 — Web soạn thẻ + ôn SRS + seed content thật

**Mục tiêu**: có thể soạn thẻ, ôn thẻ, và có 50 thẻ thật từ 1 chương sách.

Task chính:

- Trang "Soạn thẻ" — form thêm/sửa, upload ảnh R2, tag ICD-10
- Trang "Ôn (SRS)" — hiển thị thẻ due-first, 4 nút rating, implement thuật toán SM-2
- Parse 1 chương sách CK1 (VD chương "Vảy nến" trong Trần Hậu Khang tập 2) → 50 flashcard bằng MiniMax (rẻ hơn Claude ~10× cho task extract)
- Verify 50 card: chạy 4 check ở file 05 (quote literal, no placeholder, correct in options, image OK)
- Seed 50 card vào Supabase

**Kết quả cuối tuần**: web có đủ 3 tính năng core (list/soạn/ôn), 50 card thật verify xong.

### Tuần 4 — Telegram bot integration

**Mục tiêu**: bot chạy tự động 6h30 sáng, share DB với web.

Task chính:

- Viết Edge Function `send-morning-flashcards` (TypeScript/Deno)
- Setup pg_cron schedule 6h30 giờ Việt Nam
- Webhook handler cho callback rating (Nhớ/Khó/Quên) → cập nhật SRS state chung với web
- Command `/quiz` — 5 câu MCQ ngẫu nhiên từ deck tuần trước
- Test end-to-end: sáng nhận thẻ, bấm rating, mở web thấy DB đã cập nhật

**Kết quả cuối tuần**: bot chạy tự động, cả bot + web đều update chung 1 database.

### Tuần 5 — Analytics + polish

**Mục tiêu**: hiểu được mình đang học ra sao, fix bug đã lộ ra sau 1 tuần dùng thật.

- Trang Dashboard: streak, retention %, biểu đồ ôn theo tuần
- Fix bug (thường có 3-5 bug lộ ra sau 1 tuần dùng thật)
- Command `/stats` cho bot — báo cáo tuần

### Tuần 6 — Content batch 2 + refactor

**Mục tiêu**: mở rộng content, dọn code trước khi vào giai đoạn duy trì.

- Parse thêm 2-3 chương sách (~150 card mới)
- Refactor code trùng lặp (web + edge function share service layer)
- Document lại architecture trong `docs/`

**Sau 6 tuần**: có 1 hệ thống hoàn chỉnh — web PWA + Telegram bot + ~200 card verified. Từ tuần 7 → duy trì + thêm content dần theo lịch học.

---

## Không làm trong MVP (để giai đoạn sau)

Nghiêm cấm scope creep. Không thêm vào MVP:

- Share deck với đồng nghiệp (chỉ 1 user = anh) — bật sau 2 tháng khi ổn định
- Chấm câu tự luận bằng AI — làm phase 2
- Push notification web (Telegram bot đã làm rồi)
- Import/export Anki .apkg — làm phase 3
- Comment/discussion (social feature)
- Multi-user permission chi tiết

**Vì sao?** Mỗi feature thêm = 1-2 tuần code + 1 tháng maintain. Kỳ thi CK1 không đợi. Ưu tiên "chạy được + học được" hơn "đầy đủ tính năng".

---

## Chuyên khoa 1 Da liễu Hà Nội — thông tin thực tế

**Cảnh báo**: các thông tin dưới đây có phần **chưa xác thực** — trường không public đủ chi tiết. Anh cần confirm với phòng đào tạo SĐH trước khi quyết định.

### Đại học Y Hà Nội (HMU)

- Website SĐH: `sdh.hmu.edu.vn` — có mục "Bác sĩ CK I"
- Tuyển sinh 2025 (tham khảo, năm sau có thể đổi): phát hành hồ sơ tháng 4-5, đăng ký online tháng 5, nộp hồ sơ giấy giữa tháng 5
- Địa chỉ: số 1 Tôn Thất Tùng, Đống Đa, Hà Nội (Phòng 325)
- Email: `sdhhotline@hmu.edu.vn`
- **CẦN CONFIRM trực tiếp**: chuyên ngành CK1 có Da liễu không / thời gian đào tạo / học phí / môn thi đầu vào

### Học viện Quân y (VMMU)

- Có đào tạo CK1 hệ dân sự
- Với Da liễu: cần xác nhận từ giám đốc bệnh viện nơi công tác
- **CẦN CONFIRM**: website `vmmu.edu.vn` có mở CK1 Da liễu thường xuyên không

### Bệnh viện Da liễu Trung ương (15A Phương Mai)

- **Không cấp bằng CK1** — chỉ là cơ sở thực hành, phối hợp với HMU/ĐHQGHN
- Có đào tạo thực hành 18 tháng có giấy xác nhận
- Có đào tạo liên tục (CME)
- Website: `dalieu.vn` — mục Phòng Đào tạo

---

## Anti-pattern cần tránh

### 1. Cầu toàn feature trước khi có 1 user thật

Đừng làm 10 feature "có vẻ hay" trước khi chính anh dùng bot 1 tuần. Cứ MVP → dùng thật 2 tuần → thêm feature theo pain point thực tế.

### 2. AI-generated content không nguồn

Đừng nhờ Claude "viết 100 câu hỏi da liễu". Có thể sai fact, bịa liều thuốc. Luôn parse từ sách có key. Chi tiết ở file 05.

### 3. Content pipeline không backup

Trước khi sync → luôn tạo `.bak`. Xoá 100 card sai chỉ mất 1 lệnh, restore lại mất 1 tuần.

### 4. Commit `.env` lên GitHub

Rotate token ngay nếu lỡ. Chi tiết file 06.

### 5. Không dùng RLS

Table Supabase không bật RLS = ai biết URL project là đọc được. Luôn bật + write policy.

### 6. Skip verification trước "done"

Học từ Superpowers `verification-before-completion`: chưa chạy verify command trong chính message này thì không được nói "done".

### 7. Kitchen-sink CLAUDE.md

Đừng viết CLAUDE.md 500 dòng. Claude sẽ ignore. Giữ dưới 200 dòng, chỉ include thứ Claude không tự đoán được từ code.

---

## Workflow 1 ngày điển hình (khi đã setup xong)

**Sáng 6h30** (tự động): Bot gửi 5 thẻ vào Telegram → anh mở điện thoại ăn sáng → bấm rating.

**7h30 vào bệnh viện**: Nếu trực đêm trước, mở web PWA (offline cache) xem case lâm sàng gặp đêm qua.

**Trưa 12h30** (5-10 phút): Mở laptop, chạy Claude Code, gõ:

> Task hôm nay: thêm 10 flashcard từ chương "Viêm da cơ địa" sách Trần Hậu Khang tập 2 trang 45-60.

Claude sinh spec + hỏi clarify (VD "có tag ICD L20 không?"). Anh trả lời, sau ~30 phút có 10 card mới trong DB, verified.

**Tối 20h** (10 phút): Bấm `/stats` xem tiến độ. Nếu weekend, làm quiz `/quiz`.

**Chủ nhật** (1-2 giờ): Session dài hơn — refactor, thêm feature mới, plan tuần tới.

---

## Khi gặp bế tắc

Bế tắc là bình thường trong tháng đầu. 4 nguồn hỗ trợ:

1. **Đọc lại file 01-06** trong `guide-nguoi-moi/` — thường quên khái niệm cơ bản
2. **Đọc [research-notes.md](research-notes.md)** — 4 nhóm research chi tiết có URL nguồn gốc
3. **Reddit r/ClaudeAI + r/medicalschoolanki** — community lớn, câu hỏi tiếng Anh
4. **Reddit r/vibecoding + r/LocalLLaMA** — thảo luận multi-model workflow

---

## Checklist "sẵn sàng kickoff"

Trước khi tạo folder project đầu tiên:

- [ ] Đã đọc file 01, 02, 03 (khái niệm cơ bản + workflow + setup)
- [ ] Máy đã cài Claude Code, VS Code, Git, Supabase CLI (nếu cần)
- [ ] Có tài khoản: GitHub, Anthropic (nạp $10), Supabase, MiniMax, Telegram bot
- [ ] Bot Telegram đã gửi thử được "Xin chào" (test 1 tin manual)
- [ ] Đã chọn 1-2 bài toán cụ thể (VD combo A+B: thời gian + hình ảnh)
- [ ] Đã có 1 chương sách CK1 để làm nguồn content (VD chương "Vảy nến")
- [ ] Đặt tên project: `da-lieu-hoc-tap` (hoặc anh chọn tên khác)
- [ ] Timebox: cam kết 2-3 giờ/tuần trong 6 tuần tới

Nếu ≥ 6/8 tick → kickoff. Chưa đủ → hoàn thiện trước, không vội.

---

## Nếu anh chỉ nhớ được 3 điều

1. **MVP tuần 2-4** — có sản phẩm chạy được (dù chỉ 50 card) là mốc quan trọng nhất
2. **Correct-by-construction** cho content — không để AI chế đáp án y khoa
3. **6 tuần đủ cho MVP** — sau đó là duy trì, không cần feature mới ồ ạt

## Đọc tiếp

- Nếu chưa rõ multi-model setup: [09_multi_model_workflow.md](09_multi_model_workflow.md)
- Nếu cần URL nguồn cụ thể: [08_tai_lieu_tham_khao.md](08_tai_lieu_tham_khao.md)
