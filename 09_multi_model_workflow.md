# 09 — Dùng nhiều AI cùng lúc (Claude + MiniMax + local)

> Không có 1 model AI nào tốt nhất cho mọi việc. Biết phân task cho đúng model giúp anh **giảm 40-60% chi phí** mà vẫn giữ chất lượng cao.

## Vấn đề: Claude cho mọi việc = tốn tiền

Claude Sonnet mạnh, thông minh, hiểu context tốt. Nhưng đắt.

Cùng 1 task "dịch 200 câu explanation tiếng Anh sang tiếng Việt":
- Claude Sonnet: ~$1
- MiniMax highspeed: ~$0.15 (rẻ hơn 6×)
- Chất lượng thực tế: gần bằng nhau cho task dịch đơn giản

Nếu anh chỉ dùng Claude cho tất cả, sau 1 năm học CK1: ~$200. Multi-model: ~$60-80. **Chênh $120-140/năm = 3-4 triệu VND**.

Chưa kể có task nhạy cảm dữ liệu (VD note case bệnh nhân) — không muốn gửi lên cloud → cần model chạy local.

---

## Nguyên tắc phân task cho model

Bức tranh phân loại đơn giản:

### Việc "khó" — phải Claude làm

- Kiến trúc/thiết kế (VD "nên tách bảng thành 2 bảng không?")
- Refactor code phức tạp (> 200 dòng, nhiều dependency)
- Debug bug khó (đã thử fix 2 lần chưa xong)
- Review code trước commit
- Viết CLAUDE.md, spec, plan lớn
- Bất kỳ task cần **reasoning nhiều bước**

Model chọn: **Claude Sonnet** cho hầu hết. **Claude Opus** khi cần suy nghĩ sâu (1 tuần vài lần thôi vì đắt).

### Việc "vừa" — MiniMax hoặc Gemini Flash

- Extract text từ PDF/scan thành JSON
- Dịch tiếng Anh y khoa ↔ tiếng Việt
- Format lại text (normalize whitespace, escape quote)
- Generate boilerplate code (form CRUD, migration SQL đơn giản)
- Explain code từng dòng (task giáo dục, không cần deep reasoning)
- Tag tự động (VD "đọc câu hỏi này thuộc bệnh nào?")
- Summarize log/error message
- Chấm câu trả lời tự luận (comparative)

Model chọn: **MiniMax M2.7 highspeed** (rẻ nhất) hoặc **Gemini Flash**.

### Việc "đơn giản" — local model (Ollama)

- Format JSON đơn giản
- Task lặp đi lặp lại (batch process 1000 file)
- Task nhạy cảm dữ liệu (không muốn gửi cloud)
- Khi trực đêm không có wifi tốt
- Prototype nhanh không muốn tốn tiền

Model chọn: **Qwen2.5-coder 7b** hoặc **Llama 3.2 3b** qua Ollama.

---

## MiniMax là gì (giới thiệu nhanh)

**MiniMax** là công ty AI Trung Quốc (Shanghai). Họ có các model:
- **MiniMax-M3** — mạnh nhất, context 1 triệu token, đa phương thức (text + ảnh)
- **MiniMax-M2.7** — cân bằng
- **MiniMax-M2.7-highspeed** — rẻ nhất, nhanh nhất

**Điểm hay nhất cho anh**: MiniMax có **Anthropic-compatible API** — nghĩa là Claude Code có thể chạy MiniMax model **chỉ bằng cách đổi environment variable**, không cần code lại workflow gì.

Nghĩa là:
- Sáng anh gõ `claude` → dùng Claude Sonnet (task chính)
- Chiều anh gõ `claude-mini` (alias tự đặt) → cùng Claude Code nhưng backend là MiniMax
- Trải nghiệm giống hệt nhau, chỉ khác cost

---

## Setup MiniMax với Claude Code

**Bước 1**: đăng ký `platform.minimax.io`, tạo API key.

**Bước 2**: test API bằng 1 lệnh curl đơn giản (Claude Code hướng dẫn từng bước khi anh cần).

**Bước 3**: setup 2 alias trong file `.zshrc` hoặc `.bashrc`:
- `claude-real` — dùng Claude thật (Anthropic API)
- `claude-mini` — dùng MiniMax API backend

Sau đó anh có 2 lệnh:
- Task chính → gõ `claude-real`
- Task rẻ → gõ `claude-mini`

Đơn giản vậy thôi.

---

## Cline — VS Code extension đa provider

Ngoài Claude Code, anh nên cài thêm **Cline** (extension VS Code).

