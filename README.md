# CHƯƠNG 6: RỦI RO & CHẤT LƯỢNG

## 6.1. Tổng quan rủi ro

### 6.1.1. Khái niệm
Rủi ro là các sự kiện/điều kiện không chắc chắn, nếu xảy ra có thể ảnh hưởng tiêu cực đến mục tiêu dự án (phạm vi, tiến độ, chi phí, chất lượng). Dự án IoT GreenThumb có rủi ro đặc thù do phụ thuộc **phần cứng**, **kết nối mạng** và **tích hợp đa thành phần** (device–cloud–mobile).

### 6.1.2. Vai trò quản lý rủi ro
Quản lý rủi ro giúp nhóm:
- Nhận diện sớm các rủi ro trọng yếu (đặc biệt ở chuỗi cung ứng và tích hợp).
- Ưu tiên xử lý dựa trên xác suất và mức độ ảnh hưởng.
- Chuẩn bị phương án ứng phó, giảm tác động và tránh trễ tiến độ.

## 6.2. Nhận diện rủi ro

### 6.2.1. Nhóm rủi ro (phân loại)
- **Rủi ro kỹ thuật:** lỗi giao tiếp, reset do sụt áp, sai số cảm biến, lỗi phần mềm.
- **Rủi ro thiết bị/cung ứng:** chậm linh kiện, linh kiện lỗi, bơm hỏng.
- **Rủi ro quản lý/tiến độ:** ước lượng sai, phụ thuộc task tích hợp, thay đổi yêu cầu.
- **Rủi ro chất lượng:** test thiếu bao phủ, lỗi lọt qua UAT.

### 6.2.2. Thang đo đánh giá
Nhóm sử dụng thang đo 1–5 cho **Xác suất (P)** và **Ảnh hưởng (I)**:
- P: 1 rất thấp … 5 rất cao
- I: 1 nhỏ … 5 nghiêm trọng

Điểm rủi ro: **Score = P × I**
- **High:** 15–25
- **Medium:** 8–14
- **Low:** 1–7

## 6.3. Ma trận rủi ro

### 6.3.1. Xác suất
Xác suất đánh giá dựa trên kinh nghiệm nhóm, tính phổ biến của sự cố trong dự án IoT và các phụ thuộc bên ngoài.

### 6.3.2. Mức độ ảnh hưởng
Ảnh hưởng được đánh giá theo tác động lên tiến độ (trễ milestone), chi phí (mua lại linh kiện), chất lượng (không đạt tiêu chí nghiệm thu) và trải nghiệm người dùng.

### 6.3.3. Hình ma trận
**Hình 6.1** thể hiện ma trận rủi ro (Probability–Impact) và vị trí các rủi ro chính.

- File hình tham chiếu: xem [Hinh-6-1-Risk-matrix.html](Hinh-6-1-Risk-matrix.html) để xuất ảnh chèn Word.

## 6.4. Kế hoạch xử lý rủi ro

### 6.4.1. Chiến lược ứng phó
Nhóm áp dụng 4 chiến lược:
- **Tránh (Avoid):** thay đổi kế hoạch để loại bỏ rủi ro.
- **Giảm thiểu (Mitigate):** giảm xác suất hoặc giảm tác động.
- **Chuyển giao (Transfer):** chuyển trách nhiệm/tác động cho bên khác (hạn chế trong bài học phần).
- **Chấp nhận (Accept):** chấp nhận và chuẩn bị phương án dự phòng.

### 6.4.2. Sổ theo dõi rủi ro (Risk Register)
**Bảng 6.1. Risk Register**

