# 06 — Bảo mật + git (đừng lộ khoá, đừng mất dữ liệu)

> Đọc file này **trước lần commit đầu tiên**. Sai ở đây có thể mất tiền (lộ khoá AI) hoặc lộ dữ liệu bệnh nhân — 2 hậu quả nghiêm trọng.

## Vấn đề: Khoá là gì và vì sao nguy hiểm?

**Khoá** (secret) = mọi giá trị không được để lộ công khai:

- **Khoá AI**: Anthropic key, MiniMax key, Gemini key
- **Token bot**: Telegram bot token
- **Khoá cơ sở dữ liệu**: Supabase service role key, mật khẩu DB
- **Mật khẩu**: mật khẩu Moodle trường, email, hosting
- **Dữ liệu bệnh nhân**: tên, ảnh có mặt/tên, hồ sơ scan

**Tại sao lộ nguy hiểm?**

Trên GitHub public có **bot của kẻ tấn công quét liên tục**. Anh push code có chứa khoá Anthropic → sau 30 giây có kẻ biết → dùng khoá đó gọi AI → bill anh 500 nghìn hoặc hơn.

Trường hợp có thật: một dev push nhầm khoá AWS lên GitHub → kẻ tấn công đào Bitcoin qua tài khoản AWS → dev bill 30.000 USD sau 1 tuần. Amazon từ chối hoàn tiền.

Với dự án bác sĩ cá nhân, hậu quả nhẹ hơn nhưng vẫn phải cẩn thận. Bill AI 500 nghìn vì lộ khoá Claude = mất tiền không có ích.

**Chi phí**: bảo mật hoàn toàn **miễn phí** — chỉ cần tuân quy tắc.

---

## 3 tầng bảo vệ (đầy đủ mới an toàn)

### Tầng 1 — File `.env` không đưa lên git

**Nguyên tắc**: mọi khoá nằm trong 1 file tên `.env` ở gốc dự án, **KHÔNG commit** lên git.

File `.env` chứa các cặp `KEY=value`, ví dụ khoá Anthropic, khoá Supabase, token Telegram.

Kèm 1 file khác tên `.env.example` (commit được, không có giá trị thật) — làm mẫu để đồng nghiệp biết cần cấu hình gì. Chỉ có tên khoá, không có giá trị.

Trong file `.gitignore` (danh sách file git bỏ qua) phải có `.env` — để chắc chắn không commit nhầm.

### Tầng 2 — Chặn tự động khi commit khoá

Quy tắc ghi trong file hồ sơ dự án chỉ là văn bản — AI có thể quên. Cần **hook** chặn cứng.

**Hook** là 1 script chạy tự động trước mỗi `git commit`. Nó dò tìm mẫu khoá trong file sắp commit. Nếu tìm thấy → chặn commit ngay, không cho qua.

Khi anh cài Claude Code + Superpowers, quy tắc `no-secrets-in-git` tự setup hook này cho anh — không cần viết tay.

### Tầng 3 — Đổi khoá định kỳ

Kể cả không lộ, cứ 3-6 tháng đổi 1 lần:
- Vào console Anthropic → xoá khoá cũ + tạo khoá mới
- Supabase Dashboard → Settings → API → Reset khoá
- BotFather trên Telegram → `/revoke` → tạo token mới

Cập nhật `.env`, khởi động lại bot/app.

---

## Nếu lỡ lộ — 3 bước xử lý NGAY

Kịch bản: anh commit `.env` lên GitHub public, sau 5 phút phát hiện.

### Bước 1 — REVOKE khoá TRONG 5 PHÚT

**Đừng dọn git trước**. Revoke khoá TRƯỚC.

- Anthropic Console → Settings → API Keys → **Revoke** khoá bị lộ
- Supabase Dashboard → Settings → API → **Regenerate** khoá
- BotFather → `/revoke` → chọn bot → xác nhận

Sau đó tạo khoá mới, cập nhật `.env`.

### Bước 2 — Dọn lịch sử git

Chỉ xoá commit chưa đủ — lịch sử git vẫn còn khoá. Có 2 công cụ chuyên dụng: `git-filter-repo` (chính thức) và `BFG Repo-Cleaner` (nhanh hơn).

Cả 2 công cụ này viết lại lịch sử, xoá khoá khỏi mọi commit. Sau đó force push (cảnh báo đồng nghiệp vì họ phải clone lại).

**Khi làm với Claude Code**, gõ "tôi lỡ commit .env, giúp tôi dọn lịch sử git" — Claude sẽ hướng dẫn từng bước an toàn.

