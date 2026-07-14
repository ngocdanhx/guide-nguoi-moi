# 00 — Bắt đầu từ đây (đọc file này trước)

> Chào anh! Đây là bộ tài liệu tiếng Việt giúp một bác sĩ chuẩn bị học chuyên khoa 1 da liễu **tiếp cận** với cách làm việc mới cùng AI — không phải trở thành lập trình viên.

## Định vị bộ tài liệu này

**Đây KHÔNG phải là**:
- Hướng dẫn "học code từ 0 đến pro"
- Sách step-by-step làm 1 dự án cụ thể từ A đến Z
- Tài liệu kỹ thuật để tra code snippet

**Đây LÀ**:
- **Bản đồ tổng thể** cho một người từ ngành khác đi sang, để hiểu bức tranh chung
- Giới thiệu **các khái niệm quan trọng** — anh cần "quen mặt" chứ không cần thuộc
- Mô tả **các luồng chạy, luồng xử lý** — hiểu vấn đề gì giải quyết được, giải quyết ra sao
- Đưa **định hướng khởi động** — sau khi đọc, anh biết cần bắt đầu từ đâu

**Nói cách khác**: đọc xong, anh không code được ngay — nhưng anh **biết mình đang đứng đâu**, có thể mở Claude Code và nói "tôi muốn làm X" — và Claude sẽ hướng dẫn từng bước cụ thể.

Anh không phải "thợ lành nghề". Anh là **bác sĩ điều trị chính**, đưa yêu cầu, kiểm tra sản phẩm. AI làm chi tiết.

---

## Bối cảnh của người đọc

Tài liệu này viết cho:
- **Bác sĩ chuẩn bị học chuyên khoa 1 Da liễu ở Hà Nội**
- Chưa từng làm lập trình chuyên nghiệp
- Có nhu cầu **xây bộ quy trình + app hỗ trợ học tập** để cải thiện điểm thi
- Có ~2-3 giờ/tuần trong khi đang bận với công việc bác sĩ
- Chi phí ~$85-255/năm cho toàn hệ thống (tương đương 2-6 triệu VND — rẻ hơn 1 buổi cà phê + ăn tối cả năm)

Nếu anh khớp bối cảnh này, tài liệu sẽ hữu ích.

---

## Danh sách 9 file (đọc theo thứ tự)

| # | File | Nội dung | Ước lượng đọc |
|---|---|---|---|
| 01 | [01_khai_niem_can_ban.md](01_khai_niem_can_ban.md) | 8 khái niệm cơ bản (analogy y khoa): CLAUDE.md, skill, hook, sub-agent... | 15 phút |
| 02 | [02_spec_driven_workflow.md](02_spec_driven_workflow.md) | Cách "ra chỉ định" cho AI — spec-driven vs vibe coding | 15 phút |
| 03 | [03_setup_may.md](03_setup_may.md) | Cần cài cái gì, và mỗi cái làm gì | 10 phút đọc + 2 giờ cài |
| 04 | [04_tech_stack.md](04_tech_stack.md) | Các "món đồ nghề" thường dùng, mỗi món giải quyết vấn đề gì | 15 phút |
| 05 | [05_content_data_pipeline.md](05_content_data_pipeline.md) | Nguyên tắc seed content không sai (quan trọng cho da liễu) | 15 phút |
| 06 | [06_bao_mat_git.md](06_bao_mat_git.md) | Không leak secret khi commit + git workflow | 10 phút |
| 07 | [07_du_an_da_lieu_kickoff.md](07_du_an_da_lieu_kickoff.md) | Kickoff dự án cụ thể — luồng chạy + roadmap 6 tuần | 20 phút |
| 08 | [08_tai_lieu_tham_khao.md](08_tai_lieu_tham_khao.md) | Toàn bộ link nguồn để tra sâu | Tra khi cần |
| 09 | [09_multi_model_workflow.md](09_multi_model_workflow.md) | Dùng nhiều AI cùng lúc (Claude + MiniMax + local) — khi nào dùng cái nào | 15 phút |

Ngoài ra:
- **[research-notes.md](research-notes.md)** — Ghi chép research raw (~15000 từ, ~100 nguồn). Dùng khi cần verify claim cụ thể hoặc tra sâu hơn.

**Tổng thời gian đọc**: ~2 giờ. Setup: ~2 giờ. Kickoff: ~2 giờ. **Tổng ~6 giờ để "sẵn sàng bắt đầu tuần đầu tiên"**.

---

## Lộ trình đọc gợi ý (chia làm 4 buổi)

### Buổi 1 — Bức tranh lớn (60 phút)

- Đọc file **00** (file này)
- Đọc file **01** — 8 khái niệm cơ bản
- Đọc file **02** — spec-driven vs vibe coding

**Sau buổi 1**: hiểu Claude Code là gì, tại sao spec-driven quan trọng, "phòng khám AI" trong đầu anh bắt đầu hình thành.

### Buổi 2 — Chuẩn bị đồ nghề (2-3 giờ)

- Đọc file **03** — vừa đọc vừa cài (VS Code, Claude Code, Git, tài khoản)
- Đọc file **09** — hiểu multi-model
- Đọc file **06** — bảo mật secret trước khi commit đầu tiên