**Cline khác Claude Code ra sao?**
- Claude Code: terminal-first, có nhiều skill, hook, MCP mạnh
- Cline: panel bên VS Code, dropdown chọn model dễ, switch nhanh giữa nhiều provider

**Khi nào dùng Cline thay Claude Code?**

| Tình huống | Dùng |
|---|---|
| Task chính, cần full workflow, skill, MCP | Claude Code |
| Muốn switch nhanh giữa nhiều model để so sánh | Cline |
| Task đơn lẻ trong VS Code, không cần setup nhiều | Cline |
| Debug bug, cần thấy diff trực quan trong VS Code | Cả 2 đều được |

Anh có thể lưu **nhiều profile** trong Cline:
- Profile "Claude Sonnet" — Anthropic thật
- Profile "MiniMax M3" — MiniMax API
- Profile "Gemini Flash" — Google
- Profile "Qwen local" — Ollama

Click 1 nút là switch. Rất tiện khi anh muốn so sánh cùng 1 prompt trên 3 model xem model nào cho kết quả tốt nhất.

---

## Continue.dev — alternative của Cline

Continue.dev là VS Code extension khác, mạnh về **support model local (Ollama)** và có **tab autocomplete** (như GitHub Copilot).

Nếu anh muốn:
- Autocomplete inline khi gõ code → Continue.dev
- Dùng chủ yếu local model để tiết kiệm → Continue.dev

Nếu anh muốn:
- Agent mode mạnh (Cline có mode "act" tự động chạy nhiều bước) → Cline

**Có thể cài cả 2 song song** — không xung đột. Em recommend cài Cline trước.

---

## Ollama — chạy AI local miễn phí

**Ollama** cho phép chạy model AI open-source (Llama, Qwen, Gemma) trên máy anh — **không cần internet, không tốn tiền**.

**Yêu cầu máy**:
- RAM ≥ 16GB cho model 7 tỷ tham số (mạnh nhất trong tầm này)
- RAM 8GB dùng model 3 tỷ nhẹ hơn
- Không cần GPU (chạy CPU cũng được, chỉ chậm hơn)

**Model recommend cho coding**:
- **Qwen2.5-coder 7b** — mạnh nhất cho code, ~4GB
- **Llama 3.2 3b** — nhẹ hơn, ~2GB
- **Gemma 2 2b** — nhẹ nhất, ~1.5GB

**Chất lượng vs Claude**: model local yếu hơn Claude Sonnet khoảng 50-70%. Không dùng cho task quan trọng, chỉ dùng cho:
- Format text
- Dịch câu ngắn
- Task lặp đi lặp lại (batch)
- Khi offline

**Wire vào workflow**: Continue.dev có support Ollama built-in. Cline cũng support qua config.

---

## Chiến lược thực tế cho dự án da liễu

### Kịch bản 1 — Tuần bận (đang trực nhiều)

- **Claude Code** cho fix bug nhanh, review code
- **Ollama local** cho tra syntax, format JSON — không mất data lên cloud khi wifi bệnh viện không tin cậy

### Kịch bản 2 — Weekend soạn content batch

- **MiniMax M2.7 highspeed** parse 5 chương sách → 300 flashcard candidate (rẻ + nhanh)
- **Claude Sonnet** review 20 card random, verify chất lượng
- Nếu MiniMax sai > 10% → fallback sang Claude cho phần còn lại

### Kịch bản 3 — Debug bug phức tạp

- **Claude Opus** (dùng skill `systematic-debugging`) tìm root cause
- Sau khi hiểu root cause → **MiniMax highspeed** viết fix (task đơn giản)

### Kịch bản 4 — Trước ngày thi 1 tuần

- **Gemini Flash** hoặc **MiniMax** chấm 500 câu trả lời tự luận, cost thấp
- Cần đối chiếu đáp án chuẩn → không cần reasoning phức tạp

---

## Bảng giá tham khảo (2026)

Giá approximate cho **1 triệu token input / 1 triệu token output**:

| Model | Input | Output | Best cho |
|---|---|---|---|
| Claude Opus 4 | $15 | $75 | Reasoning cực sâu |
| Claude Sonnet 4 | $3 | $15 | Coding chất lượng cao |
| Claude Haiku 4 | $0.80 | $4 | Task nhanh chất lượng vừa |
| Gemini 2.5 Pro | $1.25 | $10 | Vision + long context |
| Gemini 2.5 Flash | $0.30 | $2.50 | Task rẻ + đủ chất lượng |
| MiniMax M3 | ~$0.40 | ~$1.60 | Coding + multilingual |
| MiniMax M2.7 highspeed | ~$0.20 | ~$0.80 | Task đơn giản, cost tối thiểu |
| GPT-5 | $10 | $50 | Nếu đã quen OpenAI ecosystem |
| GPT-5 mini | $0.25 | $2 | Alternative rẻ của OpenAI |
| Ollama local | $0 | $0 | Miễn phí, offline, private |

