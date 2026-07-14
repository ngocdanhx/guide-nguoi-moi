# 01 — Các khái niệm căn bản

> Anh không cần nhớ hết. Đọc để "quen mặt" thôi. Khi làm dự án thật, quay lại tra là được.

## Trước hết: dev bây giờ khác gì dev ngày xưa?

Ngày xưa, viết phần mềm giống như tự tay khâu 1 chiếc áo. Người thợ ngồi cắm mặt vào bàn máy may, đo, cắt, khâu từng mũi. Muốn thêm 1 cái túi thì phải khâu thêm.

Bây giờ, có AI làm dev — giống như anh trở thành **thợ may cả xưởng**. Anh nói "làm cho tôi cái áo có 4 túi, cổ tàu, size L", AI sẽ khâu. Nhiệm vụ của anh là **đo lường yêu cầu, kiểm tra sản phẩm, và biết chê đúng chỗ**.

Bác sĩ hình dung: giống như anh làm bác sĩ điều trị chính, còn AI là các bác sĩ nội trú thực tập giỏi. Anh vẫn phải khám bệnh, ra chỉ định, ký hồ sơ. AI làm chi tiết theo yêu cầu, nhưng nếu anh chỉ định sai thì AI cũng làm sai theo.

Từ đó ra 8 khái niệm anh cần "quen mặt":

---

## 1. Claude Code — công cụ để "sai" AI vào máy anh

**Claude Code** là 1 phần mềm anh cài vào máy tính (chạy trong Terminal hoặc VS Code). Sau khi cài, anh có thể "sai" Claude đọc file, sửa file, chạy lệnh máy tính — không cần anh gõ tay từng dòng.

**Khác gì ChatGPT trên web?** Trên web, anh copy-paste đoạn code, ChatGPT gợi ý, xong anh phải quay lại paste lên máy. Claude Code thì đi thẳng vào máy anh, tự đọc code, tự sửa, tự chạy test — anh chỉ ngồi review.

**Chạy được ở đâu?** Terminal (cửa sổ dòng lệnh), VS Code (chương trình soạn thảo mã), JetBrains IDE, hoặc web. Trong bộ tài liệu này em recommend anh dùng **VS Code** vì thân thiện với người mới nhất.

**Chi phí?** Trả theo dung lượng dùng — trung bình bác sĩ dùng cá nhân ~$5-15/tháng. Rẻ hơn 1 buổi cà phê.

---

## 2. CLAUDE.md — "sổ tay dự án" cho AI đọc

Mỗi dự án có 1 file tên `CLAUDE.md` ở thư mục gốc. File này là **hồ sơ dự án** — mỗi lần Claude mở dự án là tự đọc file này trước.

Nội dung thường có:
- Dự án làm cái gì
- Dùng công nghệ nào
- Quy ước đặt tên, format code
- Những chỗ hay sai, phải nhớ đừng phạm

**Ví dụ dễ hiểu**: giống bảng bàn giao ca trực. Bác sĩ ca sáng ghi cho ca chiều: "Giường 5 mới chuyển vào, dị ứng penicillin. Giường 12 chờ kết quả sinh thiết." Ca chiều đọc là biết ngay.

Claude cũng vậy — mỗi session mới bắt đầu, đọc CLAUDE.md là biết dự án ở đâu, đừng làm gì.

**Nên viết ngắn hay dài?** Ngắn (~50-80 dòng) là đủ cho người mới. Viết dài quá Claude bỏ qua — giống như bàn giao ca 5 trang giấy, bác sĩ đọc lướt bỏ hết chi tiết.

---

## 3. Sổ tay nhớ dài hạn — MEMORY.md hoặc NOTES.md

Nếu CLAUDE.md là "bàn giao ca", thì MEMORY.md là **cuốn sổ tổng kết bệnh án khoa** — ghi lại những bài học đã học được qua nhiều tháng.

