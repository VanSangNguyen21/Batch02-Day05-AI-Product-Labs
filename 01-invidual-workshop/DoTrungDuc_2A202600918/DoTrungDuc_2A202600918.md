# Giải phẫu Moni của MoMo: App → Flow → Path yếu → Sửa

## 0. Phạm vi thực hiện

Bài này chỉ phân tích **Moni của MoMo**, không phân tích Vietnam Airlines NEO hay V-App V-AI.

Mục tiêu là thực hiện đúng yêu cầu workshop:

> Mổ sản phẩm AI thật: app → flow → path yếu → sửa.

Đầu ra gồm:

- Một bản phân tích **as-is / to-be**.
- Một **path yếu nhất** được chọn để sửa.
- Một câu **product decision**.
- Không liệt kê bug rời rạc.

---

## 1. Chọn app để dùng thử

### App được chọn

**MoMo — Moni**

### Mô tả ngắn

Moni là trợ lý tài chính trong ứng dụng MoMo. Vai trò chính của Moni là hỗ trợ người dùng hiểu tình hình chi tiêu, phân tích giao dịch và gợi ý cách quản lý tài chính cá nhân ngay trong app.

### Promise của sản phẩm

**Promise chính của Moni:**

> Giúp người dùng hiểu tiền của mình đang đi đâu, phân tích chi tiêu cá nhân và đưa ra gợi ý tài chính dựa trên dữ liệu trong MoMo.

Nói ngắn gọn hơn:

> Moni hứa sẽ biến dữ liệu giao dịch trong MoMo thành insight tài chính dễ hiểu và có thể hành động được.

---

## 2. Dùng thử: 2–3 query thật

Phần này mô phỏng 3 tình huống người dùng thật có thể hỏi Moni khi muốn quản lý chi tiêu cá nhân.

---

### Query 1: “Tháng này tôi tiêu bao nhiêu tiền?”

#### Kỳ vọng của user

Người dùng kỳ vọng Moni trả lời trực tiếp bằng số liệu cụ thể, ví dụ:

- Tổng số tiền đã chi trong tháng này.
- Các nhóm chi tiêu chính.
- Nhóm nào chiếm tỷ trọng cao nhất.
- Có thể so sánh nhanh với tháng trước.

Ví dụ câu trả lời user kỳ vọng:

> Tháng này bạn đã chi 4.200.000đ qua MoMo. Nhóm chi nhiều nhất là ăn uống với 1.600.000đ, chiếm 38% tổng chi tiêu.

#### Thực tế có thể xảy ra

Moni có thể trả lời chung chung hoặc điều hướng người dùng sang phần lịch sử giao dịch / thống kê chi tiêu.

Ví dụ:

> Bạn có thể kiểm tra lịch sử giao dịch để xem các khoản chi trong tháng.

#### Điểm gãy

Điểm gãy nằm ở chỗ user kỳ vọng Moni hiểu dữ liệu ví MoMo của mình và tổng hợp giúp họ. Nếu Moni chỉ hướng dẫn người dùng tự đi xem lịch sử giao dịch, vai trò “trợ lý tài chính” bị yếu.

#### Nhận xét

Câu trả lời có thể đúng về mặt chức năng, nhưng chưa đúng về mặt trải nghiệm AI. User không cần một menu hướng dẫn, user cần một insight được tính toán sẵn.

---

### Query 2: “Tôi tiêu nhiều nhất vào khoản nào?”

#### Kỳ vọng của user

Người dùng kỳ vọng Moni chỉ ra nhóm chi tiêu lớn nhất, ví dụ:

- Ăn uống.
- Mua sắm.
- Di chuyển.
- Hóa đơn.
- Chuyển khoản.
- Giải trí.

User cũng kỳ vọng Moni đưa ra một nhận xét ngắn:

> Bạn chi nhiều nhất cho ăn uống. Khoản này tăng so với tháng trước.

#### Thực tế có thể xảy ra

Moni có thể trả lời theo kiểu lời khuyên chung:

> Bạn nên theo dõi các khoản chi lớn và lập ngân sách hàng tháng để quản lý tài chính tốt hơn.

#### Điểm gãy

