# 04 — Tech stack (bộ vũ khí, mỗi cái làm gì)

> File này chỉ giới thiệu **các "món đồ nghề" thường dùng** cho app học tập, và **mỗi món giải quyết vấn đề gì**. Không viết code — khi anh cần code, Claude Code viết cho anh.

## Bức tranh tổng thể

Hình dung app học tập của anh giống 1 bệnh viện:

- **Người dùng** = bệnh nhân (chính là anh, khi dùng app)
- **Web PWA** = phòng khám chính — anh vào đó làm việc chính
- **Telegram bot** = "trợ lý gọi điện" — nhắc lịch, hỏi han giờ nghỉ
- **Database (Supabase)** = kho hồ sơ bệnh án — mọi dữ liệu lưu ở đây
- **Storage (Cloudflare R2)** = kho lưu trữ ảnh (chụp X-quang, ảnh atlas)
- **Edge Functions** = phòng xét nghiệm — có bệnh phẩm gửi vào, có kết quả gửi ra
- **pg_cron** = "y tá tự động" — chạy công việc định kỳ (VD 6h30 sáng gửi flashcard)

Anh không cần cài hết ngay. Bắt đầu từ 2 món (web + database) là chạy được.

---

## Món 1 — Supabase (backend hoàn chỉnh trong 1 gói)

**Supabase là gì?** 1 dịch vụ cloud cung cấp cùng lúc:
- Database (PostgreSQL — DB SQL kinh điển)
- Đăng ký/đăng nhập user (Auth)
- Kho lưu trữ file (Storage cho ảnh, PDF)
- Function chạy server (Edge Functions)
- Scheduler chạy công việc định kỳ (pg_cron)

**Vì sao chọn Supabase?**
- Miễn phí tier đủ cho dự án cá nhân
- 1 dịch vụ thay 5 dịch vụ → đỡ đau đầu quản lý
- Dashboard đẹp, click-to-config nhiều thứ
- Có tài liệu tiếng Anh rất tốt

**Vấn đề giải quyết cho da liễu**:

| Vấn đề | Supabase giải sao |
|---|---|
| Lưu 500 flashcard tự soạn | Bảng `flashcard` trong Database |
| Đăng nhập trên điện thoại + laptop, sync tự động | Auth + Realtime |
| Lưu 1000 ảnh atlas ca lâm sàng | Storage (hoặc Cloudflare R2 nếu > 1GB) |
| Chấm câu trả lời tự luận bằng AI | Edge Function gọi Claude/Gemini API |
| 6h30 sáng tự gửi Telegram | pg_cron trigger Edge Function |

**Rule vàng**: Mọi bảng phải bật **Row Level Security (RLS)** — quy tắc chỉ user chủ hồ sơ mới đọc/sửa được record của mình. Không bật là ai biết URL Supabase project là đọc được toàn bộ DB. Rất nguy hiểm.

---

## Món 2 — Web PWA (giao diện chính)

**PWA** viết tắt "Progressive Web App" — 1 loại web đặc biệt:
- Truy cập qua browser như web thường (VD `dalieu.vercel.app`)
- Có thể **cài vào điện thoại** như app — icon xuất hiện trên home screen
- **Hoạt động offline** — cache dữ liệu, dùng được khi wifi bệnh viện chập chờn
- Push notification (giới hạn — không mạnh bằng native app)

**Vì sao PWA thay vì native app iOS/Android?**
- 1 codebase chạy trên mọi thiết bị (Mac, Windows, Android, iPhone)
- Không cần submit lên App Store (mất phí + duyệt lâu)
- Update là auto — mỗi lần anh deploy code mới, user mở app là tự nhận

**Bộ stack thường dùng để làm PWA**:

| Thư viện | Vai trò |
|---|---|
| **Vite** | Trình build code — biến TypeScript/JSX thành file browser hiểu |
| **React 19** | Framework FE phổ biến nhất — Claude Code viết cực nhanh |
| **TanStack Router** | Điều hướng giữa các trang — file-based, an toàn kiểu |
| **shadcn/ui** | Bộ component đẹp có sẵn (button, dialog, form) — copy vào project, sửa được tự do |
| **Tailwind CSS** | Viết style bằng class (VD `p-4 text-red-500`) thay vì file CSS riêng |
| **vite-plugin-pwa** | Biến app thành PWA (offline, installable) — 1 plugin, khỏi setup phức tạp |
| **Zustand** | State management — nhớ trạng thái UI (menu đang mở, tab đang chọn) |

