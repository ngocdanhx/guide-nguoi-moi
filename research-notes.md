# Research Notes (chi tiết, dùng để tra sâu)

> Đây là output từ 4 sub-agent research online + 1 Explore agent tổng hợp pattern chung. Tổng ~15000 từ, ~100 nguồn URL. Em viết 9 file tài liệu chính dựa trên notes này.
>
> Nếu 9 file chính chưa đủ cho câu hỏi của anh, tra file này để đọc chi tiết + verify nguồn gốc.

## Cấu trúc

- **Phần A** — Bộ đồ nghề AI-assisted development (pattern tổng quát, tổng hợp từ nhiều dự án tham khảo)
- **Phần B** — Nhóm 1: Spec-driven Development & AI Coding Workflow (Spec Kit, Superpowers, Claude Code)
- **Phần C** — Nhóm 2: Học tập y khoa + Anki + Chuyên khoa 1 Da liễu Hà Nội
- **Phần D** — Nhóm 3: Tech Stack cho App học tập (Telegram, Supabase, Frontend, Deploy)
- **Phần E** — Nhóm 4: Setup máy + Git + Security + Quy trình vận hành

---

# PHẦN A — Bộ đồ nghề AI-assisted development (pattern tổng hợp)

## A.1 Claude Code / AI dev workflow (nền tảng)

**`CLAUDE.md`** — File markdown ở gốc mỗi project mà Claude Code **auto-load** vào context mỗi session. Đây là "hồ sơ dự án": tech stack, quy ước code, rule đặc biệt, gotcha, checklist trước khi handoff. Độ dày điển hình:
- Mỏng (~80 dòng): mục tiêu, tech stack, secret pattern, handoff checklist
- Trung bình (~150 dòng): thêm slash command workflow, coding convention
- Dày (~300+ dòng): rules chi tiết, skill gating, hook, sub-agent registry

**`AGENTS.md`** — Format chuẩn cross-tool (agents.md open spec) — Cursor, Codex, Aider, Claude Code đều đọc được. `CLAUDE.md` thường chỉ là adapter mỏng `@AGENTS.md`.

**`MEMORY.md` / `NOTES.md`** — "Institutional memory" — ghi lại decision, root-cause bug đã sửa, gotcha (mỗi entry có ngày + vấn đề + fix). Chống lặp lại lỗi cũ khi Claude quên context.

**`.claude/skills/<name>/SKILL.md`** — "Skill" là 1 workflow đóng gói — Claude tự invoke khi user gõ trigger match `description` trong YAML frontmatter. Format: folder chứa `SKILL.md` (bắt buộc) + `references/` (deep-dive) + `agents/` optional.

**YAML frontmatter cho skill** — 4 trường then chốt: `name`, `description` (bắt đầu bằng "Use when …" để Claude match trigger), `allowed-tools` (comma-separated, restrict tool access), `disable-model-invocation` (chặn auto-trigger, chỉ user gõ mới chạy).

**`.claude/agents/<name>.md`** — Sub-agent (Claude Code Task tool) đóng vai chuyên môn hẹp (frontend / backend / reviewer …). Có YAML `name`/`description`/`tools`. User gọi bằng cách delegate. Dự án lớn thường có 5-10 vai chuyên biệt.

**`.claude/hooks/`** — Script bash mà **harness** (không phải model) chạy khi tool được gọi. 4 event thường dùng:
- **`PreToolUse`** — chạy trước Bash/Write/Edit; `exit 2` = block tool. Ví dụ: chặn xóa file quan trọng, chặn commit chứa secret.
- **`PostToolUse`** — chạy sau Edit/Write; auto-format hoặc validate. Ví dụ chạy `eslint --fix` sau khi edit `.ts` file.
- **`SessionStart`** — inject context động đầu session (branch, sprint, số liệu thật). Chống doc stale.
- **`Stop`** / **`statusLine`** — notification desktop / status bar.

Điểm mấu chốt: rule ghi trong SKILL.md **chỉ là prompt** — Claude có thể "quên". Muốn enforce cứng phải qua hook `exit 2` (kỹ thuật, không phụ thuộc agent tự giác).

**`.claude/settings.json`** — Cấu hình project-level: `model` mặc định, `hooks` wire (event → matcher → command), `permissions.allow` (Bash prefix được phép chạy không cần hỏi), `statusLine`.

**`.claude/settings.local.json`** — Personal override, **gitignored**.

**Slash commands** — 2 pattern định nghĩa:
1. **Command markdown** trong `.claude/commands/<name>.md` với YAML frontmatter (`description`/`argument-hint`/`allowed-tools`).
2. **Skill với `disable-model-invocation: true`** trong `.claude/skills/<name>/SKILL.md` — chỉ user gõ `/name` mới trigger. Pattern điển hình: `/start`, `/specify`, `/implement`, `/review`, `/commit`.

## A.2 Spec-driven development / Superpowers

**Superpowers** — Framework của Jesse Vincent (`obra/superpowers`), cài qua Claude plugin marketplace. Triết lý: "encode best practices vào skills mà AI **buộc phải dùng** khi trigger match" (dùng persuasion principles để agent không skip dù đang crunch).

**5 skill "core" đáng port sang mọi dự án**:
- **`using-superpowers`** — Anchor skill, activate đầu conversation, enforce rule "nếu skill applies thì bắt buộc dùng".
- **`writing-plans`** — Task > 30' effort → viết plan file `docs/superpowers/plans/YYYY-MM-DD-<feature>.md` trước, chia thành step 2-5 phút.
- **`systematic-debugging`** — Iron Law: "NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST". Không được patch symptom.
- **`verification-before-completion`** — Iron Law: "NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE". Trước khi báo "done" phải chạy verify command trong chính message này.
- **`test-driven-development`** — Test trước code sau. Một số dự án tắt project-wide khi đã ổn định.

**Skill Gating Protocol** (custom) — thay vì auto-apply, Claude phải hỏi "Trigger detected: X. Apply <skill> (slower, rigorous) hay skip (fast)?" rồi chờ Y/N.

**Quy trình spec → plan → implement → verify (pattern điển hình)**:
- **`/start <msg>`** — Session entrypoint
- **`/specify <ID>`** — Fetch ticket + context → sinh spec file
- **`/implement <ref>`** — Đọc spec đã duyệt → code
- **`/review [ref]`** — Single review entrypoint, auto-detect mode
- **`/commit`** — Sinh commit message + git commit

**Karpathy Rules** (pattern rule dày trong CLAUDE.md) — 4 rule kỷ luật:
1. Think Before Coding
2. Simplicity First
3. Surgical Changes
4. Goal-Driven Execution

## A.3 Project structure patterns

**Layout điển hình 1 full-stack Supabase app**:

```
project/
├── frontend/            # React + Vite + TanStack Router
│   └── src/{routes,pages,components,hooks,stores,types,lib}/
├── services/            # Supabase interaction layer — PROJECT ROOT
├── supabase/
│   ├── migrations/      # SQL migrations YYYYMMDDHHMMSS_<name>.sql
│   ├── functions/       # Edge Functions (Deno/TypeScript)
│   └── config.toml
├── scripts/             # Data pipeline / OS scheduler installers
├── docs/                # ADLC output — sprints, plans, audits, research
├── tests/               # Python pytest / cross-cutting tests
├── .claude/             # AI config
├── CLAUDE.md
└── README.md
```

**Rule tách layer tối quan trọng**:
- `services/` ở **project root**, KHÔNG trong `frontend/`
- Components/pages **không được** import trực tiếp từ `services/` — chỉ qua loader hoặc Zustand store
- `frontend/src/lib/` phải sạch React + Supabase (pure utility)
- `frontend/src/components/ui/` do shadcn CLI quản lý, KHÔNG sửa tay

## A.4 Tech stack thường gặp

### Supabase (backend platform)

- **Postgres** — DB chính. Rule: đã deploy `V1` rồi thì thay đổi phải tạo `V2`.
- **Auth** — sign-in email + anonymous session.
- **RLS (Row Level Security)** — **bắt buộc** enable trên mọi table.
- **Edge Functions (Deno)** — TypeScript function chạy hosted. Cold start ~50-200ms.
- **pg_cron** — schedule chạy Edge Function/SQL theo cron.
- **Winner-take-all dedup pattern** — table `xxx_log(date_local, key) PRIMARY KEY` + INSERT racing (bắt lỗi Postgres `23505`). Đảm bảo idempotency khi cron trigger song song.
- **`x-cron-secret` header** — auth đơn giản cho cron endpoint.
- **Timezone**: dùng `Intl.DateTimeFormat('en-CA', { timeZone: 'Asia/Ho_Chi_Minh' })`.
- **Storage / R2** — object storage cho audio, PDF chart.
- **Supabase types generation** — `supabase gen types typescript`. **KHÔNG sửa tay**.

### Frontend stack

- **React 18/19** — functional components only, hooks, strict TypeScript.
- **Vite** — dev server + build tool.
- **TanStack Router** — file-based routing. Loader handle data fetching, KHÔNG fetch trong component body.
- **shadcn/ui** + **Tailwind CSS v4** — primitives sinh vào `frontend/src/components/ui/`.
- **Zustand** — state store cho cross-page UI state, auth session, SRS queue.
- **vite-plugin-pwa** — biến app thành PWA (offline, installable).
- **Deploy**: **Vercel** (Hobby tier miễn phí).

### Python + Telegram bot

- **`curl_cffi`** — HTTP client giả TLS/JA3 fingerprint của Chrome → **vượt Cloudflare passive check**. `requests.get(url, impersonate="chrome")`.
- **Telegram Bot API** — gọi `fetch('https://api.telegram.org/bot<TOKEN>/sendMessage')` **thẳng**, KHÔNG cần grammY/Telegraf.
- **OS scheduler** (thay cron system): Windows Task Scheduler / macOS launchd / Linux systemd user timer.

### Deploy & CI

- **Vercel** cho frontend
- **Supabase Cloud** cho DB + Edge Function + pg_cron
- **`pytest`** (Python), **`vitest`** (frontend TS/TSX), **`deno test`** (Edge Function)

## A.5 Quy trình vận hành hằng ngày

**Git workflow — 3 pattern khác nhau**:
- **Pattern 1 — commit thẳng `main`**: đơn giản, phù hợp dự án cá nhân. Push khi user yêu cầu, tách commit theo scope
- **Pattern 2 — 2-repo tách AI config**: repo code + repo `.claude/` riêng, sync qua script wrapper snapshot 4 path (AGENTS.md, CLAUDE.md, .claude/, .cursor/)
- **Pattern 3 — feature branch + PR**: cho việc lớn, commit hook chặn `.env`, phù hợp khi share với đồng nghiệp

**Verification-before-completion (Iron Law)** — chưa chạy verify command trong message này thì không được claim "done".

**Bảo mật secrets**:
- File `.env` **luôn gitignored**. Có `.env.example` cho template.
- Hook `PreToolUse Bash` chạy `check-secrets.sh` trước `git commit`.
- Secret trên Edge Function: **Supabase Edge Function Secrets**.

## A.6 Content / data pipeline

**Nguyên tắc "Correct-by-construction"** — **không để AI chế đáp án**. Chỉ dùng nguồn có sẵn answer key thật. Case study điển hình: 146 câu reading thiếu explanation → dùng AI agent song song điền, verify 144/146 quote literal substring, 2/146 dùng `…` head+tail đều có thật (0 chế).

**Anti-pattern học được — Self-cloze**: script khoét từ trong passage rồi lấy chính từ đó làm đáp án → đề bất khả thi vì text informativity ≈ 0. Bài học: "đáp án đúng ≠ đề giải được".

**Cách scrape / xử lý content**:
1. **`curl_cffi impersonate="chrome"`** — Vượt Cloudflare passive fingerprint
2. **`pdftotext -layout`** (typed PDF) — Extract text an toàn
3. **`pdftoppm -png -r <dpi>`** (scan PDF) — Render page → PNG → Claude vision đọc. Answer key nhỏ cần **DPI ≥ 220-300**
4. **`pdfplumber`** — Extract data structured từ PDF digital
5. **Playwright headless** — Fallback khi có JS challenge Cloudflare Turnstile

**Invariant bất di bất dịch**: **KHÔNG DELETE items ever** (SRS history phụ thuộc). Out-of-scope → soft-flag qua `active_in_profiles`.

---

# PHẦN B — NHÓM 1: Spec-driven Development & AI Coding Workflow

## B.1 GitHub Spec Kit — Framework spec-driven chính thức của GitHub

**Bản chất & positioning**
- Spec Kit là "open-source toolkit" của GitHub để làm **Spec-Driven Development (SDD)**, "inverts traditional software development by making specifications executable and generative rather than merely descriptive". Nguồn: https://github.com/github/spec-kit
- Tagline: giúp dev "focus on product scenarios and predictable outcomes instead of vibe coding every piece from scratch"
- Repo ~121k stars, 10.7k forks, 190 releases (mới nhất v0.12.14, tháng 7/2026), MIT license