Câu trả lời nghe hợp lý nhưng không giải quyết đúng câu hỏi. User không hỏi “tôi nên quản lý tiền như thế nào”, mà hỏi “khoản nào của tôi đang lớn nhất”.

#### Nhận xét

Đây là điểm yếu nghiêm trọng vì Moni không tạo ra khác biệt so với một chatbot tài chính thông thường. Nếu không dựa trên dữ liệu cá nhân, Moni mất lợi thế lớn nhất: nằm trong một ví điện tử có dữ liệu giao dịch thật.

---

### Query 3: “Có khoản nào bất thường không?”

#### Kỳ vọng của user

Người dùng kỳ vọng Moni phát hiện các dấu hiệu bất thường như:

- Một khoản chi tăng đột biến.
- Một giao dịch lớn hơn bình thường.
- Một khoản thanh toán lặp lại mà user có thể đã quên.
- Một nhóm chi tiêu tăng mạnh so với tháng trước.
- Một giao dịch có vẻ không quen thuộc.

Ví dụ câu trả lời user kỳ vọng:

> Tháng này khoản ăn uống của bạn tăng khoảng 30% so với tháng trước. Ngoài ra, có một giao dịch 750.000đ vào ngày 12/05 cao hơn mức chi trung bình thường ngày.

#### Thực tế có thể xảy ra

Moni có thể không đủ dữ liệu hoặc không có định nghĩa rõ ràng về “bất thường”. Khi đó, Moni có thể trả lời chung:

> Bạn nên kiểm tra lại các giao dịch gần đây để phát hiện khoản chi bất thường.

#### Điểm gãy

Moni không nói rõ giới hạn của mình:

- Nó có đang xem toàn bộ giao dịch không?
- Nó chỉ xem giao dịch trong MoMo hay cả ngân hàng liên kết?
- Nó so sánh với tháng trước, 3 tháng trước hay trung bình cá nhân?
- “Bất thường” được hiểu theo tiêu chí nào?

#### Nhận xét

Vấn đề không chỉ là thiếu câu trả lời, mà là thiếu minh bạch. Khi AI không nói rõ nguồn dữ liệu và tiêu chí phân tích, user khó tin vào kết quả.

---

## 3. Vẽ flow as-is

Phần này mô tả 4 luồng chính khi user tương tác với Moni:

- Happy path.
- Low-confidence path.
- Failure path.
- Correction path.

---

## 3.1. Happy path

### Tình huống

User hỏi:

> “Tháng này tôi tiêu bao nhiêu?”

### Flow as-is

```text
User nhập câu hỏi
        ↓
Moni nhận diện intent: hỏi tổng chi tiêu
        ↓
Moni truy xuất dữ liệu giao dịch trong MoMo
        ↓
Moni tổng hợp số tiền theo thời gian
        ↓
Moni trả lời tổng chi tiêu
        ↓
User có thể hỏi tiếp hoặc xem chi tiết
```

### Trải nghiệm tốt cần đạt

Ở happy path, Moni cần trả lời bằng dữ liệu cụ thể, ví dụ:

> Tháng này bạn đã chi khoảng 4.200.000đ qua MoMo. Nhóm chi cao nhất là ăn uống với 1.600.000đ, chiếm 38% tổng chi tiêu.

### Đánh giá

Happy path tốt khi câu hỏi rõ ràng, phạm vi thời gian rõ ràng và Moni có đủ dữ liệu. Tuy nhiên, nếu câu trả lời chỉ dừng ở tổng tiền mà không có phân nhóm hoặc hành động tiếp theo, trải nghiệm vẫn chưa đủ mạnh.

---

## 3.2. Low-confidence path

### Tình huống

User hỏi:

> “Dạo này tôi tiêu hơi nhiều đúng không?”

### Vấn đề

Câu hỏi này mơ hồ vì:

- “Dạo này” có thể là tuần này, tháng này hoặc 30 ngày gần nhất.
- “Tiêu nhiều” cần một mốc so sánh.
- User có thể muốn so với tháng trước, so với ngân sách, hoặc so với thói quen cá nhân.

### Flow as-is có thể xảy ra

