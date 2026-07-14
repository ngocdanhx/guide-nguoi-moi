# 07 — Khởi động dự án Da liễu chuyên khoa 1

> Đây là **bản đồ khởi động** dự án cụ thể trong 2 tháng.
>
> **BẮT BUỘC** đọc [KET_QUA_DU_AN.md](KET_QUA_DU_AN.md) trước để rõ giá trị dự án. Nếu chưa thấy giá trị đủ mạnh, đừng bắt đầu.

## Bước 1 — Vấn đề cụ thể anh chọn giải

Học chuyên khoa 1 Da liễu ở Hà Nội có 3 khó khăn thực tế:

1. **Không có thời gian ôn** — trực nhiều, khám nhiều
2. **Khó nhớ hình ảnh** — hàng trăm bệnh da lâm sàng nhìn giống nhau
3. **Không biết đề thi hỏi gì** — không có kho câu hỏi công khai chuẩn

Tuần đầu cần chọn **1-2 khó khăn** để giải trước — đừng cố giải tất cả cùng lúc.

**Gợi ý cho anh**: chọn **khó khăn 1 + 2** (thời gian + hình ảnh). Xây bot Telegram nhắc thẻ sáng + trang web xem ảnh atlas offline. Khó khăn 3 (kho câu hỏi) đến sau khi đã có 200 thẻ.

---

## Bước 2 — Bức tranh sản phẩm cuối cùng

Sau 2 tháng, anh sẽ có 1 hệ thống chạy tự động. Kịch bản một ngày điển hình:

**Sáng 6h30**:
- Chuông điện thoại reo, mở Telegram
- Bot đã gửi 5 thẻ ôn ưu tiên nhất (thẻ cần ôn nhất trước)
- Anh vừa uống cà phê vừa bấm chọn: "Nhớ / Khó / Quên / Chưa nhớ"
- Trạng thái ôn tự cập nhật vào kho dữ liệu

**Trưa 12h30 nghỉ giữa hai ca**:
- Gõ `/quiz` vào bot
- Bot gửi 5 câu trắc nghiệm ngẫu nhiên từ chương tuần trước
- 3 phút xong, ôn nhẹ

**Tối 20h vào máy tính soạn thẻ mới**:
- Mở trang web (đã cài trên bookmark)
- Đăng nhập, vào trang "Soạn thẻ"
- Nhờ AI đọc 1 chương sách → gợi ý 20 thẻ nháp
- Duyệt từng thẻ, kiểm tra đáp án đúng sách, gắn nhãn bệnh, lưu
- Sáng mai 6h30 bot sẽ dùng thẻ mới này

**Trực đêm bệnh viện, mạng chập chờn**:
- Mở trang web đã lưu sẵn offline
- Xem lại ảnh atlas ca lâm sàng gặp trong đêm
- Ghi ca đó vào ứng dụng cho lần sau

**Cuối tuần Chủ nhật**:
- Bot gửi báo cáo tuần: đã ôn 200 thẻ, nhớ 75%, chuỗi 12 ngày
- Mở web xem bảng theo dõi chi tiết theo nhóm bệnh — biết mình yếu chỗ nào

Đây là mục tiêu cuối. Không phải làm ngay tuần 1.

---

## Bước 3 — Luồng chạy dưới capô (không cần biết code)

Kịch bản "sáng 6h30 nhận thẻ" hoạt động thế nào:

1. **Y tá tự động** (chạy trong kho hồ sơ) đúng 6h30 giờ Việt Nam → gọi phòng xử lý
2. **Phòng xử lý** truy vấn kho hồ sơ — lấy 5 thẻ ưu tiên nhất cho anh
3. Phòng xử lý gọi API Telegram → gửi 5 tin nhắn kèm 4 nút bấm
4. Anh nhận thông báo trên điện thoại
5. Bấm nút "Khó" → Telegram gọi ngược về phòng xử lý khác
6. Phòng xử lý này chạy thuật toán **ôn ngắt quãng** (thuật toán Anki) cập nhật trạng thái mới
7. Ghi lịch sử vào kho hồ sơ để sau có dữ liệu tối ưu

Xong một luồng, khoảng 5 giây. Máy anh không cần chạy gì.

