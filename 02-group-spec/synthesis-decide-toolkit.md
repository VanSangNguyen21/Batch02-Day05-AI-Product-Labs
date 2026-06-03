# Toolkit — Từ Evidence Đến Build Slice

## 1. Gom evidence thành cụm

* **Cụm 1: Ngợp thông tin & hoang mang giữa ma trận tài liệu**
  * User có quá nhiều nguồn học: YouTube, Coursera, Kaggle, blog, roadmap.
  * Điểm gãy không phải thiếu tài liệu mà là không biết học gì trước.
* **Cụm 2: Lộ trình AI cơ bản sai thứ tự module nền tảng**
  * User muốn học ML/DL/GenAI nhưng chưa có AI literacy, Python, data basics và model evaluation.
  * User học Python hoặc toán quá lâu nhưng không biết khi nào chuyển sang dữ liệu, ML, evaluation, DL hoặc prompting.
* **Cụm 3: Self-report không đáng tin**
  * User tự nói “tôi mới học” hoặc “tôi biết cơ bản” nhưng mức hiểu thật khác nhau.
  * Cần quiz 10 câu để có tín hiệu khách quan.
* **Cụm 4: AI hallucination và trust**
  * AI có thể bịa nguồn, bịa link, trả roadmap nghe hay nhưng không kiểm chứng.
  * Cần whitelist nguồn, JSON schema, confidence và fallback.
* **Cụm 5: Abuse, cost và data leakage**
  * Chatbot có thể bị spam/jailbreak.
  * Nếu không log token/cost thì không biết scale có khả thi không.
  * Không được để prompt/API key/dữ liệu feedback nội bộ leak ra frontend hoặc output.

## 2. Insight

```text
Học viên mới bắt đầu học AI không chỉ cần một danh sách khóa học.
Họ cần một hệ thống ra quyết định học tập:
- học gì trước,
- học gì có thể bỏ qua ở giai đoạn hiện tại,
- nên học theo thứ tự AI literacy -> toán-thống kê -> Python -> data basics -> ML -> model evaluation -> DL nhập môn -> GenAI/prompting -> ethics/safety như thế nào,
- khi nào roadmap cá nhân hóa chưa đủ chắc và phải dùng lộ trình nền tảng.

Vì AI là xác suất, product phải thiết kế cho uncertainty:
confidence, hỏi lại, fallback, correction, human check, eval và cost log là một phần của sản phẩm chứ không phải phần phụ.
```

## 3. Opportunity

```text
Cơ hội là dùng AI để cá nhân hóa lộ trình học AI cơ bản dựa trên:
1. Mục tiêu học của user.
2. Mô tả cá nhân.
3. Quiz 10 câu về kiến thức AI nền tảng.
4. Ràng buộc thời gian học.

Sản phẩm tạo roadmap trực quan thay vì chỉ chat text, đồng thời giảm rủi ro bằng fallback roadmap, source whitelist, guardrail và feedback loop.
```

## 4. Quyết định build slice

| Câu hỏi | Quyết định |
|---|---|
| User cụ thể chưa? | Có: người mới học AI muốn học nền tảng AI cơ bản theo 9 module. |
| Track nội dung học là gì? | Chỉ có 1 track: AI cơ bản gồm AI literacy -> toán-thống kê -> Python -> data basics -> ML -> model evaluation -> DL nhập môn -> GenAI/prompting -> ethics/safety. |
| Task đủ hẹp chưa? | Có: tạo roadmap AI cơ bản 2-8 tuần, không làm full LMS. |
| AI decision rõ chưa? | Có: AI phân loại level, tạo milestone, confidence, follow-up questions. |
| UX chính là gì? | Web app 2 tab: Chat bot và Lộ trình có fallback. |
| Failure path rõ chưa? | Có: input thiếu, mâu thuẫn, confidence thấp, JSON lỗi, nguồn không whitelist, jailbreak. |
| Có đo cost không? | Có: log token/cost/session và hiển thị estimated cost. |
| Có feedback loop không? | Có: rating, comment, report source, user correction, human-check queue. |

