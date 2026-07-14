# 00 — Bắt đầu từ đây (đọc file này trước)

> Chào anh! Đây là bộ tài liệu tiếng Việt giúp một bác sĩ chuẩn bị học chuyên khoa 1 da liễu **tiếp cận** cách làm việc với AI — không phải trở thành lập trình viên.

## Định vị bộ tài liệu này

**KHÔNG phải là**:
- Sách "học code từ 0 đến chuyên nghiệp"
- Hướng dẫn step-by-step làm dự án A đến Z
- Tài liệu kỹ thuật tra câu lệnh

**LÀ**:
- **Bản đồ tổng thể** cho một người từ ngành khác đi sang, hiểu bức tranh chung
- Giới thiệu **các khái niệm quan trọng** — chỉ cần "quen mặt", không cần thuộc
- Mô tả **các luồng chạy, các vấn đề giải quyết được** — hiểu chỗ nào miễn phí, chỗ nào trả tiền
- **Kịch bản giả lập** cho các tình huống thực tế
- Đưa **định hướng khởi động** — đọc xong biết cần bắt đầu từ đâu

**Nói cách khác**: đọc xong, anh không code được ngay — nhưng anh **biết mình đang đứng đâu**, có thể mở Claude Code và nói "tôi muốn làm X" — AI sẽ hướng dẫn từng bước cụ thể.

Anh không phải "thợ lành nghề". Anh là **bác sĩ điều trị chính**, đưa yêu cầu, kiểm tra sản phẩm. AI làm chi tiết.

---

## Bối cảnh của người đọc

Tài liệu này viết cho:
- **Bác sĩ chuẩn bị học chuyên khoa 1 Da liễu ở Hà Nội**
- Chưa từng làm lập trình chuyên nghiệp
- Có nhu cầu **xây bộ quy trình + ứng dụng hỗ trợ học tập** để cải thiện điểm thi
- Có ~2-3 giờ mỗi tuần trong khi đang bận công việc bác sĩ
- Chi phí trong 2 tháng đầu: **400-800 nghìn** (chủ yếu là phí AI dev, mọi hạ tầng đều miễn phí)

Nếu anh khớp bối cảnh này, tài liệu sẽ hữu ích.

---

## Danh sách file

### File ĐỌC ĐẦU TIÊN — bắt buộc trước tất cả

| File | Nội dung | Ước lượng đọc |
|---|---|---|
| **[KET_QUA_DU_AN.md](KET_QUA_DU_AN.md)** | **Nếu dự án thành công anh có gì sau 2 tháng? 5 kết quả đo được + 4 bảng so sánh với cách truyền thống + nghiên cứu khoa học hậu thuẫn** | **15 phút — ĐỌC TRƯỚC MỌI THỨ** |

Nếu không thấy giá trị đủ mạnh sau khi đọc file trên → dừng, đừng bắt đầu. Mấy tuần dựng đồ không đáng cho người không rõ mục tiêu.

### Nếu đã quyết bắt đầu, đọc 9 file dưới theo thứ tự

| # | File | Nội dung | Ước lượng đọc |
|---|---|---|---|
| 01 | [01_khai_niem_can_ban.md](01_khai_niem_can_ban.md) | 8 khái niệm cơ bản (analogy y khoa): CLAUDE.md, skill, hook, sub-agent... | 15 phút |
| 02 | [02_spec_driven_workflow.md](02_spec_driven_workflow.md) | Cách "ra chỉ định" cho AI theo Spec Kit (bỏ vibe coding) | 15 phút |
| 03 | [03_setup_may.md](03_setup_may.md) | Chuẩn bị máy: cần cài gì, mỗi cái miễn phí hay trả tiền | 10 phút đọc + 2 giờ cài |
| 04 | [04_tech_stack.md](04_tech_stack.md) | 7 món đồ nghề, mỗi món giải quyết vấn đề gì, miễn phí ở đâu | 15 phút |
| 05 | [05_content_data_pipeline.md](05_content_data_pipeline.md) | Nguyên tắc chuẩn bị nội dung không sai (quan trọng cho da liễu) | 15 phút |
| 06 | [06_bao_mat_git.md](06_bao_mat_git.md) | Không lộ khoá + git workflow | 10 phút |
| 07 | [07_du_an_da_lieu_kickoff.md](07_du_an_da_lieu_kickoff.md) | Khởi động dự án cụ thể — luồng chạy + kịch bản + lộ trình 2 tháng | 20 phút |
| 08 | [08_tai_lieu_tham_khao.md](08_tai_lieu_tham_khao.md) | URL nguồn để tra sâu | Tra khi cần |
| 09 | [09_multi_model_workflow.md](09_multi_model_workflow.md) | Dùng nhiều AI cùng lúc (Claude + MiniMax + trên máy) — tiết kiệm 40-60% | 15 phút |

