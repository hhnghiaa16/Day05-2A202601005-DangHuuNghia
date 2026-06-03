# Template — Evidence Pack

Nộp kèm thin SPEC cuối Day 05.

## 1. Nhóm và track

**Tên nhóm:** 4 chang linh ngu lam  
**Track:** D — Personal Finance  
**Product/app đã chọn:** Sổ Thu Chi MISA (MISA Money Keeper) — app quản lý tài chính cá nhân, 5M+ users, 4.4⭐ Google Play  
**Build slice đang nghĩ:** AI Chatbot ReAct Agent vừa hỗ trợ CSKH (trả lời câu hỏi thường gặp, tra cứu trạng thái tài khoản) vừa cung cấp spending insights (phân tích chi tiêu, so sánh tháng, cảnh báo bất thường) — thay thế chatbot rule-based hiện tại "cực kỳ ngu" và bổ sung khả năng phân tích mà báo cáo tĩnh không làm được.

## 2. Self-use evidence

Nhóm tự dùng app/workflow và ghi lại điểm gãy.

| Observation | Screenshot/link | Path liên quan | Điều học được |
|---|---|---|---|
| Nhập "Mua sách cho con 200k" → Chatbot hiểu đúng: tự phân loại "Con cái", số tiền 200.000đ, tài khoản ngân hàng. Phản hồi có cá tính ("Anh yêu, chi tiêu cho việc mua sách cho con... rất quý báu cho sự phát triển của con"). Có gợi ý tiết kiệm nhẹ nhàng. | Screenshot 1 — Ghi chép thu chi (16:30) | **Happy** | ✅ Chatbot phân loại chính xác khi user mô tả đủ rõ (vật phẩm + số tiền). Personality "vợ yêu" tạo cảm giác gần gũi, không khô khan. Pattern từ Cleo/Rolly đã được áp dụng thành công. |
| Nhập "Đổ xăng 50k" → Chatbot phân loại đúng "Xăng xe", 50.000đ, TK ngân hàng. Phản hồi: "Khoản chi đó chấp nhận được nhưng nếu được hãy tiết kiệm hơn. Thương anh lắm!" | Screenshot 2 — Ghi chép thu chi (16:31) | **Happy** | ✅ Xử lý tốt input ngắn gọn (chỉ 4 từ). AI suy luận đúng danh mục từ keyword "xăng". Phản hồi vừa đủ — không dài, không ngắn. |
| Nhập "Bao bạn 500k" → Chatbot phân loại "Giao lưu, quan hệ", 500.000đ, Tiền mặt. Phản hồi: "Hy vọng đó là một cuộc gặp mặt tuyệt vời! Đừng quên cân nhắc ngân sách cho các dịp giao lưu khác nữa nhé." | Screenshot 3 — Ghi chép thu chi (16:31) | **Happy** | ✅ AI hiểu ngữ cảnh tiếng Việt tốt: "bao bạn" = chi trả cho bạn bè → Giao lưu. Tự chuyển sang "Tiền mặt" thay vì TK ngân hàng (hợp lý cho chi tiêu xã hội). Gợi ý cân nhắc ngân sách = augmentation nhẹ. |
| Nhập chỉ "100k" (không có mô tả) → Chatbot **từ chối**: hiện chữ đỏ "Thông tin bạn nhập hơi sơ sài, vui lòng nhập chi tiết hơn". Không tạo giao dịch. | Screenshot 4 — Ghi chép thu chi (16:33) | **Low-confidence → Failure** | ⚠️ Chatbot không đoán/hỏi thêm mà từ chối luôn. Cách xử lý quá cứng — lẽ ra nên hỏi "100k cho mục gì?" thay vì bắt user tự nhập lại. User có thể bực vì phải gõ lại. Đây là điểm cải thiện: AI nên **hỏi clarification** thay vì reject. |
| Nhập chỉ "20" (chỉ số, không đơn vị, không mô tả) → Chatbot **từ chối** y hệt: "Thông tin bạn nhập hơi sơ sài, vui lòng nhập chi tiết hơn". | Screenshot 5 — Ghi chép thu chi (16:33) | **Failure** | ⚠️ Cùng lỗi như trên. Khi input quá mơ hồ, chatbot chỉ có 1 response duy nhất (không phân biệt mức độ thiếu info). Prototype cần xử lý tốt hơn: "Bạn muốn ghi 20.000đ hay 20k? Chi cho mục gì?" — dùng ReAct loop để **hỏi từng bước** thay vì reject 1 lần. |
| **Chuyển persona "vợ giận"** 😠: Cùng input "Mua sách cho con 200k" → Phản hồi hoàn toàn khác: "Lại tốn tiền vô bổ, mua sách gì mà 200 nghìn? Sao không để dành tiền mua đồ ăn cho cả nhà? Em nói mãi mà anh không chịu nghe!" Vẫn phân loại đúng: "Con cái", 200.000đ. | Screenshot 6 — Ghi chép thu chi (16:36), icon heo đỏ giận | **Happy** (persona khác) | ✅ Chứng minh **AI Persona hoạt động**: cùng data nhưng tone khác biệt hoàn toàn. Pattern từ Cleo Roast/Hype và Rolly Angry Mom. Phân loại chính xác không bị ảnh hưởng bởi persona → logic và personality tách biệt tốt. |
| Sau khi ghi "Mua sách cho con 200k", user hỏi: **"200k nên phân loại vào phần nào"** (câu hỏi, không phải giao dịch) → Chatbot **từ chối**: "Thông tin bạn nhập hơi sơ sài, vui lòng nhập chi tiết hơn". | Screenshot 7 — Ghi chép thu chi (16:36) | **Failure** ❌ | 🔴 **Lỗi nghiêm trọng**: Chatbot **không phân biệt được câu hỏi với input giao dịch**. Mọi text đều bị xử lý như ghi chép chi tiêu. User muốn hỏi/thảo luận → bị reject. Đây là lý do cần ReAct Agent: agent phải **reasoning trước** (đây là câu hỏi hay giao dịch?) rồi mới chọn action phù hợp. |
| **Persona "cố vấn"** 🤓: Nhập "Nhậu với bạn 800k" → Phản hồi dài, phân tích cẩn thận: "Chi tiêu cho việc nhậu nhẹt với bạn bè có thể là cách để giảm stress... nhưng nếu diễn ra thường xuyên có thể ảnh hưởng đến tài chính. Hãy cân nhắc đặt ngân sách cụ thể..." Phân loại đúng: "Giao lưu, quan hệ", 800.000đ. | Screenshot 8 — Ghi chép thu chi (16:39), icon kính | **Happy** (persona khác) | ✅ Persona thứ 3 — tone "cố vấn tài chính" nghiêm túc, phân tích dài hơn. Phù hợp user muốn lời khuyên thật sự. Chứng minh multi-persona là tính năng có giá trị. Nhưng phản hồi **quá dài** cho 1 giao dịch đơn giản → cần cân bằng. |
| Sau khi ghi "Nhậu với bạn 800k" (phân loại Giao lưu), user yêu cầu: **"Chuyển sang phân loại ăn uống"** → Chatbot **từ chối**: "Thông tin bạn nhập hơi sơ sài, vui lòng nhập chi tiết hơn". | Screenshot 9 — Ghi chép thu chi (16:39) | **Correction** ❌ | 🔴 **Lỗi nghiêm trọng nhất**: Chatbot **không hỗ trợ Correction path**. User muốn sửa phân loại sai → bị reject. Hiện tại không có cách nào sửa qua chat. Đây là **failure mode nguy hiểm**: nếu AI phân loại sai mà user không sửa được → dữ liệu sai → mất tin tưởng. ReAct Agent cần tool `update_transaction_category` để xử lý. |