## 5. Product contract cho prototype

### Input tối thiểu

```json
{
  "goal": "Tôi muốn học AI cơ bản để hiểu nền tảng và tự làm được project ML/GenAI nhập môn",
  "time_per_week_hours": 3,
  "duration_weeks": 4,
  "profile": {
    "role": "Sinh viên/người mới học AI",
    "coding_level": "basic",
    "math_level": "basic",
    "preferred_learning_style": "bài giảng ngắn + bài tập Python + project nhỏ",
    "dropout_risk": "không biết học AI literacy/toán/Python/data/ML/evaluation/DL/GenAI/ethics theo thứ tự nào"
  },
  "quiz_answers": ["...", "..."]
}
```

### Output tối thiểu

```json
{
  "level": "Beginner",
  "track": "AI cơ bản: AI literacy + Toán-thống kê + Python + Data basics + ML + Model evaluation + DL nhập môn + GenAI/prompting + Ethics/safety",
  "confidence": 0.84,
  "path_type": "personalized",
  "fallback_used": false,
  "reasoning_summary": "Mục tiêu rõ, thời gian phù hợp, quiz ở mức nền tảng.",
  "milestones": [
    {
      "title": "AI literacy, Python và dữ liệu nền tảng",
      "duration": "1 tuần",
      "outcome": "Hiểu AI/ML/DL/GenAI khác nhau ra sao, viết được Python cơ bản và biết feature/label/train-test là gì",
      "activities": ["Làm bài tập Python list/function", "Phân biệt feature/label trên dataset nhỏ", "Chia train/test và đọc accuracy"],
      "sources": ["Google ML Crash Course", "Kaggle Learn Python", "Kaggle Learn Intro to Machine Learning"]
    }
  ],
  "locked_branches": [],
  "follow_up_questions": [],
  "cost_estimate": {
    "tokens_in": 1200,
    "tokens_out": 900,
    "estimated_cost_usd": 0.01
  }
}
```

## 6. Fallback decision tree

```text
1. Nếu guardrail block -> refuse + fallback + security log.
2. Nếu chưa làm quiz -> không cá nhân hóa sâu; yêu cầu quiz hoặc dùng roadmap chung.
3. Nếu input thiếu mục tiêu/thời gian -> hỏi tối đa 3 câu.
4. Nếu quiz thấp nhưng mục tiêu quá nâng cao -> fallback + khóa nhánh nâng cao.
5. Nếu JSON parse fail hoặc source không whitelist -> fallback tĩnh + log model_output_invalid.
6. Nếu confidence < 50% -> fallback + human-check option.
7. Nếu confidence 50-80% -> fallback tạm + hỏi thêm.
8. Nếu confidence > 80% -> personalized roadmap, nhưng vẫn cho feedback/report.
```

## 7. Câu hỏi động: đa dạng, scale, thời điểm hỏi

| Thời điểm | Câu hỏi | Khi nào hỏi |
|---|---|---|
| Trước quiz | “Bạn muốn học AI để làm gì trong 30 ngày tới?” | Luôn hỏi. |
| Trước quiz | “Bạn có bao nhiêu giờ mỗi tuần?” | Luôn hỏi để scope roadmap. |
| Sau quiz | “Bạn đang yếu nhất ở module nào: AI literacy, toán-thống kê, Python, dữ liệu, ML, evaluation, DL, GenAI hay ethics?” | Khi cần xác định điểm bắt đầu. |
| Sau quiz | “Bạn đã từng dùng NumPy/Pandas và chia train/test chưa?” | Khi user muốn học ML nhưng chưa rõ nền tảng Python dữ liệu/evaluation. |
| Sau quiz | “Bạn có thể tăng thời gian học không?” | Khi goal quá lớn so với time budget. |
| Sau roadmap | “Lộ trình này phù hợp với bạn ở mức nào?” | Luôn hỏi bằng rating. |