Ngoài ra:
- **[research-notes.md](research-notes.md)** — Ghi chép nghiên cứu chi tiết (~15.000 từ, ~100 nguồn). Dùng khi cần verify claim cụ thể.

**Tổng thời gian**: đọc ~2 giờ + setup ~2 giờ + khởi động ~2 giờ = **~6 giờ để sẵn sàng bắt đầu tuần đầu tiên**.

---

## Lộ trình đọc gợi ý (chia làm 4 buổi)

### Buổi 1 — Bức tranh lớn (75 phút)

- Đọc file **00** (file này)
- Đọc file **KET_QUA_DU_AN.md** — nếu dự án thành công anh có gì (**quyết định có làm hay không sau file này**)
- Đọc file **01** — 8 khái niệm cơ bản
- Đọc file **02** — Spec Kit là gì

**Sau buổi 1**: hiểu Claude Code là gì, tại sao Spec Kit quan trọng.

### Buổi 2 — Chuẩn bị đồ nghề (2-3 giờ)

- Đọc file **03** — vừa đọc vừa cài (VS Code, Claude Code, Git, tài khoản)
- Đọc file **09** — hiểu đa AI (Claude + MiniMax)
- Đọc file **06** — bảo mật khoá trước khi commit đầu tiên

**Sau buổi 2**: máy đã cài đủ công cụ, hiểu vì sao không commit `.env`.

### Buổi 3 — Đồ nghề + nội dung (1 giờ)

- Đọc file **04** — 7 món đồ nghề (miễn phí ở đâu)
- Đọc file **05** — nguyên tắc chuẩn bị nội dung

**Sau buổi 3**: biết chọn công cụ nào, hiểu nguyên tắc "correct-by-construction" — đừng để AI chế đáp án y khoa.

### Buổi 4 — Khởi động (1 giờ đọc + tuỳ anh triển khai)

- Đọc file **07** — khởi động dự án da liễu
- Tạo thư mục dự án đầu tiên, viết CLAUDE.md

**Sau buổi 4**: đã có kho code mới, sẵn sàng bắt đầu tuần 2 của lộ trình 2 tháng.

---

## Nếu anh vội chỉ muốn 1 file

Đọc file **KET_QUA_DU_AN.md** trước → nếu thấy giá trị đủ mạnh → đọc file **07** (khởi động) → có đủ ngữ cảnh + link back các file khác khi cần.

---

## Nguyên tắc chung khi đọc

1. **Không cần hiểu 100% ngay lần đầu** — nhiều khái niệm chỉ sáng tỏ khi làm thực tế
2. **Đọc chậm file 05 và 06** — 2 file có nội dung "sai là đau" (nội dung + bảo mật)
3. **Đọc trên máy có mạng** — nhiều link ngoài cần tra
4. **Ghi chú riêng** khi thấy chỗ nào không hiểu — sau này quay lại

---

## Câu hỏi thường gặp

**Q: Em không biết code. Có làm được không?**

