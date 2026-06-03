# Phân tích Moni — Trợ thủ AI tài chính của MoMo

## 1. App được chọn

**App:** MoMo  
**Tính năng:** Moni — Trợ thủ AI tài chính  
**Use case:** Hỏi Moni về tiết kiệm và phân tích chi tiêu cá nhân.

**Promise kỳ vọng:** Moni không chỉ trả lời lời khuyên tài chính chung, mà có thể dùng dữ liệu giao dịch trong MoMo để giúp người dùng hiểu tiền đang đi đâu, khoản nào đang chi nhiều, và nên làm gì tiếp theo.

**User:** Người dùng MoMo muốn quản lý tài chính cá nhân đơn giản, đặc biệt là người muốn biết nên tiết kiệm bao nhiêu và khoản chi nào cần kiểm soát.

---

## 2. Các câu hỏi đã thử

### Query 1

**Người dùng hỏi:**

> Tôi nên tiết kiệm bao nhiêu mỗi tháng với lương 8 triệu?

**Moni trả lời:**

Moni gợi ý nên tiết kiệm **10% - 20% thu nhập**, tương đương:

```text
800.000đ - 1.600.000đ/tháng
```

Moni cũng khuyên người dùng nên đặt mục tiêu tiết kiệm rõ ràng.

**Đánh giá:**

* Trả lời đúng câu hỏi.
* Có con số cụ thể.
* Chưa có thao tác nhanh để tạo mục tiêu tiết kiệm.

### Query 2

**Người dùng hỏi:**

> Tôi đang tiêu quá nhiều ở khoản nào?

**Moni trả lời:**

```text
Tổng chi tiêu: 95.000đ
Số giao dịch: 3
```

Moni không chỉ ra nhóm chi tiêu nào cao nhất mà chỉ gợi ý người dùng xem báo cáo chi tiêu.

**Đánh giá:**

* Có dữ liệu nhưng chưa tạo được insight.
* Chưa trả lời đúng nhu cầu chính của người dùng.
* User muốn biết **khoản nào đang chi quá nhiều**, không chỉ tổng chi tiêu.

### Query 3

**Người dùng chọn:** Có, khi Moni hỏi có muốn đặt ngân sách tiết kiệm không.

**Moni trả lời:**

Moni đề xuất chọn một trong hai mức:

```text
800.000đ/tháng
1.600.000đ/tháng
```

**Đánh giá:**

* Có tiếp tục hội thoại theo hướng hành động.
* Cần thiết kế rõ hơn để người dùng tạo mục tiêu ngay.

---

## 3. Flow hiện tại (As-Is)

### Happy Path

```text
User hỏi mức tiết kiệm
        ↓
Moni tính theo quy tắc 10% - 20%
        ↓
Moni đề xuất số tiền tiết kiệm
        ↓
Moni hỏi có muốn đặt mục tiêu không
```

**Nhận xét:** Flow này hoạt động khá tốt vì Moni trả lời đúng câu hỏi và có gợi ý bước tiếp theo. Tuy nhiên, hành động tiếp theo vẫn chưa đủ rõ: user cần một nút tạo mục tiêu/ngân sách ngay trong flow.

### Low-confidence / Failure Path

```text
User hỏi:
"Tôi đang tiêu quá nhiều ở khoản nào?"
        ↓
Moni truy xuất giao dịch
        ↓
Moni chỉ hiển thị tổng chi tiêu
        ↓
Không xác định nhóm chi tiêu cao nhất
        ↓
User phải tự xem báo cáo hoặc hỏi lại
```

**Điểm thất bại:** Moni không trả lời được câu hỏi chính. Người dùng muốn biết nhóm chi nào đang vượt mức hoặc chiếm tỷ trọng cao nhất, không chỉ muốn xem tổng chi tiêu.

### Correction Path hiện tại

```text
User không nhận được insight đúng nhu cầu
        ↓
User phải tự hỏi lại hoặc tự mở báo cáo chi tiêu
        ↓
Không thấy cơ chế sửa phân loại / feedback / lưu correction
```

**Nhận xét:** Nếu Moni phân loại sai hoặc trả lời chưa đúng ý, user chưa có cách sửa ngay trong hội thoại.

---

## 4. Path yếu nhất được chọn

## Path được chọn

```text
"Tôi đang tiêu quá nhiều ở khoản nào?"
```

## Lý do

* Đây là câu hỏi có giá trị cao với người dùng.
* Moni có dữ liệu giao dịch nhưng chưa chuyển dữ liệu thành insight.
* Người dùng phải thực hiện thêm nhiều bước.
* Trải nghiệm chưa thể hiện rõ vai trò của AI Assistant tài chính.
* Nếu chỉ trả tổng chi tiêu, Moni chưa khác nhiều so với một màn hình báo cáo thông thường.

## Finding

