# CHƯƠNG 7: TRIỂN KHAI & KẾT LUẬN

## 7.1. Kế hoạch triển khai

### 7.1.1. Sản xuất (giả định)
Trong phạm vi học phần, nhóm chỉ mô phỏng kế hoạch triển khai và sản xuất. Ở bối cảnh thực tế, quy trình sản xuất có thể được triển khai theo các bước:

- **Chốt thiết kế & BOM:** cố định danh sách linh kiện (BOM), lựa chọn phương án thay thế cho linh kiện rủi ro cao.
- **Sản xuất thử (Pilot run):** sản xuất lô nhỏ để kiểm tra lỗi lắp ráp, độ ổn định nguồn/bơm và sai số cảm biến.
- **Kiểm tra chất lượng (QC) đầu vào/đầu ra:** kiểm tra linh kiện chính (ESP32, cảm biến, bơm), test nhanh sau lắp ráp.
- **Đóng gói:** hướng dẫn sử dụng, cảnh báo an toàn, tem nhãn.

**Bảng 7.1. Checklist triển khai sản xuất (giả định)**

| Hạng mục | Nội dung | Kết quả mong đợi |
|---|---|---|
| BOM + linh kiện thay thế | Danh sách linh kiện + phương án thay thế | Giảm rủi ro thiếu hàng |
| Quy trình lắp ráp | Các bước lắp và kiểm tra nhanh | Rút ngắn thời gian lắp |
| Test xuất xưởng | Test nguồn/bơm, cảm biến, kết nối Wi‑Fi | Phát hiện lỗi sớm |
| Tài liệu hướng dẫn | HDSD (hướng dẫn sử dụng), lưu ý an toàn, bảo hành | Người dùng dễ tiếp cận |

### 7.1.2. Triển khai ứng dụng (App Store/Google Play)
Nhóm mô phỏng kế hoạch phát hành ứng dụng theo checklist chuẩn, nhằm đảm bảo sản phẩm sẵn sàng ra mắt.

**Bảng 7.2. Checklist phát hành ứng dụng (giả định)**

| Hạng mục | Nội dung |
|---|---|
| Phiên bản (versioning) | Đặt phiên bản, ghi chú phát hành (release notes) |
| Thông tin store | Tên app, mô tả ngắn/dài, từ khóa |
| Hình ảnh | Screenshot màn hình chính, icon, banner (nếu có) |
| Chính sách | Mô tả quyền truy cập (network/notification), chính sách riêng tư mức cơ bản |
| Kiểm thử trước phát hành | Smoke test, test kết nối, test điều khiển bơm |

### 7.1.3. Phân phối sản phẩm
Kế hoạch phân phối (giả định) tập trung vào kênh bán hàng đơn giản và hỗ trợ thiết lập ban đầu:
- Kênh phân phối: online/offline (cửa hàng nhỏ, sàn thương mại điện tử – nếu mở rộng).
- Hỗ trợ onboarding: hướng dẫn kết nối Wi‑Fi, thêm thiết bị, thiết lập ngưỡng cảnh báo.
- Chính sách bảo hành/đổi trả (giả định) để tăng niềm tin người dùng.

## 7.2. Bảo trì và hỗ trợ

### 7.2.1. Bảo trì hệ thống
Nhóm đề xuất phân loại bảo trì theo 3 nhóm:
- **Bảo trì sửa lỗi (Corrective):** sửa lỗi firmware/app/cloud khi phát sinh.
- **Bảo trì phòng ngừa (Preventive):** cải thiện ổn định kết nối, giảm reset, tối ưu cảnh báo.
- **Bảo trì cải tiến (Perfective):** bổ sung tính năng mới theo phản hồi (nâng cấp biểu đồ, nhiều profile cây).

### 7.2.2. Hỗ trợ người dùng (giả định)
**Bảng 7.3. Kế hoạch hỗ trợ sau bán (giả định)**

