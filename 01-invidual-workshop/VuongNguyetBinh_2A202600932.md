# Workshop - Mổ App AI Thật

**Họ tên:** Vương Nguyệt Bình  
**MSV:** 2A202600932  
**Sản phẩm chọn:** MoMo - Moni / chatbot trợ thủ tài chính với AI  
**Workflow teardown:** User muốn biết mình đang tiêu nhiều nhất vào khoản nào và nên làm gì tiếp theo  
**Output:** finding note + sketch `as-is / to-be`

## 1. Chọn một sản phẩm để dùng thử

| Sản phẩm | AI feature | Cách truy cập |
|---|---|---|
| MoMo - Moni | Chatbot / trợ thủ tài chính với AI, hỗ trợ quản lý chi tiêu và gợi ý tài chính cá nhân | App MoMo |

**Workflow chọn:** User đã có lịch sử giao dịch trong MoMo và hỏi chatbot: "Tôi đang tiêu nhiều nhất ở khoản nào?"

**Lý do chọn:** Đây là workflow phù hợp để kiểm tra giá trị thật của AI trong app tài chính. Nếu chỉ trả lời lời khuyên chung, chatbot không khác nhiều một bài blog tài chính. Giá trị của Moni nằm ở việc đọc dữ liệu giao dịch, tổng hợp thành insight, nói rõ độ tin cậy, và đưa user đến hành động tiếp theo như xem giao dịch, đặt ngân sách, hoặc sửa phân loại.

## 2. Dùng thử: promise vs reality

**Product hứa gì?**

MoMo định vị ứng dụng là "Trợ thủ Tài chính với AI", giúp người dùng làm được nhiều hơn với tiền. Các nội dung giới thiệu của MoMo nhấn mạnh việc AI hỗ trợ cá nhân hóa trải nghiệm, quản lý chi tiêu, tối ưu hoạt động tài chính và giúp người dùng hiểu hơn về tài chính cá nhân.

Nguồn tham chiếu:

- https://www.momo.vn/
- https://www.momo.vn/tro-thu-tai-chinh
- https://www.momo.vn/hoi-dap/momo-la-gi

**User nào được hứa sẽ được giúp?**

User là người dùng MoMo phổ thông, có nhiều giao dịch nhỏ trong ngày và muốn quản lý tiền đơn giản hơn. Người này không muốn tự mở lịch sử giao dịch, tự cộng tiền, tự phân nhóm và tự kết luận. User kỳ vọng chatbot có thể đọc hiểu dữ liệu sẵn có trong MoMo và nói ngắn gọn: khoản nào đang chi nhiều, vì sao, và nên làm gì.

**Kỳ vọng AI làm được task nào?**

Trong workflow này, Moni nên làm được 5 việc:

1. Hiểu intent: user đang hỏi "nhóm chi tiêu nào cao nhất".
2. Truy xuất dữ liệu giao dịch MoMo trong một khoảng thời gian rõ ràng.
3. Phân nhóm chi tiêu và chỉ ra nhóm lớn nhất.
4. Nói rõ nguồn dữ liệu, mốc thời gian và độ tin cậy.
5. Đề xuất hành động tiếp theo: xem chi tiết, đặt ngân sách, nhắc khi vượt mức, hoặc sửa phân loại nếu bot nhầm.

**Reality quan sát được**

Moni phù hợp với các câu hỏi tài chính có công thức rõ, ví dụ "tôi nên tiết kiệm bao nhiêu với lương 8 triệu". Bot có thể đưa ra gợi ý 10%-20% thu nhập và quy đổi thành số tiền cụ thể. Tuy nhiên, khi user hỏi về phân tích chi tiêu cá nhân, điểm gãy xuất hiện: bot có xu hướng trả về tổng chi tiêu/số giao dịch hoặc hướng user sang báo cáo chi tiêu, thay vì trả lời trực tiếp nhóm chi nào đang lớn nhất và action tiếp theo.

**Prompt/input đã thử và hành vi quan sát được**