**Workflow 7 slash commands**
- `/speckit.constitution` → output `constitution.md` — nguyên tắc governance + development guidelines
- `/speckit.specify` → output `spec.md` — WHAT & WHY (không phải HOW)
- `/speckit.clarify` → thêm section Clarifications vào spec — làm rõ chỗ underspecified TRƯỚC khi plan
- `/speckit.plan` → output `plan.md`, `data-model.md`, `api-spec.json`
- `/speckit.tasks` → output `tasks.md` — task breakdown có thứ tự, có dependency
- `/speckit.implement` → output code thật
- `/speckit.converge` → so codebase với artifacts, phát hiện gap

**Cài đặt**
- Prerequisite: Python 3.11+, Git, `uv` (Astral) hoặc pipx, và một AI coding agent
- Lệnh cài: `uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z`
- Self-management: `specify self check`, `specify self upgrade`

**Triết lý (từ blog GitHub)**
- "Vibe-coding": code trả về "look right, but doesn't quite work" — non-compiling, partial solutions, architectural misalignment. Nguồn: https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/
- Chuyển paradigm: từ "code is the source of truth" → **"intent is the source of truth"**
- 4 phase tóm gọn: **Specify → Plan → Tasks → Implement**

## B.2 Superpowers (Jesse Vincent) — Framework "skills over prompts"

**Bản chất**
- "Complete software development methodology for your coding agents, built on top of a set of composable skills". Nguồn: https://github.com/obra/superpowers
- Tác giả Jesse Vincent — tạo RT ticketing, K-9 Mail, Keyboardio. ~50,000 developers đã cài trong vài tháng đầu.

**Triết lý "skills over prompts"**
- Skills không phải suggestion mà là **mandatory workflows**: "If you have a skill to do something, you _must_ use it to do that activity". Nguồn: https://blog.fsck.com/2025/10/09/superpowers/
- Skill kích hoạt tự động qua **description matching**
- Quote nổi tiếng: **"Specs are the thing that matters now. The code does not matter anymore"**
- Workflow chuẩn: **brainstorm → plan → implement**

**Skills quan trọng**
- **systematic-debugging** — 4-phase root cause analysis
- **test-driven-development** — RED → GREEN → REFACTOR
- **writing-plans** — implementation breakdown chi tiết
- **brainstorming** — Socratic design refinement
- **verification-before-completion** — evidence before claims
- **using-git-worktrees** — parallel dev branches
- **subagent-driven-development** — concurrent workflows

**Cài đặt**
- Claude Code: qua **Anthropic official plugin marketplace**
- Cursor: `/add-plugin superpowers`
- Cài thủ công: `git clone https://github.com/obra/superpowers ~/.claude/plugins/superpowers`

## B.3 Claude Code cơ bản

### Overview & Quickstart
- "Agentic coding tool that reads your codebase, edits files, runs commands, and integrates with your development tools". Nguồn: https://code.claude.com/docs/en/overview
- Cài Terminal: `curl -fsSL https://claude.ai/install.sh | bash` (macOS/Linux/WSL); Windows PowerShell: `irm https://claude.ai/install.ps1 | iex`
- Session commands: `/help`, `/clear`, `/exit`, `/resume`, `/login`, `/init`, `/memory`, `/hooks`, `/model`

### Slash commands & Skills
- **"Custom commands have been merged into skills."** File `.claude/commands/deploy.md` và `.claude/skills/deploy/SKILL.md` đều tạo `/deploy`
- Skill = folder `<skill-name>/SKILL.md` có **YAML frontmatter** + markdown body
- **Sự khác biệt cốt lõi skill vs CLAUDE.md**: CLAUDE.md load vào MỌI session (tốn token), skill body **chỉ load khi được dùng**
- Scope: Enterprise > Personal (`~/.claude/skills/`) > Project (`.claude/skills/`) > Plugin
- Bundled skills: `/doctor`, `/code-review`, `/batch`, `/debug`, `/loop`, `/claude-api`, `/run`, `/verify`, `/run-skill-generator`

### Sub-agents
- "Specialized AI assistants that handle specific types of tasks" — chạy trong context window RIÊNG
- Vị trí: `.claude/agents/*.md` (project) hoặc `~/.claude/agents/*.md` (user)
- Claude auto-delegate dựa trên **description** → viết description rõ ràng cực quan trọng

### Hooks
- "User-defined shell commands that execute at specific points in Claude Code's lifecycle" — control **deterministic**
- Events: `PreToolUse`, `PostToolUse`, `Notification`, `UserPromptSubmit`, `Stop`, `SessionStart`, `InstructionsLoaded`
- Cấu hình trong `settings.json` mục `hooks`

### CLAUDE.md vs Auto Memory (MEMORY.md)
- **Hai hệ thống memory bổ sung nhau, load MỌI session**
- **CLAUDE.md**: user viết, load full mỗi session
- **Auto memory (MEMORY.md)**: **Claude tự viết** — lưu tại `~/.claude/projects/<project>/memory/MEMORY.md`. Chỉ 200 dòng đầu load vào session

### MCP (Model Context Protocol)
- "Open source standard for AI-tool integrations" — kết nối external tools
- Ví dụ: **Figma MCP**, **Playwright MCP**, **GitHub MCP**, **Slack MCP**

## B.4 Vibe coding vs Spec-driven

**Vibe coding — origin**
- Thuật ngữ do **Andrej Karpathy** coin ngày **02/02/2025** trên X, tweet 4.5 triệu view. Nguồn: https://x.com/karpathy/status/1886192184808149383
- Định nghĩa: "There's a new kind of coding I call 'vibe coding', where you fully give in to the vibes... I just see stuff, say stuff, run stuff, and copy-paste stuff, and it mostly works."
- **Word of the Year 2025** của Collins Dictionary

**Số liệu adoption + rủi ro**
- Y Combinator W25: **25% startups** có codebase 95%+ AI-generated
- CodeRabbit 12/2025: code có AI co-author chứa **~1.7× nhiều major issues**, **2.74× security vulnerabilities**. Nguồn: https://en.wikipedia.org/wiki/Vibe_coding

**So sánh vibe vs SDD**
- Vibe: "prioritizes speed through conversational prompt-iterate cycles"
- SDD: "prioritizes maintainability through formal specifications"
- Vibe pros: dev nhanh hơn 55% avg
- Vibe cons: "hits a documented three-month wall where technical debt compounds"
- SDD pros: "eliminates requirements drift by design"

## B.5 Kiro (AWS) & framework tương tự

- **Kiro**: agentic AI dev platform do **AWS**, IDE + CLI + web. Spec-driven: prompt → **Requirements** → **Design** → **Tasks** → parallel agents. Nguồn: https://kiro.dev
- Dùng **property-based testing** (fuzz-like) để catch edge cases
- **Cursor Composer/Agent Mode**, **Windsurf Cascade** cũng có tính năng planning tương tự

---

