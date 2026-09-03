# Memo quyết định — Day 28 · Dashboard Hành Động Cho Áp Dụng AI

**Case:** FintechGuard — trợ lý AI phát hiện gian lận/lừa đảo giao dịch
**Người dùng chính:** Chuyên viên rủi ro (risk analyst) xử lý cảnh báo giao dịch nghi ngờ
**Phạm vi quy trình (2–4 bước):**
1. Hệ thống chấm điểm rủi ro (rule-based) và gắn cờ giao dịch nghi ngờ
2. AI (LLM) sinh giải thích rủi ro bằng ngôn ngữ tự nhiên cho cảnh báo được gắn cờ
3. Chuyên viên xem xét cảnh báo và ra quyết định chặn / cho qua
4. Ghi log kết quả xử lý

---

## 1. Vấn đề và nguyên nhân gốc

**Triệu chứng quan sát được:** FintechGuard đã được triển khai và dùng để chấm điểm, gắn cờ và sinh giải thích cho giao dịch nghi ngờ, nhưng chuyên viên rủi ro vẫn có xu hướng tự tra cứu lại lịch sử giao dịch thủ công trước khi quyết định, thay vì dựa vào giải thích do AI cung cấp — công cụ được dùng (usage) nhưng cách làm việc chưa thực sự đổi (chưa tới mức adoption).

**Nguyên nhân gốc (2, có căn cứ):**

1. **Độ tin cậy (GỐC):** Giải thích rủi ro hiện do LLM viết lại từ kết quả chấm điểm rule-based, nhưng không trích dẫn rõ quy tắc (rule) cụ thể nào đã kích hoạt điểm rủi ro, và chưa có cơ chế QA mẫu hay báo lỗi khi giải thích sai. Chuyên viên không có căn cứ để biết khi nào nên tin giải thích của AI.
2. **Workflow / bàn giao (GỐC):** Bước chuyển giao giữa AI (gắn cờ + giải thích) và chuyên viên (quyết định cuối) chưa có checklist xác nhận hay cơ chế escalation rõ ràng cho các ca AI không chắc (điểm rủi ro ở vùng biên).

Đây là nguyên nhân gốc, không phải chỉ là triệu chứng "ít dùng" — chuyên viên vẫn mở và đọc giải thích AI (usage vẫn có), nhưng không dùng nó làm căn cứ chính cho quyết định.

## 2. Framework đã dùng và bằng chứng

- **5 câu hỏi chẩn đoán:** xác định trục Tin cậy và Workflow là hai điểm nghẽn chính; trục Con người (Desire) liên quan nhưng là hệ quả của Tin cậy, không phải nguyên nhân độc lập.
- **Gartner-Lite:** Direction ĐẠT (mục tiêu giảm gian lận & thời gian xử lý rõ ràng); Readiness và Absorption THIẾU — dữ liệu tham chiếu (vector store) chưa được xác nhận hoạt động ổn định, tài liệu vận hành (WORKLOG/JOURNAL/README) còn thiếu tại lần rà soát gần nhất, chưa có owner rõ cho việc theo dõi chất lượng giải thích AI. Kết luận: cần pilot nhỏ củng cố readiness trước khi mở rộng.
- **Mollick:** Chuyên viên giữ quyền quyết định cuối (chặn/cho qua); AI hỗ trợ ở bước chấm điểm + sinh giải thích; AI chỉ nên tự động hoàn toàn với các ca rủi ro cực cao có tiêu chí rõ ràng — hiện ngưỡng này chưa được định nghĩa, dẫn tới việc chuyên viên phải tự kiểm tra lại mọi ca thay vì chỉ tập trung vào ca biên.
- **ADKAR:** Nghẽn chính ở Awareness (chưa rõ khi nào nên tin AI) và Desire (ngại tin vì giải thích thiếu trích dẫn nguồn) — không phải Knowledge/Ability, nên giải pháp đào tạo đơn thuần sẽ không giải quyết được gốc rễ.
- **Bằng chứng:** quan sát nội bộ tại lần rà soát gần nhất — tài liệu vận hành (WORKLOG/JOURNAL/README) là khoảng trống chính; vector store phục vụ tra cứu ngữ cảnh tồn tại nhưng chưa xác nhận đang hoạt động ổn định.

## 3. Thay đổi sau kiểm tra chéo (≥2 thay đổi so với v1)

1. **Sửa chỉ số "mức dùng"** từ activity metric (đếm lượt mở giải thích AI) sang chỉ số gắn trực tiếp với quyết định thật: tỷ lệ cảnh báo mà chuyên viên ra quyết định dựa trên giải thích AI mà không cần tự tra cứu thủ công thêm.
2. **Bổ sung chỉ số mới:** "% giải thích AI có trích rõ quy tắc (rule) đã kích hoạt điểm rủi ro" — đo trực tiếp nguyên nhân gốc về độ tin cậy, thay vì chỉ đo gián tiếp qua QA mẫu.

## 4. Quyết định: SỬA (không dừng, chưa mở rộng)

Chưa nên rollout rộng ngay vì Readiness và Absorption theo Gartner-Lite còn THIẾU. Quyết định của nhóm là **sửa** thiết kế workflow và cách đo trước khi chuyển sang giai đoạn mở rộng, tập trung vào việc làm cho giải thích AI trích dẫn được nguồn rule cụ thể và bổ sung cơ chế escalation cho ca không chắc.

## 5. Lý do, bước tiếp theo và owner

**Lý do:** Nguyên nhân gốc nằm ở độ tin cậy của giải thích và cách bàn giao giữa AI–người, không phải ở việc chuyên viên thiếu kỹ năng hay công cụ chưa được cấp phát — vì vậy giải pháp ưu tiên là sửa kiến trúc tin cậy (trích nguồn rule, QA mẫu, escalation) trước khi tính đến đào tạo hay mở rộng số lượng người dùng.

**Bước tiếp theo (0–30 ngày):**
- Khoá phạm vi pilot: 1 nhóm chuyên viên rủi ro, giữ nguyên 4 quy trình đã định.
- Chỉ định rule owner và data owner cho vector store/tài liệu tham chiếu.
- Ghi mốc ban đầu cho các chỉ số trong Dashboard v2.

**Owner:** PM (Trưởng nhóm dự án FintechGuard) điều phối chung; Agent-Backend Lead phụ trách sửa logic sinh giải thích (trích rule) và vận hành escalation; Chủ quy trình xử lý cảnh báo phụ trách theo dõi thời gian xử lý và hành vi sử dụng.