```text
Khi user hỏi "Tôi đang tiêu quá nhiều ở khoản nào?",
Moni truy xuất được dữ liệu giao dịch nhưng chỉ trả về tổng chi tiêu và số giao dịch,
hậu quả là user không biết nhóm chi nào cần kiểm soát và phải tự đi xem báo cáo.
Lỗi thuộc layer data-tool + UX recovery: AI có dữ liệu nhưng chưa tạo insight/action từ dữ liệu.
Nên sửa bằng Insight + Action Card: hiển thị nhóm chi cao nhất, nguồn dữ liệu, đề xuất ngân sách, quick action, undo và handoff khi thiếu dữ liệu.
```

---

## 5. Flow đề xuất (To-Be)

```text
User hỏi khoản chi tiêu cao nhất
        ↓
Moni phân tích dữ liệu giao dịch
        ↓
Moni xác định nhóm chi tiêu lớn nhất
        ↓
Moni hiển thị insight
        ↓
Moni đề xuất hành động nhanh
        ↓
User chọn hành động
```

## Insight đề xuất

```text
Khoản chi lớn nhất tháng này là:

Ăn uống: 1.500.000đ
Chiếm 65% tổng chi tiêu.

Khoản này tăng 25% so với tháng trước.
Bạn có thể đặt ngân sách ăn uống 1.200.000đ/tháng.
```

## Source

```text
Phân tích dựa trên:

- 23 giao dịch
- Thời gian: 01/06/2026 - 30/06/2026
- Nguồn: Lịch sử giao dịch MoMo
```

## Quick Actions

```text
[ Xem chi tiết giao dịch ]

[ Đặt ngân sách ăn uống ]

[ Nhắc tôi khi vượt hạn mức ]
```

## Undo

```text
Đã tạo ngân sách ăn uống: 1.200.000đ/tháng

[ Hoàn tác ]
```

## Handoff khi thiếu dữ liệu

```text
Moni chưa có đủ dữ liệu để xác định nhóm chi tiêu lớn nhất.

[ Xem báo cáo chi tiêu ]

[ Thử lại sau ]
```

---

## 6. Product Decision

## Quyết định sản phẩm

Triển khai **Insight + Action Card** cho Moni.

Thay vì chỉ trả lời bằng văn bản, Moni nên:

1. Trả lời trực tiếp câu hỏi.
2. Hiển thị nguồn dữ liệu đã dùng.
3. Đưa ra insight cụ thể.
4. Gợi ý hành động nhanh.
5. Cho phép hoàn tác.
6. Handoff khi dữ liệu không đủ.

## Lý do

### Đối với người dùng

* Giảm số bước phải hỏi lại.
* Biết rõ khoản nào cần kiểm soát.
* Có thể hành động ngay sau khi nhận insight.
* Trải nghiệm phù hợp hơn với mobile app.

### Đối với MoMo

* Tăng tương tác với Moni.
* Tăng khả năng người dùng tạo ngân sách.
* Tăng mức độ gắn bó với hệ sinh thái MoMo.
* Nâng Moni từ chatbot hỏi đáp thành AI Assistant tài chính.

---

## 7. Output cuối cùng

## As-Is

```text
User hỏi
        ↓
Moni trả lời bằng văn bản
        ↓
User tự quyết định bước tiếp theo
```

## User Stuck Point

```text
User hỏi:
"Tôi đang tiêu quá nhiều ở khoản nào?"
        ↓
Moni chỉ trả về tổng chi tiêu
        ↓
Không biết nhóm nào chi nhiều nhất
        ↓
User phải hỏi lại hoặc tự xem báo cáo
```

## To-Be

```text
User hỏi
        ↓
Moni phân tích giao dịch
        ↓
Moni đưa ra insight cụ thể
        ↓
Hiển thị nguồn dữ liệu
        ↓
Hiển thị Action Card
        ↓
User chọn hành động
```

## Product Decision

```text
Triển khai Insight + Action Card + Source + Undo + Handoff
để giúp Moni chuyển từ chatbot trả lời thông tin
thành AI Assistant tài chính có khả năng hỗ trợ hành động.
```

---

## 8. SPEC impact

Finding này sẽ đổi SPEC theo hướng: mọi AI output dùng dữ liệu cá nhân phải có **insight trực tiếp + source + confidence/fallback + action tiếp theo + undo/correction path**. Với sản phẩm AI cá nhân hóa, chỉ trả lời text là chưa đủ; output cần giúp user ra quyết định hoặc hành động ngay.

## 9. Checklist trước khi nộp

* [x] Có observation cụ thể từ app.
* [x] Có đủ happy path, low-confidence/failure path và correction path.
* [x] Finding được viết thành product decision.
* [x] Có sketch as-is và to-be.
* [x] Có câu nói rõ finding này sẽ đổi gì trong SPEC.