# PHẦN C — NHÓM 2: Học tập y khoa + Anki + CK1 Da liễu Hà Nội

## C.1 Nguyên tắc khoa học học tập

### Spaced Repetition (SR)
- **Ebbinghaus forgetting curve (thập niên 1880)** — Hermann Ebbinghaus (Đức) định lượng đường cong quên đầu tiên
- **H.F. Spitzer (1939)** — thử nghiệm SR trên 3600 học sinh Iowa, bằng chứng thực nghiệm sớm nhất
- **SM-2 algorithm (1987)** — Piotr Woźniak (Ba Lan), track 3 tham số: repetition `n`, easiness factor `EF`, interval `I`. Grade ≥ 3 tăng interval, < 3 reset về 1 ngày. Nguồn: https://en.wikipedia.org/wiki/SuperMemo
- **FSRS (2023)** — thay thế SM-2 trong Anki từ **v23.10**. 2 phần: scheduler + optimizer (ML). Ease Factor bị loại bỏ khi bật FSRS. Nguồn: https://github.com/open-spaced-repetition/fsrs4anki

### Active Recall / Testing Effect
- "Long-term memory is increased when part of the learning period is devoted to retrieving information from memory". Nguồn: https://en.wikipedia.org/wiki/Testing_effect
- **Roediger & Karpicke (2006)** — landmark: nhóm test-then-restudy vượt xa nhóm restudy-only ở test delayed

### Interleaving
- **Rohrer & Taylor (2007)** — "The shuffling of mathematics practice problems improves learning". Interleaving làm practice performance kém hơn nhưng **gấp đôi score ở test sau 1 ngày**
- Rất phù hợp da liễu — cần phân biệt psoriasis vs eczema vs pityriasis

### Desirable Difficulty (Bjork)
- **Bjork Learning and Forgetting Lab, UCLA** (thành lập 1974). Nguồn: https://bjorklab.psych.ucla.edu
- Định nghĩa: "certain training conditions that are difficult... yield greater long-term benefits than their easier training counterparts"

### Evidence — SR trong y khoa
- **Meta-analysis 2025** (PubMed 41601436) — "The Effectiveness of Spaced Repetition in Medical Education". Nguồn: https://pubmed.ncbi.nlm.nih.gov/41601436/
- **Frontiers Medicine 2025** — SR paediatrics undergrad: effect size **d = 0.8** (large). Nguồn: https://www.frontiersin.org/journals/medicine/articles/10.3389/fmed.2025.1601614/full
- **JMIR 2024** — Spaced Digital Education for Health Professionals: Systematic Review. Nguồn: https://www.jmir.org/2024/1/e57760
- **PMC12357012 (2025)** — Anki + preclinical exam: n=36 UNLV students, nhóm mature cards ≥ 9,390 đạt CBSE 71.5% vs 60.0% (p=0.002)

## C.2 Anki trong y khoa

### Anki cơ bản
- **Là gì**: flashcard app open-source, SR algorithm. Website: https://apps.ankiweb.net
- **Lịch sử**: tạo bởi **Damien Elmes** (Úc), ban đầu học tiếng Nhật; **05/10/2006**. Tên "Anki" = 暗記 (ghi nhớ)
- **Platform**: Windows/macOS/Linux/FreeBSD free; AnkiDroid free; AnkiMobile (iOS) ~25 USD phí duy nhất
- **Adoption y khoa**: **86.2% med student surveyed dùng Anki, 66.5% hàng ngày**; AnKing deck vượt **300,000 lượt tải**
- **AnkiHub transition**: 2026 Damien Elmes chuyển giao dần cho AnkiHub

### AnKing Step Deck (gold standard USMLE)
- **Nguồn**: https://www.theanking.com và https://www.ankihub.net/step-deck
- **Quy mô**: **30,000+ cards** cho Step 1, 2 & 3, **100,000+ med student** sử dụng
- Tag tới **First Aid, Boards & Beyond, Pathoma, Sketchy, Physeo, Costanzo, ~98% UWorld**
- **Update rate**: 200,000+ update tích lũy; Update #25 thêm 55,000 note changes
- **Tiền thân**: Zanki → AnKing pickup & maintain

### Tag hierarchy AnKing (thực tế)
Format: `#AK_Step1_v12::#B&B::12_Hematology::03_White_Blood_Cells::01_Acute_Leukemia`
- **Level 1**: deck version (`#AK_Step1_v12`, `#AK_Step2_v12`, `#AK_Step3_v12`)
- **Level 2**: resource (`#B&B`, `#FirstAid`, `#Pathoma`, `#SketchyPharm`, `#UWorld`)
- **Level 3**: organ system (Cardiology, Hematology, **Dermatology**...)
- **Level 4+**: subtopic. Nguồn: https://community.ankihub.net/t/what-do-all-the-tags-mean-in-the-anking-step-deck/134028

### Deck da liễu chuyên biệt

**Dermki Deck (AnKing)** — https://www.ankihub.net/dermki-deck
- Miễn phí (cần AnkiHub account)
- Dựa trên **Review of Dermatology (Alikhan & Hocker)**, ảnh AAD, DermNet, atlas
- Premium optional: 5 USD/tháng, 55 USD/năm, 240 USD lifetime

**Review 3 deck da liễu cho resident** (Hospitalist community):
- **Dolphin Dermatology** — 13,833 cards, VisualDx + DermNet, diverse skin tones. Yếu: không phủ dermatopath. Hiện không download được.
- **vismo_djib Review of Dermatology Anki** — 8,454 cards, color-coded. Yếu: long free-response.
- **AnKingMed Dermki** — 7,889 cards, fill-in-blank, phủ ~75% Alikhan-Hocker, real-time update qua AnkiHub. Yếu: cần premium.

Nguồn: https://community.the-hospitalist.org/content/review-3-comprehensive-anki-flash-card-decks-dermatology-residents

### Image Occlusion Enhanced
- Add-on che một phần label/vùng ảnh, người học đoán
- **Mode phổ biến**: "Hide One, Guess One"
- **Ứng dụng da liễu**: che tên tổn thương/vùng cơ thể trên atlas
- SlideToAnki tạo IO cards từ slide. Nguồn: https://slidetoanki.com/blog/image-occlusion-anki-complete-guide

### Workflow điển hình 1 ngày med student
- **Sáng**: clear all due reviews (200–300 cards/ngày với AnKing full)
- **Tối**: xem lecture / đọc First Aid → unsuspend cards → 20–30 new cards/ngày

## C.3 Chuyên khoa 1 Da liễu tại Hà Nội

