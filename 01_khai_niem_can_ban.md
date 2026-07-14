# 01 — Các khái niệm căn bản

> Anh không cần nhớ hết. Đọc để "quen mặt" thôi. Khi làm dự án thật, quay lại tra là được.

## Trước hết: dev bây giờ khác gì dev ngày xưa?

Ngày xưa, viết phần mềm giống như tự tay khâu một chiếc áo. Người thợ ngồi cắm mặt vào bàn máy may, đo, cắt, khâu từng mũi. Muốn thêm một cái túi thì phải khâu thêm.

Bây giờ, có AI làm dev — giống như anh trở thành **thợ may cả xưởng**. Anh nói "làm cho tôi cái áo có 4 túi, cổ tàu, size L", AI sẽ khâu. Nhiệm vụ của anh là **đo lường yêu cầu, kiểm tra sản phẩm, và biết chê đúng chỗ**.

Bác sĩ hình dung: giống như anh làm bác sĩ điều trị chính, còn AI là các bác sĩ nội trú thực tập giỏi. Anh vẫn phải khám bệnh, ra chỉ định, ký hồ sơ. AI làm chi tiết theo yêu cầu, nhưng nếu anh chỉ định sai thì AI cũng làm sai theo.

Từ đó ra 8 khái niệm anh cần "quen mặt".

---

## 1. Claude Code — công cụ để "sai" AI vào máy anh

**Claude Code** là một phần mềm anh cài vào máy tính (chạy trong Terminal hoặc VS Code). Sau khi cài, anh có thể "sai" AI đọc file, sửa file, chạy lệnh máy tính — không cần anh gõ tay từng dòng.

**Khác gì ChatGPT trên web?** Trên web, anh copy-paste đoạn code, ChatGPT gợi ý, xong anh phải quay lại paste lên máy. Claude Code thì đi thẳng vào máy anh, tự đọc code, tự sửa, tự chạy kiểm tra — anh chỉ ngồi xem.

**Chạy được ở đâu?** Terminal, VS Code, JetBrains IDE, hoặc web. Trong bộ tài liệu này em khuyên anh dùng **VS Code** vì thân thiện với người mới nhất.

**Chi phí**: khoảng 200-500 nghìn/tháng cho bác sĩ dùng cá nhân. Rẻ hơn 1 buổi cà phê + ăn tối cả tuần.

---

## 2. CLAUDE.md — "sổ tay dự án" cho AI đọc

Mỗi dự án có 1 file tên `CLAUDE.md` ở thư mục gốc. File này là **hồ sơ dự án** — mỗi lần Claude mở dự án là tự đọc file này trước.

Nội dung thường có:
- Dự án làm gì
- Dùng công cụ nào
- Quy ước đặt tên, định dạng code
- Những chỗ hay sai, phải nhớ đừng phạm

**Ví dụ dễ hiểu**: giống bảng bàn giao ca trực. Bác sĩ ca sáng ghi cho ca chiều: "Giường 5 mới chuyển vào, dị ứng penicillin. Giường 12 chờ kết quả sinh thiết." Ca chiều đọc là biết ngay.

AI cũng vậy — mỗi buổi làm việc mới bắt đầu, đọc CLAUDE.md là biết dự án ở đâu, đừng làm gì.

**Nên viết ngắn hay dài?** Ngắn (~50-80 dòng) là đủ cho người mới. Viết dài quá AI bỏ qua — giống như bàn giao ca 5 trang giấy, bác sĩ đọc lướt bỏ hết chi tiết.

Chạy lệnh `/init` trong Claude Code là AI tự sinh file mẫu cho anh, sau đó anh chỉ cần chỉnh lại.

---

## 3. Sổ tay nhớ dài hạn — MEMORY.md hoặc NOTES.md

Nếu CLAUDE.md là "bàn giao ca", thì MEMORY.md là **cuốn sổ tổng kết bệnh án khoa** — ghi lại những bài học đã học được qua nhiều tháng.