| ID | Rủi ro | Nhóm | Nguyên nhân | P | I | Score | Mức | Ứng phó (sơ bộ) | Hành động giảm thiểu | Risk owner | Trigger |
|---|---|---|---|---:|---:|---:|---|---|---|---|---|
| R1 | Chậm cung ứng linh kiện | Cung ứng | Nhà cung cấp/vận chuyển trễ, hết hàng | 4 | 4 | 16 | High | Giảm thiểu | Đặt mua sớm (tuần 2), có linh kiện thay thế, mua dư hạng mục quan trọng | Đặng Thị Thu Giang | Quá hạn giao hàng/không tracking |
| R2 | Lỗi giao tiếp app–cloud–device | Kỹ thuật | Contract không rõ, sai payload/topic, thiếu test tích hợp | 3 | 5 | 15 | High | Giảm thiểu | Chốt API/MQTT contract sớm; mock dữ liệu; test contract trước tích hợp | Phạm Thị Phương | Nhiều lỗi parse/timeout khi tích hợp |
| R3 | Reset ESP32 khi bật bơm (sụt áp) | Thiết bị | Bơm kéo dòng lớn, nguồn không đủ, thiết kế mạch chưa phù hợp | 3 | 4 | 12 | Med | Giảm thiểu | Tách nguồn/đệm tụ; test tải sớm; giới hạn thời gian bơm | Đặng Thị Thu Giang / Đặng Thành Dương | Thiết bị reset khi bơm chạy |
| R4 | Sai lệch cảm biến độ ẩm theo loại đất | Chất lượng | Cảm biến giá rẻ, môi trường đo biến thiên | 4 | 3 | 12 | Med | Giảm thiểu | Hiệu chuẩn 2–3 điểm; lọc số; cho phép cấu hình ngưỡng theo profile | Đặng Thành Dương | Dữ liệu dao động bất thường |
| R5 | Mất Wi‑Fi/Internet làm gián đoạn dữ liệu | Kỹ thuật | Môi trường mạng không ổn định, router lỗi | 3 | 3 | 9 | Med | Giảm thiểu | Reconnect/backoff; buffer dữ liệu; hiển thị offline | Đặng Thành Dương / Phạm Thị Phương | Mất dữ liệu liên tục, device offline |
| R6 | Lộ token/credential (bảo mật) | Bảo mật | Hard-code key, chia sẻ repo không kiểm soát | 2 | 5 | 10 | Med | Giảm thiểu | Không hard-code; dùng env/secret; token theo thiết bị; reset token khi lộ | Phạm Thị Phương | Thấy key trong source / log |
| R7 | Ước lượng sai thời gian tích hợp | Tiến độ | Tích hợp IoT thường phát sinh lỗi khó đoán | 4 | 3 | 12 | Med | Chấp nhận/giảm thiểu | Dự phòng buffer 10–20%; ưu tiên chuỗi đường găng; demo sớm | Trương Quang Huy (#) | Trễ task tích hợp/UAT |
| R8 | Lỗi cảnh báo (false positive/negative) | Chất lượng | Ngưỡng không phù hợp, dữ liệu nhiễu, rule đơn giản | 3 | 3 | 9 | Med | Giảm thiểu | Lọc dữ liệu; hysteresis đơn giản; test kịch bản đất khô/ẩm | Phạm Thị Phương / Nguyễn Thùy Dương | Cảnh báo sai nhiều |
| R9 | Bơm kẹt/chạy khô gây hỏng | Thiết bị | Không có nước, cơ khí kẹt | 2 | 4 | 8 | Med | Giảm thiểu | Giới hạn thời gian bơm; kiểm tra bơm trước demo; dự phòng bơm | Đặng Thị Thu Giang | Bơm kêu to, không hút nước |
| R10 | Thiếu test bao phủ dẫn đến lỗi lọt UAT | Chất lượng | Không có test plan rõ, thiếu test case lỗi mạng | 3 | 4 | 12 | Med | Giảm thiểu | Lập test plan; chạy test theo checklist; log lỗi & ưu tiên sửa | Phạm Thị Phương | Nhiều bug phát hiện muộn ở UAT |

### 6.4.3. Theo dõi và cập nhật
- Cập nhật Risk Register hàng tuần (trạng thái, rủi ro mới, mức ưu tiên).
- Với rủi ro High (R1, R2), nhóm theo dõi theo **mốc** và cập nhật ngay khi có tín hiệu trigger.

## 6.5. Quản lý chất lượng

### 6.5.1. Tiêu chuẩn chất lượng (mức dự án học phần)
Nhóm đặt mục tiêu chất lượng theo tiêu chí:
- Chức năng lõi hoạt động ổn định ở mức demo: **đo cảm biến – hiển thị – cảnh báo – điều khiển bơm**.
- Tích hợp end‑to‑end đạt mức tin cậy tối thiểu theo kịch bản UAT.
- Tài liệu kế hoạch (WBS/Gantt/Budget/Risk/QA) **nhất quán** và có minh chứng.

### 6.5.2. Quy trình kiểm thử phần cứng/firmware
- **Kiểm thử chức năng cảm biến:** đọc dữ liệu theo chu kỳ, kiểm tra ổn định.
- **Kiểm thử độ chính xác tương đối:** so sánh 2–3 trạng thái (đất khô/ẩm) và theo dõi xu hướng.
- **Kiểm thử tải bơm/nguồn:** bật bơm nhiều lần, không reset; đo dòng/điện áp (nếu có điều kiện).
- **Kiểm thử mất kết nối:** ngắt Wi‑Fi, quan sát reconnect và buffer.

### 6.5.3. Quy trình kiểm thử phần mềm (cloud + mobile)
- **Functional test:** dashboard, lịch sử, cảnh báo, điều khiển bơm.
- **Integration test:** app↔cloud↔device, gồm cả trường hợp lỗi (timeout/offline).
- **Connectivity test:** mạng yếu, mất mạng, reconnect.
- **Security basic check:** không hard-code key; token kiểm soát truy cập.

### 6.5.4. Vai trò và trách nhiệm chất lượng
- **Phạm Thị Phương (Cloud/QA):** lập test plan, điều phối chạy test tích hợp, tổng hợp lỗi.
- **Đặng Thị Thu Giang / Đặng Thành Dương:** chịu trách nhiệm test HW/FW (nguồn, bơm, ổn định).
- **Nguyễn Thùy Dương:** test UI/UX và luồng điều khiển/cảnh báo.
- **Trương Quang Huy (#):** chốt tiêu chí nghiệm thu và điều phối UAT.

### 6.5.5. Tiêu chí nghiệm thu (Acceptance Criteria)
**Bảng 6.2. Acceptance Criteria (tối thiểu cho demo)**

| Mã | Tiêu chí | Mức đạt |
|---|---|---|
| AC1 | App hiển thị đúng độ ẩm, nhiệt độ từ prototype #2 | Có dữ liệu cập nhật; hiển thị trạng thái online/offline |
| AC2 | Điều khiển bơm từ app qua cloud hoạt động | Lệnh bật/tắt thành công trong điều kiện mạng ổn định |
| AC3 | Cảnh báo độ ẩm thấp hoạt động | Khi độ ẩm dưới ngưỡng, có cảnh báo/in-app notification |
| AC4 | Hệ thống xử lý mất kết nối cơ bản | Device/app tự phục hồi; app không “treo”, có thông báo lỗi |

## 6.6. Test Cases mẫu

### 6.6.1. Test cases phần cứng/firmware
**Bảng 6.3. Test cases HW/FW (mẫu)**

| TC ID | Mục tiêu | Bước kiểm thử (tóm tắt) | Kết quả mong đợi |
|---|---|---|---|
| HWTC01 | Đọc cảm biến ổn định | Chạy thiết bị 30 phút, chu kỳ 10s | Dữ liệu không bị gián đoạn bất thường |
| HWTC02 | Reconnect Wi‑Fi | Ngắt Wi‑Fi 2 phút rồi bật lại | Thiết bị tự reconnect và gửi lại dữ liệu |
| HWTC03 | Điều khiển bơm an toàn | Gửi lệnh bật bơm liên tục 5 lần | Không reset; bơm tắt theo giới hạn thời gian |
| HWTC04 | Sai số tương đối | So sánh đất khô vs đất ẩm | Dữ liệu phản ánh xu hướng tăng/giảm hợp lý |

### 6.6.2. Test cases cloud
**Bảng 6.4. Test cases Cloud (mẫu)**

| TC ID | Mục tiêu | Bước kiểm thử (tóm tắt) | Kết quả mong đợi |
|---|---|---|---|
| CLTC01 | Ingest telemetry | Device gửi 10 bản ghi liên tiếp | Cloud nhận đủ và lưu trữ |
| CLTC02 | Query lịch sử | App gọi API lấy dữ liệu 24h | Trả dữ liệu đúng định dạng/thời gian |
| CLTC03 | Command delivery | App gửi lệnh bật/tắt | Cloud chuyển lệnh tới device, có ack |
| CLTC04 | Trigger cảnh báo | Đặt độ ẩm dưới ngưỡng | Cloud tạo sự kiện cảnh báo |

### 6.6.3. Test cases ứng dụng di động
**Bảng 6.5. Test cases Mobile (mẫu)**

| TC ID | Mục tiêu | Bước kiểm thử (tóm tắt) | Kết quả mong đợi |
|---|---|---|---|
| SWTC01 | Hiển thị dashboard | Mở app, xem dashboard | Hiển thị độ ẩm/nhiệt/trạng thái |
| SWTC02 | Điều khiển bơm | Nhấn bật/tắt, quan sát phản hồi | Trạng thái cập nhật đúng; có loading/error |
| SWTC03 | Offline UI | Tắt mạng điện thoại | App báo lỗi kết nối, không crash |
| SWTC04 | Cảnh báo | Độ ẩm thấp hơn ngưỡng | App hiển thị cảnh báo/in-app notification |

## 6.7. UAT (tóm tắt)
Nhóm thực hiện UAT theo các kịch bản bám sát Use Case (UC01, UC03, UC05) và chốt nghiệm thu theo các tiêu chí AC1–AC4. Kết quả UAT (nếu có) và danh sách lỗi/khắc phục có thể đưa vào Phụ lục.
