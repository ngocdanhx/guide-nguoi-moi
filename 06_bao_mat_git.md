# 06 — Bảo mật secret + Git workflow

> Đọc file này **trước lần commit đầu tiên**. Sai ở đây có thể mất tiền (leak API key) hoặc lộ dữ liệu bệnh nhân — 2 hậu quả không muốn xảy ra.

## Vấn đề: Secret là gì và vì sao nguy hiểm?

**Secret** = mọi giá trị không được để lộ công khai:

- **API key**: Anthropic key, MiniMax key, Gemini key
- **Bot token**: Telegram bot token
- **Database credential**: Supabase service role key, password DB
- **Password**: mật khẩu Moodle trường, email, hosting
- **Dữ liệu bệnh nhân**: tên, ảnh có mặt/tên, HSBA scan

**Tại sao leak nguy hiểm?**

Trên GitHub public có **bot của attacker scan liên tục**. Anh push code chứa Anthropic API key → sau 30 giây có người biết → dùng key đó gọi API → bill anh $500/tháng.

Trường hợp có thật: dev push nhầm AWS key lên GitHub public → attacker mine Bitcoin qua tài khoản AWS → dev bill $30,000 sau 1 tuần. Amazon từ chối refund.

Với dự án cá nhân bác sĩ, hậu quả nhẹ hơn nhưng vẫn phải cẩn thận. Bill AI $500 vì leak Claude key = học bổng 1 năm bay mất.

---

## 3 tầng bảo vệ (đầy đủ mới an toàn)

### Tầng 1 — File `.env` gitignored

**Nguyên tắc**: mọi secret nằm trong 1 file tên `.env` ở gốc dự án, **KHÔNG commit** lên git.

File `.env` chứa các cặp `KEY=value`:
- `ANTHROPIC_API_KEY=sk-ant-...`
- `SUPABASE_URL=https://...`
- `TG_TOKEN=123456:ABC-...`

Kèm 1 file khác tên `.env.example` (commit được, không có giá trị thật) — làm template để đồng nghiệp biết cần cấu hình gì:
- `ANTHROPIC_API_KEY=your-key-here`
- `SUPABASE_URL=your-project-url`
- `TG_TOKEN=your-telegram-token`

Trong file `.gitignore` (danh sách file git bỏ qua) phải có `.env` — để chắc chắn không commit nhầm.

### Tầng 2 — Hook chặn commit secret

Rule ghi trong CLAUDE.md hay skill chỉ là văn bản — AI có thể quên. Cần **hook** chặn cứng.

**Hook là 1 script bash** chạy tự động trước mỗi `git commit`. Nó grep tìm pattern secret trong file sắp commit. Nếu tìm thấy → chặn commit ngay, không cho qua.

Pattern hook thường grep:
- Chuỗi `sk-ant-...` (Anthropic key)
- Chuỗi `sk-...` dài 48 ký tự (OpenAI key)
- JWT token dài (Supabase service role)
- Telegram bot token pattern `<số>:<chuỗi 35 ký tự>`
- Header `-----BEGIN...PRIVATE KEY-----` (SSH/GPG key)

Khi anh cài Claude Code + Superpowers, skill `no-secrets-in-git` tự setup hook này cho anh — không cần viết tay.

### Tầng 3 — Rotate secret định kỳ

Kể cả không leak, cứ 3-6 tháng rotate 1 lần:
- Vào console Anthropic → xoá key cũ + tạo key mới
- Supabase Dashboard → Settings → API → Reset service_role
- BotFather → `/revoke` → tạo token mới

Cập nhật `.env`, restart bot/app.

---

## Nếu lỡ leak — 3 bước xử lý NGAY

Kịch bản: anh commit `.env` lên GitHub public, sau 5 phút phát hiện.

### Bước 1 — REVOKE token TRONG 5 PHÚT

**Đừng dọn git trước**. Revoke token TRƯỚC.

- Anthropic Console → Settings → API Keys → **Revoke** key bị leak
- Supabase Dashboard → Settings → API → **Regenerate** service_role
- BotFather → `/revoke` → chọn bot → confirm
- Các provider khác tương tự

Sau đó tạo key mới, update `.env`.

### Bước 2 — Dọn git history

Chỉ xoá commit chưa đủ — history git vẫn còn key. Có 2 tool chuyên dụng:

- **git-filter-repo** — tool Git chính thức khuyến nghị
- **BFG Repo-Cleaner** — nhanh hơn 10-720× so với filter-branch cũ

