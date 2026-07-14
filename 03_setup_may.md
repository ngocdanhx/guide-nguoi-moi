# 03 — Chuẩn bị máy tính (cài gì, mất tiền không)

> File này chỉ nói **cần cài cái gì và tại sao**. Câu lệnh cụ thể — không cần thuộc, khi cài thật Claude Code sẽ hướng dẫn từng bước.

## Bức tranh chung

Hình dung anh mở một phòng khám mới. Cần chuẩn bị:

1. **Điện + mạng + tài khoản** — nền tảng
2. **Bàn làm việc + tủ hồ sơ** — nơi làm việc
3. **Máy chụp X-quang, siêu âm** — công cụ chính
4. **Kho thuốc + kho vật tư** — tài nguyên
5. **Nhân viên phụ** — công cụ bổ trợ

Không cần chuẩn bị hết một lượt. Bắt đầu từ 3 nhóm đầu tiên là chạy được. Nhóm sau cài dần khi cần.

---

## Nhóm 1 — Tài khoản (làm trước tiên, đa số miễn phí)

Cần 3-4 tài khoản chính:

### GitHub (miễn phí)
Nơi lưu code. Đăng ký bằng email 2 phút. Sau này mọi dự án sẽ lưu lên đây, coi như sao lưu trên mạng.

### Anthropic Console (miễn phí đăng ký, nạp tiền để dùng)
Nơi cấp khoá để Claude Code hoạt động. Đăng ký miễn phí, nạp 200-500 nghìn để bắt đầu. Dùng cá nhân 1 tháng khoảng 200-500 nghìn là đủ.

### Supabase (miễn phí)
Nơi lưu dữ liệu (kho hồ sơ) — ứng với "Món 1" trong file 04. Miễn phí đủ cho dự án cá nhân.

### MiniMax (miễn phí đăng ký, trả tiền theo dùng)
Cung cấp AI rẻ cho việc đơn giản. Đăng ký miễn phí, nạp 50-100 nghìn để bắt đầu.

**Tuỳ chọn (không cần ngay)**:
- **Vercel** — đưa web lên mạng miễn phí
- **Telegram** — cài app trên điện thoại, tạo bot qua @BotFather khi cần

---

## Nhóm 2 — Máy tính + terminal

### Máy tính
Bất kỳ máy Mac/Windows/Linux nào có RAM ≥ 8GB. Nếu định dùng AI trên máy (Ollama, xem file 09) thì cần RAM ≥ 16GB.

### Terminal (cửa sổ dòng lệnh)
Nơi anh gõ lệnh giao tiếp với máy:
- **Mac**: có sẵn (bấm Cmd+Space, gõ "Terminal")
- **Windows**: cài "Windows Terminal" từ Microsoft Store, sau đó cài WSL2 (Linux chạy bên trong Windows) — Claude Code chạy mượt hơn trên WSL2

### Homebrew (Mac) / WinGet (Windows)
Chương trình quản lý phần mềm. Cài Homebrew rồi mọi công cụ khác cài 1 dòng lệnh là xong.

**Chi phí**: miễn phí toàn bộ.

---

## Nhóm 3 — VS Code (nơi soạn thảo — BẮT BUỘC)

Anh sẽ dùng **VS Code** làm nơi làm việc chính, không phải chỉ terminal.

**Vì sao VS Code?**
- Miễn phí, do Microsoft phát hành
- Chạy trên mọi hệ điều hành (Mac/Windows/Linux)
- Có bộ cài thêm Claude Code chính thức — bấm 1 nút mở khung chat AI bên phải màn hình
- Nhìn diff (thay đổi) trực quan khi AI sửa code
- Cài nhiều bộ cài thêm khác cho đa AI (Cline, Continue.dev)

### Bộ cài thêm bắt buộc

| Bộ cài thêm | Vai trò | Miễn phí |
|---|---|---|
| Claude Code (Anthropic) | AI làm việc chính | ✓ (chỉ trả phí AI khi dùng) |
| Prettier | Tự động định dạng code khi lưu | ✓ |
| ESLint | Báo lỗi ngay khi gõ | ✓ |
| GitLens | Xem lịch sử ai sửa dòng nào | ✓ |
| Tailwind CSS IntelliSense | Gợi ý style khi làm web | ✓ |

### Bộ cài thêm khuyến khích cho đa AI

| Bộ cài thêm | Vai trò |
|---|---|
| Cline | AI hỗ trợ nhiều loại (Claude, MiniMax, Gemini). Miễn phí. |
| Continue.dev | Copilot đa nguồn, có gợi ý tự động khi gõ. Miễn phí. |

Chi tiết cách dùng đa AI: file [09_multi_model_workflow.md](09_multi_model_workflow.md).

**Chi phí**: VS Code + tất cả bộ cài thêm đều **miễn phí**. Chỉ trả phí AI khi dùng.

---

## Nhóm 4 — Claude Code (AI làm việc chính)

Cài bằng 1 lệnh cài đặt. Khi cài Claude Code sẽ hướng dẫn.

Sau khi cài, gõ `claude` trong terminal là mở khung chat. Lần đầu nó bảo anh đăng nhập tài khoản Anthropic (tự mở trình duyệt).

### 3 chế độ quyền (quan trọng!)

Bấm `Shift + Tab` để đổi qua lại giữa 3 chế độ:

| Chế độ | Hành vi | Khi nào dùng |
|---|---|---|
| **Thủ công** | Hỏi trước mọi lần sửa | Việc lạ, chưa tin |
| **Kế hoạch** | AI viết kế hoạch trước, chờ duyệt mới sửa | Việc lớn, muốn xem xét |
| **Tự động** | Tự sửa + tự chạy lệnh được phép | Việc quen, cần tốc độ |

