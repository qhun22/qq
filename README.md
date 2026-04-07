# CHƯƠNG 3: STAKEHOLDERS & PHƯƠNG PHÁP

## 3.1. Xác định các bên liên quan
Trong dự án IoT GreenThumb, các bên liên quan (stakeholders) bao gồm cả nhóm sử dụng sản phẩm, nhóm thực hiện dự án và các bên phụ thuộc bên ngoài (nhà cung cấp, nền tảng phân phối). Việc xác định đúng stakeholders giúp nhóm xây dựng kế hoạch phạm vi, tiến độ, rủi ro và giao tiếp phù hợp.

**Bảng 3.1. Danh sách stakeholders**

| Mã | Stakeholder | Nhóm | Nhu cầu/kỳ vọng chính | Mức độ ảnh hưởng (sơ bộ) |
|---|---|---|---|---|
| SH1 | Người dùng cuối (hộ gia đình trồng cây) | Customer/User | Dễ dùng, dữ liệu rõ ràng, cảnh báo đúng, điều khiển bơm ổn định | Trung bình |
| SH2 | Nhóm dự án (05 SV) | Project Team | Kế hoạch rõ ràng, phân công minh bạch, phối hợp HW–SW tốt | Cao |
| SH3 | Giảng viên/Người chấm | Sponsor/Approver | Hồ sơ quản lý dự án đầy đủ, nhất quán, có minh chứng (WBS/Gantt/Budget/Risk/QA) | Cao |
| SH4 | Nhà cung cấp linh kiện & vận chuyển | Supplier | Đơn hàng rõ ràng, thời gian cung ứng ổn định | Trung bình |
| SH5 | Nền tảng cloud (dịch vụ demo/free-tier) | Platform | Giới hạn tài nguyên, chính sách sử dụng dịch vụ | Trung bình |
| SH6 | App Store/Google Play (mức giả định) | Platform | Tuân thủ chính sách phát hành, mô tả ứng dụng/riêng tư | Thấp–TB |

## 3.2. Vai trò và trách nhiệm

### 3.2.1. Mô tả vai trò từng bên
- **Người dùng cuối:** đưa phản hồi về trải nghiệm sử dụng, kỳ vọng về cảnh báo và điều khiển.
- **Nhóm dự án:** chịu trách nhiệm lập kế hoạch, theo dõi tiến độ, quản lý rủi ro và đảm bảo chất lượng.
- **Giảng viên:** đóng vai trò phê duyệt các deliverables học phần, đưa nhận xét để nhóm cập nhật kế hoạch.
- **Nhà cung cấp linh kiện:** ảnh hưởng trực tiếp đến tiến độ prototype và rủi ro cung ứng.
- **Nền tảng cloud/app store:** ảnh hưởng đến khả năng triển khai demo, giới hạn tài nguyên và yêu cầu chính sách.

### 3.2.2. Trách nhiệm chính (tóm tắt)
**Bảng 3.2. Vai trò & trách nhiệm (tóm tắt)**

| Stakeholder | Trách nhiệm/đóng góp | Đầu ra liên quan |
|---|---|---|
| Người dùng cuối | Phản hồi yêu cầu, xác nhận mức “dễ dùng”, tình huống sử dụng | Use case, acceptance criteria (UAT) |
| Nhóm dự án | Lập và quản lý kế hoạch; thực thi theo mốc; báo cáo | WBS, Gantt, Budget, Risk Register, Test Plan |
| Giảng viên | Nhận xét/đánh giá; xác nhận mức đáp ứng rubric | Biên bản góp ý (nếu có), mốc phê duyệt |
| Nhà cung cấp | Cung ứng linh kiện đúng chất lượng và thời gian | BOM, hóa đơn/đơn hàng (giả định) |
| Nền tảng cloud | Cung cấp dịch vụ chạy demo theo giới hạn | Kế hoạch triển khai cloud, rủi ro giới hạn dịch vụ |
| App store | Quy định phát hành (mức giả định) | Checklist phát hành (giả định) |

## 3.3. Ma trận Stakeholder (Power – Interest)

### 3.3.1. Phân loại mức độ ảnh hưởng
Nhóm sử dụng ma trận Power–Interest để ưu tiên cách quản lý stakeholders.

- **High Power / High Interest (Quản lý chặt):** Giảng viên (SH3), Nhóm dự án (SH2)
- **High Power / Low Interest (Giữ hài lòng):** Nhà cung cấp quan trọng (SH4), Nền tảng cloud (SH5)
- **Low Power / High Interest (Giữ cập nhật):** Người dùng cuối (SH1)
- **Low Power / Low Interest (Theo dõi tối thiểu):** App store (SH6) trong phạm vi mô phỏng

**Hình 3.1. Power–Interest Matrix**
- File hình tham chiếu: xem [Hinh-3-1-Power-Interest.html](Hinh-3-1-Power-Interest.html) để xuất ảnh chèn Word.