Cả 2 tool này rewrite history, xoá secret khỏi mọi commit. Sau đó force push (cảnh báo collaborator vì họ phải re-clone).

**Khi làm với Claude Code**, gõ "tôi lỡ commit .env, giúp tôi clean git history" — Claude sẽ hướng dẫn từng bước dùng git-filter-repo an toàn.

### Bước 3 — Kiểm tra GitHub Secret Scanning

GitHub tự động scan public repo tìm secret pattern. Vào Settings → Security → Secret scanning alerts xem có alert không.

GitHub cũng tự động notify các provider (AWS, Stripe, OpenAI...) khi phát hiện secret của họ trong commit của anh. Anthropic có thể tự revoke key trước khi anh kịp.

---

## Secret ở nơi khác (không chỉ code)

Ngoài `.env`, còn phải cẩn thận:

| Nơi | Rủi ro | Cách phòng |
|---|---|---|
| Screenshot đăng blog/Twitter | Terminal hiển thị key khi export env | Chụp sau khi `unset` biến |
| Log file | Print `os.environ` tình cờ | Không print value, chỉ tên biến |
| URL query string | `?token=xxx` trong browser history + server log | Luôn dùng header `Authorization: Bearer xxx` |
| Slack/Discord copy | Paste key vào chat để "hỏi bạn" | Rotate ngay, dùng ephemeral message |
| Backup DB dump | `pg_dump` chứa data người dùng | Encrypt trước khi lưu (`gpg --encrypt`) |
| Bash history | `export KEY=xxx` lưu history | Prefix space (`␣export KEY=xxx`) để skip history |

---

## Git workflow — 3 style tuỳ dự án

Chọn 1 style phù hợp:

### Style A — Commit thẳng `main` (đơn giản nhất)

Không tạo branch, cứ commit trực tiếp vào branch chính `main`. Push khi anh muốn (không auto push).

**Ưu**: đơn giản, ít conflict, git log sạch
**Nhược**: không có review trước merge; nếu team làm chung sẽ hỗn loạn

**Phù hợp cho**: dự án cá nhân, learning app 1 người dùng (đây là style em recommend cho anh trong 3-6 tháng đầu)

### Style B — Feature branch + PR

Mỗi feature/bug fix tạo 1 branch riêng (VD `feat/add-image-occlusion`). Code xong push lên → mở Pull Request trên GitHub → self-review → merge vào main.

**Ưu**: review được trước merge, git log sạch, rollback dễ
**Nhược**: phiền cho việc nhỏ (5 phút fix typo cũng phải tạo branch + PR)

**Phù hợp cho**: dự án chia sẻ với đồng nghiệp, feature phức tạp

### Style C — 2 repo tách AI config

Chia dự án thành 2 repo:
- Repo 1: `dalieu-app` — code chính
- Repo 2: `dalieu-ai-config` (private) — thư mục `.claude/` chứa skill, hook, prompt cá nhân

Lợi: AI config chứa prompt riêng, không lộ ra repo app công khai. Push code không đụng `.claude/`.

Chỉ cần khi share repo app cho người khác nhưng giữ AI workflow riêng. **Không cần cho MVP đầu tiên**.

---

## Commit message convention

Format chuẩn (gọi là **Conventional Commits**):

```
<type>(<scope>): <mô tả ngắn>
```

**Type thường dùng**:
- `feat` — feature mới
- `fix` — sửa bug
- `chore` — bảo trì (update deps, config)
- `docs` — sửa documentation
- `refactor` — refactor không đổi behavior
- `test` — thêm/sửa test

**Scope thường**:
- `fe` — frontend
- `be` — backend
- `db` — schema/migration
- `bot` — Telegram bot
- `content` — data seed
- `docs` — tài liệu

**Ví dụ tốt**:
- `feat(bot): thêm command /case gửi random case study`
- `fix(fe): flashcard image không load khi tên file có dấu tiếng Việt`
- `chore(db): thêm migration cho bảng quiz_result`
- `docs(setup): cập nhật hướng dẫn cài Claude Code cho Windows`

**Ví dụ tệ**:
- `update` — không rõ update gì
- `fix bug` — bug nào
- `WIP` — chưa xong thì đừng commit

**Vì sao quan trọng?** 3 tháng sau đọc `git log` thấy ngay thay đổi gì. Anh không nhớ nổi 100 commit "update" là update cái gì.