### Bước 3 — Kiểm tra hệ thống báo động của GitHub

GitHub tự động quét kho public tìm mẫu khoá. Vào Settings → Security → Secret scanning alerts xem có cảnh báo không.

GitHub cũng tự động báo cho các nhà cung cấp (AWS, Stripe, OpenAI...) khi phát hiện khoá của họ trong commit của anh. Anthropic có thể tự revoke khoá trước khi anh kịp phản ứng.

---

## Khoá ở nơi khác (không chỉ trong code)

Ngoài `.env`, còn phải cẩn thận:

| Nơi | Rủi ro | Cách phòng |
|---|---|---|
| Ảnh chụp màn hình đăng blog/mạng xã hội | Terminal hiển thị khoá | Chụp sau khi ẩn biến |
| File log | In `os.environ` tình cờ | Không in giá trị, chỉ tên biến |
| URL trong browser | `?token=xxx` lưu trong lịch sử + log server | Luôn dùng header `Authorization: Bearer xxx` |
| Copy vào Slack/Zalo | Paste khoá để "hỏi bạn" | Đổi khoá ngay |
| Bản sao lưu DB | Dump chứa dữ liệu người dùng | Mã hoá trước khi lưu |
| Lịch sử shell | `export KEY=xxx` được ghi lại | Thêm dấu cách đầu để không ghi lịch sử |

---

## Git workflow — 3 kiểu tuỳ dự án

Chọn 1 kiểu phù hợp:

### Kiểu A — Commit thẳng nhánh chính (đơn giản nhất)

Không tạo nhánh, cứ commit trực tiếp vào nhánh chính (`main`). Push khi anh muốn (không tự động push).

**Ưu**: đơn giản, ít xung đột, lịch sử sạch
**Nhược**: không có review trước merge; nếu nhiều người làm chung sẽ hỗn loạn

**Phù hợp cho**: dự án cá nhân, ứng dụng học tập 1 người dùng (đây là kiểu em khuyên cho anh trong 3-6 tháng đầu)

### Kiểu B — Nhánh riêng + PR

Mỗi chức năng/sửa lỗi tạo 1 nhánh riêng. Code xong push lên → mở Pull Request trên GitHub → tự review → merge vào nhánh chính.

**Ưu**: review được trước merge, lịch sử sạch, dễ rollback
**Nhược**: phiền cho việc nhỏ (5 phút sửa 1 chữ cũng phải tạo nhánh + PR)

**Phù hợp cho**: dự án chia sẻ với đồng nghiệp, chức năng phức tạp

### Kiểu C — 2 kho riêng biệt

Chia dự án thành 2 kho code:
- Kho 1: code chính
- Kho 2 (riêng tư): thư mục cấu hình AI

Lợi: cấu hình AI chứa prompt riêng, không lộ ra kho code công khai.

Chỉ cần khi share kho code cho người khác nhưng giữ workflow AI riêng. **Không cần cho giai đoạn đầu**.

---

## Cách viết thông điệp commit chuẩn

Format chuẩn (gọi là **Conventional Commits**):

```
<loại>(<phạm vi>): <mô tả ngắn>
```

**Loại thường dùng**:
- `feat` — chức năng mới
- `fix` — sửa lỗi
- `chore` — bảo trì (cập nhật thư viện, cấu hình)
- `docs` — sửa tài liệu
- `refactor` — dọn code không đổi hành vi
- `test` — thêm/sửa kiểm tra

**Phạm vi thường**:
- `fe` — giao diện
- `be` — backend
- `db` — cơ sở dữ liệu
- `bot` — bot Telegram
- `content` — nội dung dữ liệu
- `docs` — tài liệu

**Ví dụ tốt**:
- `feat(bot): thêm lệnh /case gửi case study ngẫu nhiên`
- `fix(fe): thẻ ảnh không tải khi tên file có dấu tiếng Việt`
- `chore(db): thêm bảng lưu kết quả quiz`
- `docs(setup): cập nhật hướng dẫn cài Claude Code cho Windows`

**Ví dụ tệ**:
- `update` — không rõ update gì
- `fix bug` — lỗi nào
- `WIP` — chưa xong thì đừng commit

**Vì sao quan trọng?** 3 tháng sau đọc lịch sử git thấy ngay thay đổi gì. Anh không nhớ nổi 100 commit "update" là update cái gì.

---

## Kiểm tra trước khi commit

Quy tắc vàng của Superpowers:

> **KHÔNG bao giờ nói "xong" nếu chưa chạy kiểm tra trong buổi làm việc này.**

