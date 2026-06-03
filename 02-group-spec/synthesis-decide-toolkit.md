# Toolkit — Từ Evidence Đến Build Slice

Dùng sau khi nhóm đã có evidence. Mục tiêu là chốt một build slice đủ nhỏ cho Day 06.

## 1. Gom evidence thành cụm

Gom theo **workflow/pain**, không gom theo tên feature.

**Cụm A: "Hỏi CSKH nhưng bị bơ / chatbot ngu"**
- 30+ reviews 1⭐: CSKH seen không trả lời, bơ hàng tháng, gọi điện bận
- Review NaDuo: "Chatbot tự động cực kỳ ngu, trả lời có như không"
- Screenshot 7: hỏi "200k nên phân loại vào phần nào" → chatbot reject vì không phân biệt câu hỏi vs giao dịch

**Cụm B: "Muốn hiểu chi tiêu nhưng báo cáo quá tĩnh"**
- Reviews: biểu đồ chỉ hiện chi không hiện thu, kết xuất Excel sai
- Competitor (Cleo, Monarch): đã chuyển sang natural language Q&A
- MISA chỉ có biểu đồ tĩnh, user phải tự phân tích → bỏ cuộc

**Cụm C: "AI phân loại sai nhưng không cho sửa"**
- Screenshot 9: user nói "Chuyển sang phân loại ăn uống" → chatbot reject
- Chatbot chỉ có 2 mode: accept hoặc reject, không có correction
- Reviews: phân loại sai → dữ liệu sai → mất trust → bỏ app

**Cụm D: "Input mơ hồ thì bị reject thay vì hỏi lại"**
- Screenshot 4-5: nhập "100k" hoặc "20" → reject cùng 1 message
- Chatbot không hỏi clarification, chỉ bảo "nhập chi tiết hơn"

## 2. Viết insight

**Insight 1 (từ Cụm A):**

```text
User MISA gặp vấn đề với app không chỉ cần "ai đó trả lời".
Họ thật ra cần kênh hỗ trợ HIỂU ĐƯỢC ngữ cảnh cá nhân
— biết tài khoản, biết lịch sử giao dịch, biết lỗi đang gặp —
vì 30+ reviews cho thấy CSKH khuôn mẫu ("hãy đồng bộ lại") không giải quyết
được vấn đề thực tế, và chatbot rule-based không phân biệt được câu hỏi vs giao dịch.
```

**Insight 2 (từ Cụm B):**

```text
User MISA đã ghi chép ≥1 tháng không chỉ cần "xem biểu đồ".
Họ thật ra cần AI giải thích chi tiêu bằng ngôn ngữ tự nhiên
— "tháng này chi ăn uống tăng 30%, chủ yếu do 3 lần nhậu cuối tuần" —
vì báo cáo tĩnh chỉ hiện con số mà không giải thích tại sao, và competitor
(Cleo, Monarch) đã chứng minh natural language Q&A hiệu quả hơn dashboard.
```

**Insight 3 (từ Cụm C + D):**

```text
User MISA không chỉ cần "AI phân loại đúng 100%".
Họ thật ra cần AI cho phép SỬA và HỎI LẠI — vì trust trong tài chính
đến từ khả năng kiểm soát, không phải độ chính xác tuyệt đối,
vì Screenshot 9 cho thấy user muốn sửa nhưng bị reject, và Screenshot 4-5
cho thấy input mơ hồ bị reject thay vì được hỏi clarification.
```

## 3. Viết opportunity

**Opportunity 1 (CSKH thông minh):**

```text
Cơ hội là dùng AI ReAct Agent để thay thế CSKH rule-based,
query DB giao dịch user → trả lời cá nhân hóa ("số dư lệch vì giao dịch X chưa đồng bộ"),
giúp user được giải đáp ngay thay vì chờ CSKH hàng tháng,
trong khi vẫn kiểm soát bằng cách chỉ đọc DB (read-only), không tự sửa dữ liệu.
```

**Opportunity 2 (Spending insights):**

```text
Cơ hội là dùng AI để augment việc phân tích chi tiêu,
nhận câu hỏi tự nhiên → query DB → tính toán → trả lời có giải thích,
giúp user hiểu thói quen chi tiêu mà không cần tự đọc biểu đồ/Excel,
trong khi vẫn kiểm soát bằng cách luôn show source (khoảng thời gian, số giao dịch)
và disclaimer khi dữ liệu thiếu.
```

**Opportunity 3 (Correction path):**