### Đại học Y Hà Nội (HMU)
- **Trang chủ SĐH**: https://sdh.hmu.edu.vn — có mục "Bác sĩ CK I"
- **Thông báo tuyển sinh 2025**: https://sdh.hmu.edu.vn/news/tID12008_thong-bao-tuyen-sinh-sau-dai-hoc-nam-2025.html
  - Bậc: Tiến sĩ, Thạc sĩ, CK II, **CK I**, Bác sĩ Nội trú
  - Phát hành hồ sơ 8/4–10/5; đăng ký online 1/5–10/5; nộp hồ sơ giấy **13–14/5/2025**
  - Địa chỉ: số 1 Tôn Thất Tùng, Đống Đa, Hà Nội (Phòng 325)
  - Email: `sdhhotline@hmu.edu.vn`
- ❌ **Chưa xác thực**: Chuyên ngành CK1 có Da liễu cụ thể / Thời gian đào tạo / Điều kiện đầu vào chi tiết / Môn thi tuyển / Học phí

### Học viện Quân y (VMMU)
- Wikipedia: đào tạo SĐH có **CK I và II, thạc sĩ, tiến sĩ, BS nội trú** cả hệ quân sự và dân sự. Nguồn: https://vi.wikipedia.org/wiki/H%E1%BB%8Dc_vi%E1%BB%87n_Qu%C3%A2n_y_(Vi%E1%BB%87t_Nam)
- Da liễu: cần "xác nhận từ giám đốc bệnh viện nơi công tác"
- ❌ **Chưa xác thực**: trang tuyển sinh SĐH chính thức của VMMU liệt kê CK1 Da liễu cụ thể — cần user vào https://vmmu.edu.vn

### Trường Đại học Y Dược — ĐHQGHN
- Website SĐH: https://ump.vnu.edu.vn/acategory-dao-tao-sau-dai-hoc-3379-1.html
- Thành lập 27/10/2020
- ❌ **Chưa xác thực**: có mở CK1 Da liễu chưa

