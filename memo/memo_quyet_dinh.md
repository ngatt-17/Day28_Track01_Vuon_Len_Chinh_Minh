# MEMO QUYẾT ĐỊNH ÁP DỤNG AI (DECISION MEMO)
**Dự án:** Internova AI Platform — Trợ lý tra cứu thông tin thực tập sinh viên  
**Nhóm thực hiện:** Vươn Lên Chính Mình (Track 01)  
**Ngày lập:** 03/09/2026  
**Phiên bản:** v2.0 (Sau phản biện chéo)

---

## 1. Vấn đề thực tế và Nguyên nhân gốc (Problem & Root Causes)

### 1.1. Triệu chứng quan sát được (Symptoms)
Hệ thống **Internova AI Platform** đã xây dựng hoàn thiện tính năng AI Chatbot tra cứu thông tin thực tập và tích hợp Knowledge Base. Tuy nhiên, trong các đợt dùng thử ban đầu:
- Sinh viên chỉ thử hỏi 1–2 câu rồi nhanh chóng quay lại thói quen cũ: nhắn tin trực tiếp qua Zalo/Email cho Cán bộ quản lý thực tập hoặc hỏi bạn bè.
- Cán bộ quản lý thực tập tiếp tục bị quá tải bởi hàng trăm tin nhắn lặp lại về cùng một nội dung (thời hạn nộp, điều kiện tín chỉ, mẫu đơn...).

### 1.2. Chẩn đoán 02 Nguyên nhân gốc thực sự (Root Causes)
1. **Nguyên nhân 1 — Thiếu kiến trúc độ tin cậy và nguồn kiểm chứng (Trust Architecture Gap):**  
   Câu trả lời của AI trước đây trả về dạng văn bản tự do, không dẫn link trích dẫn điều khoản cụ thể trong Sổ tay thực tập và không ghi rõ ngày cập nhật tài liệu. Sinh viên sợ AI bị "ảo giác" (hallucination) dẫn đến làm sai quy định và bị trượt học phần thực tập.
2. **Nguyên nhân 2 — Thiếu cơ chế bàn giao và xử lý ngoại lệ trong quy trình (Handoff & Exception Handling Gap):**  
   Quy trình cũ chỉ chèn AI vào một cách cơ học mà không định nghĩa: Khi AI gặp câu hỏi phức tạp hoặc trường hợp đặc thù (thực tập tại doanh nghiệp ngoài danh sách liên kết, thực tập sớm...), sinh viên cần làm gì tiếp theo. Thiếu nút kết nối trực tiếp với cán bộ phụ trách khiến sinh viên mất niềm tin và bỏ qua chatbot.

---

## 2. Framework chẩn đoán đã sử dụng và Bằng chứng thực tế

Nhóm đã sử dụng phối hợp 3 công cụ chẩn đoán chuyên sâu:

| Framework | Trục chẩn đoán | Phát hiện chính | Kết luận hành động |
| :--- | :--- | :--- | :--- |
| **Gartner-Lite** | *Direction — Readiness — Absorption* | - Hướng đi: ĐẠT (Mục tiêu giảm tải và nâng cao tự phục vụ rõ ràng).<br>- Readiness: THIẾU (Chưa có quy trình kiểm duyệt dữ liệu định kỳ).<br>- Absorption: THIẾU (Chưa có cơ chế học từ lỗi phản hồi). | Tập trung chuẩn hóa Knowledge Base và quy trình vận hành trước khi mở rộng quy mô lớn. |
| **Mollick** | *Phân chia công việc Người — AI* | - AI hỗ trợ: Tự động tra cứu, tóm tắt điều khoản và trích dẫn văn bản gốc.<br>- Sinh viên: Đối chiếu nguồn trích dẫn, tự quyết định và chịu trách nhiệm nộp hồ sơ.<br>- Cán bộ quản trị: Phê duyệt hồ sơ chính thức và xử lý ngoại lệ. | Không để AI tự động quyết định thay người; định rõ trách nhiệm từng bên. |
| **ADKAR** | *Điểm nghẽn tâm lý người dùng* | - Awareness: Đạt (Sinh viên biết công cụ).<br>- **Desire (NGHẼN NẶNG NHẤT):** Sinh viên sợ sai quy chế nên không dám tin dùng.<br>- Knowledge/Ability: Cần hướng dẫn cách đọc nguồn kiểm chứng. | Không giải quyết bằng việc "mở lớp đào tạo sử dụng", mà phải giải quyết bằng **cung cấp nguồn kiểm chứng minh bạch**. |

### Bằng chứng thực tế (Evidence):
- **Dữ liệu kiểm tra thực tế (User Testing):** Thử nghiệm với 5 sinh viên VinUni chuẩn bị thực tập kỳ Thu 2026 cho thấy 4/5 sinh viên ($80\%$) không sử dụng thông tin của bot để điền biểu mẫu vì không thấy trích dẫn văn bản chính thức của trường.
- **Log hệ thống:** $65\%$ các câu hỏi sinh viên bỏ dở là những câu hỏi mang tính đặc thù mà AI trả lời chung chung nhưng không cung cấp kênh liên hệ hỗ trợ tiếp theo.

---

## 3. Ít nhất 02 Thay đổi cụ thể sau phản biện chéo (Peer Review Changes)