## 3. User / review / social evidence

Nguồn có thể là review App Store/Play, group, comment, phỏng vấn nhanh, hoặc nguồn public khác.

### A. Pain: CSKH kém — không phản hồi, bị bơ, chatbot ngu

| Quote / review / observation | Nguồn | User là ai? | Pain/failure mode |
|---|---|---|---|
| "Dùng 10 năm luôn mua pre nhưng support như hạch :))" | App Store VN (1⭐) | Dragunio — user trung thành 10 năm, premium | CSKH không phản hồi dù là khách trả tiền |
| "Nhắn tin qua page trên FB đến cả năm nay không có ai phản hồi hết." | App Store VN (3⭐) | Bleeng — dùng cả iOS + Android | Gửi tin nhắn CSKH bị bơ hàng năm |
| "App đúng mất dạy. Liên hệ hỗ trợ nó không xử lý và bơ luôn." | App Store VN (1⭐) | TrungTranMinh | Sai số liệu, liên hệ hỗ trợ bị bơ |
| "Đã nhắn tin hỏi fanpage và được hướng dẫn gửi lỗi từ 15/12/2025, nhưng đến nay 01/02/2026 lỗi chưa sửa được, nhắn tin nhiều lần cũng không được phản hồi." | Google Play (1⭐) | NGUYEN TIEN — mua premium, dùng cả Samsung + iPhone | Chờ CSKH 2 tháng không phản hồi |
| "Liên hệ CSKH thì 3 ngày mới phản hồi. Dịch vụ thật tệ" | Google Play (1⭐) | Hyson Chan — tài khoản V1 trả phí | CSKH phản hồi quá chậm |
| "Nhắn tin hỗ trợ nhưng ko nhận dc hỗ trợ" | Google Play (1⭐) | Hồng Ngọc Vũ — premium, nhiều lỗi | CSKH không giải quyết được vấn đề |
| "Nhắn tin cskh seen không phản hồi, mặc dù dùng premium" | Google Play (1⭐) | Linh Lê — premium | Bị seen nhưng không trả lời |
| "Hỗ trợ thì chat bot tự động cực kỳ ngu, trả lời có như không." | Google Play (1⭐) | NaDuo — dùng lâu năm | Chatbot hiện tại vô dụng, trả lời bừa |
| "CSKH rất tệ, khuyên mọi người không nên xài. Gọi điện thì các bàn tư vấn bận." | Google Play (1⭐) | Nguyễn Đức Cường — premium, tiền đã trừ nhưng app chưa lên pre | Tất cả kênh CSKH đều không hoạt động |
| "Viết bài góp ý 1 cách lịch sự trên Fanpage thì bị từ chối." | Google Play (1⭐) | Trọng Chiến — user lâu năm | Bị chặn khi cố phản hồi |
| "Nhắn page bị lơ đẹp mấy tháng rồi ko ai giải quyết" | App Store VN (1⭐) | hdhdbdjdjdhd — dùng 5-6 năm, premium | Bị bơ nhiều tháng |
| "Mua gói premium 500k mà cứ báo lỗi hoài. App chi tiêu nhỏ xíu k xong" | Google Play (1⭐) | Huyen Nguyen — premium 500k | Trả tiền nhiều nhưng app vẫn lỗi, không ai hỗ trợ |