### Bệnh viện Da liễu Trung ương
- **Địa chỉ**: 15A Phương Mai, Kim Liên, Hà Nội — 024.32222944 — `bvdalieutrunguong@gmail.com`
- **Phòng Đào tạo** (https://dalieu.vn/khoa-phong-ban/phong-dao-tao):
  - Phối hợp với **HMU, ĐHQGHN, Học viện Y Dược học cổ truyền VN** cho thực hành sinh viên đại học **và học viên sau đại học** chuyên ngành Da liễu
  - Đào tạo liên tục (CME)
  - **Đào tạo thực hành 18 tháng** có giấy xác nhận
- **Tuyển sinh 2025 các khóa**: https://dalieu.vn/tuyen-sinh-cac-khoa-dao-tao-nam-2025-tai-benh-vien-da-lieu-trung-uong-d4853.html
- Quan trọng: **BV Da liễu TW không cấp bằng CK1** — chỉ là cơ sở thực hành

## C.4 Tài liệu học da liễu

### Sách chuẩn quốc tế
- **Fitzpatrick's Dermatology 9e (2019)** — McGraw-Hill, ISBN 9780071837798. Nguồn: https://accessmedicine.mhmedical.com/book.aspx?bookid=2570
- **Bolognia Dermatology 5e (2024)** — Elsevier, 2717 trang, ISBN 9780702082252. Nguồn: https://shop.elsevier.com/books/dermatology/bolognia/978-0-7020-8225-2
- **Andrews' Diseases of the Skin 13e (2020)** — Elsevier, ~992 trang, ISBN 9780323547536. Nguồn: https://shop.elsevier.com/books/andrews-diseases-of-the-skin/james/978-0-323-54753-6
- **Habif's Clinical Dermatology 7e (2020)** — Elsevier, ISBN 9780323612692

### Atlas ảnh & database lâm sàng
- **DermNet NZ** — https://dermnetnz.org — thành lập **1996** (Amanda Oakley et al., NZ Dermatological Society). **2,500+ topic, 25,000+ ảnh, ~20 triệu reader/năm**
- **VisualDx** — thương mại
- **NIH Open-i** — https://openi.nlm.nih.gov — index từ PMC Open Access
- **Wikimedia Commons — Category:Dermatology** — https://commons.wikimedia.org/wiki/Category:Dermatology — CC BY-SA + CC0

### Tài liệu tiếng Việt & guideline VN
- **Quyết định 4416/QĐ-BYT (06/12/2023)** — "Hướng dẫn Chẩn đoán và điều trị các bệnh Da liễu", Thứ trưởng Trần Văn Thuấn ký, áp dụng toàn quốc. Nguồn: https://luatvietnam.vn/y-te/quyet-dinh-4416-qd-byt-2023-huong-dan-chan-doan-va-dieu-tri-cac-benh-da-lieu-281365-d1.html
- **QĐ 75/QĐ-BYT (2015)** — tiền thân, bị 4416 thay thế
- **Bệnh học Da liễu (3 tập) — ĐHYHN** — GS.TS. Trần Hậu Khang chủ biên
- **Da liễu học (Bộ Y tế)** — PGS.TS Phạm Văn Hiển chủ biên

### Guideline nước ngoài
- **AAD**: https://www.aad.org
- **EADV**: https://www.eadv.org

## C.5 Content pipeline best practice

### DermNet NZ license (ĐỌC KỸ)
Nguồn: https://dermnetnz.org/image-licence
- **Watermarked images**: **CC BY-NC-ND 4.0**
  - Attribution required: "Image sourced from DermNet" + active link
  - **Cấm** commercial, remove watermark, derivative
  - **Cấm** print circulation > 500
  - **Cấm rõ**: "using ... images for the purpose of training or testing artificial intelligence models"
- **Commercial Images**: license riêng, tính phí

Kết luận cho app: hiển thị ảnh watermarked cho user với credit + không AI training → OK; seed vào backend hoặc train model → phải mua commercial license.

### Nguồn ảnh open cho seed
- **Wikimedia Commons — Dermatology**: CC BY-SA / CC0, quy mô nhỏ
- **NIH Open-i**: kế thừa license article gốc (thường CC BY hoặc CC BY-NC)

### ICD-10 codes cho da liễu
- **Block L00-L99** = "Diseases of the skin and subcutaneous tissue"
- URL: https://icd.who.int/browse10/2019/en#/L00-L99
- Sub-block:
  - L00–L08 Nhiễm khuẩn da & mô dưới da
  - L10–L14 Bệnh bóng nước
  - L20–L30 Viêm da và eczema
  - L40–L45 Bệnh sẩn vảy (psoriasis L40)
  - L50–L54 Mày đay & ban đỏ
  - L55–L59 Bệnh da do bức xạ
  - L60–L75 Bệnh phần phụ da (móng, tóc, tuyến)
  - L80–L99 Rối loạn khác

---

# PHẦN D — NHÓM 3: Tech Stack cho App học tập

## D.1 Telegram Bot chi tiết

### BotFather + tạo bot đầu tiên
- **Telegram Bot API**: HTTPS interface đơn giản hoá. Platform host **hơn 10 triệu bot**, **miễn phí cho user**. Bot không tự chủ động — user phải /start trước. Nguồn: https://core.telegram.org/bots
- **BotFather (@BotFather)**: bot chính thức để đăng ký. `/newbot` → nhận token dạng `4839574812:AAFD39kkdpWt3ywyRZergyOLMaJhac60qc`. Nguồn: https://core.telegram.org/bots/tutorial
- **Bảo mật token**: "treat it like a password and don't share it"

### python-telegram-bot library
- Python wrapper, license LGPLv3, "a wrapper you can't refuse". Nguồn: https://python-telegram-bot.org
- Compatible Python 3.10+, hỗ trợ Bot API 10.0, fully asynchronous. Nguồn: https://github.com/python-telegram-bot/python-telegram-bot
- Install: `pip install python-telegram-bot --upgrade`
- Kiến trúc 2 tầng: (1) API wrapper thuần `telegram`; (2) High-level `telegram.ext` với `Application`, `CommandHandler`, `MessageHandler`, `JobQueue`
- **JobQueue** cho reminder daily — natively hỗ trợ
- Version 20+ đã chuyển async/await

### Alternative libraries
- **Aiogram (Python)**: modern async framework. Install: `pip install aiogram`. Nguồn: https://aiogram.dev
- **Telegraf.js (Node.js)**: TypeScript, serverless-compatible. Install: `npm install telegraf`. Nguồn: https://telegraf.js.org

### Use case cho bác sĩ
- **`sendPoll` (quiz mode)** parameters: `chat_id`, `question` (1-300 ký tự), `options`, `type: "quiz"`, `correct_option_ids`, `explanation` (≤200 ký tự — hiện khi trả lời sai), `explanation_media`, `shuffle_options`. Nguồn: https://core.telegram.org/bots/api#sendpoll
- **Reminder daily**: (a) JobQueue của python-telegram-bot; hoặc (b) Supabase pg_cron trigger Edge Function
- **Registration**: user chat `/start` → CommandHandler lưu `update.effective_user.id` → `chat_id`
- **Deep linking**: `https://t.me/bot?start=quiz_id`
- **Inline keyboards**: buttons dưới message, callback không gửi tin vào chat — quiz "Đúng/Sai/Không rõ", flashcard "Nhớ/Quên"

## D.2 Supabase

### Free tier limits (2026, quote chính xác)
Nguồn: https://supabase.com/pricing
- **500 MB database size**
- **5 GB egress** + 5 GB cached
- **1 GB file storage**
- **500,000 Edge Function invocations**
- **50,000 monthly active users**
- **Free projects paused sau 1 tuần không active**
- **Limit 2 active projects**

### Pro tier
- Base **$25/month**
- **8 GB DB disk** included
- **250 GB egress** included
- **100 GB file storage**
- **2 million Edge Function invocations**
- Daily backups 7 ngày

### Postgres
- Table Editor + SQL Editor trên Dashboard. Extensions: pgvector, PostGIS, pg_cron. Daily backup + PITR (paid). Nguồn: https://supabase.com/docs/guides/database

### Auth
- Email/password, magic link/OTP, **19+ social providers**, phone SMS, SAML 2.0 SSO, custom OAuth2/OIDC. JWT-based. Nguồn: https://supabase.com/docs/guides/auth

### Row Level Security (RLS)
Nguồn: https://supabase.com/docs/guides/database/postgres/row-level-security
- **Enable**: `alter table <table_name> enable row level security;`
- **USING vs WITH CHECK**: SELECT/DELETE dùng `USING`; INSERT dùng `WITH CHECK`; UPDATE cần cả 2
- **Cảnh báo null**: `auth.uid()` = null khi chưa login → viết explicit: `"auth.uid() IS NOT NULL AND auth.uid() = user_id"`
- **Perf tips**: Index cột dùng trong policy tăng tốc **99.94%**; wrap `SELECT (select auth.uid())` tăng **99.99%**

### Storage
- 3 loại bucket: **Files** (traditional, có RLS), **Analytics** (Apache Iceberg), **Vector** (AI embeddings, HNSW index)
- **CDN 285+ cities**, image transformation on-the-fly
- **S3 compatible**, RESTful API, TUS resumable uploads

### Edge Functions
- **Deno runtime** (TypeScript first). Deploy globally at edge. Nguồn: https://supabase.com/docs/guides/functions
- Function = `.ts` file export handler
- Local dev: `supabase functions serve`
- Use cases: webhook receiver, image gen, LLM orchestration, transactional email, **messaging bot**
- Constraints: "short-lived, idempotent operations"

### pg_cron
- Supabase Cron = wrapper của pg_cron. Nguồn: https://supabase.com/docs/guides/cron
- Frequency: **every second → yearly**
- 2 kiểu: (a) execute SQL / call DB function; (b) HTTP request → **Supabase Edge Function**
- Tables: `cron.job`, `cron.job_run_details`
- **Guidelines**: ≤ 8 jobs concurrent, mỗi job ≤ 10 phút

### CLI + Secrets
- Install: `npm install supabase --save-dev` hoặc `brew install supabase/tap/supabase`
- Commands: `supabase init`, `supabase start` (local `localhost:54323`), migrations, TypeScript type gen
- Secrets: `supabase secrets set --env-file .env` — **không cần re-deploy**

## D.3 Frontend stack

### Vite
- "The build tool for the web", instant server start, lightning fast HMR. Nguồn: https://vite.dev/
- **Current version: v8.1.4**. Create: `npm create vite@latest`
- Traction: 80,000+ GitHub stars, 80M+ weekly npm downloads. Thay create-react-app (deprecated)

### React 19
- Function components (viết hoa chữ cái đầu), JSX, hooks, props, event handlers, conditional/list rendering
- 2025 recommend: framework/build tool có JSX built-in (Vite, Next.js). Nguồn: https://react.dev/learn

### TanStack Router
- Type-safe router, "the route tree is the application contract". Nguồn: https://tanstack.com/router
- File-based routing (`routes/_app.invoices.$id.tsx`), auto-gen `RouteTree.gen.ts`
- **Search params**: behave like state manager, không phải chuỗi raw
- Route loaders chạy parallel, có cache + prefetch
- Traction: 281.4M downloads

### shadcn/ui
- **Không phải library** — bộ component copy-paste, bạn own code. Nguồn: https://ui.shadcn.com
- Stack: **Radix UI primitives** + **Tailwind CSS**
- Install per-component: `npx shadcn@latest add button`

### Tailwind CSS v4
- Utility-first CSS. Nguồn: https://tailwindcss.com
- v4.3 (2025) features: CSS Variables theme với `@theme`, dark mode `dark:` prefix, container queries `@container`, P3 wide-gamut colors, 3D transforms
- Perf: bundle **< 10kB**, build **2.7x faster**

### PWA
- 3 requirements: **HTTPS**, **manifest.json**, **service worker**. Nguồn: https://web.dev/progressive-web-apps/
- Capabilities: install to home screen, background caching, push notifications, app-like UX mobile
- Case: Clipchamp +97% installs, Rakuten 24 +450% retention

### vite-plugin-pwa
- "Zero-config and framework-agnostic PWA Plugin for Vite" — tự gen SW + manifest. Nguồn: https://vite-pwa-org.netlify.app
- Features: offline (Workbox), auto-update stale-while-revalidate, static asset precaching
- Support: React, Vue 3, Svelte, SvelteKit, SolidJS, Preact, Astro, Nuxt 3, Remix
- **Use case bác sĩ**: trực đêm wifi chập chờn → PWA cache flashcard offline

### TanStack Query
- Server-state manager: caching, deduplication, stale time, background refetch. Nguồn: https://tanstack.com/query
- Install: `npm install @tanstack/react-query`

## D.4 Local scraper pattern

### Vì sao chạy local
- Website nhiều nguồn y khoa dùng **Cloudflare bot protection** — IP Vercel/Supabase bị challenge
- Nhiều site cần login `.edu.vn` hoặc VPN cơ quan
- Rate limit + IP reputation

### Playwright (Python)
- "One API to drive Chromium, Firefox, and WebKit". Nguồn: https://playwright.dev/python
- **Auto-wait + web-first assertions** — không cần sleep()
- Install: `pip install playwright && playwright install`

### curl_cffi (impersonate TLS)
- "Impersonate browsers' TLS/JA3 and HTTP/2 fingerprints" — bypass Cloudflare. Nguồn: https://github.com/lexiforest/curl_cffi
- Install: `pip install curl_cffi --upgrade`

## D.5 Deploy

### Vercel — host frontend PWA
**Hobby (free) tier** — Nguồn: https://vercel.com/pricing
- **100 GB/month** Fast Data Transfer
- **1M/month** Vercel Function invocations
- **1 developer seat** duy nhất
- "Free forever"

**Environment variables** — Nguồn: https://vercel.com/docs/projects/environment-variables
- 3 environments: **Production**, **Preview**, **Development**
- Encrypted at rest
- **Size limit: 64 KB total per deployment**
- Local dev: `.env.local` file, hoặc `vercel env pull`
- Vite: prefix `VITE_` để expose client-side

### Cloudflare R2 — object storage
Nguồn: https://developers.cloudflare.com/r2 + https://developers.cloudflare.com/r2/pricing
- S3-compatible. **Key differentiator: zero egress fees**
- **Free tier**:
  - **10 GB-month storage**
  - **1M Class A operations/month** (writes)
  - **10M Class B operations/month** (reads)
  - **Egress FREE**
- Paid Standard: $0.015/GB-month, $4.50/1M Class A, $0.36/1M Class B
- **Use case bác sĩ**: 10,000 ảnh ca lâm sàng ~ 5 GB → nằm gọn trong free tier R2

### GitHub Actions
- **Public repos**: "The use of standard GitHub-hosted runners is free" — **unlimited minutes**
- **Private repos**: GitHub Free **2,000 phút**; Pro **3,000 phút**; Team **3,000 phút**; Enterprise **50,000 phút**
- **Artifact storage**: Free 500 MB; Pro 1 GB; Team 2 GB; Enterprise 50 GB
- **Cache storage**: 10 GB per repository

---

# PHẦN E — NHÓM 4: Setup máy + Git + Security + Quy trình

## E.1 Cài Claude Code + terminal cơ bản

### System requirements
Nguồn: https://code.claude.com/docs/en/setup
- **OS**: macOS 13.0+, Windows 10 build 1809+, Ubuntu 20.04+, Debian 10+, Alpine 3.19+
- **Hardware**: 4 GB RAM, x64 hoặc ARM64
- **Shell**: Bash, Zsh, PowerShell, CMD
- Cần account Pro / Max / Team / Enterprise / Console — **free plan không truy cập được**

### Install commands
- **macOS / Linux / WSL** (recommend): `curl -fsSL https://claude.ai/install.sh | bash`. Binary tại `~/.local/bin/claude`
- **Windows PowerShell**: `irm https://claude.ai/install.ps1 | iex`
- **Homebrew**: `brew install --cask claude-code` — KHÔNG auto-update
- **WinGet**: `winget install Anthropic.ClaudeCode`
- **npm**: `npm install -g @anthropic-ai/claude-code` (cần Node 22+, KHÔNG `sudo`)

### Kiểm tra + login
- `claude --version` — kiểm tra cài
- `claude doctor` — health check
- `claude` — khởi động interactive, lần đầu mở browser login

### VS Code extension
Nguồn: https://code.claude.com/docs/en/ide-integrations
- Yêu cầu VS Code 1.98.0+
- Cài: Extensions → search "Claude Code" → Install
- Shortcut chính:
  - `Cmd/Ctrl + Esc` — toggle focus editor ↔ Claude
  - `Cmd/Ctrl + Shift + Esc` — mở conversation mới trong tab
  - `Option/Alt + K` — insert @-mention với selection
- **Permission modes**: Manual, Plan, Edit automatically

### WSL2 trên Windows
- Yêu cầu Windows 10 build 19041+ hoặc Windows 11
- PowerShell as Admin: `wsl --install` → restart. Nguồn: https://learn.microsoft.com/en-us/windows/wsl/install
- **Sandboxing của Claude Code**: chỉ WSL 2 support (native Windows và WSL 1 không có)

### Git for Windows
- Optional trên native Windows nhưng recommended
- Download: https://git-scm.com/downloads/win

### Homebrew
- Install: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/brew/install/HEAD/install.sh)"`. Nguồn: https://brew.sh

