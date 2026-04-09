# CHƯƠNG 5: TIẾN ĐỘ & NGÂN SÁCH

## 5.1. Xác định công việc

### 5.1.1. Danh sách task chính
Dựa trên WBS ở Chương 4, nhóm xây dựng danh sách các công việc chính để lập tiến độ tổng hợp trong 07 tuần.

**Bảng 5.1. Danh sách task (tổng hợp)**

| Mã task | Nhóm | Nội dung công việc | Thời lượng (ước tính) |
|---|---|---|---|
| T1 | PM | Kickoff + chốt scope + FR/NFR + stakeholders | 1 tuần |
| T2 | Cloud/PM | Kiến trúc + API/MQTT contract (payload/topic) | 1 tuần |
| T3 | HW | BOM + thiết kế nguồn/bơm + phương án linh kiện thay thế | 1 tuần |
| T4 | HW | Mua linh kiện cho 02 prototypes | 1 tuần |
| T5 | HW | Lắp prototype #1 + kiểm tra nguồn | 0.5 tuần |
| T6 | FW | Firmware đọc cảm biến + telemetry + reconnect | 1–1.5 tuần |
| T7 | Cloud | Ingest telemetry + lưu trữ + API truy vấn | 1–1.5 tuần |
| T8 | Mobile | App skeleton + dashboard + mock data | 1 tuần |
| T9 | Cloud/FW | Command path (cloud↔device) + bơm an toàn | 1 tuần |
| T10 | Mobile | App tích hợp dữ liệu thật + điều khiển bơm | 1 tuần |
| T11 | HW/FW | Prototype #2 ổn định + hiệu chuẩn | 1 tuần |
| T12 | QA | Integration test end-to-end | 1 tuần |
| T13 | All | UAT + sửa lỗi + chốt nghiệm thu | 1 tuần |
| T14 | PM | Hoàn thiện báo cáo + phụ lục + slide | 1 tuần |

Để đảm bảo bám sát mốc 07 tuần, các task trên được triển khai **song song** theo 2 nhánh (HW/FW và Cloud/Mobile) và hội tụ ở giai đoạn tích hợp.

**Bảng 5.1a. Kế hoạch 07 tuần (tóm tắt theo sprint/tuần)**

| Tuần/Sprint | Trọng tâm | Đầu ra mong đợi |
|---|---|---|
| Tuần 1 (Sprint 0) | Kickoff, chốt phạm vi/yêu cầu, contract v0, BOM sơ bộ, setup Jira | Baseline scope + contract v0 + backlog/sprint |
| Tuần 2 (Sprint 1) | BOM chốt sớm + mua linh kiện; FW đọc cảm biến (local); cloud ingest skeleton | Linh kiện đặt mua; payload chạy được (local) |
| Tuần 3 (Sprint 2) | Lắp Prototype #1; FW gửi telemetry; cloud lưu DB + API latest | Prototype #1 gửi telemetry lên cloud |
| Tuần 4 (Sprint 3) | Mobile skeleton + dashboard; command path v0 (cloud↔device) | App xem dữ liệu thật; command path sẵn sàng |
| Tuần 5 (Sprint 4) | Prototype #2 ổn định + hiệu chuẩn; app điều khiển bơm | Demo: app điều khiển bơm qua cloud + prototype ổn định |
| Tuần 6 (Sprint 5) | Integration test end‑to‑end + hardening | KQ test tích hợp đạt mức demo |
| Tuần 7 (Sprint 6) | UAT + sửa lỗi + hoàn thiện báo cáo/phụ lục/slide | Nộp báo cáo + minh chứng Jira + demo |

