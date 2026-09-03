# MEMO QUYẾT ĐỊNH ÁP DỤNG AI

**Dự án:** Internova AI Platform — chatbot RAG tra cứu thông tin thực tập

**Nhóm:** Vươn Lên Chính Mình — Track 01

**Ngày lập:** 03/09/2026

**Phiên bản:** v2.1 — sau phản biện và đối chiếu telemetry

---

## Tóm tắt dành cho người ra quyết định

Internova đã có nền tảng kỹ thuật cần thiết cho một pilot RAG: Knowledge Base, hybrid retrieval, trace theo từng stage và dashboard theo dõi latency/chi phí. Tuy nhiên, bằng chứng hiện tại mới chứng minh hệ thống **chạy được**, chưa chứng minh hệ thống **trả lời đáng tin và làm thay đổi workflow**. Khuyến nghị của nhóm là tiếp tục đầu tư trong phạm vi pilot, nhưng khóa việc mở rộng bằng ba điều kiện: evaluator phải sinh score, handoff phải có owner/SLA và các chỉ số workflow phải cải thiện trên cùng một định nghĩa dữ liệu.

---

## 1. Vấn đề và nguyên nhân gốc

Snapshot vận hành ngày 03/09/2026 cho thấy hệ thống không ghi nhận lỗi dịch vụ (`error rate = 0%`) nhưng chỉ đạt `Answer rate = 31,8%`; một trace có trạng thái `insufficient_evidence`. Đồng thời các evaluator Retrieval success, Groundedness, RAG confidence, Faithfulness và Answer relevance chưa ghi score. Vì vậy vấn đề không còn là “chatbot có chạy hay không”, mà là **có đủ bằng chứng để tin và hoàn tất quy trình hay không**.

Hai nguyên nhân gốc:

1. **Quality governance chưa hoàn chỉnh:** có RAG index, trace và các stage pipeline nhưng chưa có score để chứng minh nguồn truy xuất đúng, câu trả lời bám nguồn và phù hợp câu hỏi.
2. **Fallback/handoff chưa đo được:** các yêu cầu thiếu bằng chứng đã được nhận diện, nhưng chưa có dữ liệu chứng minh chúng được chuyển đúng cán bộ, có owner và được xử lý trong SLA.

---

## 2. Framework và bằng chứng

| Framework | Phát hiện | Hành động |
| :--- | :--- | :--- |
| **Gartner-Lite** | Direction rõ; Readiness/Absorption còn thiếu evaluator, data owner và feedback loop. | Chưa rollout; hoàn thiện kiểm soát chất lượng và vận hành trong pilot. |
| **Mollick** | AI phù hợp với tìm kiếm, trích nguồn và tóm tắt; quyết định ngoại lệ vẫn thuộc cán bộ. | Thiết kế handoff có trace ID, owner, SLA; không để AI suy đoán khi thiếu nguồn. |
| **ADKAR** | Desire/Reinforcement là điểm nghẽn: người dùng cần thấy bằng chứng và thấy lỗi được xử lý. | Hiển thị citation, nút feedback và trạng thái xử lý ticket ngay trong workflow. |

Bằng chứng telemetry:

- **Dashboard:** 44 requests, 1 active user, latency trung bình 2,67s, P95 10,0s, P99 11,0s, answer rate 31,8%.
- **RAG Quality:** tất cả score chất lượng quan trọng đang ở trạng thái “Chưa có score”.
- **Pipeline:** `evidence = 8,36s` và `evidence_semantic_fallback = 6,17s`, cao hơn `generation = 1,76s`; evidence/fallback là ưu tiên tối ưu.
- **Index:** 9 tài liệu active/published, 176 chunks; danh mục hiển thị 11 tài liệu, cần làm rõ 2 tài liệu chưa vào active build.
- **Traces:** 89 traces, 0 error traces và có trạng thái `insufficient_evidence`. Chênh lệch 44 requests/89 traces cần được giải thích bằng time window, filter và định nghĩa metric trước khi báo cáo chính thức.

Kết luận dữ liệu: **0% lỗi kỹ thuật không đồng nghĩa 0% hallucination hoặc 100% groundedness**.

### Vì sao chưa dùng token và chi phí làm tiêu chí chính?

Dashboard ghi nhận baseline `$0.0107/request`. Đây là chỉ số phụ để kiểm soát hiệu quả, chưa phải tiêu chí chất lượng chính vì chưa chuẩn hóa được cost per successful answer. Nhóm chỉ tối ưu chi phí sau khi quality gate đạt yêu cầu, tránh cắt giảm retrieval/evidence trong khi vấn đề chính vẫn là độ tin cậy.

---

## 3. Thay đổi sau phản biện

Phản biện được tổng hợp theo ba trục:

- **Vũ Huy Hoàng — 2A202601057:** tách service error khỏi lỗi chất lượng RAG; bổ sung evaluator và audit citation.
- **Tạ Thị Nga — 2A202601125:** định nghĩa self-service bằng dữ liệu workflow, bổ sung feedback/handoff thay cho chỉ đếm số câu hỏi.
- **Trần Hoài Nam — 2A202601751:** biến lộ trình 30–60–90 thành các gate bằng chứng có owner và điều kiện dừng/sửa/tiếp tục.

Các thay đổi đã đưa vào v2:

1. Thay baseline ước tính bằng số telemetry thật hoặc proxy có tử số/mẫu số rõ: evaluator coverage `0/5`, insufficient evidence `1/89`, handoff `0/1` và chi phí `$0.0107/request`.
2. Bổ sung bộ score Retrieval/Groundedness/Faithfulness/Answer relevance, audit citation và phân biệt lỗi nội dung với lỗi dịch vụ.
3. Bổ sung KPI latency P95, handoff/SLA và hành động cụ thể khi chỉ số xấu; ưu tiên xử lý stage evidence/fallback.

---

## 4. Quyết định

> **TIẾP TỤC PILOT CÓ ĐIỀU KIỆN (CONDITIONAL GO / ADJUST); CHƯA MỞ RỘNG TOÀN TRƯỜNG.**

Hệ thống có nền tảng vận hành và quan sát tốt, nhưng chưa đủ bằng chứng về chất lượng câu trả lời. Mở rộng ngay khi evaluator chưa hoạt động và Answer rate còn 31,8% sẽ làm tăng quy mô của rủi ro thay vì chứng minh adoption.

---

## 5. Bước tiếp theo, gate và owner

| Giai đoạn | Việc chính | Điều kiện qua gate | Owner |
| :--- | :--- | :--- | :--- |
| **0–30 ngày: chứng minh đo được** | Kích hoạt evaluator; chuẩn hóa 11 tài liệu và active build; gắn feedback/handoff với trace ID; thống nhất request/trace. | Có score trên ≥50 trace mẫu; 100% tài liệu active có owner/phiên bản; xác định được nguyên nhân chênh 44/89; P95 ≤8s. | Vũ Huy Hoàng |
| **31–60 ngày: chứng minh chất lượng** | Pilot một nhóm sinh viên; audit citation hằng tuần; vận hành ticket/SLA; tối ưu evidence/fallback. | Citation đúng/còn hiệu lực ≥95%; Answer rate ≥60%; ≥90% ticket xử lý ≤24h; P95 ≤5s. | Tạ Thị Nga |
| **61–90 ngày: chứng minh giá trị** | Đánh giá self-service, chi phí trên request thành công và tải hỗ trợ; họp quyết định mở rộng. | Answer rate ≥80%; cost ≤$0.010/request sau khi quality gate đạt; không có sự cố nghiêm trọng chưa xử lý; owner nghiệp vụ phê duyệt. | Trần Hoài Nam |

Nếu bất kỳ gate nào không đạt, nhóm giữ nguyên phạm vi pilot, phân tích intent/document/chunk gây lỗi và chạy lại đánh giá sau khi sửa. Không chuyển giai đoạn chỉ vì đã hết 30, 60 hoặc 90 ngày.

### Rủi ro và cơ chế kiểm soát

| Rủi ro | Dấu hiệu cảnh báo | Kiểm soát | Owner |
| :--- | :--- | :--- | :--- |
| Nguồn hết hiệu lực hoặc trích dẫn sai | Citation accuracy <95%; nhiều feedback cùng document | Version tài liệu, data owner, audit 50 trace/tuần; tạm ẩn chủ đề và re-index | Vũ Huy Hoàng |
| Hallucination dù dịch vụ không báo lỗi | Service error = 0% nhưng groundedness/faithfulness thấp | Evaluator độc lập, bộ câu hỏi chuẩn và regression test trước khi publish build | Vũ Huy Hoàng |
| Ngoại lệ không đến đúng cán bộ | Ticket thiếu owner hoặc quá SLA | Routing theo intent, cảnh báo quá hạn và lịch trực dự phòng | Tạ Thị Nga |
| Lộ dữ liệu cá nhân trong prompt/log | Trace chứa trường dữ liệu không cần thiết | Data minimization, phân quyền log, masking và thời hạn lưu dữ liệu | Trần Hoài Nam |
| Latency làm người dùng bỏ cuộc | P95 >5s; evidence/fallback chiếm phần lớn thời gian | Cache nguồn ổn định, timeout/fallback có kiểm soát và tối ưu evidence stage | Vũ Huy Hoàng |

### Tiêu chuẩn một câu trả lời đạt

Một câu trả lời chỉ được tính là thành công khi đồng thời: đúng intent; có evidence đủ ngưỡng; trích đúng tài liệu còn hiệu lực; không mâu thuẫn với nguồn; trả lời đúng câu hỏi; hoàn thành trong ngưỡng latency; và không phát sinh phản hồi sai hoặc handoff do thiếu thông tin. Tiêu chuẩn này ngăn nhóm tối ưu riêng Answer rate bằng cách trả lời nhiều nhưng thiếu căn cứ.

### Phân công trách nhiệm

| Thành viên | Vai trò | Trách nhiệm |
| :--- | :--- | :--- |
| **Vũ Huy Hoàng** | AI/RAG & Observability Lead | Evaluator, citation audit, index/version, latency và chất lượng pipeline. |
| **Tạ Thị Nga** | Workflow & User Feedback Lead | ADKAR, UX feedback, định nghĩa self-service, handoff và SLA. |
| **Trần Hoài Nam** | Business & Decision Lead | Baseline nghiệp vụ, Gartner-Lite, gate 30–60–90 và quyết định mở rộng. |