Sau khi nhận góp ý phản biện từ **Nhóm 05** tại Chặng 3, nhóm đã nâng cấp toàn diện bản thiết kế từ v1 lên v2:

1. **Thay đổi 1 — Bổ sung chỉ số đo lường chất lượng lỗi và nút "Báo cáo thông tin sai lệch":**
   - *Ở bản v1:* Nhóm chỉ đo "Tỷ lệ câu trả lời có trích nguồn".
   - *Nâng cấp ở bản v2:* Bổ sung chỉ số **Tỷ lệ báo lỗi thông tin sai lệch (Error/Hallucination Report Rate)** với mục tiêu $< 3\%$. Tích hợp ngay nút bấm *"Báo cáo câu trả lời sai"* dưới từng phản hồi của AI để sinh viên phản ánh tức thì.
2. **Thay đổi 2 — Thiết lập quy định cam kết thời gian xử lý (SLA 24h) và cơ chế khóa tạm dữ liệu nghi vấn:**
   - *Ở bản v1:* Cột hành động khi chỉ số xấu chỉ ghi chung chung "kiểm tra lại dữ liệu".
   - *Nâng cấp ở bản v2:* Quy định rõ khi một câu hỏi bị báo sai từ 2 lần trở lên:
     + Hệ thống tự động chuyển câu hỏi cho Cán bộ quản lý thực tập xử lý.
     + Tạm ẩn câu trả lời tự động cho câu hỏi đó cho đến khi Admin kiểm duyệt lại trong vòng **24 giờ**.
3. **Thay đổi 3 (Bổ sung):** Bổ sung điều kiện kiểm tra quyền riêng tư dữ liệu sinh viên (Privacy Check) vào tiêu chí vượt cổng của Gate 60 ngày.

---

## 4. Quyết định cuối cùng (Final Decision)

> ### **QUYẾT ĐỊNH: TIẾP TỤC TRIỂN KHAI (GO)**
> Triển khai giai đoạn Pilot chính thức (Gate 0–30 ngày) cho 1 khoa thử nghiệm (Khoa Viện Khoa học & Giáo dục Khai phóng - VinUni) trước khi mở rộng toàn trường.

---

## 5. Lý do, Lộ trình bước tiếp theo & Phân công trách nhiệm (Next Steps & Owners)

### 5.1. Lý do ra quyết định
- Vấn đề quá tải tin nhắn tư vấn thực tập là bài toán cấp thiết của nhà trường.
- Giải pháp mới đã tháo gỡ đúng 2 nguyên nhân gốc: **minh bạch nguồn kiểm chứng** và **quy trình chuyển tiếp cán bộ rõ ràng**.
- Đội ngũ kỹ thuật và vận hành đã có đủ công cụ đo lường và quy trình ứng phó rủi ro cụ thể.

### 5.2. Lộ trình thực hiện 30–60–90 ngày
* **Giai đoạn 0–30 ngày (Chứng minh kiểm soát & Dữ liệu sạch):**
  - Cập nhật phiên bản RAG có trích dẫn văn bản chính thức và nút báo lỗi.
  - Phân công cán bộ quản trị dữ liệu quy chế thực tập.
  - Pilot trên nhóm 20 sinh viên đầu tiên.
  - *Gate Criteria:* $100\%$ câu trả lời có trích dẫn nguồn; xác định được Data Owner.
* **Giai đoạn 31–60 ngày (Chứng minh chất lượng & Giảm tải thực tế):**
  - Mở rộng thử nghiệm cho 100 sinh viên thuộc 1 khoa.
  - Vận hành quy trình hỗ trợ chuyển tiếp và cam kết SLA 24h.
  - *Gate Criteria:* Tỷ lệ báo lỗi $< 3\%$; số tin nhắn hỏi thủ công gửi cán bộ giảm $\ge 40\%$.
* **Giai đoạn 61–90 ngày (Nghiệm thu & Mở rộng toàn trường):**
  - Đánh giá tổng kết các chỉ số giá trị nghiệp vụ.
  - Họp Hội đồng Quản lý Thực tập để phê duyệt chuyển sang trạng thái Normalization (vận hành mặc định).
  - *Gate Criteria:* Giảm $\ge 70\%$ thời gian xử lý thủ tục; Ban Quản lý thực tập ký duyệt nghiệm thu.

### 5.3. Phân công trách nhiệm (Owners)
| Thành viên | Vai trò | Trách nhiệm chính trong giai đoạn tiếp theo |
| :--- | :--- | :--- |
| **Vũ Huy Hoàng** | Technical & AI Lead | Hoàn thiện tính năng trích dẫn nguồn chi tiết, tích hợp log báo lỗi và giám sát chỉ số kỹ thuật RAG. |
| **Tạ Thị Nga** | User Experience & QA Lead | Biên soạn cẩm nang hướng dẫn sử dụng cho sinh viên, theo dõi khảo sát độ hài lòng và điều phối thử nghiệm 20 sinh viên. |
| **Trần Hoài Nam** | Business & Operations Lead | Làm việc với Ban quản lý thực tập để chuẩn hóa tài liệu Knowledge Base, thiết lập quy trình SLA 24h và chuẩn bị báo cáo Gate 30. |