| Prompt | Kỳ vọng của user | Hành vi/observation | Path liên quan |
|---|---|---|---|
| "Tôi nên tiết kiệm bao nhiêu mỗi tháng với lương 8 triệu?" | Bot tính nhanh mức tiết kiệm phù hợp và gợi ý tạo mục tiêu | Bot có thể gợi ý 10%-20% thu nhập, tương đương 800.000đ-1.600.000đ/tháng. Câu trả lời đúng nhưng action tạo mục tiêu chưa thật rõ. | Happy |
| "Tôi đang tiêu quá nhiều ở khoản nào?" | Bot chỉ ra nhóm chi cao nhất, tỉ lệ, số giao dịch và đề xuất kiểm soát | Bot có dữ liệu giao dịch nhưng câu trả lời có thể dừng ở tổng chi tiêu/số giao dịch hoặc yêu cầu user xem báo cáo. User chưa biết khoản nào cần giảm. | Failure |
| "Khoản ăn uống này bị phân loại sai, đây là tiền học phí." | Bot cho sửa category và ghi nhớ correction | Chưa thấy correction path rõ ràng ngay trong hội thoại: user phải tự sửa ngoài flow hoặc bỏ qua. | Correction |
| "Dạo này tôi tiêu hơi nhiều đúng không?" | Bot hỏi lại mốc so sánh: tuần này/tháng này/30 ngày gần nhất/so với ngân sách | Nếu bot đoán mốc thời gian, user không biết kết luận dựa trên tiêu chí nào. Cần low-confidence path tốt hơn. | Low-confidence |

**Evidence cụ thể**

```text
Prompt chính: "Tôi đang tiêu quá nhiều ở khoản nào?"

Observation:
Đây là câu hỏi có giá trị cao vì user không chỉ cần xem tổng chi tiêu, mà cần biết nhóm chi nào đang vượt mức. Nếu chatbot chỉ trả tổng tiền, số giao dịch, hoặc hướng user sang báo cáo, AI chưa biến dữ liệu giao dịch thành insight có thể hành động.
```

## 3. Vẽ 4 paths

### Happy path - AI đúng và tự tin

```text
User:
"Tôi nên tiết kiệm bao nhiêu mỗi tháng với lương 8 triệu?"

Moni:
Nhận diện intent = tư vấn tiết kiệm theo thu nhập

Moni:
Áp dụng guideline 10%-20% thu nhập

Moni:
Trả lời:
"Bạn có thể tiết kiệm khoảng 800.000đ-1.600.000đ/tháng."

User thấy:
Câu trả lời đúng câu hỏi, có con số cụ thể, dễ hiểu.

Status:
Một phần đạt. Cần thêm quick action tạo mục tiêu tiết kiệm ngay trong flow.
```

**Nhận xét:** Happy path tốt khi câu hỏi có công thức rõ và không cần truy xuất/phân tích sâu dữ liệu cá nhân. Điểm thiếu là hành động tiếp theo chưa đủ mạnh.

### Low-confidence path - AI không chắc vì câu hỏi mơ hồ

```text
User:
"Dạo này tôi tiêu hơi nhiều đúng không?"

Moni:
Nhận diện intent = đánh giá chi tiêu

Vấn đề:
"Dạo này" chưa rõ là tuần này, tháng này hay 30 ngày gần nhất.
"Tiêu nhiều" chưa rõ so với ngân sách, tháng trước hay thói quen cá nhân.

As-is:
Moni có thể đoán mốc thời gian hoặc đưa lời khuyên chung.

User thấy:
Không biết kết luận dựa trên mốc nào, nên khó tin và khó hành động.

Status:
Chưa đạt nếu bot không hỏi lại.
```

**Low-confidence path nên có:**

```text
Moni:
"Bạn muốn mình so sánh chi tiêu theo mốc nào?"

Quick options:
[Tuần này]
[Tháng này]
[30 ngày gần nhất]
[So với tháng trước]
[So với ngân sách]
```

### Failure path - AI có dữ liệu nhưng chưa tạo insight

```text
User:
"Tôi đang tiêu quá nhiều ở khoản nào?"

Moni:
Truy xuất dữ liệu giao dịch

As-is:
Bot chỉ trả tổng chi tiêu/số giao dịch hoặc điều hướng user sang báo cáo chi tiêu.

Điểm gãy:
User hỏi "khoản nào", nhưng bot chưa chỉ ra category lớn nhất, tỉ lệ, xu hướng tăng/giảm, và hành động tiếp theo.

User thấy:
Vẫn phải tự mở báo cáo, tự đọc giao dịch, tự kết luận khoản nào cần giảm.

Status:
Không đạt. Đây là path yếu nhất.
```

