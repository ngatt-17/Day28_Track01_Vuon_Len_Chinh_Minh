# Báo Cáo Thực Hành Lab 28 — Áp Dụng AI & Dashboard Hành Động
**Track 01 — Nhóm: Vươn Lên Chính Mình**  
**Dự án:** Internova AI Platform (Hệ thống AI hỗ trợ thực tập sinh viên VinUni)  
**Demo hệ thống:** [https://internova-ai-platform.vercel.app/](https://internova-ai-platform.vercel.app/)

---

## 1. Bảng thành viên & Đóng góp phản biện

| Họ tên | MSSV | Phần phụ trách trong Lab | Góp ý đã đưa cho nhóm bạn (Nhóm 05) |
| :--- | :--- | :--- | :--- |
| **Vũ Huy Hoàng** | 2A202601057 | Trưởng nhóm, Kỹ thuật AI, Thiết kế Dashboard v1 & v2 | Nhóm 05: Đề xuất chuyển chỉ số "Số lượng đăng nhập" (tầng 1) sang "Tỷ lệ tra cứu thành công không cần hỏi lại cán bộ" (tầng 3) để đo giá trị thực. |
| **Tạ Thị Nga** | 2A202601125 | Chẩn đoán ADKAR, Thiết kế quy trình AS-IS / TO-BE & Khảo sát User | Nhóm 05: Khuyến nghị bổ sung nút "Báo lỗi câu trả lời" và quy định thời hạn xử lý dữ liệu sai lệch (SLA 24h) để củng cố niềm tin của người dùng. |
| **Trần Hoài Nam** | 2A202601751 | Đánh giá Gartner-Lite, Lộ trình 30–60–90 ngày & Memo quyết định | Nhóm 05: Đề xuất mốc chuyển giai đoạn 30–60–90 phải dựa trên bằng chứng dữ liệu kiểm tra mẫu thay vì chỉ tính theo ngày lịch cứng. |

---

## 2. Phạm vi bài toán đã khóa
* **01 Sản phẩm AI:** Internova AI Chatbot (Trợ lý tra cứu quy chế, thủ tục và biểu mẫu thực tập thuộc hệ thống Internova AI Platform).
* **01 Nhóm người dùng chính:** Sinh viên VinUni chuẩn bị hoặc đang trong kỳ thực tập học kỳ.
* **03 Quy trình nghiệp vụ cốt lõi:**
  1. *Quy trình 1:* Tra cứu điều kiện tiên quyết và quy định xét duyệt thực tập.
  2. *Quy trình 2:* Tra cứu mốc thời gian (timeline), thủ tục nộp hồ sơ và biểu mẫu tiếp nhận.
  3. *Quy trình 3:* Tra cứu quy định nộp báo cáo tiến độ và tiêu chí đánh giá kết quả thực tập.
* **Vấn đề quan sát được (Triệu chứng):** Sinh viên dùng thử chatbot 1–2 lần rồi quay lại nhắn tin hỏi trực tiếp cán bộ thực tập hoặc hỏi group chat thủ công, khiến cán bộ quá tải tin nhắn lặp lại.

---

## 3. Nguyên nhân gốc & Bằng chứng thực tế
* **Nguyên nhân gốc 1 (Độ tin cậy — Trust Architecture):** Câu trả lời của chatbot trước đây chưa hiển thị trích dẫn nguồn văn bản chính thức (Sổ tay thực tập điều mấy, trang nào, ban hành ngày nào), khiến sinh viên lo ngại AI bị "ảo giác" (hallucination) dẫn đến làm sai quy chế và bị trượt thực tập.
* **Nguyên nhân gốc 2 (Quy trình bàn giao — Workflow Handoff):** Chưa có cơ chế chuyển giao câu hỏi cho cán bộ phụ trách khi AI gặp câu hỏi phức tạp/ngoại lệ, làm người dùng cảm thấy bế tắc khi AI trả lời không trúng ý.
* **Framework đã sử dụng:**
  - `Gartner-Lite`: Hướng đi đạt yêu cầu, nhưng mức sẵn sàng về kiểm duyệt dữ liệu Knowledge Base và khả năng hấp thụ vận hành còn thiếu.
  - `Mollick`: Xác định rõ AI chỉ đóng vai trò hỗ trợ trích dẫn và tóm tắt; Sinh viên là người kiểm chứng đối chiếu; Ban quản lý thực tập phê duyệt hồ sơ và xử lý ngoại lệ.
  - `ADKAR`: Xác định điểm nghẽn nghiêm trọng nhất nằm ở **Desire** (Sợ sai quy chế nên không dám tin dùng AI).
* **Bằng chứng thực tế:** Khảo sát phỏng vấn nhanh 5 sinh viên VinUni trong đợt thử nghiệm Day 27: Có 4/5 sinh viên phản hồi họ phải nhắn tin hỏi lại cán bộ vì chatbot không dẫn link văn bản quy định chính thức.

---

## 4. Cách làm mới (Quy trình TO-BE)
Quy trình mới được thiết kế lại bắt buộc phải có đủ 3 yếu tố:
1. **Nguồn kiểm chứng (Trust Architecture):** Mọi câu trả lời của AI bắt buộc đi kèm trích dẫn văn bản chính thức: *Tên văn bản + Điều khoản/Trang + Ngày cập nhật mới nhất*.
2. **Người chịu trách nhiệm (Accountability):** 
   - Sinh viên chịu trách nhiệm kiểm tra nguồn đối chiếu trước khi nộp hồ sơ.
   - Admin Trường (Internship Coordinator) chịu trách nhiệm cập nhật và kiểm duyệt văn bản vào Knowledge Base.
3. **Cách xử lý khi AI không chắc chắn / Báo lỗi (Fallback & Escalation):** 
   - Tích hợp nút *"Gửi câu hỏi này tới Cán bộ phụ trách"* khi AI không tìm thấy tài liệu với độ tin cậy cao.
   - Tích hợp nút *"Báo câu trả lời sai"* với cam kết xác minh dữ liệu trong vòng 24 giờ (SLA 24h).

---

## 5. Chỉ số đo lường quyết định (Dashboard Summary)
* **Product Metric (Cấp sản phẩm):** 
  - *Chỉ số:* **Tỷ lệ câu trả lời có trích nguồn chính xác và được kiểm tra mẫu**
  - *Baseline:* $50\%$ | *Target:* $\ge 95\%$ | *Nguồn:* Audit log kiểm tra mẫu 50 câu/tuần | *Owner:* Vũ Huy Hoàng
* **Workflow Metric (Cấp quy trình):** 
  - *Chỉ số:* **Tỷ lệ sinh viên tự giải quyết tra cứu thành công (không phải hỏi lại cán bộ)**
  - *Baseline:* $30\%$ | *Target:* $\ge 80\%$ | *Nguồn:* Log hệ thống & Phản hồi hữu ích | *Owner:* Tạ Thị Nga
* **Value Metric (Giá trị nghiệp vụ):**
  - *Chỉ số:* **Số lượng tin nhắn hỏi thủ công lặp lại gửi tới cán bộ thực tập**
  - *Baseline:* ~150 tin nhắn/tuần | *Target:* $\le 30$ tin nhắn/tuần (Giảm 80%) | *Nguồn:* Thống kê kênh hỗ trợ | *Owner:* Trần Hoài Nam

---

## 6. Quyết định cuối cùng
* **Quyết định:** **TIẾP TỤC TRIỂN KHAI (GO)** theo lộ trình 30–60–90 ngày đã điều chỉnh.
* **Lý do:** Nhu cầu tra cứu của sinh viên là rất lớn, nguyên nhân gốc đã được xác định chính xác và giải quyết triệt để thông qua cơ chế trích nguồn, quy trình bàn giao cán bộ và cam kết SLA 24h.
* **02 Thay đổi lớn so với bản v1 (sau phản biện chéo):**
  1. Bổ sung chỉ số đo lường *Tỷ lệ báo lỗi thông tin sai lệch ($< 3\%$)* kèm nút báo cáo trực tiếp tại giao diện chat.
  2. Bổ sung cam kết thời gian xử lý sự cố (SLA 24h) và quy trình tạm khóa câu trả lời khi có phản ánh sai sót để bảo vệ an toàn thông tin cho sinh viên.

---

## 7. Cấu trúc thư mục bài nộp
```text
Day28_Track01_Vuon_Len_Chinh_Minh/
├── README.md                          ← Báo cáo tổng quan bài Lab 28
├── dashboard/
│   └── dashboard_hanh_dong_v2.xlsx    ← Bảng Dashboard Hành Động v2 hoàn thiện
├── memo/
│   └── memo_quyet_dinh.md             ← Bản Memo quyết định chi tiết 5 phần
└── v1/
    └── dashboard_hanh_dong_v1.xlsx    ← Bảng Dashboard v1 ban đầu để đối chiếu
```