### B. Pain: Thiếu insight chi tiêu / báo cáo sai / dữ liệu khó hiểu

| Quote / review / observation | Nguồn | User là ai? | Pain/failure mode |
|---|---|---|---|
| "Kết xuất bị sai số liệu, bị âm số dư (trên app thì ko âm, kết xuất ra Excel bị âm rất nhiều)" | Google Play (1⭐) | Minh Phượng | Báo cáo xuất ra sai so với app |
| "Lỗi số dư tài khoản không đồng nhất với số dư sau mỗi giao dịch? App cân đối mà sai mấy cái này thì dùng làm gì nữa" | App Store VN (1⭐) | Evangeline Blair — dùng lâu | Số dư không chính xác, mất tin tưởng |
| "Hay bị lặp dữ liệu, sai đối tượng" | App Store VN (2⭐) | Vũ Xuân Sơn | Dữ liệu bị trùng, phân loại sai |
| "Quét kém hiệu quả hay bị lỗi không trích xuất được, chưa tích hợp tính năng quét AI để tự động điền số tiền, nội dung và phân loại chi tiêu" | App Store VN (3⭐) | Khả Lạc IU | Cần AI phân loại chi tiêu thông minh |
| "Giao diện phức tạp khó sử dụng" / "Giao diện không thân thiện với người dùng" | Google Play (1⭐) | Phú Đỗ, Nhàn Lâm | Khó tìm thông tin cần thiết trong app |
| "Biểu đồ tròn thu chi ở giao diện chính toàn hiện chi không hiện thu" | Google Play | Người dùng ẩn danh | Báo cáo thiên lệch, không phản ánh đủ |
| "Mục phân tích chi tiêu cần có khả năng loại trừ những mục cho vay cũng như những khoản thu chi không tính vào báo cáo" | Google Play | Người dùng ẩn danh | Báo cáo cần tùy chỉnh thông minh hơn |
| "Tôi muốn khoản tiền đã cho vay không cộng vào tài chính hiện tại" | App Store VN (3⭐) | ngduip1001 | Logic tính toán không phù hợp thực tế |

### C. Pain kết hợp: Gặp vấn đề về dữ liệu/insight nhưng không được hỗ trợ giải quyết