## E.2 Git cơ bản cho non-developer

### First-time setup
Nguồn: https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup
```
git config --global user.name "Ho Ten"
git config --global user.email you@example.com
git config --global init.defaultBranch main
```

### Workflow tối thiểu
- `git init`, `git status`, `git add <file>`, `git commit -m "..."`, `git log --oneline -10`, `git pull`, `git push`, `git branch <name>` + `git switch -c <name>`

### SSH key setup
Nguồn: https://docs.github.com/en/authentication/connecting-to-github-with-ssh
- Generate: `ssh-keygen -t ed25519 -C "email@example.com"`
- **macOS** — lưu passphrase vào Keychain:
  ```
  eval "$(ssh-agent -s)"
  ssh-add --apple-use-keychain ~/.ssh/id_ed25519
  ```
- Config `~/.ssh/config`:
  ```
  Host github.com
    AddKeysToAgent yes
    UseKeychain yes
    IdentityFile ~/.ssh/id_ed25519
  ```
- Add public key vào GitHub Settings → SSH and GPG keys
- Test: `ssh -T git@github.com`

### Personal Access Token (nếu HTTPS)
Nguồn: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens
- Ưu tiên **Fine-grained PAT** (secure hơn Classic)
- Path: Settings → Developer settings → Personal access tokens → Fine-grained tokens
- Bắt buộc set expiration (1–366 ngày)
- Push code: cần **contents:write**
- Fine-grained max 50 token/account