```text
Cơ hội là dùng AI để cho phép sửa giao dịch qua chat (conditional automation),
user nói "chuyển sang ăn uống" → Agent tìm giao dịch → hỏi xác nhận → sửa,
giúp user kiểm soát dữ liệu mà không cần thoát chat vào menu chỉnh tay,
trong khi vẫn kiểm soát bằng cách luôn hỏi xác nhận trước khi thay đổi dữ liệu.
```

## 4. Chọn build slice

Build slice tốt phải qua 5 câu hỏi:

| Câu hỏi | Đạt khi | Nhóm check |
|---|---|---|
| User cụ thể chưa? | Nói được ai dùng, trong bối cảnh nào. | ✅ User MISA đã ghi chép ≥1 tháng, muốn hiểu chi tiêu + cần hỗ trợ |
| Task đủ hẹp chưa? | Demo được trong 3-5 phút. | ✅ Demo 3 flow: hỏi insight → sửa phân loại → hỏi ngoài scope |
| AI decision rõ chưa? | AI gợi ý/tự làm một việc cụ thể. | ✅ AI query DB → trả lời insight, hoặc sửa category khi user xác nhận |
| Failure path rõ chưa? | Có một case AI không chắc hoặc sai để test. | ✅ Dữ liệu thiếu (chỉ có nửa tháng) → AI phải disclaimer, không bịa |
| Có evidence không? | Có bằng chứng từ self-use/review/user/competitor. | ✅ 200+ reviews + 9 screenshots self-use + 7 competitors |

## 5. Quyết định: giữ, giảm scope, hay đổi hướng?

| Tình huống | Quyết định | Nhóm áp dụng |
|---|---|---|
| Evidence yếu, user mơ hồ | Dừng build sâu; quay lại research 20 phút. | ❌ Không áp dụng — evidence rất mạnh (200+ reviews, 9 screenshots, 7 competitors) |
| Ý tưởng quá rộng | Giữ domain, cắt xuống một flow. | ⚠️ Áp dụng một phần — ban đầu muốn làm cả voice/receipt scan, đã cắt còn 3 flow core (insight + CSKH + correction) |
| AI không cần thiết | Dùng rule/manual prototype; ghi rõ vì sao không dùng AI sâu. | ❌ Không áp dụng — chatbot rule-based hiện tại "cực kỳ ngu" (evidence), AI LLM là bắt buộc |
| Rủi ro cao | Chọn augmentation hoặc conditional automation. | ✅ Áp dụng — chọn Conditional Automation: AI tự trả lời case rõ, hỏi lại case mơ hồ, chờ xác nhận case sửa dữ liệu |
| Không demo được trong 1 ngày | Đưa phần lớn vào backlog, giữ một path nhỏ. | ✅ Áp dụng — đưa 7 tính năng vào backlog (voice, receipt scan, bank sync, persona, gamification...) |

## 6. Câu chốt cuối

Điền câu này trước khi rời lớp:

```text
Dựa trên 200+ reviews tiêu cực (CSKH bị bơ, chatbot ngu, báo cáo tĩnh)
và 9 screenshots self-use (chatbot không hiểu câu hỏi, không cho sửa phân loại),
nhóm sẽ build AI Chatbot ReAct Agent tích hợp CSKH + spending insights,
cho user MISA đã ghi chép chi tiêu ≥1 tháng,
để giải quyết pain "muốn hiểu chi tiêu nhưng báo cáo tĩnh, hỏi CSKH bị bơ",
bằng cách AI query DB giao dịch → trả lời ngôn ngữ tự nhiên + cho sửa qua chat
(conditional automation: tự trả lời case rõ, hỏi lại case mơ hồ, chờ xác nhận case sửa),
và sẽ test failure path: AI trả lời số liệu thiếu mà không disclaimer.
```

## 7. Backlog

Những thứ **không build trong Day 06**:

- Voice input (nhập chi tiêu bằng giọng nói) — cần Speech-to-Text API
- Receipt scanning (chụp hóa đơn → OCR) — cần Vision API + tiền xử lý
- Bank sync tự động (liên kết ngân hàng) — cần bank API, vấn đề bảo mật
- Proactive weekly recap (tự gửi tóm tắt cuối tuần) — cần scheduler/notification
- AI Persona chọn được (vợ yêu / vợ giận / cố vấn) — nice-to-have, thêm sau
- Gamification (streaks, badges) — cần thiết kế UX riêng
- Multi-wallet / multi-currency — mở rộng schema DB