**Lý do path này quan trọng:** MoMo có lợi thế là dữ liệu giao dịch thật. Nếu Moni không chuyển dữ liệu đó thành insight, vai trò "trợ thủ tài chính với AI" bị giảm xuống thành chatbot hỏi đáp chung.

### Correction path - User sửa phân loại / phản hồi sai

```text
User:
"Khoản này không phải ăn uống, đây là học phí."

As-is:
Chưa thấy rõ một correction flow ngay trong chatbot.
User có thể phải tự tìm cách sửa category trong màn hình khác, hoặc bỏ qua.

Điểm gãy:
Nếu bot phân loại sai mà không có nút sửa/ghi nhớ, insight lần sau vẫn có thể sai tiếp.

User thấy:
AI không học từ correction của mình, làm giảm trust.

Status:
Chưa đạt. Cần có correction path rõ ràng.
```

**Correction path nên có:**

```text
Moni:
"Mình đã đổi giao dịch này từ 'Ăn uống' sang 'Học phí'. Bạn có muốn áp dụng cách phân loại này cho các giao dịch tương tự trong tương lai không?"

Options:
[Áp dụng lần sau]
[Chỉ sửa giao dịch này]
[Hoàn tác]
```

## 4. Viết finding thành quyết định

```text
Khi user hỏi "Tôi đang tiêu quá nhiều ở khoản nào?",
Moni có thể truy xuất dữ liệu giao dịch nhưng chưa trả lời trực tiếp bằng insight về nhóm chi tiêu cao nhất, tỉ lệ, xu hướng và hành động tiếp theo,
hậu quả là user vẫn không biết khoản nào cần kiểm soát và phải tự mở báo cáo chi tiêu để tự phân tích.
Lỗi thuộc layer data-tool + intent + UX recovery: AI có dữ liệu nhưng chưa chuyển dữ liệu thành quyết định tài chính có thể hành động.
Nên sửa bằng Insight + Action Card: Moni phải hiện nhóm chi cao nhất, mốc thời gian, nguồn dữ liệu, confidence, nút xem giao dịch, nút đặt ngân sách, nút sửa phân loại và fallback khi dữ liệu không đủ.
```

**Câu đổi SPEC:**

```text
Finding này làm SPEC cần thêm requirement "AI output phải có insight + action + recovery", không chỉ trả lời text. Với sản phẩm chatbot/roadmap AI của nhóm, mỗi gợi ý của AI cần có confidence, lý do, nguồn dữ liệu/nguồn học, fallback khi không chắc và feedback loop để user sửa.
```

## 5. Sketch as-is / to-be

| As-is | To-be |
|---|---|
| User hỏi: "Tôi đang tiêu quá nhiều ở khoản nào?" | User hỏi: "Tôi đang tiêu quá nhiều ở khoản nào?" |
| Moni nhận intent hỏi về chi tiêu. | Moni nhận intent hỏi về nhóm chi tiêu cao nhất. |
| Moni truy xuất một phần dữ liệu giao dịch. | Moni lấy dữ liệu giao dịch theo mốc tháng này/30 ngày gần nhất. |
| Moni trả tổng chi tiêu, số giao dịch, hoặc hướng user sang báo cáo. | Moni phân nhóm giao dịch và tính nhóm chi cao nhất. |
| Điểm gãy: user vẫn chưa biết khoản nào đang vượt mức. | Moni hiện Insight Card: category, số tiền, tỉ lệ, so với tháng trước, confidence. |
| User phải tự mở báo cáo và tự kết luận. | Moni đưa quick actions: xem giao dịch, đặt ngân sách, nhắc khi vượt mức, sửa phân loại. |
| Nếu bot phân loại sai, correction không rõ trong hội thoại. | User sửa category ngay trong chat; Moni lưu correction và cho hoàn tác. |

### Sketch bằng flow

```text
AS-IS

User hỏi khoản nào tiêu nhiều
        ↓
Moni đọc intent chung về chi tiêu
        ↓
Moni trả tổng chi tiêu / số giao dịch / link báo cáo
        ↓
User vẫn phải tự xem báo cáo
        ↓
Điểm gãy: không có insight, không có action, không có correction
```