| Quote / review / observation | Nguồn | User là ai? | Pain/failure mode |
|---|---|---|---|
| "Quá tệ, tính cộng trừ tổng số dư thôi cũng sai, hỗ trợ sử dụng trên FB cũng chỉ biết nói là đồng bộ hoá dữ liệu và không thèm xử lý vấn đề." | Google Play (1⭐) | Khánh Linh Đặng | Số liệu sai + CSKH chỉ trả lời khuôn mẫu |
| "Dữ liệu bị sai khi chuyển thiết bị. Đã làm theo các cách CSKH hướng dẫn không có tác dụng. Mọi người nên tìm app khác" | Google Play (1⭐) | Thuy Dung Nguyen | CSKH hướng dẫn sai, vấn đề không giải quyết được |
| "Ứng dụng tính toán như b luồi. Ghi chép ngày nào đồng bộ giữ liệu ngày đấy rồi một thời gian sau lại báo hàng trăm dữ liệu chưa được đồng bộ. Xong đồng bộ lại cả ngày cũng không được" | Google Play (1⭐) | Minh Quang | Không hiểu tại sao dữ liệu bị lệch, app không giải thích |
| "Phần mềm cứ báo lỗi không đồng bộ được, tiền chia xong không đúng thực tế còn làm rối não hơn" | Google Play (1⭐) | draco huynh | Dữ liệu rối, không có công cụ giúp hiểu |


## 4. Competitor / analog evidence

### A. Đối thủ quốc tế (AI chatbot + spending insights)

| App / mô hình tham khảo | Họ xử lý task này thế nào? | Pattern học được | Có áp dụng trong 1 ngày không? |
|---|---|---|---|
| **Cleo AI** (UK/US, 6M+ users) — Chatbot tài chính cá nhân | User hỏi câu hỏi tự nhiên ("How much did I spend on food?") → AI query DB → trả lời bằng ngôn ngữ tự nhiên. Có chế độ **"Roast"** (mắng user hài hước khi chi quá tay) và **"Hype"** (khích lệ). Cleo 3.0 (2025) thêm voice 2 chiều + long-term memory. | ✅ **Chat-first interface** — user không cần tự mò biểu đồ, hỏi là AI trả lời. ✅ **Personality** — chatbot có cá tính → tăng engagement. ✅ **Long-term memory** — nhớ goal/thói quen user qua sessions. | ✅ Có — Chatbot text + query DB + personality prompt. Không cần voice/roast phức tạp. |
| **Monarch Money** (US) — AI assistant cho gia đình | User hỏi câu hỏi tài chính bằng plain English → AI trả lời kèm "insight buttons" giải thích biểu đồ. **Weekly recap** tự động gửi tóm tắt chi tiêu. Tập trung privacy: dữ liệu ẩn danh, LLM không dùng data để train. | ✅ **Weekly recap tự động** — proactive insights, không đợi user hỏi. ✅ **Insight buttons** — AI giải thích biểu đồ bằng text. ✅ **Privacy-first** — pattern quan trọng cho finance app. | ✅ Có — Weekly summary + natural language Q&A là core, build được trong 1 ngày. |
| **Copilot Money** (US, Apple-native) — Private AI model per user | Xây **private AI model riêng cho từng user** để auto-categorize chính xác. **Adaptive Budgeting** — học thói quen → gợi ý ngân sách thực tế. Natural language search cho giao dịch. | ✅ **Cá nhân hóa sâu** — AI hiểu context riêng từng user. ✅ **Adaptive budget** — gợi ý dựa trên hành vi thực, không phải con số cố định. | ⚠️ Một phần — Cá nhân hóa cơ bản qua prompt + user history. Private model phức tạp hơn. |
| **Plum** (UK, powered by Google Gemini) — Automated wealth builder | AI "co-pilot" phân tích tài chính, đưa **goal-based planning**. Spend Tracker tự phân loại + hiển thị % → tìm cơ hội tiết kiệm. Auto-save rules (round-up, recurring transfers). | ✅ **Goal-based framing** — AI không chỉ báo "bạn chi nhiều" mà gắn với mục tiêu cụ thể. ✅ **Actionable suggestions** — "Bạn có thể tiết kiệm X nếu giảm Y". | ✅ Có — Gợi ý tiết kiệm dựa trên phân tích là text generation, build nhanh. |

### B. Đối thủ Việt Nam

