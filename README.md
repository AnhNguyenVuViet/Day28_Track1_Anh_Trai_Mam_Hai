# Day28_Track01_Anh_Trai_Mam_Hai

Lab Day 28 — Dashboard Hành Động Cho Áp Dụng AI
**Case:** FintechGuard — hệ thống AI phát hiện gian lận/lừa đảo giao dịch

## 1. Bảng thành viên

| Họ tên | MSSV | Phần phụ trách | Góp ý đã đưa cho nhóm bạn |
|---|---|---|---|
| Nguyễn Vũ Việt Anh | 2A202601742 | Chẩn đoán Gartner-Lite + kiến trúc tin cậy; tổng hợp dashboard & memo | Nhóm 01 (case AI tuyển dụng): dashboard v1 chỉ đếm lượt dùng mà chưa gắn với quyết định tuyển thật; Gartner-Lite thiếu đánh giá Absorption — chưa có bằng chứng HR thực sự thay đổi cách sàng lọc |
| Nguyễn Đình Quốc | 2A202601935 | ADKAR + thiết kế AS-IS/TO-BE | Nhóm 02 (case AI chẩn đoán y tế): sơ đồ AS-IS/TO-BE chưa thể hiện điểm bàn giao giữa AI và bác sĩ; ADKAR dừng ở Awareness mà chưa phân tích Desire — cần chỉ rõ vì sao bác sĩ ngại tin kết quả AI |
| Mai Tiến Mạnh | 2A202601922 | Mollick + lộ trình 30-60-90 | Nhóm 03 (case AI hỗ trợ chấm bài): lộ trình chưa chia rõ mốc 30-60-90 với deliverable cụ thể; phân vai Mollick chưa định nghĩa khi nào giáo viên cần can thiệp thay vì để AI chấm tự động |
| Nguyễn Đức Anh | 2A202601870 | Dashboard + QA số liệu | Nhóm 04 (case AI chatbot CSKH): một số metric thiếu baseline/target và nguồn dữ liệu; chưa có hành động khắc phục khi tỷ lệ hài lòng khách hàng dưới ngưỡng — cần thêm owner và escalation plan |

## 2. Phạm vi

- **1 sản phẩm AI:** FintechGuard — chấm điểm rủi ro (rule-based) + sinh giải thích rủi ro bằng LLM cho giao dịch bị gắn cờ
- **1 nhóm người dùng chính:** Chuyên viên rủi ro (risk analyst) xử lý cảnh báo giao dịch nghi ngờ
- **4 quy trình:** (1) chấm điểm & gắn cờ → (2) AI sinh giải thích → (3) chuyên viên xem xét & quyết định chặn/cho qua → (4) ghi log kết quả

## 3. Nguyên nhân gốc

1. **Độ tin cậy (GỐC):** giải thích AI không trích dẫn rõ quy tắc (rule) đã kích hoạt điểm rủi ro; chưa có QA mẫu hay cơ chế báo lỗi. → Chuyên viên vẫn tự tra cứu thủ công thay vì dựa vào AI.
2. **Workflow/bàn giao (GỐC):** chưa có checklist xác nhận hay cơ chế escalation rõ ràng cho ca AI không chắc (điểm rủi ro ở vùng biên).

Bằng chứng: quan sát nội bộ — tài liệu vận hành (WORKLOG/JOURNAL/README) là khoảng trống tại lần rà soát gần nhất; vector store tham chiếu tồn tại nhưng chưa xác nhận hoạt động ổn định (Gartner-Lite: Readiness & Absorption THIẾU).

## 4. Cách làm mới (AS-IS → TO-BE)

| TRƯỚC | SAU |
|---|---|
| AI chấm điểm, gắn cờ, sinh giải thích ngắn | Giữ nguyên chấm điểm, nhưng giải thích **trích rõ quy tắc/rule đã kích hoạt** (nguồn kiểm chứng) |
| Chuyên viên tự tra cứu thủ công song song vì không tin giải thích | Chuyên viên xác nhận theo **checklist ngắn**, là **người chịu trách nhiệm** cuối cùng |
| Không có xử lý riêng cho ca AI không chắc | Ca có điểm biên/độ chắc thấp **tự động chuyển (escalate)** cho chuyên viên cấp cao |
| Log quyết định không đồng nhất | Log kèm nhãn "AI đúng/sai" làm dữ liệu phản hồi (feedback loop) |

Ba thay đổi bắt buộc: **nguồn kiểm chứng** (trích rule) · **người chịu trách nhiệm** (chuyên viên xác nhận + escalation) · **cách xử lý khi AI không chắc** (chuyển ca).

## 5. Chỉ số (xem `dashboard/dashboard_hanh_dong_v2.xlsx`)

- ≥1 **product-level metric:** tỷ lệ cảnh báo chuyên viên ra quyết định dựa trên giải thích AI mà không cần tự tra cứu thủ công thêm
- ≥1 **workflow-level metric:** thời gian xử lý 1 cảnh báo (từ lúc gắn cờ đến lúc ra quyết định)
- Thêm chỉ số tin cậy trực tiếp: % giải thích AI có trích rõ rule đã kích hoạt
- Mỗi chỉ số đủ baseline · target · nguồn dữ liệu · owner · hành động khi chỉ số xấu (xem file xlsx)

## 6. Quyết định

**SỬA** — chưa mở rộng, chưa dừng. Ưu tiên sửa kiến trúc tin cậy (trích nguồn rule, QA mẫu, escalation) trước khi tính đến mở rộng số lượng người dùng. Chi tiết đầy đủ 5 phần tại `memo/memo_quyet_dinh.md`.

## 7. Cấu trúc repo

```
Day28_Track01_TenNhom/
├── README.md
├── dashboard/
│   └── dashboard_hanh_dong_v2.xlsx   ← bản v2 sau kiểm tra chéo
├── memo/
│   └── memo_quyet_dinh.md
└── v1/
    └── dashboard_hanh_dong_v1.xlsx   ← bản trước phản biện, để đối chiếu
```

## 8. Checklist trước khi nộp

- [x] Đổi tên thư mục đúng mẫu `Day28_Track01_<Ten_Nhom>`
- [x] Điền đầy đủ bảng thành viên (họ tên · MSSV · phần phụ trách · góp ý cho nhóm bạn)
- [x] Repo GitHub ở chế độ **Public**, nhánh `main`
- [x] Mở link ở cửa sổ ẩn danh để xác nhận xem được mà không cần đăng nhập
- [x] Không có dữ liệu nội bộ nhạy cảm của doanh nghiệp trong file
- [x] Mọi thành viên dán **cùng một link** vào LMS của mình