Cùng dữ liệu đó khi anh mở trang web: đăng nhập → thấy tiến độ đã cập nhật vì cùng kho hồ sơ.

---

## Bước 4 — Các món đồ nghề cần dùng (không đi vào chi tiết công nghệ)

| Vai trò | Đại lượng | Miễn phí không? |
|---|---|---|
| Kho hồ sơ chính | Lưu 500-800 thẻ + đăng nhập + lịch tự động | Miễn phí |
| Trang web | Anh làm việc chính, cài vào điện thoại, offline được | Miễn phí |
| Bot Telegram | Nhắc thẻ sáng, quiz trưa, báo cáo tuần | Miễn phí |
| Kho ảnh atlas | Lưu 100-200 ảnh ca lâm sàng | Miễn phí (đến 10GB) |
| AI dev | Claude Code làm việc chính | 300-600 nghìn/tháng |
| AI dev rẻ | MiniMax cho việc đơn giản (đọc sách, dịch) | 100-200 nghìn/tháng |
| Nơi soạn code | VS Code (chương trình soạn thảo) | Miễn phí |

Chi tiết vai trò từng món: xem file [04_tech_stack.md](04_tech_stack.md).

Chi phí tổng trong 2 tháng đầu: **400-800 nghìn**.

---

## Bước 5 — Lộ trình 2 tháng (mỗi tuần 2-3 giờ)

### Tuần 1 — Dựng nền + học lý thuyết

**Mục tiêu**: máy sẵn sàng, hiểu quy trình, có 1 kho code khởi tạo.

- Đọc 10 file trong `guide-nguoi-moi/` (không cần thuộc, đọc để quen)
- Cài Claude Code + Git + đăng ký các tài khoản cần thiết (miễn phí)
- Cài Superpowers (miễn phí, một lệnh)
- Đăng ký MiniMax lấy khoá dùng AI rẻ
- Tạo thư mục dự án tên `da-lieu-hoc-tap`
- Gõ `/init` trong Claude Code để nó sinh file hồ sơ dự án mẫu

**Kết quả cuối tuần**: thư mục dự án có cấu trúc cơ bản, mở Claude Code chạy được.

### Tuần 2 — Dựng trang web phần 1 (khung + đăng nhập)

**Mục tiêu**: trang web hiển thị được, đăng nhập được, có 1 trang trống chờ thẻ.

- Gõ `/speckit.specify` mô tả chức năng "Khung web + đăng nhập + trang danh sách thẻ"
- Claude sinh kế hoạch + hỏi làm rõ, anh trả lời
- Gõ `/speckit.implement` — Claude làm từng phần, dừng để anh xem
- Đưa lên mạng (miễn phí) → có địa chỉ web `dalieu-xxx.xxx`
- Cài vào điện thoại, thử đăng nhập

**Kết quả cuối tuần**: web mở được trên điện thoại, đăng nhập được, có 1 trang trống chờ nội dung.

### Tuần 3 — Trang soạn thẻ + trang ôn tập + nội dung đầu tiên

**Mục tiêu**: có thể soạn thẻ, ôn thẻ, có 50 thẻ thật từ 1 chương sách.

- Trang "Soạn thẻ" — biểu mẫu thêm/sửa, tải ảnh lên
- Trang "Ôn tập" — hiển thị thẻ ưu tiên, 4 nút bấm
- Nhờ MiniMax đọc 1 chương sách (ví dụ chương "Vảy nến") → 50 thẻ nháp
- Duyệt 50 thẻ: kiểm tra đáp án khớp sách, gắn nhãn bệnh, lưu

**Kết quả cuối tuần**: web đủ 3 chức năng chính (danh sách/soạn/ôn), 50 thẻ thật.

### Tuần 4 — Bot Telegram tích hợp

**Mục tiêu**: bot chạy tự động 6h30 sáng, dùng chung dữ liệu với web.

- Viết chức năng "Gửi thẻ ôn sáng" cho phòng xử lý
- Đặt lịch tự động 6h30 giờ Việt Nam
- Xử lý nút bấm rating từ Telegram → cập nhật trạng thái
- Thêm lệnh `/quiz` — 5 câu trắc nghiệm ngẫu nhiên
- Chạy thử: sáng nhận thẻ, bấm rating, mở web thấy dữ liệu đã cập nhật