```text
User hỏi câu mơ hồ
        ↓
Moni nhận diện intent: đánh giá chi tiêu
        ↓
Moni không chắc phạm vi thời gian và tiêu chí so sánh
        ↓
Moni vẫn cố trả lời hoặc đưa advice chung
        ↓
User nhận câu trả lời thiếu tin cậy
```

### Điểm user bị kẹt

User không biết Moni đang hiểu “dạo này” là khoảng thời gian nào. Nếu Moni đoán sai, user phải hỏi lại hoặc sửa lại câu hỏi.

### Cách xử lý tốt hơn

Moni nên hỏi lại bằng lựa chọn nhanh:

> Bạn muốn mình so sánh chi tiêu theo mốc nào?

Các button có thể là:

```text
[Tuần này]
[Tháng này]
[30 ngày gần nhất]
[So với tháng trước]
```

---

## 3.3. Failure path

### Tình huống

User hỏi:

> “Khoản nào tháng này không cần thiết?”

### Vấn đề

Đây là câu hỏi khó vì “không cần thiết” là đánh giá chủ quan. Một khoản ăn uống, mua sắm hay giải trí có thể cần thiết với người này nhưng không cần thiết với người khác.

### Flow as-is có thể xảy ra

```text
User hỏi câu có tính chủ quan
        ↓
Moni nhận diện intent: đánh giá khoản chi
        ↓
Moni không đủ tiêu chí cá nhân để xác định khoản nào không cần thiết
        ↓
Moni trả lời chung chung hoặc né tránh
        ↓
User không nhận được quyết định rõ ràng
```

### Điểm user bị kẹt

User muốn Moni giúp lọc ra khoản nên xem lại, nhưng Moni không đưa được tiêu chí phân loại rõ ràng.

### Cách xử lý tốt hơn

Moni không nên tự kết luận cứng rằng khoản nào “không cần thiết”. Thay vào đó, Moni nên chuyển câu hỏi thành phân tích có tiêu chí:

> Mình không thể chắc khoản nào là không cần thiết, nhưng có thể chỉ ra các khoản nên xem lại: khoản tăng mạnh, khoản lặp lại, khoản giá trị lớn, hoặc khoản ngoài nhóm thiết yếu.

Sau đó Moni có thể đưa button:

```text
[Xem khoản tăng mạnh]
[Xem khoản lặp lại]
[Xem khoản giá trị lớn]
[Xem khoản giải trí/mua sắm]
```

---

## 3.4. Correction path

### Tình huống

User hỏi ban đầu:

> “Tôi tiêu nhiều nhất vào khoản nào?”

Sau đó user sửa:

> “Không, ý tôi là tuần này.”

### Flow as-is có thể xảy ra

```text
User hỏi câu ban đầu
        ↓
Moni trả lời theo mặc định, ví dụ tháng này
        ↓
User sửa lại phạm vi thời gian
        ↓
Moni không giữ context tốt
        ↓
User phải nhập lại câu đầy đủ
```

### Điểm user bị kẹt

User phải lặp lại ý định ban đầu:

> “Tôi tiêu nhiều nhất vào khoản nào trong tuần này?”

Điều này làm flow sửa sai bị dài và kém tự nhiên.

### Cách xử lý tốt hơn

Moni cần hiểu câu “Không, ý tôi là tuần này” là một correction của tham số thời gian, không phải một intent mới.

Flow tốt hơn:

```text
User hỏi: “Tôi tiêu nhiều nhất vào khoản nào?”
        ↓
Moni trả lời theo tháng này
        ↓
User sửa: “Không, ý tôi là tuần này.”
        ↓
Moni giữ intent cũ: top spending category
        ↓
Moni chỉ đổi time range: tuần này
        ↓
Moni trả lời lại ngay
```

Ví dụ:

> À, nếu tính trong tuần này thì bạn chi nhiều nhất cho ăn uống: 420.000đ, chiếm 34% tổng chi qua MoMo.

---

## 4. Đánh dấu user kẹt ở đâu

Các điểm user dễ bị kẹt nhất trong flow của Moni:

| Vị trí trong flow | User muốn gì | Moni dễ làm gì | Vì sao user kẹt |
|---|---|---|---|
| Hỏi tổng chi tiêu | Muốn số liệu cụ thể | Điều hướng sang lịch sử giao dịch | User phải tự tìm |
| Hỏi khoản chi lớn nhất | Muốn insight cá nhân | Trả lời advice chung | Không có dữ liệu cụ thể |
| Hỏi khoản bất thường | Muốn phát hiện vấn đề | Không nói rõ tiêu chí | User không biết có nên tin không |
| Hỏi câu mơ hồ | Muốn được hiểu tự nhiên | Đoán sai hoặc trả lời chung | User phải hỏi lại |
| Sửa câu hỏi | Muốn Moni giữ context | Bắt nhập lại từ đầu | Flow bị đứt đoạn |

---

## 5. Chọn path yếu nhất

### Path yếu nhất được chọn

> User hỏi câu phân tích tài chính cá nhân, nhưng Moni trả lời chung chung, không dựa trên dữ liệu cụ thể hoặc không nói rõ giới hạn dữ liệu.

### Ví dụ path yếu

User hỏi:

> “Tôi tiêu nhiều nhất vào khoản nào tháng này?”

As-is có thể xảy ra:

```text
User hỏi:
“Tôi tiêu nhiều nhất vào khoản nào tháng này?”
        ↓
Moni nhận diện intent: phân tích chi tiêu
        ↓
Moni không chắc có đủ dữ liệu hoặc không phân loại rõ giao dịch
        ↓
Moni trả lời chung:
“Bạn nên kiểm tra các khoản chi lớn và lập kế hoạch chi tiêu.”
        ↓
User kẹt:
Không biết khoản nào cao nhất, không có số liệu, không có hành động tiếp theo.
```

### Vì sao đây là path yếu nhất?

Đây là path yếu nhất vì nó đánh trực tiếp vào promise cốt lõi của Moni.

Nếu Moni chỉ trả lời như một chatbot tư vấn tài chính chung, user sẽ nghĩ:

> “Câu này Google hoặc ChatGPT cũng trả lời được, không cần Moni trong MoMo.”

Moni chỉ thật sự có giá trị khi dùng được dữ liệu giao dịch trong app để đưa ra insight cá nhân.

---

## 6. Sửa path yếu nhất

### Mục tiêu sửa

Biến câu trả lời của Moni từ:

> Advice chung chung.

Thành:

> Insight tài chính cá nhân có số liệu, có nguồn dữ liệu, có giới hạn rõ ràng và có hành động tiếp theo.

---

## 6.1. To-be flow

```text
User hỏi:
“Tôi tiêu nhiều nhất vào khoản nào tháng này?”
        ↓
Moni nhận diện intent:
Top spending category
        ↓
Moni xác định tham số:
- Time range: tháng này
- Data source: giao dịch trong MoMo
- Metric: tổng chi theo danh mục
        ↓
Moni kiểm tra dữ liệu:
- Có đủ giao dịch không?
- Có phân loại danh mục không?
- Có giao dịch nào chưa phân loại không?
        ↓
Nếu đủ dữ liệu:
Moni trả lời bằng số liệu + insight + action
        ↓
Nếu thiếu dữ liệu:
Moni nói rõ giới hạn + hỏi lại bằng button
        ↓
User chọn hành động tiếp theo
```

---

## 6.2. Câu trả lời to-be khi đủ dữ liệu

Ví dụ Moni nên trả lời:

> Tháng này bạn chi nhiều nhất cho **Ăn uống**: 1.600.000đ, chiếm 38% tổng chi tiêu qua MoMo. Khoản này tăng 18% so với tháng trước.  
> Bạn có muốn đặt giới hạn 1.300.000đ cho nhóm Ăn uống trong tháng sau không?

Button gợi ý:

```text
[Xem chi tiết]
[So với tháng trước]
[Đặt ngân sách]
[Ẩn chuyển khoản]
```

### Vì sao tốt hơn?

Câu trả lời này tốt hơn vì có:

- Số liệu cụ thể.
- Danh mục rõ ràng.
- Tỷ trọng trong tổng chi.
- So sánh với quá khứ.
- Một hành động tiếp theo.

