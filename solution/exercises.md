# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature thấp như 0.0, phản hồi thường ổn định, trực tiếp và ít thay đổi giữa các lần gọi. Ở mức 0.5, câu trả lời vẫn khá nhất quán nhưng bắt đầu linh hoạt hơn trong cách diễn đạt. Khi tăng lên 1.0 hoặc 1.5, câu trả lời có xu hướng đa dạng và sáng tạo hơn, nhưng cũng dễ lan man hoặc kém nhất quán hơn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature khoảng 0.2–0.4 cho chatbot hỗ trợ khách hàng. Chatbot cần trả lời chính xác, nhất quán theo chính sách và không nên tự sáng tạo thông tin; mức này vẫn giúp lời văn tự nhiên, thân thiện.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Mỗi ngày có 10.000 × 3 × 350 = 10.500.000 token đầu ra. Theo bảng giá, GPT-4o đắt hơn GPT-4o-mini khoảng 16,7 lần cho output token. GPT-4o phù hợp khi cần phân tích chuyên sâu hoặc xử lý câu hỏi phức tạp; GPT-4o-mini phù hợp cho FAQ, phân loại yêu cầu và chatbot có lượng truy cập lớn.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với persona giáo viên tiểu học, câu trả lời thường ngắn, dùng từ đơn giản và có ví dụ gần gũi, như ví blockchain là một cuốn sổ chung. Với persona chuyên gia tài chính, câu trả lời sẽ chuyên sâu hơn, chứa các thuật ngữ như sổ cái phân tán, cơ chế đồng thuận và tính bất biến. System prompt định hướng vai trò, giọng điệu, đối tượng người đọc và mức độ chi tiết của model. Vì vậy, cùng một câu hỏi nhưng phản hồi có thể rất khác nhau.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Khi đếm một đoạn tiếng Việt 134 từ, tiktoken cho kết quả 157 token, trong khi công thức số từ / 0.75 ước lượng khoảng 179 token. Hai kết quả chênh lệch khoảng 12,1%. Tiếng Việt thường tốn token do dấu thanh và các ký tự có dấu có thể bị tách thành nhiều token hơn so với nhiều từ tiếng Anh phổ biến.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng khi phản hồi dài hoặc mất nhiều thời gian tạo, ví dụ chatbot tư vấn, sinh code hay tóm tắt tài liệu, vì người dùng thấy kết quả ngay thay vì chờ toàn bộ câu trả lời. Điều này làm giảm cảm giác chờ đợi và người dùng có thể dừng khi đã đủ thông tin. Non-streaming phù hợp khi hệ thống cần nhận toàn bộ kết quả trước để kiểm tra JSON, kiểm duyệt nội dung hoặc lưu dữ liệu.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm áp lực lên API đang quá tải bằng cách chờ lâu dần sau mỗi lần lỗi, tạo thời gian để hệ thống phục hồi. Nếu hàng nghìn client đều retry sau đúng một giây, chúng có thể đồng thời gửi lại request và tạo thêm một đợt quá tải. Trong thực tế nên thêm jitter, tức một khoảng chờ ngẫu nhiên nhỏ, để các lần retry được phân tán.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona tôi chọn: "Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt, ưu tiên ví dụ thực tế và nêu rõ khi không chắc chắn." Cụm "trả lời ngắn gọn" giúp phản hồi dễ đọc trong CLI và giảm chi phí token. Cụm "nêu rõ khi không chắc chắn" giúp hạn chế việc model trả lời quá tự tin khi thiếu thông tin.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn là trợ lý chỉ lưu ba lượt hội thoại gần nhất nên có thể quên thông tin quan trọng ở đầu cuộc trò chuyện. Có thể cải thiện bằng cách tóm tắt lịch sử cũ thành một đoạn ngắn khi history sắp vượt giới hạn, rồi gửi đoạn tóm tắt đó cùng các tin nhắn mới nhất. Cách này giúp giữ được ngữ cảnh dài hơn nhưng vẫn kiểm soát số token và chi phí.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