### .gitignore
Nguồn: https://git-scm.com/docs/gitignore + https://github.com/github/gitignore
- **Chỉ áp dụng cho file CHƯA track**
- Pattern syntax: `*`, `?`, `/`, `**/`, `!pattern` (negate), `#` (comment)
- **Template chính thức**: repo https://github.com/github/gitignore có 150+ template

## E.3 Bảo mật secrets

### 12-Factor Config
Nguồn: https://12factor.net/config
- **"Store config in the environment"** — tách nghiêm ngặt config khỏi code

### Pattern .env + dotenv
- File `.env` chứa `KEY=value`
- **NEVER commit `.env`**
- **Node.js**: `npm install dotenv --save` → `require('dotenv').config()`
- **Python**: `pip install python-dotenv`. Nguồn: https://pypi.org/project/python-dotenv/
- Best practice: `.env.example` (placeholder) commit vào git

### Supabase secrets
Nguồn: https://supabase.com/docs/guides/functions/secrets
```
supabase secrets set OPENAI_API_KEY=sk-...
supabase secrets set --env-file .env
supabase secrets list
```
- Sẵn sàng ngay — không cần redeploy

### Vercel environment variables
- Set ở team hoặc project level; encrypted at rest
- 3 environments: **Production**, **Preview**, **Development**
- Local: `.env.local` hoặc `vercel env pull`
- Limit: 64 KB tổng per-deployment

### GitHub Actions secrets
Nguồn: https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions
- 3 level: **repository**, **environment**, **organization**
- CLI: `gh secret set SECRET_NAME`
- Reference trong workflow: `${{ secrets.MY_SECRET }}`
- Size limit 48 KB
- Best practice: pass via `env:` (không via command-line arguments)

### Xử lý khi leak secret

**Bước 1 — REVOKE ngay**:
- Telegram bot: `/revoke` với @BotFather
- Supabase: Dashboard → Settings → API → Reset service_role
- OpenAI: https://platform.openai.com/api-keys → revoke
- GitHub PAT: Settings → Developer settings → Delete

**Bước 2 — Cập nhật env** với key mới

**Bước 3 — Xoá khỏi git history**:
- **git-filter-repo** (recommend): https://github.com/newren/git-filter-repo
- **BFG Repo-Cleaner**: https://rtyley.github.io/bfg-repo-cleaner/. Ví dụ:
  ```
  bfg --replace-text passwords.txt my-repo.git
  bfg --delete-files id_rsa my-repo.git
  ```
- Sau BFG: `git gc --prune=now --aggressive`
- Force push (cảnh báo collaborator)

**Bước 4 — Notify**:
- GitHub Secret Scanning (public repo auto-scan free): https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning
- Push Protection: https://docs.github.com/en/code-security/secret-scanning/introduction/about-push-protection

## E.4 Quy trình 1 ngày làm việc

### Essential commands
Shell:
- `claude` — mở interactive
- `claude "task"` — one-time
- `claude -p "query"` — one-off headless
- `claude -c` — continue conversation
- `claude -r` — resume và chọn từ list
- `claude doctor` — health check

Session:
- `/help`, `/clear`, `/exit`, `/model`, `/login`, `/init`, `/compact`, `/rewind`, `/usage`, `/permissions`, `/mcp`, `/rename`
- `Shift + Tab` — cycle permission mode
- `Esc` — interrupt Claude giữa chừng
- `Tab` — command completion

### Workflow "Explore → Plan → Implement → Commit"
Nguồn chính thức Anthropic: https://code.claude.com/docs/en/best-practices

1. **Explore (Plan mode)**: `Shift+Tab` → yêu cầu Claude đọc file, KHÔNG sửa
2. **Plan (Plan mode)**: "tôi muốn add feature X. File nào cần đụng?" `Ctrl+G` mở plan trong editor
3. **Implement**: thoát Plan mode → "implement plan. Viết test, chạy suite, fix"
4. **Commit**: "commit với message descriptive và mở PR"

**Note**: Plan có overhead — task nhỏ skip, làm thẳng.

### Fix bug — 4 rule vàng
- **Symptom + likely location + "fixed" là gì**
- **Address root cause, không suppress error**
- **Give Claude a check to run** — test suite, linter, screenshot compare
- Correct Claude 2 lần cho cùng issue → `/clear` và viết prompt mới

### CLAUDE.md
- Chạy `/init` để sinh starter
- Locations: `~/.claude/CLAUDE.md` (mọi session), `./CLAUDE.md` (repo — commit), `./CLAUDE.local.md` (personal — gitignore)
- **Include**: bash command Claude không đoán được, code style khác default, testing instruction, repo etiquette, architectural decision
- **Exclude**: thứ Claude tự đọc code hiểu được, standard convention, "write clean code" self-evident
- Rule check: "Nếu bỏ dòng này Claude có làm sai không?" Không → cắt
- Emphasis: `IMPORTANT` hoặc `YOU MUST` để tăng adherence

### Common failure patterns cần tránh
- **Kitchen sink session**: nhảy giữa nhiều task → context ô nhiễm → `/clear`
- **Correcting over and over**: sửa 2+ lần cùng bug → `/clear` + prompt mới
- **Over-specified CLAUDE.md**: quá dài → Claude ignore → cắt bớt
- **Trust-then-verify gap**: code plausible nhưng không handle edge case
- **Infinite exploration**: `investigate X` không scope → 100 file → dùng subagent hoặc scope hẹp

### Verification-first mindset
- Reviewing evidence nhanh hơn tự chạy verification lại
- Yêu cầu Claude **show evidence**: test output, command return, screenshot
- Bác sĩ non-dev: verify bằng cách gửi `/quiz` cho bot, mở URL app, check message trả về

---

## Tổng danh sách nguồn (~100 URL)

Xem đầy đủ ở [08_tai_lieu_tham_khao.md](08_tai_lieu_tham_khao.md).