---

## 6.3. Câu trả lời to-be khi thiếu dữ liệu

Nếu Moni không có đủ dữ liệu, không nên trả lời chung chung. Moni nên nói rõ giới hạn:

> Mình chỉ thấy các giao dịch qua MoMo, chưa bao gồm tiền mặt hoặc tài khoản ngân hàng ngoài MoMo. Bạn muốn mình phân tích riêng chi tiêu qua MoMo trong tháng này không?

Button gợi ý:

```text
[Phân tích MoMo]
[Đổi mốc thời gian]
[Thêm dữ liệu khác]
```

### Vì sao tốt hơn?

Vì Moni không giả vờ biết tất cả. Nó minh bạch về nguồn dữ liệu, giúp user hiểu kết quả chỉ phản ánh phần chi tiêu qua MoMo.

---

## 6.4. Câu trả lời to-be khi câu hỏi mơ hồ

Nếu user hỏi:

> “Dạo này tôi tiêu nhiều không?”

Moni nên hỏi lại:

> Bạn muốn mình so sánh chi tiêu theo mốc nào?

Button:

```text
[Tuần này]
[Tháng này]
[30 ngày gần nhất]
[So với tháng trước]
```

Hoặc Moni có thể đưa ra mặc định nhưng phải nói rõ:

> Nếu tính theo 30 ngày gần nhất, bạn đã chi 4.800.000đ qua MoMo, tăng 12% so với 30 ngày trước đó.

---

## 6.5. Câu trả lời to-be khi user sửa câu hỏi

Nếu user sửa:

> “Không, ý tôi là tuần này.”

Moni nên giữ intent cũ và chỉ đổi tham số thời gian:

> À, nếu tính trong tuần này thì bạn chi nhiều nhất cho **Ăn uống**: 420.000đ, chiếm 34% tổng chi tiêu qua MoMo.

Không nên bắt user nhập lại toàn bộ câu hỏi.

---

## 7. Các thay đổi UX đề xuất

### 7.1. Hỏi lại khi thiếu thông tin

Moni nên hỏi lại khi câu hỏi thiếu:

- Mốc thời gian.
- Tiêu chí so sánh.
- Nguồn dữ liệu.
- Danh mục chi tiêu.
- Ý nghĩa của từ mơ hồ như “nhiều”, “bất thường”, “không cần thiết”.

Ví dụ:

> Bạn muốn mình xem theo tuần này, tháng này hay 30 ngày gần nhất?

---

### 7.2. Luôn hiển thị source dữ liệu

Mỗi câu phân tích nên ghi rõ nguồn:

> Dựa trên giao dịch qua MoMo trong tháng này.

Hoặc:

> Mình chỉ phân tích các giao dịch trong MoMo, chưa bao gồm tiền mặt và tài khoản ngân hàng ngoài MoMo.

Điều này giúp tăng niềm tin và tránh hiểu sai.

---

### 7.3. Thêm button hành động nhanh

Sau mỗi câu trả lời, Moni nên có button để user đi tiếp:

```text
[Xem chi tiết]
[So với tháng trước]
[Đặt ngân sách]
[Ẩn chuyển khoản]
[Phân loại lại giao dịch]
```

Button giúp user không phải tự nghĩ câu hỏi tiếp theo.

---

### 7.4. Cho phép undo hoặc correction

Nếu Moni phân loại sai giao dịch, user cần sửa được:

Ví dụ:

> Giao dịch này không phải ăn uống, đây là mua sắm.

Sau đó Moni nên ghi nhận correction:

> Mình đã chuyển giao dịch này sang nhóm Mua sắm. Các phân tích sau sẽ dùng phân loại mới.

---

### 7.5. Có correction log

Moni nên lưu các sửa đổi ngắn hạn của user:

- User đổi mốc thời gian.
- User đổi danh mục.
- User loại trừ chuyển khoản.
- User sửa phân loại giao dịch.

Ví dụ:

```text
Correction log:
- User đổi phạm vi từ “tháng này” sang “tuần này”.
- User yêu cầu không tính giao dịch chuyển khoản.
- User sửa giao dịch Highlands từ “mua sắm” sang “ăn uống”.
```