**Sau buổi 2**: máy đã cài đủ tool, hiểu vì sao không commit `.env`, có Claude Code + MiniMax cùng chạy được.

### Buổi 3 — Tech stack + nguyên tắc content (1 giờ)

- Đọc file **04** — các món đồ nghề
- Đọc file **05** — nguyên tắc seed content

**Sau buổi 3**: biết chọn stack nào cho MVP, hiểu nguyên tắc "correct-by-construction" — đừng để AI chế đáp án y khoa.

### Buổi 4 — Kickoff (1 giờ đọc + tùy anh triển khai)

- Đọc file **07** — kickoff dự án da liễu
- Tạo folder project đầu tiên, viết CLAUDE.md

**Sau buổi 4**: đã có repo mới, sẵn sàng bắt đầu tuần 2 của roadmap 6 tuần.

---

## Nếu anh vội chỉ muốn 1 file

Đọc file **07** (kickoff) — có đủ ngữ cảnh + link back các file khác khi cần.

---

## Nguyên tắc chung khi đọc

1. **Không cần hiểu 100% ngay lần đầu** — nhiều khái niệm chỉ sáng tỏ khi làm thực tế
2. **Đọc chậm file 05 và 06** — 2 file có nội dung "sai là đau" (data pipeline + secrets)
3. **Đọc trên máy có internet** — nhiều link ngoài cần verify
4. **Ghi chú riêng** khi thấy chỗ nào không hiểu — sau này quay lại

---

## Câu hỏi thường gặp

**Q: Em không biết code. Có làm được không?**

A: Có. Claude Code viết code cho anh — nhiệm vụ của anh là **viết yêu cầu đúng, review kết quả, verify**. Kỹ năng này gần với "quản lý ê-kíp mổ" hơn "tự mổ". Nhưng anh vẫn cần hiểu **luồng chạy** để không bị AI qua mặt.

**Q: Chi phí bao nhiêu?**

A: ~$85-255/năm cho toàn stack. Xem chi tiết trong file 03 mục "Chi phí ước tính".

**Q: Có thể học vừa làm CK1 vừa xây app không?**

A: Có, nếu timebox 2-3 giờ/tuần. Xem roadmap 6 tuần trong file 07. **Đừng** cố xây app phức tạp — MVP tối giản trước, mở rộng sau khi thi xong.

**Q: Có mua khóa học Anki/dev không?**

A: Chưa cần. Tài liệu này đủ cho 3-6 tháng đầu. Sau đó nếu muốn học sâu:
- Anki: AnKing YouTube (free)
- Claude Code: docs.claude.com/docs/en/best-practices (chính thức)
- Spec Kit: github.com/github/spec-kit
- Superpowers: blog.fsck.com

**Q: Có nên dùng ChatGPT/Cursor thay Claude Code không?**

A: Cursor cũng dùng được. ChatGPT web không thay được vì không có tool access thật vào máy. Bộ tài liệu này thiết kế theo hướng multi-model: dùng Claude Code cho việc chính, kèm MiniMax cho task rẻ hơn. Chi tiết ở file **09**.

**Q: Có bắt buộc dùng Telegram bot không? Chỉ web được không?**

A: Chỉ web hoàn toàn được. Nhưng Telegram bot có ưu điểm push notification tự nhiên — 6h30 sáng chuông reo là anh mở, còn web thì anh phải nhớ tự mở. Cứ web + bot combo là mạnh nhất. Nếu muốn đơn giản MVP tuần đầu, làm web trước rồi bot sau cũng OK.

---

## Cấu trúc thư mục sau khi làm xong

```
~/projects/ngocdanh/
├── guide-nguoi-moi/          ← Tài liệu anh đang đọc
│   ├── 00_START_HERE.md
│   ├── 01_khai_niem_can_ban.md
│   ├── ... (8 file khác)
│   ├── 09_multi_model_workflow.md
│   └── research-notes.md
│
└── da-lieu-hoc-tap/          ← Dự án anh sẽ tạo
    ├── CLAUDE.md              ← Hồ sơ dự án
    ├── .claude/               ← Skill, hook, agent config
    ├── frontend/              ← Web PWA (Vite + React)
    ├── supabase/
    │   ├── migrations/        ← Schema database
    │   └── functions/         ← Telegram bot Edge Function
    ├── services/              ← Service layer chung
    ├── scripts/               ← Data pipeline (parse sách CK1)
    ├── docs/                  ← Plans, audits, specs
    ├── .env                   ← Secret (gitignored)
    ├── .env.example
    └── README.md
```

---

## Cách feedback / cập nhật tài liệu

Tài liệu này viết bởi Claude, có thể có chỗ chưa chính xác. Khi anh phát hiện chỗ nào:

- **Sai fact** → sửa trực tiếp file .md
- **Chưa rõ** → mở Claude Code trong folder này, gõ "sửa file NN để rõ hơn phần X"
- **Cần thêm ví dụ** → gõ "thêm ví dụ cho phần Y trong file NN"

Vì đây là folder cá nhân, không cần PR review. Cứ sửa thoải mái, coi như notebook riêng.

---

**Sẵn sàng chưa?** Bắt đầu bằng [01_khai_niem_can_ban.md](01_khai_niem_can_ban.md).