**Vấn đề PWA giải cho da liễu**:
- Trực đêm không có wifi tốt → xem flashcard đã ôn hôm qua vẫn được (offline cache)
- Cài icon vào home điện thoại → mở 1 chạm như app native
- Update deck mới sáng nay → chiều mở app là thấy

---

## Món 3 — Telegram Bot (trợ lý push)

**Vì sao có web rồi vẫn cần bot?**

Web tuyệt vời khi anh **chủ động mở**. Nhưng anh không chủ động mở nếu quên. Bot Telegram làm ngược — **nó chủ động push tin cho anh**.

**Kịch bản dùng bot**:

- 6h30 sáng: bot gửi 5 flashcard, anh vừa uống cà phê vừa bấm rating trên điện thoại
- 12h trưa nghỉ giữa 2 ca khám: gõ `/quiz` vào bot, làm 5 câu MCQ ngẫu nhiên 3 phút
- Có case lạ đêm trực: gõ `/case`, bot gửi 1 case study da liễu ngẫu nhiên có ảnh, đọc giết thời gian chờ
- Cuối tuần: bot gửi báo cáo — tuần này anh đã ôn 250 thẻ, retention 78%, streak 12 ngày

**Bot hoạt động ra sao?**

Anh nói chuyện với `@BotFather` trên Telegram → nó cấp cho anh 1 "bot token" (dạng chuỗi ký tự dài). Sau đó code của anh (chạy trên máy hoặc trên Supabase Edge Function) dùng token đó gọi Telegram API để gửi tin.

**Chạy bot ở đâu?** 3 lựa chọn:

| Nơi chạy | Ưu | Nhược |
|---|---|---|
| Máy laptop cá nhân | Miễn phí, đơn giản | Phải bật máy đúng giờ |
| Supabase Edge Function + pg_cron | Cloud, luôn on, không cần máy anh chạy | Code phải viết bằng TypeScript (Deno runtime) |
| VPS Việt Nam (~50k/tháng) | IP Việt Nam để scrape site trường | Tốn tiền + quản server |

**Recommend cho bác sĩ**: **Supabase Edge Function**. Free, không cần máy chạy, share code + DB với web.

---

## Món 4 — Cloudflare R2 (kho ảnh giá rẻ)

**Vấn đề**: Supabase Storage free tier chỉ 1GB. 1 ảnh atlas cỡ 500KB. Chỉ chứa được ~2000 ảnh.

**Giải pháp**: **Cloudflare R2** — dịch vụ lưu ảnh giá rẻ của Cloudflare. Free tier **10GB**, và (điểm đặc biệt) **không tính phí egress** (mỗi lần user tải ảnh về).

**So sánh nhanh**:

| Dịch vụ | Free storage | Egress fee |
|---|---|---|
| Supabase Storage | 1 GB | Có tính (theo bandwidth) |
| Cloudflare R2 | 10 GB | Miễn phí |
| AWS S3 | 5 GB (12 tháng đầu) | $0.09/GB (đắt) |

**Ứng dụng cho da liễu**: 10,000 ảnh ca lâm sàng ~ 5GB → nằm gọn trong free tier R2. Web/bot load ảnh không lo bill tăng.

**Cách dùng**: script data pipeline upload ảnh lên R2, database chỉ lưu URL. Web/bot load ảnh trực tiếp từ R2 URL.

---

## Món 5 — Vercel (deploy web miễn phí)

**Vercel là gì?** Dịch vụ host website. Free tier "Hobby" cho cá nhân là quá đủ:
- 100 GB bandwidth/tháng
- Deploy không giới hạn
- Auto SSL (HTTPS)
- Custom domain (VD `dalieu.vn`)

**Cách hoạt động** (magic):

1. Anh push code lên GitHub (`git push`)
2. Vercel tự phát hiện có commit mới
3. Trong ~1 phút: build code, deploy, gửi anh URL mới
4. User mở app là thấy version mới ngay

Không cần setup server. Không cần config nginx. Không cần chăm nom.

---

## Món 6 — Python + curl_cffi (khi cần scrape web)

**Khi nào cần Python?** Khi anh muốn:
- Scrape đề thi từ trang moodle của trường Y
- Parse PDF sách CK1 thành flashcard
- Xử lý hàng loạt (batch process) — VD upload 500 ảnh cùng lúc

**Vì sao dùng Python?** Nhiều thư viện tốt cho scrape/parse. Cộng đồng lớn. Claude Code viết Python rất giỏi.