**Người mới nên bắt đầu Thủ công**. Sau 1 tuần chuyển sang Kế hoạch cho việc lớn.

**Chi phí**: khoảng 200-500 nghìn/tháng cho dùng cá nhân.

---

## Nhóm 5 — Git + GitHub (nơi lưu code)

### Git
Chương trình quản lý phiên bản code. Mac có sẵn; Windows cài từ trang chủ.

Cài 1 lần, cấu hình 3 thứ:
- Tên anh
- Email của anh (dùng email GitHub)
- Đặt tên nhánh mặc định là `main`

### Đăng nhập GitHub — 2 cách

**Cách 1 — HTTPS + khoá cá nhân**: dễ nhất cho người mới. Vào GitHub Settings, tạo khoá, dùng khoá thay mật khẩu khi push code.

**Cách 2 — SSH**: bảo mật hơn, cài phức tạp hơn một chút.

Cả 2 cách đều miễn phí. Khi anh gõ vào Claude Code "hướng dẫn tôi cài git và đăng nhập GitHub", nó sẽ chỉ từng bước.

**Chi phí**: Git + GitHub đều **miễn phí**.

---

## Nhóm 6 — Các công cụ phụ (cài khi cần)

Khi Claude Code báo thiếu công cụ nào thì cài công cụ đó. Không cần cài hết một lượt.

| Công cụ | Khi nào cần | Miễn phí |
|---|---|---|
| Python | Khi làm bot bằng Python hoặc xử lý dữ liệu | ✓ |
| Node.js | Khi làm trang web (React/Vite) | ✓ |
| Bun | Khi dự án dùng Bun (nhanh hơn npm 10 lần) | ✓ |
| Supabase CLI | Quản lý kho hồ sơ | ✓ |
| GitHub CLI | Tạo PR/issue từ terminal | ✓ |
| Ollama | Nếu muốn dùng AI trên máy | ✓ |

**Chi phí**: tất cả đều miễn phí.

---

## Chi phí ước tính 2 tháng đầu

Cho bác sĩ dùng cá nhân:

| Hạng mục | Chi phí 2 tháng | Miễn phí không? |
|---|---|---|
| GitHub | 0 | ✓ Miễn phí |
| VS Code + bộ cài thêm | 0 | ✓ Miễn phí |
| Git + Homebrew | 0 | ✓ Miễn phí |
| Supabase (kho hồ sơ) | 0 | ✓ Miễn phí (đủ dùng) |
| Vercel (đưa web lên) | 0 | ✓ Miễn phí |
| Cloudflare R2 (kho ảnh) | 0 | ✓ Miễn phí (10GB) |
| Ollama (AI trên máy) | 0 | ✓ Miễn phí |
| Claude Code API | 300-600 nghìn | ✗ Trả theo dùng |
| MiniMax API | 100-200 nghìn | ✗ Trả theo dùng |
| Tên miền tuỳ chọn | 200-300 nghìn/năm | ✗ Không bắt buộc |
| **Tổng 2 tháng** | **400-800 nghìn** | |

So sánh: học phí chuyên khoa 1 khoảng 20-40 triệu/năm. Chi phí công cụ chỉ 2-4% học phí — rất đáng nếu ứng dụng giúp anh học tốt hơn.

**Mẹo tiết kiệm**:
- Dùng đa AI workflow (file 09) giảm 40-60% chi phí AI
- Dùng Ollama (miễn phí) cho việc đơn giản
- Nạp 200-300 nghìn trước để thử, xem đủ dùng bao lâu rồi nạp thêm

---

## Luồng cài đặt gợi ý (1 buổi tối 2 giờ)

Nếu anh có ~2 giờ:

1. **15 phút** — Đăng ký 4 tài khoản (GitHub, Anthropic có nạp tiền, Supabase, MiniMax)
2. **20 phút** — Cài Homebrew (Mac) hoặc WSL2 (Windows) + VS Code
3. **10 phút** — Cài Claude Code, đăng nhập
4. **20 phút** — Cài bộ cài thêm trong VS Code (Claude Code, Prettier, GitLens, Cline)
5. **15 phút** — Cấu hình Git + đăng nhập GitHub
6. **30 phút** — Mở Claude Code, gõ "tạo dự án thử nghiệm tên thu-nghiem, làm file test.md ghi Xin chào" → xem Claude làm việc
7. **10 phút** — Chạy `/init` để Claude sinh file hồ sơ mẫu

Xong buổi 1. Ngày mai có thể bắt đầu dự án chính.

---

## Bảng kiểm tra "máy sẵn sàng"

Trước khi bắt đầu dự án da liễu, đảm bảo:

- [ ] Có 4 tài khoản (GitHub, Anthropic đã nạp tiền, Supabase, MiniMax)
- [ ] VS Code cài xong, mở được
- [ ] Bộ cài thêm Claude Code hiển thị icon
- [ ] Gõ `claude` trong terminal chạy được, đăng nhập thành công
- [ ] Gõ `git status` trong bất kỳ thư mục nào không báo "command not found"
- [ ] Đã tạo 1 thư mục thử nghiệm và Claude sửa được file trong đó

Nếu ≥ 5/6 tick → sang file 04.

---

## Nếu anh chỉ nhớ được 3 điều

1. **Đa số miễn phí** — chỉ AI dev tính tiền theo dùng (~400-800 nghìn/2 tháng)
2. **VS Code là nhà** — mở nó là bắt đầu ngày làm việc
3. **Đừng cài hết một lượt** — thiếu công cụ nào Claude Code báo, cài công cụ đó

## Đọc tiếp

- [04_tech_stack.md](04_tech_stack.md) — Các món đồ nghề và vai trò từng cái
- [06_bao_mat_git.md](06_bao_mat_git.md) — Đọc trước lần commit đầu tiên