**Kết quả cuối tuần**: cả bot + web đều cập nhật chung 1 kho dữ liệu.

### Tuần 5 — Bảng theo dõi + hoàn thiện

**Mục tiêu**: hiểu được mình đang học ra sao, sửa lỗi lộ ra sau 1 tuần dùng thật.

- Trang bảng theo dõi: chuỗi ngày, % nhớ, biểu đồ ôn theo tuần
- Sửa lỗi (thường có 3-5 lỗi lộ sau 1 tuần dùng thật)
- Thêm lệnh `/stats` cho bot — báo cáo tuần

### Tuần 6 — Thêm nội dung + kịch bản dùng đủ

**Mục tiêu**: mở rộng nội dung, chuẩn bị vào giai đoạn duy trì.

- Nhờ AI đọc thêm 2-3 chương sách (~150 thẻ mới)
- Duyệt và đưa vào kho
- Thử đủ 5 kịch bản (sáng ôn, trưa quiz, đêm trực xem ảnh, tối soạn, chủ nhật báo cáo)

### Tuần 7-8 — Duy trì và dùng thật

**Mục tiêu**: dùng đủ 2 tuần liên tục để đánh giá xem có phù hợp không.

- Không thêm chức năng lớn
- Chỉ dùng, sửa lỗi nhỏ khi thấy
- Cuối tuần 8: đánh giá — có duy trì lâu dài không? Cần thêm/bỏ gì?

**Sau 2 tháng**: có 1 hệ thống hoàn chỉnh — web + bot + khoảng 500-800 thẻ đã kiểm tra. Từ tháng 3 trở đi → duy trì + thêm nội dung dần theo lịch học.

---

## Bước 6 — Kịch bản giả lập cho 5 tình huống thường gặp

### Kịch bản A — "Sáng dậy trực"

**5h30**: chuông báo thức. Anh dậy chuẩn bị.

**6h00**: đứng bếp uống cà phê. Điện thoại kêu — bot đã gửi 5 thẻ.

**6h05-6h10**: bấm chọn từng thẻ, mỗi thẻ 30 giây. Trong đó có 1 thẻ khó, bấm "Chưa nhớ" → thẻ đó sẽ quay lại sau 15 phút.

**6h20**: bot gửi lại 1 thẻ đó. Bấm "Khó" → sẽ quay lại sau 1 ngày.

**6h30**: đi trực.

**Kết quả**: 5 phút biến thành 5 thẻ ôn có ý nghĩa.

### Kịch bản B — "Trưa giữa hai ca khám"

**12h30**: xong ca khám sáng, ngồi nghỉ ở phòng nội trú.

**12h32**: gõ `/quiz` vào bot. Bot gửi 5 câu trắc nghiệm.

**12h35**: xong quiz. Điểm: 4/5. Bot gửi giải thích câu sai.

**12h40**: đi ăn trưa.

**Kết quả**: 10 phút biến thành 5 lần "chấm" — active recall mạnh nhất.

### Kịch bản C — "Đêm trực, gặp ca hiếm"

**23h**: nhận 1 bệnh nhân ban đỏ đa hình mặt. Chưa rõ chẩn đoán.

**23h05**: mở trang web trên điện thoại. Nhờ mạng chập chờn, chuyển sang chế độ offline — vẫn xem được ảnh atlas đã tải sẵn.

**23h10**: so ảnh atlas → khả năng là hồng ban đa dạng. Tra thêm sách.

**23h30**: chẩn đoán tạm. Ghi ca vào ứng dụng: tình huống, chẩn đoán, cách xử trí.

**Sau đó**: sáng hôm sau khi có mạng, ứng dụng tự đồng bộ. Trong tuần sau, bot tự đưa ca này vào quiz để anh nhớ.

**Kết quả**: ca hiếm không bị lãng quên.

### Kịch bản D — "Tối cuối tuần soạn thẻ"

**Chủ nhật 20h**: rảnh, mở máy tính.

**20h15**: mở trang web, vào "Soạn thẻ". Chọn chương "Viêm da tiếp xúc" trong sách CK1.