```text
TO-BE

User hỏi khoản nào tiêu nhiều
        ↓
Moni hỏi lại nếu thiếu mốc thời gian
        ↓
Moni phân tích giao dịch theo category
        ↓
Moni hiện Insight Card:
- Nhóm chi cao nhất
- Số tiền và tỉ lệ
- So với tháng trước/ngân sách
- Nguồn dữ liệu và confidence
        ↓
Moni đề xuất Action Card:
[Xem giao dịch]
[Đặt ngân sách]
[Nhắc khi vượt mức]
[Sửa phân loại]
        ↓
Nếu user sửa:
Moni lưu correction + cho hoàn tác
        ↓
Nếu dữ liệu không đủ:
Moni nói rõ giới hạn + chuyển sang báo cáo chi tiêu / hướng dẫn thêm dữ liệu
```

## 6. Product decision

**Quyết định sản phẩm:** Triển khai **Insight + Action Card** cho chatbot Moni trong các câu hỏi phân tích chi tiêu.

Thay vì chỉ trả lời bằng text hoặc điều hướng user sang báo cáo, Moni nên trả về một card có cấu trúc:

| Thành phần | Nội dung cần có |
|---|---|
| Direct answer | Nhóm chi tiêu cao nhất là gì |
| Evidence | Số tiền, số giao dịch, mốc thời gian |
| Confidence | Bot chắc đến mức nào và vì sao |
| Explanation | Vì sao đây là khoản đang cần chú ý |
| Action | Xem chi tiết, đặt ngân sách, nhắc khi vượt mức |
| Recovery | Sửa phân loại, hoàn tác, hỏi lại khi dữ liệu thiếu |

**Ví dụ output to-be:**

```text
Tháng này bạn chi nhiều nhất cho "Ăn uống".

Số tiền: 1.500.000đ
Tỉ lệ: 38% tổng chi tiêu qua MoMo
Số giao dịch: 23
Xu hướng: tăng 25% so với tháng trước
Confidence: 82% vì 3 giao dịch có thể đang bị phân loại chưa chắc chắn

Gợi ý:
[Xem 23 giao dịch]
[Đặt ngân sách ăn uống 1.200.000đ]
[Nhắc tôi khi vượt 80% ngân sách]
[Sửa phân loại]
```

**Lý do quyết định:**

- Với user: giảm công tự phân tích, biết ngay khoản nào cần kiểm soát, có hành động tiếp theo rõ ràng.
- Với MoMo: tăng giá trị của Moni từ chatbot hỏi đáp thành trợ lý tài chính có khả năng chuyển dữ liệu thành hành động.
- Với AI product: có đủ 4 paths, bao gồm happy, low-confidence, failure và correction.

## 7. Liên hệ với SPEC của nhóm

Bài học từ Moni có thể áp dụng trực tiếp vào SPEC của nhóm **AI Learning Path Personalizer**:

```text
Không nên để AI chỉ trả lời bằng một đoạn text dài.
AI cần biến input của user thành một output có cấu trúc, có confidence, có lý do, có action tiếp theo và có đường sửa sai.
```

Cụ thể, SPEC của nhóm nên giữ các yêu cầu sau:

| Bài học từ Moni | Áp dụng vào AI Learning Path Personalizer |
|---|---|
| User cần insight, không chỉ cần dữ liệu thô | Roadmap phải có milestone ưu tiên, không chỉ list tài liệu |
| Câu hỏi mơ hồ cần low-confidence path | Nếu user nói "tôi muốn học AI" quá chung, bot phải hỏi thêm/quiz |
| AI sai phân loại cần correction | User phải sửa được level, milestone, tài liệu sai |
| Cần nói rõ nguồn và confidence | Roadmap phải có confidence, lý do gợi ý, nguồn học whitelist |
| Cần action tiếp theo | Sau roadmap phải có việc cần làm ngay: học module nào, trong bao lâu, bài tập gì |

## 8. Tự kiểm trước khi nộp

- [X] Có ít nhất 1 observation cụ thể.
- [X] Có đủ 4 paths: Happy, Low-confidence, Failure, Correction.
- [X] Finding được viết thành product decision, không chỉ là nhận xét.
- [X] Sketch có as-is và to-be.
- [X] Có một câu nói rõ finding này sẽ đổi gì trong SPEC.
