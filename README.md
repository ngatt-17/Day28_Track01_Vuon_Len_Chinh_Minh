# Báo cáo thực hành Lab 28 — Áp dụng AI & Dashboard hành động

**Track 01 — Nhóm: Vươn Lên Chính Mình**

**Dự án:** Internova AI Platform — chatbot RAG hỗ trợ sinh viên tra cứu thông tin thực tập

**Demo:** [https://internova-ai-platform.vercel.app/](https://internova-ai-platform.vercel.app/)

---

## Tóm tắt điều hành

Internova đã vượt qua mức **Deployment** vì chatbot, RAG Index và hệ thống quan sát đều đang hoạt động, nhưng chưa đủ bằng chứng để kết luận đã đạt **Adoption**. Snapshot ngày 03/09/2026 cho thấy hệ thống ổn định về kỹ thuật nhưng Answer rate mới đạt 31,8%, P95 là 10,0s và các score chất lượng RAG chưa được ghi nhận. Vì vậy, nhóm chọn **tiếp tục pilot có điều kiện**, ưu tiên chứng minh chất lượng, hoàn thiện handoff và chỉ mở rộng khi vượt các cổng dữ liệu 30–60–90 ngày.

---

## 1. Thành viên và đóng góp phản biện

| Họ tên | MSSV | Phần phụ trách | Góp ý phản biện cho Nhóm 05 |
| :--- | :--- | :--- | :--- |
| **Vũ Huy Hoàng** | **2A202601057** | Kỹ thuật AI/RAG, telemetry và Dashboard v1–v2 | Cần tách **lỗi dịch vụ** khỏi **lỗi chất lượng RAG**: error rate bằng 0% không chứng minh câu trả lời đúng hoặc có căn cứ; nên bổ sung evaluator cho groundedness, faithfulness và answer relevance. |
| **Tạ Thị Nga** | **2A202601125** | Chẩn đoán ADKAR, thiết kế AS-IS/TO-BE và trải nghiệm phản hồi | Chỉ số “số câu hỏi” mới đo hoạt động; nên đo tỷ lệ tự giải quyết với tử số/mẫu số rõ ràng, đồng thời có nút “Không hữu ích/Báo sai” và luồng chuyển cán bộ khi thiếu bằng chứng. |
| **Trần Hoài Nam** | **2A202601751** | Gartner-Lite, lộ trình 30–60–90 ngày và memo quyết định | Các mốc 30–60–90 phải là **cổng bằng chứng**, không chỉ là ngày lịch; mỗi cổng cần owner, nguồn dữ liệu và điều kiện tiếp tục/sửa/dừng định lượng được. |

> Nội dung phản biện trên cần được nhóm đối chiếu với biên bản trao đổi thực tế trước khi nộp chính thức.

---

## 2. Phạm vi đã khóa

- **01 sản phẩm AI:** Internova AI Chatbot sử dụng RAG để tra cứu quy chế, thủ tục và biểu mẫu thực tập.
- **01 nhóm người dùng chính:** sinh viên VinUni chuẩn bị hoặc đang thực tập.
- **03 quy trình:** tra cứu điều kiện thực tập; tra cứu timeline/hồ sơ/biểu mẫu; tra cứu yêu cầu báo cáo và tiêu chí đánh giá.
- **Triệu chứng đo được:** snapshot ngày 03/09/2026 ghi nhận `Answer rate = 31,8%`; trong danh sách trace có trạng thái `insufficient_evidence`. Hệ thống chạy ổn nhưng chưa trả lời được đủ nhiều yêu cầu bằng bằng chứng phù hợp.
- **Mức độ trưởng thành hiện tại:** **Pilot/Deployment**, chưa phải Adoption; công cụ đã được đưa vào hệ thống nhưng workflow, tiêu chí tin cậy và vòng phản hồi chưa được chứng minh ở quy mô người dùng thật.

---

## 3. Nguyên nhân gốc, framework và bằng chứng

### Nguyên nhân gốc

1. **Khoảng trống kiểm chứng chất lượng:** pipeline RAG và index đã hoạt động, nhưng Retrieval success, Groundedness, RAG confidence, Faithfulness và Answer relevance đều chưa có score. Vì vậy `0% error rate` hiện chỉ phản ánh độ ổn định kỹ thuật, không phản ánh độ đúng của nội dung.
2. **Khoảng trống hoàn tất quy trình và chuyển người:** Answer rate còn thấp và đã xuất hiện trace `insufficient_evidence`; chưa có telemetry chứng minh câu hỏi ngoại lệ được chuyển đúng cán bộ và xử lý đúng SLA.

### Chuỗi lập luận chẩn đoán

> **0% lỗi dịch vụ** không đồng nghĩa **câu trả lời đúng** → evaluator chưa có score nên độ tin cậy chưa được chứng minh → người dùng khó hình thành Desire → Answer rate thấp và xuất hiện `insufficient_evidence` → cần sửa quality governance và handoff trước khi mở rộng.

### Framework sử dụng

- **Gartner-Lite:** Direction đạt; Readiness và Absorption chưa đạt vì evaluator, data owner, feedback loop và quy tắc gate chưa hoàn chỉnh.
- **Mollick:** AI tìm kiếm/tóm tắt/trích nguồn; sinh viên kiểm tra nguồn; cán bộ quyết định trường hợp ngoại lệ và chịu trách nhiệm nghiệp vụ.
- **ADKAR:** điểm nghẽn nằm ở **Desire** và **Reinforcement** — người dùng cần bằng chứng có thể kiểm tra và thấy phản hồi sai được xử lý, không chỉ cần hướng dẫn sử dụng.

### Snapshot telemetry ngày 03/09/2026

| Nguồn quan sát | Bằng chứng chính | Ý nghĩa hành động |
| :--- | :--- | :--- |
| Dashboard tổng quan | 44 requests, 1 active user, avg 2,67s, P95 10,0s, P99 11,0s, error rate 0%, answer rate 31,8% | Hệ thống ổn định kỹ thuật nhưng mức trả lời và P95 chưa đạt kỳ vọng pilot. |
| RAG Quality | Các evaluator chất lượng đều “Chưa có score” | Chưa đủ căn cứ tuyên bố câu trả lời đáng tin cậy. |
| Pipeline latency | `evidence` 8,36s; `evidence_semantic_fallback` 6,17s; generation 1,76s; retrieve 1,60s | Ưu tiên tối ưu tầng evidence/fallback trước model generation. |
| RAG Index | 9 tài liệu được publish/index thành 176 chunks; danh mục hiển thị 11 tài liệu | Cần làm rõ 2 tài liệu chưa vào active build và gắn owner/phiên bản. |
| Traces/Logs | Trang trace hiển thị 89 traces, 0 error traces; có trace `insufficient_evidence` 8,25s | Cần phân loại failure nghiệp vụ riêng với lỗi kỹ thuật. |

> Dashboard tổng quan hiển thị 44 requests trong khi trang Traces hiển thị 89 traces. Trước khi dùng làm báo cáo chính thức, nhóm cần thống nhất time window, bộ lọc và định nghĩa request/trace.

---

## 4. Quy trình TO-BE

| Bước | AS-IS | TO-BE | Người chịu trách nhiệm |
| :--- | :--- | :--- | :--- |
| 1. Tiếp nhận câu hỏi | Câu hỏi được xử lý nhưng chưa có tiêu chí phân loại rõ | Gắn intent, user/session và trace ID; loại dữ liệu cá nhân không cần thiết trước khi truy xuất | Hệ thống + Admin |
| 2. Truy xuất | Có vector search/BM25 nhưng chưa có score Retrieval success | Hybrid retrieval → RRF/rerank → chọn evidence; ghi document ID, chunk và phiên bản | Vũ Huy Hoàng |
| 3. Tạo câu trả lời | Có thể trả lời dù bằng chứng chưa đủ rõ | Chỉ generation khi evidence vượt ngưỡng; câu trả lời kèm tên tài liệu, điều khoản/trang và ngày hiệu lực | AI hỗ trợ, sinh viên kiểm tra |
| 4. Kiểm tra | Error rate kỹ thuật đang bị hiểu nhầm là chất lượng | Chạy groundedness, faithfulness, answer relevance và validation; lưu score vào trace | Vũ Huy Hoàng |
| 5. Ngoại lệ | Chưa chứng minh được luồng chuyển người | Nếu `insufficient_evidence`, không suy đoán; tạo ticket gắn trace ID, owner và SLA 24 giờ | Tạ Thị Nga + Cán bộ phụ trách |
| 6. Học từ lỗi | Feedback chưa khép kín về nguồn gây lỗi | Liên kết feedback với document/chunk, sửa tài liệu, re-index và chạy regression test trước khi mở lại | Data Owner + QA |

Nguyên tắc phân quyền theo Mollick: **AI đề xuất và trích nguồn; sinh viên kiểm tra; cán bộ giữ quyền quyết định nghiệp vụ và xử lý ngoại lệ**.

---

## 5. Chỉ số quyết định

| Chỉ số | Baseline | Target | Nguồn | Owner | Khi chỉ số xấu |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Tỷ lệ evaluator chất lượng có ghi score | 0% (0/5 evaluator có score) | 100% evaluator hoạt động; citation accuracy ≥95% trên ≥50 trace/tuần | Langfuse RAG Quality + audit văn bản | Vũ Huy Hoàng | Kiểm tra cấu hình evaluator/dataset; không cho qua gate khi thiếu score chất lượng. |
| Answer rate (proxy cho self-service) | 31,8% | ≥60% ở Gate 60; ≥80% ở Gate 90 | Trace status + phản hồi người dùng | Tạ Thị Nga | Phân nhóm intent thất bại, bổ sung tài liệu hoặc chuyển cán bộ. |
| P95 end-to-end latency | 10,0s | ≤5,0s | Langfuse traces/observations | Vũ Huy Hoàng | Tối ưu evidence/fallback, cache và timeout; không ưu tiên model khi generation không phải nút thắt. |
| Tỷ lệ trace thiếu bằng chứng tạo handoff | 0% (0/1 trace `insufficient_evidence` hiển thị ticket/owner) | ≥95% có owner trong 5 phút; ≥90% xử lý ≤24h | Ticket log liên kết trace ID | Tạ Thị Nga | Kiểm tra routing, lịch trực và cảnh báo quá hạn. |
| Chi phí LLM trung bình mỗi request | $0.0107/request | ≤$0.010/request sau khi đạt quality gate | Dashboard cost/tokens theo cùng time window | Trần Hoài Nam | Phân tích cost per answered request, giảm retry và cache truy vấn lặp lại nhưng không cắt giảm bước kiểm chứng. |

`Answer rate` chỉ là proxy; “tự giải quyết thành công” chỉ được xác nhận khi có phản hồi người dùng hoặc khi nghiệp vụ hoàn tất mà không phát sinh handoff/tin nhắn hỏi lại.

### Công thức và quy tắc đo

- **Evaluator coverage** = số evaluator đã ghi score / 5 evaluator chất lượng bắt buộc × 100%; citation accuracy được audit riêng trên tối thiểu 50 trace/tuần.
- **Answer rate** = số request có trạng thái `answered` / tổng request hợp lệ trong cùng time window × 100%.
- **Handoff đúng SLA** = số ticket ngoại lệ xử lý trong 24 giờ / tổng ticket ngoại lệ × 100%.
- **Chi phí mỗi request** = tổng chi phí LLM / tổng request hợp lệ; đồng thời theo dõi cost per answered request để tránh tối ưu chi phí bằng cách từ chối trả lời.

Các metric phải dùng chung timezone, time window và bộ lọc. Số `44 requests` và `89 traces` chưa được dùng để suy ra conversion cho đến khi thống nhất quan hệ giữa request, trace và observation.

---

## 6. Quyết định cuối cùng

**TIẾP TỤC PILOT CÓ ĐIỀU KIỆN (CONDITIONAL GO / ADJUST), chưa mở rộng toàn trường.**

Lý do: hệ thống đã có index, trace và không ghi nhận lỗi dịch vụ trong snapshot, nhưng chất lượng chưa có evaluator score, Answer rate mới đạt 31,8% và P95 là 10,0s. Nhóm chỉ chuyển gate khi có đủ bằng chứng, không chuyển vì hết số ngày.

Hai thay đổi chính từ v1 sang v2:

1. Loại bỏ baseline ước tính không có nguồn; dùng telemetry thật và ghi rõ các chỉ số “chưa đo”.
2. Tách độ ổn định kỹ thuật khỏi chất lượng RAG; bổ sung evaluator, latency evidence, handoff/SLA và quy tắc hành động cụ thể.

### Quy tắc ra quyết định

| Kết quả gate | Quyết định | Hành động |
| :--- | :--- | :--- |
| Đạt toàn bộ ngưỡng chất lượng, workflow và giá trị | **CONTINUE** | Chuyển sang gate tiếp theo; chỉ mở rộng phạm vi từng bước. |
| Chất lượng hoặc SLA chưa đạt nhưng không có sự cố nghiêm trọng | **ADJUST** | Giữ phạm vi pilot, sửa intent/document/chunk hoặc routing rồi đo lại. |
| Có rò rỉ dữ liệu, nguồn sai nghiêm trọng hoặc không xác định được owner | **STOP/ROLLBACK** | Tạm dừng chủ đề hoặc active build liên quan, điều tra và chỉ mở lại sau kiểm định. |

---

## 7. Cấu trúc bài nộp

```text
Day28_Track01_Vuon_Len_Chinh_Minh/
├── README.md
├── dashboard/
│   └── dashboard_hanh_dong_v2.xlsx
├── memo/
│   └── memo_quyet_dinh.md
└── v1/
    └── dashboard_hanh_dong_v1.xlsx
```