**20h20**: nhờ AI đọc chương (dùng MiniMax cho rẻ) → sinh 30 thẻ nháp.

**20h30-21h30**: duyệt từng thẻ. 25 thẻ đúng, 5 thẻ AI bịa → xoá. Còn 25 thẻ, gắn nhãn bệnh + độ khó, lưu.

**Kết quả**: 1,5 giờ có 25 thẻ mới. Sáng mai sẽ vào lịch ôn.

### Kịch bản E — "Chủ nhật xem báo cáo tuần"

**Chủ nhật 21h30**: bot tự gửi tin: "Tuần này bạn ôn 250 thẻ, nhớ 78%, chuỗi 12 ngày. Nhóm bệnh yếu nhất: Vảy nến (62%)."

**21h35**: mở web xem bảng chi tiết. Thấy 8 thẻ vảy nến hay quên. Ghi chú: tuần tới đọc lại chương Vảy nến.

**Kết quả**: biết đường tự điều chỉnh, không học lan man.

---

## Không làm trong 2 tháng đầu

Nghiêm cấm mở rộng phạm vi. Không thêm vào giai đoạn đầu:

- Chia sẻ deck với đồng nghiệp (chỉ 1 người dùng = anh) — làm sau khi ổn định
- Chấm câu tự luận bằng AI — làm giai đoạn sau
- Thông báo đẩy trên web (Telegram bot đã làm rồi)
- Nhập / xuất Anki .apkg — làm giai đoạn 3
- Bình luận / thảo luận (chức năng xã hội)
- Phân quyền nhiều user chi tiết

**Vì sao?** Mỗi chức năng thêm = 1-2 tuần code + 1 tháng bảo trì. Kỳ thi chuyên khoa không đợi. Ưu tiên "chạy được + học được" hơn "đầy đủ chức năng".

---

## Chuyên khoa 1 Da liễu Hà Nội — thông tin thực tế

**Cảnh báo**: các thông tin dưới đây có phần **chưa xác thực** — trường không công khai đủ chi tiết. Anh cần xác nhận trực tiếp với phòng đào tạo sau đại học trước khi quyết định.

### Đại học Y Hà Nội

- Website sau đại học: `sdh.hmu.edu.vn` — có mục "Bác sĩ CK I"
- Tuyển sinh 2025 (tham khảo): phát hành hồ sơ tháng 4-5, đăng ký online tháng 5
- Địa chỉ: số 1 Tôn Thất Tùng, Đống Đa, Hà Nội (Phòng 325)
- Email: `sdhhotline@hmu.edu.vn`
- **CẦN XÁC NHẬN**: chuyên ngành có Da liễu không / thời gian đào tạo / học phí / môn thi

### Học viện Quân y

- Có đào tạo chuyên khoa 1 hệ dân sự
- Với Da liễu: cần xác nhận từ giám đốc bệnh viện nơi công tác
- **CẦN XÁC NHẬN**: website `vmmu.edu.vn` có mở chuyên khoa 1 Da liễu thường xuyên không

### Bệnh viện Da liễu Trung ương (15A Phương Mai)

- **Không cấp bằng chuyên khoa 1** — chỉ là cơ sở thực hành
- Có đào tạo thực hành 18 tháng có giấy xác nhận
- Có đào tạo liên tục
- Website: `dalieu.vn`

---

## Sai lầm cần tránh

### 1. Cầu toàn chức năng trước khi có 1 người dùng thật

Đừng làm 10 chức năng "có vẻ hay" trước khi chính anh dùng bot 1 tuần. Cứ làm bản đầu → dùng thật 2 tuần → thêm chức năng theo nhu cầu thực tế.

### 2. Nội dung do AI tự sinh, không nguồn

Đừng nhờ AI "viết 100 câu hỏi da liễu". Có thể sai fact, bịa liều thuốc. Luôn cho AI đọc sách có sẵn đáp án. Chi tiết ở file 05.

### 3. Không sao lưu trước khi thay đổi lớn

Trước khi đưa hàng loạt thẻ mới vào → luôn tạo bản sao lưu. Xoá 100 thẻ sai chỉ mất 1 lệnh, khôi phục lại mất 1 tuần.

