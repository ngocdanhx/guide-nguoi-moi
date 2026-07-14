# 03 — Setup máy tính (chuẩn bị đồ nghề)

> File này chỉ mô tả **cần cài cái gì và tại sao**. Câu lệnh cài đặt cụ thể — em không viết, khi cần anh gõ vào Claude Code là nó hướng dẫn từng bước cho anh.

## Bức tranh chung

Hình dung anh mở 1 phòng khám mới. Cần chuẩn bị:

1. **Điện, nước, WiFi** (nền tảng) — máy tính, tài khoản
2. **Bàn làm việc + tủ hồ sơ** (nơi làm việc) — VS Code, Terminal
3. **Máy chụp X-quang, máy siêu âm** (công cụ chính) — Claude Code, Cline
4. **Kho thuốc + kho vật tư** (tài nguyên) — Supabase, GitHub
5. **Nhân viên phụ** (bổ trợ) — Git, MiniMax API

Anh không cần setup hết 1 lượt. Bắt đầu từ 3 cái đầu tiên là chạy được. Cái sau cài dần khi cần.

---

## Nhóm 1 — Tài khoản (làm trước tiên, miễn phí)

3 tài khoản bắt buộc:

**GitHub** — nơi lưu code. Miễn phí, đăng ký bằng email 2 phút. Sau này anh sẽ push mọi dự án lên đây, coi như "cloud backup" cho code.

**Anthropic Console** — nơi cấp API key để Claude Code chạy. Miễn phí đăng ký. Sau đăng ký nạp $5-10 vào tài khoản để bắt đầu. Dùng cá nhân 1 tháng ~$5-15 là đủ.

**Supabase** — nơi host database + auth cho app. Miễn phí tier đủ cho dự án cá nhân (500MB DB, 1GB storage, 50k user hoạt động/tháng).

Tùy chọn (không cần ngay):
- **Vercel** — deploy web miễn phí. Đăng ký sau
- **Telegram** — cài app trên điện thoại, tạo bot qua `@BotFather` khi cần

---

## Nhóm 2 — Máy tính + terminal

**Máy tính**: bất kỳ máy Mac/Windows/Linux nào có ≥ 8GB RAM là chạy tốt.

**Terminal** (cửa sổ dòng lệnh) — nơi anh gõ lệnh giao tiếp với máy:
- Mac: có sẵn (Cmd+Space, gõ "Terminal")
- Windows: cài "Windows Terminal" từ Microsoft Store, sau đó cài WSL2 (Windows Subsystem for Linux) — chạy Linux bên trong Windows. Claude Code chạy mượt hơn trên WSL2 so với native Windows

**Homebrew** (Mac) — trình quản lý phần mềm. Cài Homebrew rồi mọi tool khác cài 1 dòng lệnh là xong. Windows thì tương đương là WinGet.

---

## Nhóm 3 — VS Code (IDE chính — BẮT BUỘC)

Anh sẽ dùng **VS Code** làm nơi làm việc chính, không phải chỉ terminal.

**Vì sao VS Code?**
- Miễn phí, do Microsoft phát hành
- Cross-platform (Mac/Windows/Linux)
- Có Claude Code extension chính thức — click 1 nút mở panel Claude bên phải màn hình
- Nhìn diff trực quan khi Claude sửa code (chỗ nào thêm, chỗ nào xoá, có highlight)
- Cài extension khác được — Cline (multi-model), Prettier (auto format), GitLens (xem lịch sử git), …

**Extension bắt buộc** (cài trong VS Code, tab Extensions):

| Extension | Vai trò |
|---|---|
| Claude Code (Anthropic) | AI dev chính — panel bên phải, shortcut `Cmd/Ctrl + Esc` |
| Prettier | Tự động format code khi save |
| ESLint | Báo lỗi TypeScript ngay khi gõ |
| GitLens | Xem git blame — ai sửa dòng nào khi nào |
| Tailwind CSS IntelliSense | Autocomplete class Tailwind (khi làm frontend) |

**Extension khuyến khích cho multi-model workflow**:

| Extension | Vai trò |
|---|---|
| Cline | AI agent hỗ trợ **nhiều model** — Claude, MiniMax, Gemini, OpenAI. Switch bằng 1 click |
| Continue.dev | Copilot đa provider, có support model local (Ollama) |

Chi tiết cách dùng multi-model: file [09](09_multi_model_workflow.md).

---

## Nhóm 4 — Claude Code (AI dev chính)

Cài Claude Code có 3 cách:

1. **Homebrew** (Mac): 1 dòng lệnh, khuyến khích
2. **Script install** từ trang chủ Anthropic: 1 dòng lệnh, chạy được trên Mac/Linux/WSL
3. **npm install global**: nếu anh đã có Node.js

Sau khi cài xong, gõ `claude` trong terminal là mở giao diện chat. Lần đầu tiên nó bảo anh đăng nhập Anthropic account (mở browser tự động).

**3 chế độ permission** (quan trọng!) — bấm `Shift + Tab` để cycle:

| Chế độ | Hành vi | Khi nào dùng |
|---|---|---|
| **Manual** | Hỏi trước mọi edit | Task lạ, chưa tin |
| **Plan** | Claude viết plan trước, chờ approve mới sửa | Task lớn, muốn review kế hoạch |
| **Edit automatically** | Tự sửa + tự chạy lệnh được allow | Task quen, cần tốc độ |

Người mới nên bắt đầu **Manual**. Sau 1 tuần chuyển **Plan** cho task lớn.

---

## Nhóm 5 — Git + GitHub (nơi lưu code)

**Git** — trình quản lý phiên bản code. Mac có sẵn; Windows cài từ git-scm.com.

Cài 1 lần, cấu hình 3 thứ:
- Tên anh
- Email của anh (dùng chính email GitHub)
- Đặt default branch là `main`

**Đăng nhập GitHub** — 2 cách:

- **HTTPS + Personal Access Token** — dễ nhất cho người mới. Vào GitHub Settings → tạo token → dùng token thay password khi git push
- **SSH key** — bảo mật hơn, dùng cho lâu dài. Có hướng dẫn chi tiết trên trang GitHub

Mọi tài liệu này, khi anh gõ vào Claude Code "hướng dẫn tôi cài git và đăng nhập GitHub", nó sẽ chỉ từng bước.

---

## Nhóm 6 — Các tool phụ (cài khi cần, không cần ngay)

Khi Claude Code báo lỗi thiếu tool nào thì mới cài — sẽ nhớ lâu hơn.

| Tool | Khi nào cần |
|---|---|
| Python 3.11+ | Khi làm bot Telegram bằng Python hoặc data script |
| Node.js 22+ | Frontend web (React/Vite) |
| Bun | Nếu dự án dùng Bun (nhanh hơn npm ~10×) |
| Supabase CLI | Quản lý migration DB, deploy edge function |
| jq | Parse JSON trong terminal |
| ripgrep | Grep nhanh (Claude Code dùng) |
| GitHub CLI (`gh`) | Tạo PR, issue từ terminal |

---

## Chi phí ước tính 1 năm

Cho use case bác sĩ CK1 dùng cá nhân:

| Dịch vụ | Chi phí/năm |
|---|---|
| Claude Code API | ~$60-180 (60% việc chính) |
| MiniMax API | ~$24-60 (40% việc rẻ — extract, dịch) |
| Supabase | $0 (free tier) |
| Vercel | $0 (Hobby tier) |
| Cloudflare R2 (ảnh atlas) | $0 (10GB free) |
| GitHub | $0 |
| Domain (nếu muốn `dalieu.xxx`) | ~$15/năm optional |
| **Tổng** | **~$85-255/năm (2-6 triệu VND)** |

So sánh: học phí CK1 khoảng 20-40 triệu/năm. Chi phí AI tools ~10-30% học liệu 1 năm. ROI cao nếu app thật sự giúp anh học tốt hơn.

**Tip tiết kiệm**:
- Multi-model workflow (file 09) giảm 40-60% chi phí AI
- Dùng model local Ollama (miễn phí) cho task đơn giản
- Chỉ nạp $5-10 trước, xem hết bao lâu rồi nạp thêm

---

## Luồng cài đặt gợi ý (1 buổi tối)

Nếu anh có ~2 giờ:

1. **15 phút** — Đăng ký 3 tài khoản (GitHub, Anthropic, Supabase)
2. **20 phút** — Cài Homebrew (Mac) hoặc WSL2 (Windows) + VS Code
3. **10 phút** — Cài Claude Code, đăng nhập
4. **20 phút** — Cài extension trong VS Code (Claude Code, Prettier, GitLens, Cline)
5. **15 phút** — Cấu hình Git + đăng nhập GitHub
6. **30 phút** — Mở Claude Code, gõ "tạo dự án thử nghiệm tên `thu-nghiem`, làm file test.md ghi 'Xin chào'" → xem Claude làm việc
7. **10 phút** — Chạy `/init` để Claude tự tạo CLAUDE.md mẫu, đọc thử

Xong buổi 1. Ngày mai có thể bắt đầu dự án chính.

---

## Checklist "máy sẵn sàng"

Trước khi bắt đầu dự án da liễu, đảm bảo:

- [ ] Có 3 tài khoản (GitHub, Anthropic có nạp tiền, Supabase)
- [ ] VS Code cài xong, mở được
- [ ] Extension Claude Code hiển thị icon spark
- [ ] Gõ `claude` trong terminal chạy được, đăng nhập thành công
- [ ] Gõ `git status` trong bất kỳ folder nào không báo "command not found"
- [ ] Đã tạo 1 folder thử nghiệm và Claude sửa được file trong đó

Nếu ≥ 5/6 tick → sang file 04.

---

## Nếu anh chỉ nhớ được 3 điều

1. **VS Code là nhà** — mở nó là bắt đầu ngày làm việc
2. **Đừng cài hết tool 1 lượt** — thiếu tool nào Claude báo, cài tool đó
3. **Bắt đầu chế độ Manual** — sau 1 tuần chuyển Plan mode

## Đọc tiếp

- [04_tech_stack.md](04_tech_stack.md) — Các stack công nghệ và vai trò từng cái
- [06_bao_mat_git.md](06_bao_mat_git.md) — Đọc trước lần commit đầu tiên