| App / mô hình tham khảo | Họ xử lý task này thế nào? | Pattern học được | Có áp dụng trong 1 ngày không? |
|---|---|---|---|
| **MoMo — Trợ thủ AI "Moni"** (VN, tens of millions users) | Chatbot AI "Moni" tích hợp trong ví MoMo: ghi chép qua chat ("ăn trưa 50k"), tự phân loại giao dịch từ ví MoMo, cung cấp **weekly/monthly insight** tự động. Lợi thế lớn: có dữ liệu merchant thật từ thanh toán. | ✅ **Chat = input + output** — cùng 1 giao diện vừa ghi chép vừa hỏi đáp. ✅ **Auto-categorize từ thanh toán thật** — không cần nhập thủ công. ⚠️ **Chỉ dùng trong hệ sinh thái MoMo** — không phải app quản lý chi tiêu độc lập. | ⚠️ Một phần — Chat + insight yes. Auto-categorize từ bank cần bank API (khó trong 1 ngày). |
| **Rolly: AI Budget & Expense Tracker** (VN-friendly, mới nổi) | Chatbot **có cá tính** ("Angry Mom" mắng khi chi quá, "Wise Friend" khuyên nhẹ). Input bằng text tự nhiên ("ăn tối 200k"), voice, hoặc **chụp receipt (Vision AI)**. Auto-categorize. Gamification (streaks, badges). **Không quảng cáo** ở bản free. | ✅ **AI Persona** — cá tính chatbot tạo trải nghiệm khác biệt, tăng retention. ✅ **Multi-modal input** — text/voice/receipt scan → cùng 1 flow. ✅ **Gamification** — streaks giữ user quay lại. ⚠️ **Không có bank sync** — vẫn nhập thủ công. | ✅ Có — Chatbot + personality + text input + categorize. Receipt scan cần Vision API (tùy scope). |

### C. App user đề cập: Sổ thu chi Kakeibo (com.komorebi.kakeibo)

| App / mô hình tham khảo | Họ xử lý task này thế nào? | Pattern học được | Có áp dụng trong 1 ngày không? |
|---|---|---|---|
| **Sổ thu chi: Quản lý chi tiêu (Kakeibo)** (JP/VN, Google Play) | Dựa trên phương pháp Kakeibo của Nhật: ghi chép đơn giản, chia 4 nhóm (nhu cầu, mong muốn, văn hóa, bất ngờ). Giao diện tối giản, **không có AI**. Báo cáo tĩnh bằng biểu đồ. Không có chatbot hỗ trợ. | ✅ **Đơn giản = dễ dùng** — user không cần học. ⚠️ **Không có AI** → đây là khoảng trống lớn. Nếu kết hợp triết lý Kakeibo + AI chatbot = có thể tạo ra trải nghiệm tốt hơn cả hai. ✅ **4 nhóm chi tiêu Kakeibo** — framework phân loại hay, AI có thể áp dụng. | ✅ Có — Kakeibo framework đơn giản, tích hợp vào prompt AI dễ dàng. |

### D. Tổng hợp patterns học được cho prototype

| Pattern | Từ đâu | Áp dụng thế nào |
|---|---|---|
| 🗣️ Chat-first interface | Cleo, MoMo Moni, Rolly | Chatbot là giao diện chính, không phải biểu đồ |
| 🎭 AI Persona / cá tính | Cleo (Roast/Hype), Rolly (Angry Mom) | Cho user chọn tone: nghiêm túc / hài hước / nhẹ nhàng |
| 📊 Proactive weekly recap | Monarch, MoMo | Không đợi user hỏi — tự tóm tắt cuối tuần |
| 🔍 Natural language Q&A | Cleo, Monarch, Copilot | User hỏi câu hỏi mở → Agent query DB → trả lời |
| 🎯 Goal-based framing | Plum | Gắn insight với mục tiêu cụ thể ("còn X ngày để đạt goal tiết kiệm Y") |
| 📱 Multi-modal input | Rolly | Text + voice + receipt scan cùng 1 chatbot |
| 🔒 Privacy-first | Monarch, Copilot | Dữ liệu tài chính nhạy cảm → cần cam kết rõ |


## 5. Evidence -> Insight