---

## Verification trước khi commit

Quy tắc vàng học từ Superpowers `verification-before-completion`:

> **KHÔNG bao giờ nói "xong" nếu chưa chạy verify command trong session này.**

Trước khi commit, chạy check phù hợp:

| Loại thay đổi | Check trước commit |
|---|---|
| Frontend TypeScript | typecheck + lint + build |
| Frontend test | test suite |
| Python bot | pytest |
| Supabase migration | test local `supabase db reset && push` |
| Edge Function | deno test |
| Data seed | 4 check ở file 05 (quote literal, no placeholder, correct in options, image OK) |

Đừng skip. Đừng nghĩ "sửa 1 dòng chắc OK". Case study đau: sửa 1 dòng typo → không chạy typecheck → deploy → app trắng cho user 1 tiếng.

---

## Bảo vệ dữ liệu bệnh nhân (cực kỳ quan trọng cho bác sĩ)

Ngoài secret kỹ thuật, dữ liệu bệnh nhân cần cực kỳ cẩn thận.

**Nguyên tắc bất di bất dịch**:

1. **KHÔNG upload ảnh có mặt/tên/thông tin định danh** lên bất kỳ cloud nào (Supabase Storage vẫn là cloud)
2. **KHÔNG commit HSBA scan** vào git
3. **Chỉ dùng ảnh public** (DermNet, atlas sách) hoặc ảnh anh đã **anonymize đầy đủ**
4. Nếu muốn dùng ảnh ca lâm sàng bản thân → **che mặt + xoá metadata EXIF** (metadata EXIF có thể chứa GPS chỗ chụp, thời gian, model camera → suy ra bệnh nhân)

**Compliance (nếu có ý định share app cho đồng nghiệp)**:

- **Nghị định 13/2023/NĐ-CP** về bảo vệ dữ liệu cá nhân — dữ liệu y tế là "dữ liệu nhạy cảm", có quy định riêng
- **HIPAA** (nếu có user Mỹ) — cực kỳ nghiêm ngặt, cần compliance officer
- **GDPR** (nếu có user EU)

**Kết luận cho giai đoạn học tập cá nhân**: **tránh hoàn toàn dữ liệu bệnh nhân thật**. Dùng atlas ảnh chuẩn (DermNet, Wikipedia Commons) + case study public từ sách textbook.

---

## Backup strategy

Nếu app anh chứa data quý (flashcard tự soạn, note case study 6 tháng), cần backup:

**Backup Postgres (Supabase)**:
- Free tier Supabase có backup 7 ngày rolling. Không đủ nếu anh cần dài hơn
- Setup manual daily backup: chạy `pg_dump` mỗi tối, encrypt, upload Google Drive/Dropbox

**Backup Storage**:
- Supabase Storage → dùng CLI download tất cả file định kỳ

**Nguyên tắc 3-2-1**:
- 3 bản copy (1 gốc + 2 backup)
- 2 loại storage khác nhau (VD Supabase + Google Drive)
- 1 bản offsite (khác nhà — VD upload lên cloud)

---

## Checklist commit đầu tiên

Trước khi push commit đầu tiên lên GitHub public:

- [ ] `.env` có trong `.gitignore` (kiểm: `git status` không thấy `.env`)
- [ ] `.env.example` có, không chứa giá trị thật
- [ ] `README.md` không có API key example (dùng placeholder `xxx`)
- [ ] Không có file `credentials.json`, `*.pem`, `*.key` được stage
- [ ] `git diff --cached` không có pattern secret
- [ ] Repo private nếu chứa business logic riêng
- [ ] Có `LICENSE` file (MIT nếu muốn open source, hoặc bỏ qua nếu private)

---

## Nếu anh chỉ nhớ được 3 điều

1. **`.env` KHÔNG BAO GIỜ commit** — thêm vào `.gitignore` trước khi tạo file `.env`
2. **Lỡ leak → REVOKE trước, dọn git sau** — không đảo thứ tự
3. **KHÔNG dùng ảnh bệnh nhân thật** cho app học tập cá nhân — dùng atlas public

## Đọc tiếp

- [07_du_an_da_lieu_kickoff.md](07_du_an_da_lieu_kickoff.md) — Kickoff dự án cụ thể
- [09_multi_model_workflow.md](09_multi_model_workflow.md) — Nhiều key hơn → nhiều rủi ro hơn, đọc để phòng