Mỗi khi anh sửa 1 bug lớn, hoặc phát hiện ra 1 "gotcha" (chỗ dễ mắc lừa), ghi vào NOTES.md 1 đoạn ngắn:
- Ngày phát hiện
- Vấn đề gì
- Root cause (nguyên nhân gốc)
- Đã fix thế nào
- Bài học rút ra

Sau vài tháng, cuốn sổ này rất giá trị. Claude khi cần cũng đọc để không lặp lại lỗi cũ.

**Vì sao quan trọng?** AI không có trí nhớ dài hạn. Không có sổ này, anh sửa 1 bug hôm nay, 3 tháng sau AI lại sinh code sinh cùng bug đó. Nhiều lần phát điên.

---

## 4. Skill — "quy trình đóng gói" mà AI phải theo

Trong bệnh viện, "quy trình rửa tay 6 bước", "quy trình kiểm tra 5 đúng khi phát thuốc" là những **quy trình chuẩn bắt buộc** — ai vào cũng phải theo, không có ngoại lệ.

**Skill** trong Claude Code là tương tự — 1 quy trình chuẩn được đóng gói thành 1 file, khi tình huống match thì AI **bắt buộc** phải theo.

Ví dụ 4 skill "phải có" cho mọi dự án:

| Skill | Tình huống trigger | Bắt buộc làm gì |
|---|---|---|
| `verification-before-completion` | Trước khi báo "xong" | Phải chạy test/build, đọc output, chắc chắn PASS mới nói xong |
| `systematic-debugging` | Khi có bug | Phải tìm root cause, không được vá triệu chứng |
| `writing-plans` | Task lớn > 30 phút | Phải viết plan trước, không code luôn |
| `no-secrets-in-git` | Trước khi commit | Phải grep secret trong diff, block nếu có |

Skill là **cách tự bảo vệ mình**. AI thường "vibe" — làm nhanh, làm ẩu, tự khen "xong rồi" khi thật ra chưa. Skill là bộ khung ép AI đi chậm, làm đúng.

---

## 5. Sub-agent — Claude đóng vai "bác sĩ chuyên khoa"

Trong 1 ca hội chẩn, anh không hỏi 1 bác sĩ đa khoa mà hỏi từng chuyên khoa: nội tiết, tim mạch, ngoại tổng quát. Mỗi người có kiến thức chuyên sâu hẹp.

**Sub-agent** cũng vậy. Anh có thể tạo nhiều "vai" cho Claude:
- Vai frontend engineer — chuyên về giao diện
- Vai backend engineer — chuyên về database
- Vai reviewer — chuyên đọc code tìm bug
- Vai content curator — chuyên parse sách, verify flashcard

Khi anh cần review 1 đoạn code, Claude spawn ra "sub-agent reviewer" — nó chỉ đọc code, không sửa. Nó chỉ tập trung tìm lỗi.

**Lợi ích thứ 2**: chạy song song. 1 buổi trưa anh muốn parse 5 chương sách CK1 → spawn 5 sub-agent, mỗi cái 1 chương, chạy đồng thời. Xong sau ~5 phút thay vì 30 phút tuần tự.

Người mới không cần dùng ngay — nhưng biết là có, để sau này khi task lớn thì lôi ra dùng.

---

## 6. Hook — "camera an ninh" trong bệnh viện

Rule ghi trong CLAUDE.md hay skill chỉ là **văn bản khuyến cáo** — Claude có thể quên hoặc bỏ qua khi vội.

**Hook** là **camera + cảm biến báo động** — nó không phải Claude, mà là 1 chương trình nhỏ chạy trên máy anh, tự động kiểm tra mỗi khi Claude định làm gì.

4 tình huống hook thường dùng:

- **Trước khi Claude chạy 1 lệnh** — hook kiểm tra: nếu lệnh là `git commit` mà file có chứa mật khẩu, hook chặn ngay, Claude không chạy được. Giống chuông báo khi ai đó cầm file bệnh án ra khỏi khoa.
- **Sau khi Claude sửa 1 file** — hook tự chạy trình format code, làm cho code đẹp. Giống camera thấy 1 y tá bỏ dụng cụ sai chỗ → tự động di chuyển về đúng vị trí.
- **Đầu mỗi session** — hook cho Claude biết hôm nay anh đang làm việc trên branch nào, có file gì chưa commit. Giống bảng thông tin ca trực đầu buổi.
- **Cuối session** — hook gửi notification cho máy tính báo "Claude đã xong việc, quay lại đọc kết quả".

Người mới có thể bỏ qua hook 1-2 tuần đầu. Khi hiểu về secret + git rồi thì cài hook chặn commit `.env` (bước bắt buộc — em có nói kỹ ở file 06).

---

## 7. Slash command — "phím tắt" để chạy quy trình quen

Trong bệnh viện có phím tắt trên máy — bấm F5 in đơn thuốc, bấm F8 mở hồ sơ. Không cần click chuột nhiều bước.

**Slash command** trong Claude Code là như vậy. Anh gõ `/` là 1 danh sách lệnh hiện ra:

- `/init` — sinh CLAUDE.md khởi tạo cho dự án mới
- `/help` — xem hướng dẫn
- `/clear` — xoá bộ nhớ ngắn hạn của Claude (khi session bị nhiễu)
- `/model` — đổi model AI (Claude, MiniMax, …)
- Anh tự viết được: `/specify`, `/implement`, `/review`, `/commit` — quy trình 4 bước làm 1 feature mới

Sau 1 tuần dùng, anh sẽ quen 5-10 slash command mình hay dùng — công việc chạy nhanh hơn nhiều.

---

## 8. Settings — cấu hình cho Claude Code

Mỗi dự án có 1 file `settings.json` để cấu hình:
- Dùng model AI nào mặc định
- Wire các hook vào đâu
- Cho phép Claude tự chạy lệnh nào mà không hỏi permission (VD `git status`, `npm test` là những lệnh an toàn — cho tự chạy để đỡ bị hỏi)
- Hiển thị status line dưới cùng ra sao

Anh không cần viết tay — Claude tự sinh. Anh chỉ cần biết là có file này để khi cần chỉnh thì tra vào đó.

---

## 9. Tổng kết: bức tranh chung

Hình dung 1 phòng khám:

- **Anh** = bác sĩ trưởng khoa
- **Claude Code** = phòng làm việc + máy tính điện tử
- **CLAUDE.md** = bảng bàn giao ca dán trên tường
- **MEMORY.md** = tủ hồ sơ tổng hợp bài học 5 năm qua
- **Skill** = tập quy trình chuẩn (SOP) treo trên tường
- **Sub-agent** = danh sách bác sĩ chuyên khoa gọi hội chẩn
- **Hook** = camera + hệ thống báo động
- **Slash command** = phím tắt trên máy
- **Settings** = bảng cấu hình phòng (nhiệt độ, ánh sáng, …)

Anh không cần thiết lập hết trong tuần đầu. Bắt đầu bằng Claude Code + CLAUDE.md + 2-3 skill "phải có" là đủ. Các thứ khác thêm dần khi cần.

---

## Nếu anh chỉ nhớ được 3 điều

1. **CLAUDE.md là bảng bàn giao** — viết ngắn, viết đúng, Claude sẽ hiểu dự án
2. **Skill là quy trình bắt buộc** — cài các skill "verification-before-completion" và "systematic-debugging" để AI không làm ẩu
3. **AI không nhớ dài hạn** — mỗi bug lớn sửa xong, ghi 1 đoạn vào NOTES.md để 3 tháng sau không lặp lại

## Đọc tiếp

- [02_spec_driven_workflow.md](02_spec_driven_workflow.md) — Cách "ra chỉ định" cho AI để nó làm đúng: spec-driven vs vibe coding
