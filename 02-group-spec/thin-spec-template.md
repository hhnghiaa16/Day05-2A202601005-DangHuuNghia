# Template — Thin SPEC Cuối Day 05

Thin SPEC không phải PRD đầy đủ. Đây là bản cam kết đủ rõ để sáng Day 06 nhóm build ngay.

## 1. Track, product/app và user

**Track:** D — Personal Finance  
**Product/app thật:** Sổ Thu Chi MISA (MISA Money Keeper) — app quản lý tài chính cá nhân phổ biến nhất Việt Nam, 5M+ users, 4.4⭐ Google Play, do MISA JSC phát triển (30+ năm kinh nghiệm phần mềm kế toán).  
**User cụ thể:** Người dùng MISA đã ghi chép chi tiêu ≥1 tháng, muốn hiểu mình chi gì nhiều nhất và cần hỗ trợ khi gặp vấn đề với app — nhưng hiện tại CSKH không phản hồi và báo cáo trong app quá tĩnh, khó hiểu.  
**Nhóm có phải user thật không? Nếu không, khác ở đâu?** Nhóm có thành viên đã/đang dùng app quản lý chi tiêu (MISA, Money Lover). Khác ở chỗ nhóm là sinh viên với mức chi tiêu đơn giản hơn user trung bình (gia đình, người đi làm quản lý nhiều ví/tài khoản).

## 2. Evidence summary

| Evidence | Nguồn | User/pain nói lên điều gì? | SPEC phải đổi gì? |
|---|---|---|---|
| 30+ reviews phàn nàn CSKH không phản hồi, bị bơ hàng tuần/tháng, chatbot hiện tại "cực kỳ ngu" | App Store VN + Google Play (1-2⭐), ~200 review phân tích | User gặp vấn đề → cần hỗ trợ → không ai giải đáp → bỏ app. Chatbot rule-based hiện tại không giải quyết được vấn đề thực tế. | Prototype cần chatbot AI thông minh hơn: hiểu ngữ cảnh, truy xuất DB user, trả lời cá nhân hóa thay vì khuôn mẫu. |
| 35+ reviews về lỗi đồng bộ, sai số dư, mất dữ liệu — user không hiểu tại sao dữ liệu sai | App Store VN + Google Play (1-3⭐) | User ghi chép chi tiêu mà không tin được số liệu → mất ý nghĩa sử dụng. Báo cáo tĩnh (biểu đồ) không giải thích được tại sao số dư lệch. | Chatbot cần query DB giao dịch thực tế để giải thích cho user: "Số dư lệch vì giao dịch X chưa đồng bộ" thay vì bảo "hãy đồng bộ lại". |
| Reviews đề cập báo cáo thiên lệch, biểu đồ chỉ hiện chi không hiện thu, kết xuất Excel sai | Google Play, App Store | User cần insight chi tiêu nhưng công cụ báo cáo hiện tại quá thô sơ, phải tự phân tích bằng Excel. | Chatbot cần trả lời câu hỏi mở bằng ngôn ngữ tự nhiên ("tháng này chi gì nhiều nhất?") thay vì ép user đọc biểu đồ tĩnh. |
| Cleo AI (6M+ users) và Rolly chứng minh chat-first + AI personality = engagement cao, user trẻ yêu thích | Competitor research (Cleo, Rolly, MoMo Moni, Monarch) | Thị trường quốc tế đã chuyển sang AI chatbot cho personal finance. MISA chưa có tính năng tương đương → khoảng trống lớn. | Prototype nên lấy chatbot làm giao diện chính (chat-first), không phải dashboard biểu đồ. |
| Review "quét hóa đơn kém hiệu quả, chưa tích hợp AI để tự phân loại chi tiêu" | App Store VN (3⭐) — Khả Lạc IU | User kỳ vọng AI phân loại tự động nhưng MISA chưa đáp ứng. Keyword matching hiện tại sai với merchant đa ngành. | Chatbot có thể kiêm luôn vai trò phân loại: user mô tả giao dịch → AI suggest category. |

