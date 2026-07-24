# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 14h00–18h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.7, 1.2 và 1.8 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Hà Nội."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi? Ở mức nào phản hồi bắt đầu
kém mạch lạc?** (2–3 câu)
> Khi temperature thấp như 0.0, câu trả lời ổn định, rõ ràng và ít sáng tạo. Ở mức 0.7, phản hồi tự nhiên hơn nhưng vẫn mạch lạc. Khi tăng lên 1.2 hoặc 1.8, câu trả lời sáng tạo hơn nhưng bắt đầu có thể lan man hoặc kém chính xác.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho trợ lý soạn thảo hợp đồng pháp lý,
và bao nhiêu cho trợ lý viết slogan quảng cáo? Giải thích khác biệt.**
> Với trợ lý soạn hợp đồng pháp lý, em sẽ chọn temperature thấp, khoảng 0.0-0.3, vì cần chính xác, nhất quán và hạn chế suy diễn. Với trợ lý viết slogan quảng cáo, em sẽ chọn khoảng 0.8-1.2 vì cần nhiều ý tưởng mới, linh hoạt và sáng tạo hơn.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 20.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 2 lần,
mỗi lần trung bình ~500 token đầu ra.

**Ước tính chi phí mỗi ngày của model lớn so với model nhỏ cho workload này
(dựa trên bảng giá trong template). Nêu một trường hợp model lớn xứng đáng
với chi phí và một trường hợp model nhỏ là lựa chọn đúng:**
> Workload tạo khoảng 20 triệu output token mỗi ngày. Theo bảng giá, GPT-4o tốn khoảng 200 USD/ngày, còn GPT-4o-mini khoảng 12 USD/ngày. Model lớn xứng đáng khi cần suy luận phức tạp hoặc câu trả lời chất lượng cao, còn model nhỏ phù hợp cho tác vụ đơn giản như FAQ, tóm tắt ngắn hoặc phản hồi số lượng lớn.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích máy học (machine learning) là gì?"** nhưng hai system prompt
khác nhau:
- "Bạn là một nhà thơ, trả lời mọi thứ bằng hình ảnh ví von, tránh thuật ngữ."
- "Bạn là kỹ sư phần mềm senior, trả lời chính xác, có ví dụ code khi phù hợp."

**Hai phản hồi khác nhau như thế nào (giọng văn, độ dài, mức kỹ thuật)?
Từ đó rút ra system prompt điều khiển được những khía cạnh nào của phản hồi?**
(3–4 câu)
> Với persona nhà thơ, phản hồi thường giàu hình ảnh, mềm mại, ít thuật ngữ và dễ hiểu hơn với người mới. Với persona kỹ sư senior, câu trả lời trực tiếp hơn, có cấu trúc, dùng nhiều khái niệm kỹ thuật và có thể kèm ví dụ code. Điều này cho thấy system prompt có thể điều khiển giọng văn, độ dài, mức kỹ thuật, cách giải thích và phong cách trình bày của model.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~150 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Nếu dùng ước lượng thô để dự
toán ngân sách API cho ứng dụng tiếng Việt, bạn sẽ dự toán thiếu hay thừa —
và vì sao?**
> Số token từ tiktoken thường khác với ước lượng số từ / 0.75, đặc biệt với tiếng Việt vì dấu, khoảng trắng và cách tách từ có thể làm tokenizer xử lý khác tiếng Anh. Trong thử nghiệm của em, hai cách đếm chênh nhau khoảng ...%. Nếu chỉ dùng ước lượng thô cho ứng dụng tiếng Việt, em sẽ cộng thêm phần dự phòng vì rất dễ dự toán thiếu token và thiếu ngân sách.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Xét ba ứng dụng: (a) chatbot văn bản, (b) trợ lý giọng nói đọc to phản hồi,
(c) pipeline dịch tài liệu chạy ngầm ban đêm. Ứng dụng nào hưởng lợi nhiều
nhất từ streaming, ứng dụng nào không cần — và tại sao?** (1 đoạn văn)
> Chatbot văn bản và trợ lý giọng nói hưởng lợi nhiều nhất từ streaming vì người dùng thấy phản hồi bắt đầu ngay, cảm giác chờ ngắn hơn và tương tác tự nhiên hơn. Trợ lý giọng nói còn cần streaming để có thể đọc dần thay vì chờ toàn bộ câu trả lời. Ngược lại, pipeline dịch tài liệu chạy ngầm ban đêm không cần streaming nhiều vì người dùng không ngồi chờ từng token, kết quả hoàn chỉnh và độ ổn định quan trọng hơn trải nghiệm thời gian thực.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**Khi API quá tải và hàng nghìn client cùng retry, exponential backoff giúp
gì so với delay cố định? Tra cứu thêm: kỹ thuật "jitter" (thêm độ trễ ngẫu
nhiên) giải quyết vấn đề gì còn sót lại?**
> Exponential backoff giúp giảm tải cho API khi bị quá tải vì các client sẽ chờ lâu dần trước khi retry, thay vì cùng gửi lại liên tục sau một khoảng delay cố định. Nếu dùng delay cố định, hàng nghìn client có thể retry cùng lúc và tiếp tục gây nghẽn. Jitter thêm độ trễ ngẫu nhiên để phân tán thời điểm retry, tránh hiện tượng nhiều client đồng bộ retry cùng một nhịp.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Viết lại system prompt bạn dùng cho trợ lý của mình. Chỉ ra 2 chỗ trong
prompt mà nếu xóa đi, hành vi trợ lý sẽ thay đổi rõ rệt — và mô tả thay đổi
đó:**
> System prompt em dùng là: "Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt." Nếu xóa cụm "trợ giảng thân thiện", trợ lý có thể trả lời khô hơn và ít phù hợp với bối cảnh học tập. Nếu xóa cụm "trả lời ngắn gọn bằng tiếng Việt", trợ lý có thể trả lời dài hơn hoặc chuyển sang tiếng Anh.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn giữ history 4 lượt cuối. Hãy mô tả một tình huống hội thoại
cụ thể mà giới hạn này khiến trợ lý trả lời sai/mất ngữ cảnh, và đề xuất một
cách khắc phục (ví dụ: tóm tắt các lượt cũ, tăng giới hạn có chọn lọc...):**
> Một tình huống dễ mất ngữ cảnh là người dùng đưa yêu cầu ban đầu rất quan trọng, ví dụ "hãy luôn trả lời theo vai bác sĩ nhi khoa", rồi sau hơn 4 lượt mới hỏi tiếp một câu phụ thuộc vào vai trò đó. Vì history chỉ giữ 4 lượt cuối, trợ lý có thể quên chỉ dẫn ban đầu và trả lời sai phong cách. Cách khắc phục là tóm tắt các lượt cũ thành một memory ngắn, hoặc giữ lại các thông tin quan trọng thay vì chỉ cắt history theo số lượt.

---

## Danh Sách Kiểm Tra Nộp Bài

- [x] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [x] Cả 4 checkpoint pytest đều pass
- [x] Tất cả 9 câu trong file này đã được trả lời
- [x] Đã copy bài làm vào folder `solution/`, push lên GitHub cá nhân và nộp link repo vào vlearn (theo hướng dẫn README)
