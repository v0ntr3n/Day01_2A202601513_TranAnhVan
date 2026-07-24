# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay các dòng placeholder bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature thấp như 0.0, phản hồi thường ổn định, trực tiếp và ít thay
> đổi nếu gọi lại nhiều lần. Khi tăng lên 0.5 rồi 1.0, cách diễn đạt linh hoạt
> và phong phú hơn; ở mức 1.5, câu trả lời sáng tạo hơn nhưng cũng dễ lan man
> hoặc kém nhất quán hơn. Quy luật chung là temperature càng cao thì độ ngẫu
> nhiên tăng, còn tính dự đoán và độ an toàn của câu trả lời giảm.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt khoảng 0.2-0.3. Chatbot hỗ trợ khách hàng cần trả lời nhất quán,
> bám chính sách và hạn chế bịa thông tin hơn là sáng tạo. Mức này vẫn đủ tự
> nhiên để câu trả lời không quá máy móc, nhưng giảm rủi ro đưa ra hướng dẫn
> sai.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Workload tạo khoảng 10.000 x 3 x 350 = 10.500.000 token đầu ra mỗi ngày.
> Theo bảng giá output trong `template.py`, GPT-4o tốn khoảng
> 10.500 x 0.010 = 105 USD/ngày, còn GPT-4o-mini khoảng
> 10.500 x 0.0006 = 6,30 USD/ngày, tức GPT-4o đắt hơn khoảng 16,7 lần. GPT-4o
> xứng đáng cho các ca cần lập luận sâu như xử lý khiếu nại phức tạp hoặc phân
> tích tài liệu quan trọng; GPT-4o-mini phù hợp cho FAQ, phân loại ý định, hoặc
> trả lời các câu hỏi lặp lại có rủi ro thấp.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với persona giáo viên tiểu học, phản hồi ngắn hơn, dùng từ đơn giản và ví dụ
> gần gũi như cuốn sổ hoặc các khối nối tiếp nhau để trẻ dễ hình dung. Với
> persona chuyên gia tài chính, phản hồi dài và kỹ thuật hơn, có các khái niệm
> như sổ cái phân tán, đồng thuận, bất biến dữ liệu, tài sản số hoặc smart
> contract. System prompt không thay đổi câu hỏi của người dùng, nhưng định
> hướng giọng văn, mức độ chi tiết, loại ví dụ và lượng thuật ngữ mà model ưu
> tiên sử dụng.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Với đoạn tiếng Việt mẫu 100 từ, cách ước lượng `số từ / 0.75` cho khoảng
> 133,33 token, còn `tiktoken` đếm được 120 token, chênh khoảng 10%. Kết quả
> cụ thể có thể đổi theo đoạn văn, nhưng điểm quan trọng là token không trùng
> với từ: tiếng Việt có dấu, nhiều âm tiết tách bằng khoảng trắng và một số từ
> có thể bị tách thành nhiều mảnh token. Vì vậy dùng `tiktoken` đáng tin cậy
> hơn nhiều so với chỉ lấy số từ để dự đoán chi phí.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất khi phản hồi dài hoặc người dùng cần cảm giác hệ
> thống đang làm việc ngay, ví dụ chatbot tư vấn, trợ lý viết nội dung, giải
> thích bài học hoặc sinh báo cáo nhiều đoạn. Khi token hiện dần, thời gian chờ
> cảm nhận được giảm dù tổng thời gian sinh có thể không đổi. Non-streaming phù
> hợp hơn khi phản hồi ngắn, cần xử lý trọn vẹn trước khi hiển thị, cần parse
> JSON ổn định, hoặc phải kiểm duyệt/validate toàn bộ output trước khi đưa cho
> người dùng.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff làm khoảng cách giữa các lần thử tăng dần, nên client
> giảm áp lực lên API khi hệ thống đang quá tải và cho server thêm thời gian
> hồi phục. Nếu tất cả client đều retry với delay cố định giống nhau, chúng có
> thể cùng gửi lại request vào đúng một nhịp, tạo thành các đợt tải mới và làm
> lỗi kéo dài hơn. Backoff, đặc biệt nếu thêm jitter, giúp phân tán thời điểm
> retry và tăng khả năng lần thử sau thành công.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Tôi chọn persona trợ giảng thân thiện cho khóa AI. System prompt:
> "Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt,
> ưu tiên giải thích từng bước và dùng ví dụ dễ hiểu khi người học hỏi về code,
> API hoặc khái niệm LLM." Tôi yêu cầu "trả lời ngắn gọn" để câu trả lời phù
> hợp với CLI và không tốn quá nhiều token. Tôi chỉ định "bằng tiếng Việt" vì
> người dùng trong lab đang học bằng tài liệu tiếng Việt, nên phản hồi cùng
> ngôn ngữ sẽ dễ theo dõi hơn.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là history chỉ giữ 3 lượt gần nhất, nên trợ lý có thể quên
> thông tin quan trọng ở đầu cuộc trò chuyện. Một cải thiện cụ thể là thêm bộ
> nhớ tóm tắt: khi history vượt quá 6 message, dùng model tạo một bản tóm tắt
> ngắn các ý quan trọng rồi lưu vào biến `conversation_summary` hoặc file cục
> bộ. Ở mỗi lượt sau, đưa bản tóm tắt này vào messages như một context message
> trước history gần nhất để giữ ngữ cảnh dài hạn mà vẫn kiểm soát chi phí token.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