## 3. Pain statement

```text
User MISA đã ghi chép chi tiêu ≥1 tháng, đang gặp khó ở bước "hiểu dữ liệu & nhận hỗ trợ",
vì (1) báo cáo trong app chỉ là biểu đồ tĩnh — không giải thích tại sao chi nhiều,
    (2) chatbot hiện tại chỉ xử lý ghi chép — mọi câu hỏi/yêu cầu sửa đều bị reject,
    (3) CSKH qua fanpage bị bơ hàng tháng, chatbot rule-based "cực kỳ ngu",
dẫn tới user không tin dữ liệu, không hiểu thói quen chi tiêu, và bỏ app.
Bằng chứng chính là:
  - 30+ reviews 1⭐ phàn nàn CSKH bị bơ (App Store + Google Play)
  - Screenshot 7: hỏi "200k nên phân loại vào phần nào" → bị reject
  - Screenshot 9: yêu cầu "chuyển sang phân loại ăn uống" → bị reject
  - Review NaDuo: "Chatbot tự động cực kỳ ngu, trả lời có như không"
```

## 4. Build slice

```text
Cho user MISA đã ghi chép chi tiêu ≥1 tháng, đang muốn hiểu chi tiêu và cần hỗ trợ,
prototype sẽ dùng AI ReAct Agent để:
  (a) Trả lời câu hỏi về chi tiêu bằng ngôn ngữ tự nhiên
      ("tháng này chi ăn uống bao nhiêu?", "so sánh với tháng trước")
  (b) Hỗ trợ CSKH thông minh — query DB user để trả lời cá nhân hóa
      ("tại sao số dư sai?", "giao dịch nào chưa đồng bộ?")
  (c) Cho phép sửa/chỉnh giao dịch qua chat
      ("chuyển khoản vừa rồi sang mục ăn uống")
tạo ra phản hồi text có ngữ cảnh, dựa trên dữ liệu thực của user,
và xử lý input mơ hồ bằng clarification loop (hỏi lại từng bước thay vì reject),
     câu hỏi ngoài phạm vi bằng fallback message rõ ràng,
     sửa sai bằng tool update_transaction với xác nhận trước khi thực hiện.
```

## 5. Auto/Aug decision

Chọn một:

- [ ] **Augmentation:** AI gợi ý/draft/phân loại, user quyết cuối.
- [x] **Conditional automation:** AI tự làm trong case hẹp; case mơ hồ/rủi ro chuyển người.
- [ ] **Automation:** AI tự quyết và tự hành động.

**Lý do chọn:** Tài chính cá nhân là domain nhạy cảm — AI sai phân loại hoặc sửa nhầm giao dịch có thể làm sai lệch toàn bộ báo cáo. Do đó: (1) Case rõ ràng (hỏi tổng chi ăn uống, so sánh tháng) → AI tự trả lời luôn. (2) Case mơ hồ (input thiếu info, phân loại không chắc) → AI hỏi clarification. (3) Case rủi ro (xóa/sửa giao dịch) → AI đề xuất + chờ user xác nhận trước khi thực hiện. Self-use screenshots chứng minh: chatbot hiện tại chỉ có accept/reject, thiếu bước clarification ở giữa — đây chính là khoảng trống Conditional Automation lấp vào.  
**Human role:** **Decider** — User quyết định có chấp nhận insight / sửa giao dịch hay không. AI không tự động thay đổi dữ liệu tài chính mà không có xác nhận.

## 6. Four paths