| Nội dung | Mô tả |
|---|---|
| Kênh hỗ trợ | Email/Zalo/Fanpage (tùy điều kiện) |
| Thời gian phản hồi | 24–48 giờ làm việc (giả định) |
| Phân loại vấn đề | Kết nối Wi‑Fi, cảnh báo, điều khiển bơm, phần cứng (bơm/cảm biến) |
| Quy trình xử lý | Tiếp nhận → phân loại → hướng dẫn khắc phục → ghi nhận lỗi → cập nhật bản vá |
| Tài liệu hỗ trợ | FAQ, hướng dẫn reset thiết bị, hướng dẫn kết nối lại |

## 7.3. KPIs theo dõi (giả định)
Để đo lường hiệu quả quản lý và “sức khỏe” dự án, nhóm đề xuất các KPIs đơn giản (có thể báo cáo theo tuần/sprint):

**Bảng 7.4. KPIs đề xuất**

| KPI | Công thức/định nghĩa | Mục tiêu (giả định) |
|---|---|---|
| KPI1 – Tỷ lệ hoàn thành đúng hạn | Task Done đúng deadline / Task planned | ≥ 80% |
| KPI2 – Test pass rate | Test case Pass / Tổng test case chạy | ≥ 90% |
| KPI3 – Lỗi phát hiện ở UAT | Số lỗi phát hiện ở UAT | Giảm dần theo tuần |
| KPI4 – Tỷ lệ lệnh điều khiển thành công | Số lệnh thành công / tổng lệnh | ≥ 95% (mạng ổn định) |

*(Nếu học phần yêu cầu EVM, nhóm có thể mở rộng thêm SPI/CPI dựa trên dữ liệu giả định.)*

## 7.4. Kết luận
GreenThumb được hoàn thiện dưới dạng hồ sơ lập kế hoạch dự án IoT, bao phủ các nội dung cốt lõi từ yêu cầu–phạm vi, stakeholders, WBS, tiến độ/ngân sách đến rủi ro–chất lượng và kế hoạch triển khai/hỗ trợ, qua đó tạo cơ sở để nhóm quản lý tốt các phụ thuộc phần cứng–phần mềm và giảm rủi ro trong giai đoạn tích hợp end‑to‑end.

## 7.5. Bài học kinh nghiệm (giả định)
- Ước lượng cho các hạng mục tích hợp IoT nên có buffer do độ bất định cao (đề xuất hệ số 1.3–1.5 cho task tích hợp).
- Chốt **API/MQTT contract** sớm giúp phần mềm phát triển song song với phần cứng và giảm rework.
- Rủi ro về **nguồn/bơm** (sụt áp, reset) cần được test sớm ngay từ prototype #1 để tránh dồn lỗi đến giai đoạn tích hợp.
- Việc dùng công cụ quản lý (Jira) và cập nhật trạng thái thường xuyên giúp minh bạch tiến độ và hỗ trợ báo cáo theo tuần.

## 7.6. Hướng phát triển
- Bổ sung thêm cảm biến (ánh sáng, độ ẩm không khí) và xây dựng profile theo loại cây.
- Tự động tưới theo lịch/tham số (auto mode) thay vì chỉ bật/tắt.
- Nâng cấp bảo mật (xoay token, audit log), cải thiện trải nghiệm onboarding.
- Tối ưu phần cứng: vỏ chống nước, cải thiện tiêu thụ năng lượng, pin/solar (nếu mở rộng).

## (Gợi ý) Hình 7.1 – Luồng triển khai (giả định)
Hình 7.1 dưới đây mô tả luồng triển khai tổng quan.

```mermaid
flowchart LR
  A[Chốt BOM + thiết kế] --> B[Pilot run (lô thử)]
  B --> C[QC + Test xuất xưởng]
  C --> D[Đóng gói + HDSD]
  D --> E[Phân phối thiết bị]

  F[Chuẩn bị phát hành app] --> G[Smoke test]
  G --> H[Đăng tải store (giả định)]
  H --> I[Người dùng cài app]

  E --> J[Onboarding: kết nối Wi‑Fi + thêm thiết bị]
  I --> J
  J --> K[Vận hành + hỗ trợ sau bán]
```

- File hình tham chiếu: xem [Hinh-7-1-Deployment-flow.html](Hinh-7-1-Deployment-flow.html) để xuất ảnh chèn Word.

- Tài liệu tham khảo: xem [Tai-lieu-tham-khao.md](Tai-lieu-tham-khao.md).