### 3.3.2. Chiến lược quản lý từng nhóm
**Bảng 3.3. Chiến lược quản lý stakeholders**

| Nhóm | Stakeholder | Chiến lược | Tần suất |
|---|---|---|---|
| Quản lý chặt | SH2, SH3 | Báo cáo tiến độ + minh chứng, cập nhật thay đổi kế hoạch | Hàng tuần |
| Giữ hài lòng | SH4, SH5 | Theo dõi rủi ro cung ứng/giới hạn dịch vụ; có phương án thay thế | Theo mốc |
| Giữ cập nhật | SH1 | Thu thập phản hồi về tính dễ dùng/cảnh báo | 1–2 lần/sprint |
| Theo dõi tối thiểu | SH6 | Lập checklist phát hành giả định, nêu ràng buộc chính sách | Cuối dự án |

## 3.4. Phương pháp quản lý dự án

### 3.4.1. Giới thiệu các phương pháp
- **Waterfall:** phù hợp khi yêu cầu ổn định, ít thay đổi; ưu điểm là kiểm soát tài liệu và mốc rõ ràng.
- **Agile (Scrum/Kanban):** phù hợp khi yêu cầu có thể thay đổi; ưu điểm là lặp nhanh, phản hồi sớm.
- **Hybrid:** kết hợp Waterfall/Stage-gate cho phần ít linh hoạt (thường là phần cứng) và Agile cho phần cần lặp nhanh (thường là phần mềm).

### 3.4.2. Lựa chọn phương pháp
Nhóm lựa chọn **Hybrid (Stage-gate cho phần cứng + Agile/Scrum cho cloud & mobile)**.

### 3.4.3. Lý do lựa chọn
- **Tính song song HW–SW:** dự án có 2 nhánh công việc (phần cứng/firmware và phần mềm). Hybrid giúp vừa giữ được mốc cứng cho prototype, vừa cho phép phần mềm phát triển theo sprint.
- **Rủi ro cung ứng & vật lý:** phần cứng phụ thuộc linh kiện, lắp ráp, nguồn/bơm → cần mốc kiểm soát (gate) rõ ràng.
- **Độ bất định ở phần mềm:** giao diện, trải nghiệm, logic cảnh báo thường cần điều chỉnh theo phản hồi → phù hợp sprint.
- **Phù hợp học phần:** dễ trình bày WBS, Gantt, rủi ro và minh chứng tiến độ theo tuần.

### 3.4.4. So sánh với phương pháp khác
**Bảng 3.4. So sánh lựa chọn phương pháp (tóm tắt)**

| Tiêu chí | Waterfall thuần | Agile thuần | Hybrid (lựa chọn) |
|---|---|---|---|
| Phù hợp phần cứng | Tốt | Trung bình | **Tốt** |
| Phù hợp phần mềm | Trung bình | Tốt | **Tốt** |
| Quản lý phụ thuộc HW→SW | Trung bình | Trung bình | **Tốt** (có gate + sprint) |
| Linh hoạt thay đổi UI/logic | Thấp | **Cao** | **Cao** (ở nhánh SW) |
| Dễ bám rubric (WBS/Gantt) | **Cao** | Trung bình | **Cao** |

### 3.4.5. Cách áp dụng Hybrid trong dự án (mô tả ngắn)
- **Phần cứng/firmware (Stage-gate):**
  - Gate 1: Chốt BOM + thiết kế nguồn/bơm
  - Gate 2: Prototype #1 chạy telemetry
  - Gate 3: Prototype #2 ổn định + hiệu chuẩn
- **Phần mềm (Sprint 1 tuần):**
  - Lập backlog theo FR/NFR; mỗi sprint có mục tiêu rõ ràng (dashboard, lịch sử, cảnh báo, điều khiển).
  - Cuối sprint có demo/đánh giá và cập nhật kế hoạch (nếu có thay đổi).

## 3.5. Công cụ quản lý dự án (Jira) – không thuộc kiến trúc hệ thống
Nhóm dự kiến sử dụng **Jira** để quản lý backlog, phân công công việc, theo dõi tiến độ theo sprint/tuần và trích xuất báo cáo (ví dụ: % hoàn thành, burndown). Jira chỉ phục vụ **quản lý dự án** và **không nằm trong phạm vi kiến trúc sản phẩm GreenThumb** (không đưa Jira vào sơ đồ hệ thống).

**Bảng 3.5. Thiết lập Jira (gợi ý tối thiểu)**

| Hạng mục | Thiết lập đề xuất |
|---|---|
| Board | Scrum board (sprint 1 tuần) hoặc Kanban (To Do/In Progress/Done) |
| Issue types | Epic (HW/FW/Cloud/App/Test), Story/Task, Bug |
| Trường cần dùng | Assignee, Due date, Priority, Labels, Sprint |
| Báo cáo | Burndown (nếu Scrum), Velocity (tuỳ chọn), % Done theo tuần |

*(Có thể đưa link board hoặc ảnh chụp màn hình Jira vào Phụ lục để làm minh chứng.)*