| Path | Prototype phải thể hiện gì? |
|---|---|
| **Happy** | User hỏi "Tháng này chi ăn uống bao nhiêu?" → Agent reasoning: đây là câu hỏi insight → gọi tool `query_spending_by_category(category="ăn uống", period="this_month")` → nhận kết quả 2.350.000đ → trả lời: "Tháng 6 bạn chi 2.350.000đ cho ăn uống, chiếm 35% tổng chi. Tăng 12% so với tháng 5, chủ yếu do 3 lần nhậu cuối tuần." *(Tham khảo: Screenshot 1-3 cho thấy chatbot đã xử lý tốt happy path với ghi chép)* |
| **Low-confidence** | User hỏi "100k" (chỉ số tiền, không rõ ý định) → Agent reasoning: thiếu info, không rõ muốn ghi chép hay hỏi gì → **hỏi clarification**: "Bạn muốn ghi khoản chi 100.000đ? Nếu vậy, chi cho mục gì ạ? Hay bạn muốn hỏi gì về số tiền 100k?" → Chờ user trả lời → tiếp tục xử lý. *(Cải thiện từ: Screenshot 4-5 — chatbot hiện tại reject luôn thay vì hỏi lại)* |
| **Failure** | User hỏi "Giúp tôi đầu tư chứng khoán" (ngoài phạm vi) → Agent reasoning: không liên quan đến chi tiêu/CSKH → trả lời fallback: "Mình chỉ hỗ trợ quản lý chi tiêu và giải đáp thắc mắc về app MISA thôi ạ. Nếu cần tư vấn đầu tư, bạn nên liên hệ chuyên gia tài chính nhé!" → Không tạo giao dịch, không query DB. *(Cải thiện từ: Screenshot 7 — chatbot không phân biệt được câu hỏi vs giao dịch)* |
| **Correction** | User: "Khoản nhậu 800k vừa rồi chuyển sang mục ăn uống đi" → Agent reasoning: đây là yêu cầu sửa → gọi tool `get_recent_transactions(keyword="nhậu")` → tìm thấy giao dịch 800k/Giao lưu → hỏi xác nhận: "Bạn muốn chuyển giao dịch 'Nhậu với bạn 800k' từ 'Giao lưu, quan hệ' sang 'Ăn uống' đúng không?" → User: "Đúng" → gọi `update_transaction_category(id=..., new_category="Ăn uống")` → "Đã chuyển thành công!" *(Khắc phục: Screenshot 9 — chatbot hiện tại reject hoàn toàn yêu cầu sửa)* |

## 7. Failure mode nguy hiểm nhất

```text
Nếu user hỏi "Tháng này chi bao nhiêu?" nhưng DB chỉ có dữ liệu đến ngày 15
(do lỗi đồng bộ hoặc user chưa nhập đủ),
AI có thể trả lời "Tháng này bạn chi 1.200.000đ" (con số thiếu nửa tháng)
mà không disclaimer rằng dữ liệu chưa đầy đủ,
hậu quả là user tin rằng mình chi ít → chi tiêu thoải mái → cuối tháng hết tiền,
hoặc user phát hiện sai → mất trust hoàn toàn vào AI → bỏ tính năng.
Prototype sẽ xử lý bằng:
  (1) Show source: luôn kèm "Dữ liệu từ 01/06 đến 15/06, 45 giao dịch"
  (2) Cảnh báo nếu phát hiện gap: "⚠️ Chưa có giao dịch nào từ 16/06, có thể bạn chưa nhập đủ"
  (3) Không bịa số: nếu không có data → nói rõ "Chưa có dữ liệu cho khoảng thời gian này"
Owner kiểm thử path này là [nhóm tự điền tên thành viên phụ trách test].
```

## 8. Owner plan cho sáng Day 06

| Thành viên | Việc phụ trách | Bằng chứng cần có trong repo |
|---|---|---|
| **Hoàng, Nghĩa** | Research | File Markdown phân tích đối thủ & kiến trúc |
| **Hiếu** | Evidence | File TXT/MD tổng hợp review, screenshots |
| **Hoàng** | SPEC | File Thin SPEC hoàn thiện |
| **Hoàng** | Prototype | Code Agent (ví dụ: `react_agent.py`) |
| **Nghĩa** | Test / failure path | Báo cáo test log (đặc biệt là lỗi thiếu data) |
| **Hưng** | Demo script / repo | Kịch bản demo (3 flow), Github Repo |