**Ước tính rough cho dự án cá nhân**:
- Task lớn (viết feature, refactor): ~10k-50k token/task
- Task nhỏ (extract, format): ~1k-5k token/task
- 1 tháng: ~200 task lớn + 500 task nhỏ ≈ 5-10M token
- **Chỉ Claude**: $60-150/tháng
- **Multi-model**: $10-30/tháng

---

## Rule tự viết trong CLAUDE.md (setup nhẹ nhàng nhất)

Không cần cài LiteLLM Proxy hay Claude Code Router phức tạp. Đơn giản là viết vào CLAUDE.md:

> ## Model routing rule
>
> - Task "extract từ PDF" → gõ `claude-mini` (MiniMax M2.7 highspeed)
> - Task "refactor > 200 lines" → gõ `claude-real` (Claude Sonnet)
> - Task "translate en to vi" → gõ `claude-mini`
> - Debug bug > 30 phút chưa xong → escalate `claude-real` (Claude Opus)

Rule này là prompt, Claude tự nhắc anh switch khi thấy task phù hợp. Anh làm quen dần rồi tự có phản xạ.

---

## Bảo mật API key nhiều provider

Nhiều key = nhiều rủi ro leak. Rule:

- Mọi key nằm trong 1 file `.env` duy nhất, **gitignored**
- Trong `.zshrc` load `.env` tự động mỗi lần mở terminal
- Alias `claude-real` và `claude-mini` set env var trước khi chạy `claude`
- Rotate tất cả key mỗi 3-6 tháng

Chi tiết secret management: file [06](06_bao_mat_git.md).

---

## Anti-pattern cần tránh

### 1. Dùng Claude Opus cho mọi task

Opus đắt gấp 5-20× các model khác. Chỉ dùng khi thật sự cần reasoning phức tạp (hội chẩn ca khó, không phải khám lâm sàng thường).

### 2. Dùng MiniMax cho task cần precision cao

MiniMax M2.7 highspeed rất nhanh nhưng đôi khi sai đáp án tinh vi. Task "chấm câu trả lời y khoa" nên dùng Claude Sonnet trở lên — kẻo học viên bị chấm sai.

### 3. Switch model giữa chừng 1 session dài

Context bị reset khi đổi backend → Claude/MiniMax không hiểu "session trước em đang làm gì". Nên finish 1 task, `/clear`, rồi mới switch model.

### 4. Không theo dõi cost

Vào dashboard Anthropic + MiniMax weekly xem đã tiêu bao nhiêu. Nếu tăng bất thường → có thể do prompt loop, infinite exploration, hoặc key leak (attacker dùng).

### 5. Commit `.env` chứa nhiều key

Càng nhiều key càng đau khi leak. Rotate mọi key + revoke ngay khi phát hiện.

---

## Checklist "multi-model đã setup xong"

- [ ] Có API key Claude (Anthropic Console) — đã nạp $5-10
- [ ] Có API key MiniMax (platform.minimax.io) — đã nạp $2-5
- [ ] File `.env` chứa cả 2 key, gitignored
- [ ] Alias `claude-real` và `claude-mini` chạy được
- [ ] Đã test 1 prompt đơn giản với cả 2 model, thấy output
- [ ] (Optional) Cài Cline hoặc Continue.dev trong VS Code
- [ ] (Optional) Cài Ollama + pull 1 model local (nếu RAM ≥ 16GB)
- [ ] Hiểu khi nào dùng model nào (mục "Nguyên tắc phân task")

---

## Nếu anh chỉ nhớ được 3 điều

1. **Claude cho khó, MiniMax cho vừa, Ollama cho đơn giản** — quy tắc phân task cơ bản
2. **MiniMax có Anthropic-compatible API** — dùng Claude Code với MiniMax chỉ bằng 2 env var
3. **Multi-model tiết kiệm 40-60% chi phí** — cùng 1 chất lượng thực tế

## Đọc tiếp

- [03_setup_may.md](03_setup_may.md) — Setup VS Code + Claude Code (nền)
- [06_bao_mat_git.md](06_bao_mat_git.md) — Chi tiết secret management (nhiều key hơn thì càng cần chặt)
- [07_du_an_da_lieu_kickoff.md](07_du_an_da_lieu_kickoff.md) — Áp dụng vào roadmap 6 tuần