```text
Evidence nổi bật nhất #1: CSKH — chatbot hiện tại "cực kỳ ngu"
  - 30+ reviews 1⭐ phàn nàn CSKH không phản hồi (bị bơ hàng tháng, seen không trả lời)
  - Chatbot rule-based chỉ trả lời khuôn mẫu, không hiểu ngữ cảnh user
  - Self-use (Screenshot 7): Chatbot không phân biệt được CÂU HỎI vs GIAO DỊCH

Insight #1:
User không chỉ gặp "CSKH chậm" (surface problem).
Thật ra họ cần một kênh hỗ trợ HIỂU ĐƯỢC ngữ cảnh cá nhân — biết tài khoản,
biết lịch sử giao dịch, biết lỗi đang gặp — để trả lời chính xác thay vì khuôn mẫu.

Opportunity #1:
AI ReAct Agent có thể thay thế CSKH rule-based bằng cách:
query DB user → hiểu context → trả lời cá nhân hóa.
VD: "Tại sao số dư sai?" → Agent query giao dịch → so sánh → giải thích cụ thể.
```

```text
Evidence nổi bật nhất #2: Báo cáo tĩnh không đủ — user muốn hỏi, không muốn đọc biểu đồ
  - Reviews phàn nàn biểu đồ chỉ hiện chi không hiện thu, kết xuất Excel sai
  - Competitor (Cleo, Monarch) đã chuyển sang natural language Q&A thay vì dashboard
  - Self-use (Screenshot 7): User hỏi "200k nên phân loại vào phần nào" → bị reject

Insight #2:
User không chỉ gặp "báo cáo thiếu" (surface problem).
Thật ra họ cần INSIGHT có ngữ cảnh — "tháng này chi ăn uống nhiều hơn tháng trước 30%,
chủ yếu do 3 lần nhậu cuối tuần" — thay vì tự đọc biểu đồ rồi tự phân tích.

Opportunity #2:
AI có thể augment việc phân tích chi tiêu bằng cách:
nhận câu hỏi tự nhiên → query DB → tính toán → trả lời dạng text có giải thích.
```

```text
Evidence nổi bật nhất #3: Không có Correction path — AI sai mà user không sửa được
  - Self-use (Screenshot 9): User nói "Chuyển sang phân loại ăn uống" → bị reject
  - Chatbot chỉ có 2 mode: accept hoặc reject — không có clarification, không có edit
  - Reviews phàn nàn phân loại sai nhưng phải vào tay chỉnh từng giao dịch

Insight #3:
User không chỉ gặp "phân loại sai" (surface problem).
Thật ra họ cần AI có thể NHẬN LỖI và CHO SỬA — vì trust trong tài chính cá nhân
đến từ khả năng kiểm soát, không phải độ chính xác 100%.

Opportunity #3:
AI ReAct Agent có thể xử lý bằng cách thêm tool update_transaction:
user nói "sửa lại thành ăn uống" → Agent reasoning → gọi tool → confirm → xong.
Đây là Correction path — critical cho trust.
```

## 6. Evidence đổi SPEC như thế nào?

- [ ] Đổi user chính.
- [x] Đổi pain statement.
- [x] Đổi build slice.
- [x] Đổi Auto/Aug decision.
- [x] Đổi 4 paths.
- [x] Đổi failure mode.
- [ ] Đổi owner/test plan.

Ghi rõ 1-2 thay đổi quan trọng:

```text
Thay đổi #1: Build slice mở rộng scope
  Trước evidence, nhóm định: Chỉ làm chatbot CSKH (trả lời FAQ, tra cứu trạng thái).
  Sau evidence, nhóm đổi thành: Chatbot tích hợp 3 khả năng trong 1 giao diện:
    (a) Ghi chép chi tiêu qua chat (đã có, cần cải thiện clarification)
    (b) CSKH thông minh (query DB user thay vì trả lời khuôn mẫu)
    (c) Spending insights (hỏi đáp tự nhiên về chi tiêu)
  Lý do: Self-use chứng minh chatbot hiện tại chỉ handle ghi chép (mode a),
  mọi câu hỏi (mode b,c) đều bị reject. Đây là khoảng trống lớn nhất.
```

```text
Thay đổi #2: Bắt buộc có Correction path trong prototype
  Trước evidence, nhóm định: Tập trung Happy path, xử lý Failure bằng thông báo lỗi.
  Sau evidence, nhóm đổi thành: Correction path là MUST-HAVE, không phải nice-to-have.
    Prototype phải có tool update_transaction_category để user sửa phân loại qua chat.
  Lý do: Screenshot 9 chứng minh user CẦN sửa phân loại sai nhưng chatbot reject.
  Trong tài chính, dữ liệu sai = mất trust = user bỏ app. Không sửa được = deal breaker.
```