**Nguyên tắc scale:** Không gọi AI cho mọi câu hỏi form. Thu dữ liệu bằng form/quiz trước, chỉ gọi model khi đủ input. Với cohort đông, dùng fallback/cache cho các persona phổ biến.

## 8. Eval plan

### Test dataset tối thiểu

| # | Persona | Expected path |
|---|---|---|
| 1 | Người mới học, chưa biết Python, 3h/tuần, quiz 2/10 | Fallback: AI literacy + Python + data basics |
| 2 | Sinh viên biết Python, yếu toán-thống kê, 6h/tuần, quiz 5/10 | Personalized: toán-thống kê cho ML + data basics + ML nhập môn |
| 3 | User biết Python/NumPy, quiz 7/10 | Personalized: ML cơ bản + model evaluation + project regression/classification |
| 4 | User muốn học DL/GenAI nhưng chưa biết ML/evaluation, quiz 4/10 | Fallback: khóa nhánh DL/GenAI, học ML + evaluation trước |
| 5 | User nhập “học AI” rất ngắn | Ask clarification |
| 6 | User mâu thuẫn: chưa biết Python/data basics/evaluation, 2h/tuần, muốn build model DL hoặc GenAI app | Fallback + locked advanced branch |
| 7 | User spam/jailbreak | Refuse + security log |
| 8 | Model trả source ngoài whitelist | Remove source + fallback |
| 9 | User rating 1 sao | Feedback log + human check |
| 10 | Confidence >80% nhưng guardrail block | Human-check required |

### Rubric chấm output

| Tiêu chí | Đạt khi |
|---|---|
| Relevance | Roadmap khớp mục tiêu và vai trò. |
| Feasibility | Thời lượng phù hợp với time budget. |
| Level fit | Không quá dễ/quá khó so với quiz. |
| Trust | Có confidence, lý do và nguồn. |
| Safety | Không leak prompt/key, không trả lời ngoài phạm vi. |
| Recovery | Low-confidence/failure có fallback rõ. |
| Cost awareness | Có log token/cost/session. |

## 9. Architecture prototype đề xuất

```text
Frontend
- index.html: 2 tab Chat bot / Lộ trình có fallback
- app.js: state session, quiz, render roadmap, feedback
- styles.css: responsive UI

Backend
- /api/chat: chat tư vấn trong phạm vi học AI
- /api/roadmap: nhận profile + quiz, trả JSON roadmap
- /api/feedback: lưu rating/comment/correction
- guardrail middleware: chặn prompt injection/spam/out-of-scope
- cost logger middleware: token/cost/latency/session

Data
- SQLite hoặc JSONL cho demo
- tables/logs: sessions, cost_events, feedback_events, roadmap_outputs, human_check_queue

AI
- system prompt cố định
- JSON schema roadmap
- fallback roadmap hardcoded
- source whitelist
```

## 10. Backlog không build trong 2 ngày

* Đăng nhập thật và lưu tiến độ dài hạn.
* Tích hợp thanh toán/mentor marketplace.
* Recommender học liệu real-time theo YouTube/Coursera API.
* Fine-tuning model thật từ feedback.
* Dashboard admin đầy đủ cho human review.
* Adaptive quiz sinh câu hỏi mới theo thời gian thực.

## 11. Câu chốt cuối

```text
Dựa trên evidence người mới học AI bị ngợp thông tin, lộ trình generic và thiếu trust vào câu trả lời AI,
nhóm sẽ build web chatbot cá nhân hóa lộ trình học AI cơ bản,
cho user nhập mục tiêu, mô tả cá nhân và làm quiz 10 câu,
để tạo roadmap trực quan có confidence, nguồn học, cost log và feedback loop,
đồng thời test failure path bằng input mâu thuẫn, low-confidence, hallucinated source và prompt injection.
```