Trước khi commit, chạy kiểm tra phù hợp với việc vừa làm:

| Loại thay đổi | Kiểm tra trước commit |
|---|---|
| Sửa giao diện | typecheck + lint + build |
| Sửa logic web | chạy bộ test |
| Sửa bot Python | pytest |
| Sửa schema kho | test local trước |
| Sửa nội dung dữ liệu | 4 kiểm tra ở file 05 |

Đừng bỏ qua. Đừng nghĩ "sửa 1 dòng chắc OK". Case đau: sửa 1 chữ typo → không chạy typecheck → deploy → app trắng cho user 1 tiếng.

---

## Bảo vệ dữ liệu bệnh nhân (cực kỳ quan trọng)

Ngoài khoá kỹ thuật, dữ liệu bệnh nhân cần cực kỳ cẩn thận.

**Nguyên tắc bất di bất dịch**:

1. **KHÔNG đưa ảnh có mặt/tên/thông tin định danh** lên bất kỳ đám mây nào
2. **KHÔNG commit hồ sơ bệnh án scan** vào git
3. **Chỉ dùng ảnh công khai** (DermNet, atlas sách) hoặc ảnh anh đã **ẩn danh đầy đủ**
4. Nếu muốn dùng ảnh ca lâm sàng bản thân → **che mặt + xoá metadata EXIF** (metadata có thể chứa GPS chỗ chụp, thời gian → suy ra bệnh nhân)

**Tuân thủ pháp luật (nếu định chia sẻ ứng dụng cho đồng nghiệp)**:

- **Nghị định 13/2023/NĐ-CP** về bảo vệ dữ liệu cá nhân — dữ liệu y tế là "dữ liệu nhạy cảm", có quy định riêng
- **HIPAA** (nếu có user Mỹ) — cực kỳ nghiêm ngặt
- **GDPR** (nếu có user EU)

**Kết luận cho giai đoạn học tập cá nhân**: **tránh hoàn toàn dữ liệu bệnh nhân thật**. Dùng atlas ảnh chuẩn (DermNet, Wikipedia Commons) + case study công khai từ sách textbook.

---

## Sao lưu dữ liệu

Nếu ứng dụng chứa dữ liệu quý (thẻ tự soạn, case study 6 tháng), cần sao lưu:

**Sao lưu kho hồ sơ**:
- Miễn phí tier Supabase có sao lưu 7 ngày. Không đủ nếu anh cần dài hơn
- Setup sao lưu hàng ngày thủ công: chạy `pg_dump` mỗi tối, mã hoá, đưa lên Google Drive/Dropbox

**Sao lưu kho ảnh**:
- Định kỳ dùng CLI tải tất cả file về

**Nguyên tắc 3-2-1**:
- 3 bản copy (1 gốc + 2 sao lưu)
- 2 loại lưu trữ khác nhau (ví dụ Supabase + Google Drive)
- 1 bản ngoài nhà (khác vị trí — ví dụ đám mây)

**Chi phí**: miễn phí (dùng Google Drive/Dropbox miễn phí).

---

## Bảng kiểm tra commit đầu tiên

Trước khi push commit đầu tiên lên GitHub public:

- [ ] `.env` có trong `.gitignore` (kiểm: `git status` không thấy `.env`)
- [ ] `.env.example` có, không chứa giá trị thật
- [ ] `README.md` không có khoá AI ví dụ (dùng placeholder `xxx`)
- [ ] Không có file `credentials.json`, `*.pem`, `*.key` được stage
- [ ] `git diff --cached` không có mẫu khoá
- [ ] Repo riêng tư nếu chứa logic riêng
- [ ] Có file `LICENSE` (MIT nếu muốn mở nguồn, hoặc bỏ qua nếu riêng tư)

---

## Nếu anh chỉ nhớ được 3 điều

1. **`.env` KHÔNG BAO GIỜ commit** — thêm vào `.gitignore` trước khi tạo file `.env`
2. **Lỡ lộ → REVOKE trước, dọn git sau** — không đảo thứ tự
3. **KHÔNG dùng ảnh bệnh nhân thật** cho ứng dụng học tập cá nhân — dùng atlas công khai

## Đọc tiếp

- [07_du_an_da_lieu_kickoff.md](07_du_an_da_lieu_kickoff.md) — Khởi động dự án cụ thể
- [09_multi_model_workflow.md](09_multi_model_workflow.md) — Nhiều khoá hơn → nhiều rủi ro hơn, đọc để phòng