A: Có. AI viết code cho anh — nhiệm vụ của anh là **viết yêu cầu đúng, xem kết quả, kiểm tra**. Kỹ năng này gần với "quản lý ê-kíp mổ" hơn "tự mổ". Nhưng anh vẫn cần hiểu **luồng chạy** để không bị AI qua mặt.

**Q: Chi phí bao nhiêu?**

A: Trong 2 tháng đầu: **400-800 nghìn**. Chủ yếu là phí AI dev. Mọi hạ tầng (kho hồ sơ, đưa web lên mạng, kho ảnh) đều miễn phí ở mức dùng cá nhân. Chi tiết ở file 03.

**Q: Có thể học vừa làm CK1 vừa xây ứng dụng không?**

A: Có, nếu dành 2-3 giờ mỗi tuần. Xem lộ trình 2 tháng trong file 07. **Đừng** cố xây ứng dụng phức tạp — bản đơn giản nhất trước, mở rộng sau khi thi xong.

**Q: Có mua khoá học Anki/dev không?**

A: Chưa cần. Tài liệu này đủ cho 3-6 tháng đầu. Sau đó nếu muốn học sâu:
- Anki: AnKing YouTube (miễn phí)
- Claude Code: docs.claude.com/docs/en/best-practices (chính thức)
- Spec Kit: github.com/github/spec-kit
- Superpowers: blog.fsck.com

**Q: Có nên dùng ChatGPT/Cursor thay Claude Code không?**

A: Cursor cũng dùng được. ChatGPT web không thay được vì không có tool truy cập máy thật. Bộ tài liệu này thiết kế theo hướng đa AI: dùng Claude Code cho việc chính, kèm MiniMax cho việc rẻ hơn. Chi tiết ở file **09**.

**Q: Có bắt buộc dùng Telegram bot không? Chỉ web được không?**

A: Chỉ web hoàn toàn được. Nhưng Telegram bot có lợi thế đẩy thông báo tự nhiên — 6h30 sáng chuông reo là anh mở, còn web thì anh phải nhớ tự mở. Combo web + bot là mạnh nhất. Nếu muốn đơn giản, làm web trước rồi bot sau cũng được.

---

## Cấu trúc thư mục sau khi làm xong

```
~/projects/ngocdanh/
├── guide-nguoi-moi/          ← Tài liệu anh đang đọc
│   ├── 00_START_HERE.md
│   ├── KET_QUA_DU_AN.md       ← Đọc trước tất cả
│   ├── 01_khai_niem_can_ban.md
│   ├── ... (7 file khác)
│   ├── 09_multi_model_workflow.md
│   └── research-notes.md
│
└── da-lieu-hoc-tap/          ← Dự án anh sẽ tạo
    ├── CLAUDE.md              ← Hồ sơ dự án
    ├── .claude/               ← Skill, hook, agent cấu hình
    ├── frontend/              ← Trang web
    ├── supabase/              ← Kho hồ sơ + hàm xử lý
    ├── services/              ← Lớp dịch vụ chung
    ├── scripts/               ← Xử lý dữ liệu (đọc sách CK1)
    ├── docs/                  ← Kế hoạch, audit, spec
    ├── .env                   ← Khoá (không commit)
    ├── .env.example
    └── README.md
```

---

## Cách feedback / cập nhật tài liệu

Tài liệu này viết bởi AI, có thể có chỗ chưa chính xác. Khi anh phát hiện chỗ nào:

- **Sai fact** → sửa trực tiếp file .md
- **Chưa rõ** → mở Claude Code trong thư mục này, gõ "sửa file NN để rõ hơn phần X"
- **Cần thêm ví dụ** → gõ "thêm ví dụ cho phần Y trong file NN"

Vì đây là thư mục cá nhân, không cần review. Cứ sửa thoải mái, coi như sổ tay riêng.

---

**Sẵn sàng chưa?** Bắt đầu bằng [KET_QUA_DU_AN.md](KET_QUA_DU_AN.md) để quyết định có làm hay không.