Correction log giúp Moni nhất quán hơn trong các lượt hỏi sau.

---

### 7.6. Có handoff khi vượt khả năng

Nếu user hỏi vấn đề liên quan đến hỗ trợ tài khoản, giao dịch lỗi, hoàn tiền hoặc tranh chấp, Moni nên chuyển sang bộ phận phù hợp.

Ví dụ:

> Câu hỏi này liên quan đến giao dịch cụ thể. Mình có thể chuyển bạn sang hỗ trợ MoMo để kiểm tra chi tiết.

Button:

```text
[Liên hệ hỗ trợ]
[Xem giao dịch liên quan]
[Quay lại phân tích chi tiêu]
```

---

## 8. Sketch As-is / To-be

## 8.1. Sketch As-is

```text
User:
“Tôi tiêu nhiều nhất vào khoản nào tháng này?”
        ↓
Moni:
Nhận diện đây là câu hỏi về chi tiêu
        ↓
Vấn đề:
Không chắc có đủ dữ liệu / phân loại giao dịch chưa rõ
        ↓
Moni trả lời:
“Bạn nên kiểm tra các khoản chi lớn và lập kế hoạch chi tiêu.”
        ↓
User kẹt:
- Không biết khoản nào lớn nhất
- Không có số tiền cụ thể
- Không biết dữ liệu lấy từ đâu
- Không có bước tiếp theo
```

---

## 8.2. Sketch To-be

```text
User:
“Tôi tiêu nhiều nhất vào khoản nào tháng này?”
        ↓
Moni:
Nhận diện intent = top spending category
        ↓
Moni xác định:
- Thời gian = tháng này
- Nguồn = giao dịch MoMo
- Chỉ số = tổng chi theo danh mục
        ↓
Moni kiểm tra dữ liệu:
- Có giao dịch trong tháng không?
- Có danh mục chi tiêu không?
- Có giao dịch chưa phân loại không?
        ↓
Nếu đủ dữ liệu:
“Bạn chi nhiều nhất cho Ăn uống: 1.600.000đ,
chiếm 38% tổng chi qua MoMo.
Khoản này tăng 18% so với tháng trước.”
        ↓
Button:
[Xem chi tiết] [So với tháng trước] [Đặt ngân sách]
        ↓
Nếu thiếu dữ liệu:
“Mình chỉ thấy giao dịch qua MoMo.
Bạn muốn phân tích riêng chi tiêu MoMo không?”
        ↓
Button:
[Phân tích MoMo] [Đổi mốc thời gian] [Thêm dữ liệu khác]
```

---

## 9. Product decision

### Product decision đầy đủ

> Moni không nên định vị như một chatbot tư vấn tài chính chung. Moni nên tập trung vào vai trò trợ lý insight tài chính cá nhân, dùng dữ liệu giao dịch trong MoMo để trả lời bằng số liệu cụ thể, luôn ghi rõ nguồn dữ liệu, hỏi lại khi thiếu ngữ cảnh và đưa ra hành động tiếp theo cho user.

### Product decision ngắn gọn

> Biến Moni từ chatbot trả lời chung thành trợ lý insight tài chính cá nhân: có số liệu, có source, có hỏi lại và có action tiếp theo.

---

## 10. Kết luận

Moni có promise mạnh vì nằm trong MoMo, nơi có dữ liệu giao dịch thật của người dùng. Tuy nhiên, path yếu nhất là khi user hỏi câu cần phân tích tài chính cá nhân nhưng Moni trả lời như một chatbot tư vấn chung.

Cách sửa không nên tập trung vào từng bug nhỏ, mà nên sửa flow cốt lõi:

```text
Câu hỏi của user
        ↓
Xác định intent + tham số
        ↓
Kiểm tra dữ liệu + source
        ↓
Trả lời bằng insight cụ thể
        ↓
Đưa button hành động tiếp theo
        ↓
Ghi nhận correction nếu user sửa
```

Nếu làm tốt flow này, Moni sẽ tạo được giá trị khác biệt: không chỉ trả lời câu hỏi, mà giúp user ra quyết định tài chính cá nhân ngay trong app.