### 4. Đưa mật khẩu vào code

Rotate ngay nếu lỡ đưa lên GitHub. Chi tiết file 06.

### 5. Không dùng khoá bảo mật cho kho hồ sơ

Không bật khoá bảo mật = ai biết địa chỉ kho hồ sơ là đọc được. Luôn bật + viết quy tắc phân quyền.

### 6. Bỏ qua kiểm tra trước khi nói "xong"

Chưa chạy kiểm tra trong chính buổi đó thì không được nói "xong". (Đây là quy tắc thép của Superpowers.)

### 7. File hồ sơ dự án quá dài

Đừng viết file `CLAUDE.md` 500 dòng. AI sẽ bỏ qua. Giữ dưới 200 dòng, chỉ ghi những gì AI không tự đọc từ code được.

---

## Ngày làm việc điển hình (khi đã ổn định sau 6 tuần)

**Sáng 6h30** (tự động): Bot gửi 5 thẻ vào Telegram → anh mở điện thoại uống cà phê → bấm rating.

**7h30 vào bệnh viện**: Nếu trực đêm trước, mở trang web (offline cache) xem lại case gặp đêm qua.

**Trưa 12h30** (5-10 phút): Nếu có 5 phút rảnh, gõ `/quiz` làm 5 câu.

**Chiều tối 20h** (10 phút): Bấm `/stats` xem tiến độ. Nếu weekend, làm quiz dài hơn.

**Chủ nhật** (1-2 giờ): Session dài hơn — thêm thẻ mới, đọc lại chương yếu, plan tuần tới.

---

## Khi bí không biết làm gì

Bí là bình thường trong tháng đầu. 4 nguồn hỗ trợ:

1. **Đọc lại file 01-06** trong `guide-nguoi-moi/` — thường quên khái niệm cơ bản
2. **Đọc [research-notes.md](research-notes.md)** — nghiên cứu chi tiết + URL nguồn
3. **Reddit r/ClaudeAI + r/medicalschoolanki** — cộng đồng lớn, câu hỏi tiếng Anh
4. **Nhờ Claude Code**: chỉ cần nói "tôi bí ở chỗ X" — nó sẽ hỏi làm rõ + giải thích

---

## Bảng kiểm tra "sẵn sàng khởi động"

Trước khi tạo thư mục dự án đầu tiên:

- [ ] Đã đọc [KET_QUA_DU_AN.md](KET_QUA_DU_AN.md) và thấy giá trị đủ mạnh
- [ ] Đã đọc file 01, 02, 03 (khái niệm cơ bản + workflow + setup)
- [ ] Máy đã cài Claude Code, VS Code, Git
- [ ] Có tài khoản: GitHub, Anthropic (nạp 200 nghìn), kho hồ sơ, MiniMax, Telegram bot
- [ ] Bot Telegram đã gửi thử được "Xin chào" (test 1 tin thủ công)
- [ ] Đã chọn 1-2 vấn đề cụ thể (ví dụ: thời gian + hình ảnh)
- [ ] Đã có 1 chương sách CK1 để làm nội dung đầu tiên (ví dụ chương "Vảy nến")
- [ ] Đặt tên dự án: `da-lieu-hoc-tap` (hoặc anh chọn tên khác)
- [ ] Cam kết được 2-3 giờ mỗi tuần trong 2 tháng tới

Nếu ≥ 7/9 tick → khởi động. Chưa đủ → hoàn thiện trước, không vội.

---

## Nếu anh chỉ nhớ được 3 điều

1. **Mục tiêu 2 tháng** — có sản phẩm chạy được với 500-800 thẻ, không cần chức năng đầy đủ
2. **Correct-by-construction** cho nội dung — không để AI chế đáp án y khoa
3. **6 tuần setup + 2 tuần dùng thử** — sau đó đánh giá xem có duy trì lâu dài không

## Đọc tiếp

- Nếu chưa rõ đa AI setup: [09_multi_model_workflow.md](09_multi_model_workflow.md)
- Nếu cần URL nguồn cụ thể: [08_tai_lieu_tham_khao.md](08_tai_lieu_tham_khao.md)