Mỗi khi anh sửa xong 1 lỗi lớn, hoặc phát hiện ra 1 chỗ dễ mắc lừa, ghi vào NOTES.md một đoạn ngắn:
- Ngày phát hiện
- Vấn đề gì
- Nguyên nhân gốc
- Đã sửa thế nào
- Bài học rút ra

Sau vài tháng, cuốn sổ này rất giá trị. AI khi cần cũng đọc để không lặp lại lỗi cũ.

**Vì sao quan trọng?** AI không có trí nhớ dài hạn. Không có sổ này, anh sửa 1 lỗi hôm nay, 3 tháng sau AI lại sinh code sinh cùng lỗi đó. Nhiều lần phát điên.

---

## 4. Skill — "quy trình đóng gói" mà AI phải theo

Trong bệnh viện, "quy trình rửa tay 6 bước", "quy trình kiểm tra 5 đúng khi phát thuốc" là những **quy trình chuẩn bắt buộc** — ai vào cũng phải theo, không có ngoại lệ.

**Skill** trong Claude Code là tương tự — một quy trình chuẩn được đóng gói thành 1 file, khi tình huống khớp thì AI **bắt buộc** phải theo.

Ví dụ 4 skill "phải có" cho mọi dự án:

| Skill | Khi nào kích hoạt | Bắt buộc làm gì |
|---|---|---|
| `verification-before-completion` | Trước khi báo "xong" | Phải chạy kiểm tra, đọc kết quả, chắc chắn PASS mới nói xong |
| `systematic-debugging` | Khi có lỗi | Phải tìm nguyên nhân gốc, không được vá triệu chứng |
| `writing-plans` | Việc lớn > 30 phút | Phải viết kế hoạch trước, không làm luôn |
| `no-secrets-in-git` | Trước khi commit | Phải dò tìm khoá trong file, chặn nếu có |

Skill là **cách tự bảo vệ mình**. AI thường "làm liều" — làm nhanh, làm ẩu, tự khen "xong rồi" khi thật ra chưa. Skill là bộ khung ép AI đi chậm, làm đúng.

**Chi phí**: miễn phí (skill là file văn bản trong dự án).

---

## 5. Sub-agent — AI đóng vai "bác sĩ chuyên khoa"

Trong 1 ca hội chẩn, anh không hỏi 1 bác sĩ đa khoa mà hỏi từng chuyên khoa: nội tiết, tim mạch, ngoại. Mỗi người có kiến thức chuyên sâu hẹp.

**Sub-agent** cũng vậy. Anh có thể tạo nhiều "vai" cho AI:
- Vai giao diện — chuyên về màn hình
- Vai cơ sở dữ liệu — chuyên về kho hồ sơ
- Vai người kiểm tra — chuyên đọc code tìm lỗi
- Vai người soạn nội dung — chuyên đọc sách, kiểm tra thẻ ôn

Khi anh cần xem lại một đoạn code, AI có thể sinh ra "sub-agent kiểm tra" — nó chỉ đọc, không sửa. Nó chỉ tập trung tìm lỗi.

**Lợi ích thứ 2**: chạy song song. Một buổi trưa anh muốn đọc 5 chương sách CK1 → sinh ra 5 sub-agent, mỗi cái 1 chương, chạy đồng thời. Xong sau ~5 phút thay vì 30 phút tuần tự.

Người mới không cần dùng ngay — nhưng biết là có, để sau này khi việc lớn thì lôi ra dùng.

---

## 6. Hook — "camera an ninh" trong bệnh viện

Quy tắc ghi trong CLAUDE.md hay skill chỉ là **văn bản khuyến cáo** — AI có thể quên hoặc bỏ qua khi vội.

**Hook** là **camera + cảm biến báo động** — nó không phải AI, mà là một chương trình nhỏ chạy trên máy anh, tự động kiểm tra mỗi khi AI định làm gì.

4 tình huống hook thường dùng:

- **Trước khi AI chạy 1 lệnh** — hook kiểm tra: nếu lệnh là `git commit` mà file có chứa mật khẩu, hook chặn ngay, AI không chạy được. Giống chuông báo khi ai đó cầm hồ sơ bệnh án ra khỏi khoa.
- **Sau khi AI sửa 1 file** — hook tự chạy chương trình định dạng, làm cho code đẹp. Giống camera thấy y tá bỏ dụng cụ sai chỗ → tự động di chuyển về đúng vị trí.
- **Đầu mỗi buổi** — hook cho AI biết hôm nay anh đang làm việc trên nhánh nào, có file gì chưa commit. Giống bảng thông tin ca trực đầu buổi.
- **Cuối buổi** — hook gửi thông báo cho máy tính báo "AI đã xong việc, quay lại đọc kết quả".

Người mới có thể bỏ qua hook 1-2 tuần đầu. Khi hiểu về khoá + git rồi thì cài hook chặn commit `.env` (bước bắt buộc — em nói kỹ ở file 06).

**Chi phí**: miễn phí.

---

## 7. Slash command — "phím tắt" chạy quy trình quen

Trong bệnh viện có phím tắt trên máy — bấm F5 in đơn thuốc, bấm F8 mở hồ sơ. Không cần click chuột nhiều bước.

**Slash command** trong Claude Code tương tự. Anh gõ `/` là danh sách lệnh hiện ra:

- `/init` — sinh CLAUDE.md khởi tạo cho dự án mới
- `/help` — xem hướng dẫn
- `/clear` — xoá bộ nhớ ngắn hạn của AI (khi buổi bị nhiễu)
- `/model` — đổi model AI (Claude, MiniMax, ...)
- Anh tự viết được: `/specify`, `/implement`, `/review`, `/commit` — quy trình 4 bước làm 1 chức năng mới

Sau 1 tuần dùng, anh sẽ quen 5-10 lệnh tắt mình hay dùng — công việc chạy nhanh hơn nhiều.

**Chi phí**: miễn phí.

---

## 8. Cấu hình — nơi wire tất cả lại

Mỗi dự án có 1 file cấu hình chính:
- Dùng model AI nào mặc định
- Wire các hook vào đâu
- Cho phép AI tự chạy lệnh nào mà không hỏi (ví dụ `git status`, `npm test` là những lệnh an toàn — cho tự chạy để đỡ bị hỏi)
- Hiển thị thanh trạng thái dưới cùng ra sao

Anh không cần viết tay — AI tự sinh. Anh chỉ cần biết là có file này để khi cần chỉnh thì tra vào đó.

**Chi phí**: miễn phí.

---

## Tổng kết: bức tranh chung

Hình dung một phòng khám:

- **Anh** = bác sĩ trưởng khoa
- **Claude Code** = phòng làm việc + máy tính điện tử
- **CLAUDE.md** = bảng bàn giao ca dán trên tường
- **MEMORY.md** = tủ hồ sơ tổng hợp bài học 5 năm qua
- **Skill** = tập quy trình chuẩn treo trên tường
- **Sub-agent** = danh sách bác sĩ chuyên khoa gọi hội chẩn
- **Hook** = camera + hệ thống báo động
- **Slash command** = phím tắt trên máy
- **Cấu hình** = bảng cấu hình phòng (nhiệt độ, ánh sáng, ...)

Anh không cần thiết lập hết trong tuần đầu. Bắt đầu bằng Claude Code + CLAUDE.md + 2-3 skill "phải có" là đủ. Các thứ khác thêm dần khi cần.

---

## Nếu anh chỉ nhớ được 3 điều

1. **CLAUDE.md là bảng bàn giao** — viết ngắn, viết đúng, AI sẽ hiểu dự án
2. **Skill là quy trình bắt buộc** — cài các skill "verification-before-completion" và "systematic-debugging" để AI không làm ẩu
3. **AI không nhớ dài hạn** — mỗi lỗi lớn sửa xong, ghi một đoạn vào NOTES.md để 3 tháng sau không lặp lại

## Đọc tiếp

- [02_spec_driven_workflow.md](02_spec_driven_workflow.md) — Cách "ra chỉ định" cho AI theo Spec Kit
