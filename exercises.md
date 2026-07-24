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
> Temperature càng cao, phản hồi càng có tính sáng tạo, ngẫu nhiên nhưng cũng càng dễ bị lặp từ hoặc bất logic. Ở mức 1.8, phản hồi bắt đầu mất tính mạch lạc rõ rệt, xuất hiện các chuỗi từ vô nghĩa hoặc câu văn không thành cấu trúc. Mức 0.0–0.7 cho phản hồi chuẩn xác, nhất quán nhất.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho trợ lý soạn thảo hợp đồng pháp lý,
và bao nhiêu cho trợ lý viết slogan quảng cáo? Giải thích khác biệt.**
> Hợp đồng pháp lý (Temperature = 0.0): Cần sự chính xác tuyệt đối, nhất quán, logic nghiêm ngặt và không được phép sáng tạo hay bịa đặt.Slogan quảng cáo (Temperature = 0.8 - 1.0): Cần sự mới lạ, độc đáo, khác biệt và bay bổng trong ngôn từ để tạo điểm nhấn thương hiệu

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 20.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 2 lần,
mỗi lần trung bình ~500 token đầu ra.

**Ước tính chi phí mỗi ngày của model lớn so với model nhỏ cho workload này
(dựa trên bảng giá trong template). Nêu một trường hợp model lớn xứng đáng
với chi phí và một trường hợp model nhỏ là lựa chọn đúng:**
> *Câu trả lời của bạn*

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
> Phản hồi 1 (Nhà thơ) mang giọng văn bay bổng, giàu hình ảnh, dùng nghệ thuật ẩn dụ và không chứa thuật ngữ; trong khi Phản hồi 2 (Kỹ sư senior) mang giọng văn súc tích, chuyên nghiệp, sử dụng thuật ngữ chính xác và kèm theo đoạn code minh họa. Từ đó rút ra: System prompt điều khiển được Persona (văn phong/nhập vai), định dạng đầu ra (code/thơ/bullet points), độ sâu kiến thức và mức độ sử dụng thuật ngữ chuyên ngành.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~150 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Nếu dùng ước lượng thô để dự
toán ngân sách API cho ứng dụng tiếng Việt, bạn sẽ dự toán thiếu hay thừa —
và vì sao?**
> Nếu dùng ước lượng thô 0.75, bạn sẽ dự toán THIẾU ngân sách cho ứng dụng tiếng Việt. Nguyên nhân là do các thuật toán Tokenizer (như BPE của OpenAI) được tối ưu cho tiếng Anh; các ngôn ngữ có dấu/đa âm tiết như tiếng Việt thường bị tách thành nhiều mảnh token nhỏ hơn (subwords/bytes).

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Xét ba ứng dụng: (a) chatbot văn bản, (b) trợ lý giọng nói đọc to phản hồi,
(c) pipeline dịch tài liệu chạy ngầm ban đêm. Ứng dụng nào hưởng lợi nhiều
nhất từ streaming, ứng dụng nào không cần — và tại sao?** (1 đoạn văn)
> Chatbot văn bản hưởng lợi nhiều nhất từ streaming vì giúp giảm độ trễ cảm nhận (perceived latency), người dùng thấy chữ xuất hiện ngay lập tức mà không phải chờ full response. (c) Pipeline dịch tài liệu ban đêm hoàn toàn không cần streaming vì chạy batch tự động ngầm, quan trọng là kết quả cuối cùng hoàn chỉnh chứ không có người ngồi chờ giao diện. (Đối với (b) Trợ lý giọng nói, thường cần gom đủ câu hoàn chỉnh rồi mới chuyển Text-to-Speech nên streaming theo từng token lẻ không mang lại nhiều giá trị giao diện).

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**Khi API quá tải và hàng nghìn client cùng retry, exponential backoff giúp
gì so với delay cố định? Tra cứu thêm: kỹ thuật "jitter" (thêm độ trễ ngẫu
nhiên) giải quyết vấn đề gì còn sót lại?**
> Kỹ thuật Jitter: Giải quyết vấn đề "đồng bộ hóa ngẫu nhiên". Nếu hàng nghìn client cùng bị lỗi tại thời điểm T0 và cùng retry theo đúng công thức backoff, chúng sẽ lại dội một lượng request khổng lồ vào server cùng một thời điểm ở các mốc T1,T2. Jitter thêm một khoảng thời gian ngẫu nhiên (ví dụ +±50ms) để rải đều các lượt retry ra

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Viết lại system prompt bạn dùng cho trợ lý của mình. Chỉ ra 2 chỗ trong
prompt mà nếu xóa đi, hành vi trợ lý sẽ thay đổi rõ rệt — và mô tả thay đổi
đó:**
> System Prompt của tôi "Bạn là một trợ lý lập trình Python súc tích. Luôn trả lời bằng Tiếng Việt, tập trung vào code chạy được và KHÔNG giải thích dông dài trừ khi được yêu cầu."Chỗ 1 nếu xóa: "Luôn trả lời bằng Tiếng Việt" → Trợ lý sẽ tự động trả lời bằng tiếng Anh (ngôn ngữ mặc định của các câu hỏi code/tài liệu).Chỗ 2 nếu xóa: "KHÔNG giải thích dông dài trừ khi được yêu cầu" → Trợ lý sẽ sinh ra các đoạn văn giải thích lý thuyết rất dài dòng trước và sau khối code, làm mất tính súc tích.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn giữ history 4 lượt cuối. Hãy mô tả một tình huống hội thoại
cụ thể mà giới hạn này khiến trợ lý trả lời sai/mất ngữ cảnh, và đề xuất một
cách khắc phục (ví dụ: tóm tắt các lượt cũ, tăng giới hạn có chọn lọc...):**
> Tình huống mất ngữ cảnh: Lượt 1: User: "Tôi muốn viết web bằng FastAPI." Lượt 2-5: User hỏi về cách cài đặt, config database, tạo router, deploy Docker...Lượt 6: User hỏi: "Quay lại bước đầu tiên, tôi nên dùng thư viện ORM nào phù hợp nhất cho dự án này?" Hậu quả: Do history đã bị cắt mất 4 lượt đầu (chỉ giữ 4 lượt gần nhất), trợ lý không còn nhớ dự án đang dùng FastAPI và có thể tư vấn sai sang Django ORM hoặc SQLAlchemy chung chung.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên GitHub cá nhân và nộp link repo vào vlearn (theo hướng dẫn README)