### 5.1.2. Thứ tự thực hiện (nguyên tắc)
- Các hạng mục **thiết kế/contract** cần chốt sớm để HW/FW và SW có thể làm **song song**.
- **Tích hợp end-to-end** chỉ thực hiện hiệu quả khi có **prototype ổn định** (prototype #2) và command path hoạt động.

## 5.2. Biểu đồ Gantt

### 5.2.1. Mô tả biểu đồ
Biểu đồ Gantt được xây dựng theo kế hoạch 07 tuần, thể hiện các công việc HW/FW và Cloud/Mobile chạy song song, đồng thời thể hiện các quan hệ phụ thuộc quan trọng để kiểm soát rủi ro tiến độ.

### 5.2.2. Thời gian thực hiện
**Hình 5.1** (Gantt Chart) được xuất từ sơ đồ Mermaid (file HTML kèm theo) để chèn vào Word.

- File hình tham chiếu: xem [Hinh-5-1-Gantt.html](Hinh-5-1-Gantt.html) để xuất ảnh chèn Word.

## 5.3. Mối quan hệ phụ thuộc (Dependencies)

### 5.3.1. Quan hệ giữa các task (tóm tắt)
**Bảng 5.2. Dependencies chính**

| Task | Phụ thuộc | Lý do |
|---|---|---|
| T2 (contract) | T1 | Chỉ chốt contract sau khi thống nhất phạm vi/yêu cầu |
| T3 (BOM/thiết kế) | T1 | Thiết kế theo yêu cầu và ràng buộc |
| T4 (mua linh kiện) | T3 | Mua theo BOM đã chốt |
| T5 (prototype #1) | T4 | Có linh kiện mới lắp được |
| T6 (FW telemetry) | T5, T2 | Cần thiết bị để chạy FW và cần contract để gửi dữ liệu đúng |
| T7 (cloud ingest/storage) | T2 | Cần contract để thiết kế endpoint/topic/schema |
| T8 (app skeleton) | T2 | Cần biết dữ liệu/luồng chính để dựng UI đúng |
| T9 (command + bơm) | T6, T7 | Command dựa trên FW + cloud |
| T10 (app integrate + control) | T8, T9 | App tích hợp sau khi command path có sẵn |
| T11 (prototype #2 ổn định) | T6 | Ổn định/hiệu chuẩn sau khi telemetry cơ bản hoạt động |
| T12 (integration test) | T10, T11 | Test end-to-end chỉ hiệu quả khi app + prototype ổn định |
| T13 (UAT) | T12 | UAT sau khi integration test đạt mức tối thiểu |
| T14 (final) | T13 | Chốt kết quả và tài liệu sau nghiệm thu |

### 5.3.2. Ảnh hưởng tiến độ
Các phụ thuộc T4→T5→T6→T11→T12 là chuỗi quan trọng vì liên quan đến phần cứng và khả năng tích hợp. Nhóm cần đặt mua linh kiện sớm và có phương án dự phòng để giảm nguy cơ trễ.

## 5.4. Các mốc quan trọng (Milestones)

### 5.4.1. Mốc chính
**Bảng 5.3. Milestones**

| Mã mốc | Thời điểm (tuần) | Mô tả |
|---|---|---|
| M1 | Cuối tuần 1 | Baseline scope + phương pháp + danh sách yêu cầu |
| M2 | Cuối tuần 3 | Prototype #1 gửi telemetry lên cloud + API đọc latest chạy demo |
| M3 | Cuối tuần 5 | Prototype #2 ổn định/hiệu chuẩn + app điều khiển bơm qua cloud (demo) |
| M4 | Cuối tuần 6 | Integration test đạt mức demo + UAT đạt tiêu chí tối thiểu |
| M5 | Cuối tuần 7 | Chốt bug + nộp báo cáo/phụ lục + slide |

### 5.4.2. Ý nghĩa từng mốc
- **M1** giúp “đóng” phạm vi để lập WBS/Gantt/Budget nhất quán.
- **M2–M3** là các mốc kỹ thuật then chốt để giảm rủi ro tích hợp (IoT thường trễ ở giai đoạn này).
- **M4** là mốc xác nhận chất lượng ở mức demo và sẵn sàng tổng kết.

## 5.5. Dự toán ngân sách

### 5.5.1. Nguyên tắc và giả định dự toán
- Dự toán phục vụ mục tiêu lập kế hoạch học phần; số liệu mang tính ước tính dựa trên giá thị trường phổ biến.
- Dự án giả định có **02 prototypes** để phục vụ kiểm thử tích hợp và dự phòng rủi ro linh kiện.
- Không bắt buộc tính chi phí nhân sự; nếu cần có thể xem như **chi phí cơ hội**.

### 5.5.2. Bảng ngân sách sơ bộ (có phân loại)
**Bảng 5.4. Ngân sách sơ bộ**

| Hạng mục | Nhóm chi phí | Tính chất | Số lượng | Đơn giá (VND) | Thành tiền (VND) | Ghi chú/giả định |
|---|---|---|---:|---:|---:|---|
| Linh kiện prototype | Direct | Variable | 2 | 1,050,000 | 2,100,000 | ESP32 + sensor + relay/MOSFET + bơm + vật tư cơ bản |
| Vật tư phụ | Direct | Variable | 1 | 400,000 | 400,000 | Dây, ống, jack, keo, phụ kiện |
| Dụng cụ (nếu cần) | Direct | Fixed | 1 | 600,000 | 600,000 | Có thể = 0 nếu mượn/đã có |
| Ship/đi lại/in ấn | Indirect | Fixed | 1 | 300,000 | 300,000 | Ước tính |
| Cloud (free-tier) dự phòng | Indirect | Fixed | 1 | 200,000 | 200,000 | Phát sinh dịch vụ/duy trì demo |
| **Tổng tạm tính** |  |  |  |  | **3,600,000** | Không bao gồm chi phí nhân sự |

### 5.5.3. Dự phòng rủi ro chi phí (Contingency)
Nhóm đề xuất dự phòng **10%** cho các phát sinh thường gặp (thiếu linh kiện, hỏng cảm biến/bơm, phí vận chuyển tăng).

- Dự phòng (10%): 360,000 VND
- **Tổng dự kiến (bao gồm dự phòng): 3,960,000 VND**

### 5.5.4. Chi phí nhân sự (tuỳ chọn – chi phí cơ hội)
Nếu cần mô phỏng chi phí nhân sự theo giờ:
- Giả định 05 thành viên × 07 tuần × 06 giờ/tuần = 210 giờ
- Đơn giá giả định: 30,000 VND/giờ → 6,300,000 VND

*(Khoản này dùng để minh họa phân tích chi phí, không bắt buộc trong phạm vi học phần nếu giảng viên không yêu cầu.)*

## 5.6. Đường găng (Critical Path) – mức tổng quan
Do dự án IoT có 2 nhánh công việc chạy song song (HW/FW và Cloud/Mobile), đường găng ở mức tổng quan được xem như **hai chuỗi găng hội tụ** tại giai đoạn tích hợp:

- **Nhánh HW/FW:** T1 → T3 → T4 → T5 → T6 → T11
- **Nhánh Cloud/Mobile:** T1 → T2 → T7 → T9 → T10

Hai nhánh này hội tụ tại: **T12 → T13 → T14**.

Nhóm ưu tiên theo dõi chặt các điểm rủi ro gây trễ cao (cung ứng linh kiện, ổn định prototype, command path và tích hợp end‑to‑end) và chuẩn bị phương án dự phòng để giảm nguy cơ trễ tiến độ.