**Vấn đề đặc biệt**: nhiều site (British Council, test-english, các trang có Cloudflare) chặn HTTP request thường theo "TLS fingerprint" — nghĩa là chúng nhận ra request không phải từ browser thật là chặn.

**Giải pháp**: thư viện tên **`curl_cffi`** — giả TLS fingerprint của Chrome, qua được Cloudflare check dễ dàng. Đây là bí quyết mà nhiều dev không biết.

**Chạy Python ở đâu?**
- Trên laptop anh + scheduler OS (Task Scheduler Windows / launchd Mac / systemd Linux) — miễn phí, đơn giản
- Trên VPS nếu cần chạy 24/7

---

## Món 7 — Ollama (AI local, miễn phí, offline)

**Ollama là gì?** Wrapper cho phép chạy model AI open-source (Llama, Qwen, Gemma) **trên máy anh**, không cần internet, không tốn tiền.

**Vấn đề giải**: 
- Trực đêm không có wifi tốt → dùng Ollama offline
- Dữ liệu nhạy cảm không muốn gửi lên cloud → dùng local
- Task đơn giản (format JSON, dịch câu ngắn) → không cần Claude "xịn"

**Yêu cầu máy**: RAM ≥ 16GB cho model 7 tỷ tham số. RAM 8GB dùng model 3 tỷ nhẹ hơn.

**Chất lượng vs Claude**: model local yếu hơn Claude Sonnet ~50-70% quality nhưng miễn phí. Dùng cho task lặp đi lặp lại không cần chính xác cao.

---

## Bảng chọn stack theo nhu cầu

| Anh muốn làm gì | Món cần |
|---|---|
| Nhận flashcard sáng qua Telegram | Bot + Supabase pg_cron |
| Xem ảnh atlas offline khi trực | Web PWA + Cloudflare R2 |
| Chấm bài tập tự luận bằng AI | Supabase Edge Function + Claude/Gemini API |
| Lưu progress SRS theo Anki algorithm | Supabase Database + logic trong Edge Function |
| Scrape đề thi từ moodle trường | Python + curl_cffi + laptop scheduler |
| Share deck với đồng nghiệp | Supabase Auth + RLS sharing rule |
| Dùng offline không mạng | PWA + Ollama local |

---

## Luồng data điển hình (bức tranh chạy)

Kịch bản "sáng 6h30 nhận flashcard":

1. **pg_cron** trong Supabase (giống báo thức) đúng 6h30 giờ VN → gọi Edge Function
2. **Edge Function** truy vấn Database, lấy 5 flashcard due nhất cho user X
3. Edge Function gọi Telegram Bot API, gửi 5 tin nhắn kèm inline button
4. Anh nhận thông báo trên điện thoại, mở Telegram, bấm rating
5. Telegram gọi webhook về Edge Function khác (`bot-callback`)
6. Callback function cập nhật SRS state vào Database
7. Web app anh mở lên → thấy progress đã cập nhật (vì cùng chung 1 DB)

Xong 1 luồng, 5 phút, không cần máy anh chạy gì.

---

## Chi phí toàn stack (1 năm, dự án cá nhân)

| Món | Chi phí |
|---|---|
| Supabase | $0 (free tier) |
| Vercel | $0 (Hobby) |
| Cloudflare R2 | $0 (10GB free) |
| GitHub | $0 |
| Claude Code API | $60-180 |
| MiniMax API | $24-60 |
| Ollama | $0 (điện + phần cứng anh có sẵn) |
| Domain optional | $15 |
| **Tổng** | **$85-255/năm (~2-6 triệu VND)** |

Toàn bộ hạ tầng miễn phí. Chỉ tốn AI API — mà multi-model workflow (file 09) tiết kiệm được 40-60%.

---

## Nếu anh chỉ nhớ được 3 điều

1. **Supabase là "backend all-in-one"** — 1 dịch vụ thay 5 dịch vụ
2. **Web PWA + Telegram bot** là combo mạnh nhất cho app học tập cá nhân
3. **Cloudflare R2 rẻ hơn Supabase Storage** cho lưu ảnh — dùng khi >1GB

## Đọc tiếp

- [05_content_data_pipeline.md](05_content_data_pipeline.md) — Nguyên tắc seed content không sai (quan trọng cho da liễu)
- [07_du_an_da_lieu_kickoff.md](07_du_an_da_lieu_kickoff.md) — Áp dụng vào dự án cụ thể
