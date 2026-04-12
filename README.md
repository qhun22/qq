# CHƯƠNG 3. BIỂU ĐỒ MẠNH MẼ (ROBUSTNESS DIAGRAM)

## 3.1 Nền tảng phân tích và quy ước ký hiệu
Trong tiến trình mô hình hóa định hướng Use-Case (ICONIX), Sơ đồ Robustness đóng vai trò là "cầu nối" (phân tích) kết nối giữa các kịch bản Use Case (Chương 2) và các lớp tĩnh trong Domain Model (Chương 1). 

Mục tiêu của sơ đồ là phân tích luồng sự kiện (BASIC/ALTERNATE COURSE) thành ba nhóm đối tượng nhằm chứng minh mô hình miền đã đủ sức mạnh để chạy kịch bản Use Case:
1. **Tác nhân (Actor)**: Yếu tố bên ngoài tương tác với hệ thống.
2. **Giao diện (Boundary)**: Nơi Actor tương tác (Màn hình, Cổng API v.v).
3. **Xử lý (Control)**: Đóng vai trò thực thi nghiệp vụ, kiểm tra tính hợp lệ.
4. **Thực thể (Entity)**: Các dữ liệu nghiệp vụ có từ Domain Model.

**Bốn quy tắc vàng (ICONIX rules) áp dụng nghiêm ngặt:**
*   Actor KHÔNG trực tiếp gọi Entity.
*   Boundary KHÔNG trực tiếp gọi Entity (mà phải giao tiếp qua Control).
*   Boundary KHÔNG nối trực tiếp với Boundary (ngoại trừ điều hướng hiển thị).
*   Actor chỉ tương tác với Boundary.

Để thống nhất cách trình bày sơ đồ trong báo cáo (dễ đọc và dễ đối chiếu theo ICONIX), phần Robustness được mô tả theo chuẩn Boundary–Control–Entity (BCE) với 4 nhóm đối tượng:

*   **Actor**: tác nhân bên ngoài hệ thống.
*   **Boundary**: điểm tương tác (màn hình, form, trang, cổng/gateway API, v.v.).
*   **Control**: lớp điều phối xử lý nghiệp vụ, kiểm tra hợp lệ, áp quy tắc.
*   **Entity**: dữ liệu nghiệp vụ (các lớp trong Domain Model).

Khi vẽ bằng StarUML, có thể gắn stereotype tương ứng (<<boundary>>, <<control>>, <<entity>>) để phân biệt; Actor dùng ký pháp UML chuẩn.

---

## 3.2 Các sơ đồ Robustness chi tiết cho từng Use Case

### 3.2.1 Sơ đồ thiết kế cho UC-00 – Đăng ký/Đăng nhập & quản lý hồ sơ (RB-UC00)

Sơ đồ thể hiện chu trình nhận form Đăng nhập đến việc đối chiếu và khởi tạo `CustomerAccount` hoặc `CustomerProfile`.

```mermaid
flowchart TD
  A_CUS[[Khách hàng]]
  B_UI([Màn hình Đăng ký/Đăng nhập])
  B_PROF([Màn hình Hồ sơ])
  
  C_AUTH((Xác thực / Đăng ký))
  C_PROF((Cập nhật Hồ sơ))
  
  E_ACC[(CustomerAccount)]
  E_CRED[(Credential)]
  E_PROF[(CustomerProfile)]
  E_INS[(InsuranceInfo)]

  A_CUS -->|1. Nhập liệu| B_UI
  B_UI -->|2. Request| C_AUTH
  
  C_AUTH -.->|Báo lỗi trùng/sai| B_UI
  C_AUTH -->|3. Đối chiếu Credential| E_CRED
  C_AUTH -->|4. Lưu CustomerAccount| E_ACC
  
  A_CUS -->|5. Sửa thông tin| B_PROF
  B_PROF -->|6. Gửi request| C_PROF
  C_PROF -->|7. Cập nhật| E_PROF
  C_PROF -->|Cập nhật BH| E_INS
  C_PROF -.->|Báo lỗi format| B_PROF
```

**Hình 3.1 – RB-UC00: Đăng ký/Đăng nhập & quản lý hồ sơ**

Nội dung Hình 3.1: Khách hàng thao tác trên màn hình Đăng ký/Đăng nhập và Hồ sơ (Boundary). Hệ thống xử lý xác thực/đăng ký và cập nhật hồ sơ (Control), đọc/ghi các Entity `Credential`, `CustomerAccount`, `CustomerProfile`, `InsuranceInfo`; có nhánh báo lỗi khi trùng/sai hoặc dữ liệu không hợp lệ.

### 3.2.2 Sơ đồ thiết kế cho UC-01 – Tìm kiếm & xem chi tiết (RB-UC01)

```mermaid
flowchart TD
  A_CUS[[Khách hàng]]
  
  B_SEARCH([Màn hình Tìm Kiếm])
  B_DETAIL([Trang Chi tiết])
  
  C_QUERY((Xử lý Truy vấn))
  C_FETCH((Lấy Chi tiết & Slot))
  
  E_CAT[(Catalog)]
  E_CLINIC[(ProviderClinic)]
  E_DOC[(Doctor)]
  E_SPEC[(Specialty)]
  E_SERV[(MedicalService)]
  E_SLOT[(AvailabilitySlot)]

  A_CUS -->|1. Nhập tiêu chí| B_SEARCH
  B_SEARCH --> C_QUERY
  C_QUERY -->|2. Lọc danh sách| E_CAT
  C_QUERY -->|Trả data view| B_SEARCH
  
  A_CUS -->|3. Click xem chi tiết| B_DETAIL
  B_DETAIL --> C_FETCH
  C_FETCH --> E_CLINIC
  C_FETCH --> E_DOC
  C_FETCH --> E_SPEC
  C_FETCH --> E_SERV
  C_FETCH --> E_SLOT
  C_FETCH -->|4. Đẩy data view| B_DETAIL
  
  C_FETCH -.->|Nếu hết Slot cảnh báo| B_DETAIL
```

**Hình 3.2 – RB-UC01: Tìm kiếm & xem chi tiết**

Nội dung Hình 3.2: Khách hàng tìm kiếm và xem chi tiết (Boundary). Control xử lý truy vấn và tải chi tiết/slot, truy cập `Catalog` và các Entity `ProviderClinic`, `Doctor`, `Specialty`, `MedicalService`, `AvailabilitySlot`; có nhánh cảnh báo khi slot vừa hết.

### 3.2.3 Sơ đồ thiết kế cho UC-02 – Quản lý Wishlist (RB-UC02)

```mermaid
flowchart TD
  A_CUS[[Khách hàng]]
  
  B_WISH([Trang Wishlist])
  C_MAN_WISH((Quản lý Wishlist))
  
  E_WISH[(Wishlist)]
  E_WISH_ITEM[(WishlistItem)]

  A_CUS -->|1. Add/Remove| B_WISH
  B_WISH --> C_MAN_WISH
  C_MAN_WISH -->|Kiểm tra List| E_WISH
  C_MAN_WISH -->|Cập nhật item| E_WISH_ITEM
  C_MAN_WISH -.->|Lỗi trạng thái| B_WISH
```

**Hình 3.3 – RB-UC02: Quản lý Wishlist**

Nội dung Hình 3.3: Khách hàng thêm/xóa mục yêu thích trên Trang Wishlist (Boundary). Control quản lý wishlist kiểm tra trạng thái và cập nhật `Wishlist`/`WishlistItem`, trả thông báo lỗi khi trạng thái không hợp lệ.

### 3.2.4 Sơ đồ thiết kế cho UC-03 – Quản lý Cart (RB-UC03)

```mermaid
flowchart TD
  A_CUS[[Khách hàng]]
  
  B_CART([Trang Giỏ Hàng])
  C_MAN_CART((Quản lý Cart))
  
  E_CART[(Cart)]
  E_CART_ITEM[(CartItem)]
  E_SLOT[(AvailabilitySlot)]

  A_CUS -->|1. Thêm vào Giỏ| B_CART
  B_CART --> C_MAN_CART
  C_MAN_CART -->|Check khả dụng| E_SLOT
  C_MAN_CART -->|Kiểm tra trạng thái| E_CART
  C_MAN_CART -->|Tạo Item| E_CART_ITEM
  C_MAN_CART -.->|Báo lỗi xung đột| B_CART
```

**Hình 3.4 – RB-UC03: Quản lý Cart (giỏ đặt lịch)**

Nội dung Hình 3.4: Khách hàng thao tác trên Trang Giỏ hàng (Boundary). Control quản lý cart kiểm tra khả dụng của `AvailabilitySlot`, kiểm tra `Cart` và tạo/cập nhật `CartItem`; có nhánh báo lỗi khi xung đột/lỗi trạng thái.

### 3.2.5 Sơ đồ thiết kế cho UC-04 – Đặt lịch & Thanh toán (RB-UC04)

```mermaid
flowchart TD
  A_CUS[[Khách hàng]]
  A_PAYG[[Cổng thanh toán]]
  
  B_CHK_UI([Màn hình Checkout])
  B_GATEWAY([Gateway Giao tiếp API])
  
  C_CHK((Khởi tạo BookingOrder))
  C_PAY((Xử lý Thanh Toán))
  C_CALL_12((Gọi UC-12 phát hành QR))
  
  E_CART_ITEM[(CartItem)]
  E_SLOT[(AvailabilitySlot)]
  E_ORDER[(BookingOrder)]
  E_ORDER_ITEM[(BookingItem)]
  E_TRANS[(PaymentTransaction)]

  A_CUS -->|1. Chốt giỏ| B_CHK_UI
  B_CHK_UI --> C_CHK
  C_CHK -->|Lock Slot| E_SLOT
  C_CHK -->|Đọc| E_CART_ITEM
  C_CHK -->|Tạo List Item| E_ORDER_ITEM
  C_CHK -->|Tạo Chờ TT| E_ORDER
  C_CHK -.->|Hết Slot| B_CHK_UI
  
  A_CUS -->|2. Đồng ý TT| B_CHK_UI
  B_CHK_UI --> C_PAY
  C_PAY -->|3. Gọi API| B_GATEWAY
  B_GATEWAY <--> A_PAYG
  
  B_GATEWAY -->|4. Nhận Payload| C_PAY
  C_PAY -->|Lưu vết GD| E_TRANS
  C_PAY -->|Đổi Confirmed| E_ORDER
  C_PAY --> C_CALL_12
```

**Hình 3.5 – RB-UC04: Đặt lịch & Thanh toán**

Nội dung Hình 3.5: Khách hàng checkout (Boundary). Control khởi tạo đơn lock slot (`AvailabilitySlot`), đọc `CartItem`, tạo `BookingItem` và `BookingOrder`. Control thanh toán giao tiếp với gateway/cổng thanh toán, ghi `PaymentTransaction`, cập nhật trạng thái đơn và gọi UC-12 khi đủ điều kiện.

### 3.2.6 Sơ đồ thiết kế cho UC-05 & UC-06 – Hủy lịch & Hoàn tiền (RB-UC05-06)

```mermaid
flowchart TD
  A_CUS[[Khách hàng]]
  A_PAYG[[Cổng thanh toán]]
  
  B_ORDER([Trang Lịch Khám])
  B_GATEWAY([Gateway Refund API])
  
  C_CANCEL((Xử lý Hủy Đơn))
  C_REFUND((Xử lý Hoàn Tiền))
  C_CALL_12((Gọi UC-12 cập nhật))
  
  E_ORD[(BookingOrder)]
  E_POL[(CancelPolicy)]
  E_REF[(RefundRequest)]

  A_CUS -->|1. Yêu cầu Hủy| B_ORDER
  B_ORDER --> C_CANCEL
  C_CANCEL -->|Đọc Cutoff| E_POL
  C_CANCEL -->|Đọc t/g tạo| E_ORD
  C_CANCEL -.->|Từ chối do quá giờ| B_ORDER
  
  C_CANCEL -->|Set Canceled| E_ORD
  C_CANCEL --> C_CALL_12
  
  C_CANCEL -->|Gửi request ngầm nếu thoả đk| C_REFUND
  A_CUS -->|Trực tiếp Y/C Hoàn tiền| B_ORDER
  B_ORDER --> C_REFUND
  
  C_REFUND -->|Tạo Phiếu Hoàn| E_REF
  C_REFUND --> B_GATEWAY
  B_GATEWAY <--> A_PAYG
  B_GATEWAY -->|Trạng thái| C_REFUND
  C_REFUND --> C_CALL_12
```

**Hình 3.6 – RB-UC05-06: Hủy lịch & Hoàn tiền**

Nội dung Hình 3.6: Khách hàng yêu cầu hủy/hoàn tiền trên Trang Lịch khám (Boundary). Control hủy đơn kiểm tra `CancelPolicy` và `BookingOrder` theo cut-off, cập nhật trạng thái; Control hoàn tiền tạo `RefundRequest` và giao tiếp gateway thanh toán. Sau các thay đổi quan trọng, hệ thống gọi UC-12 để cập nhật ticket/thông báo.

### 3.2.7 Sơ đồ thiết kế cho UC-07 & UC-08 – Đánh giá & Kiểm duyệt (RB-UC07-08)

Đã bổ sung liên kết Review tới cả Doctor và ProviderClinic như kịch bản đặt ra.

```mermaid
flowchart TD
  A_CUS[[Khách hàng]]
  A_MOD[[NV Kiểm duyệt]]
  
  B_REV([Form Đánh Giá])
  B_MOD([Trang Kiểm duyệt])
  
  C_SEND((Lưu & Chờ duyệt))
  C_MOD((Duyệt Trạng Thái))
  
  E_REV[(Review)]
  E_REV_MOD[(ReviewModeration)]
  E_DOC[(Doctor)]
  E_CLINIC[(ProviderClinic)]

  A_CUS -->|1. Submit sao| B_REV
  B_REV --> C_SEND
  C_SEND -->|Gắn khóa Bác sĩ| E_DOC
  C_SEND -->|hoặc khóa Cơ sở| E_CLINIC
  C_SEND -->|Insert| E_REV
  C_SEND -.->|Cảnh báo spam| B_REV
  
  A_MOD -->|2. Check| B_MOD
  B_MOD --> C_MOD
  C_MOD -->|Tạo log| E_REV_MOD
  C_MOD -->|Update Approved| E_REV
```

**Hình 3.7 – RB-UC07-08: Đánh giá & Kiểm duyệt**

Nội dung Hình 3.7: Khách hàng gửi đánh giá qua Form Đánh giá (Boundary). Control lưu & chờ duyệt tạo `Review` và liên kết tới `Doctor` hoặc `ProviderClinic`. Nhân viên kiểm duyệt thao tác trên Trang Kiểm duyệt (Boundary); Control duyệt trạng thái cập nhật kết quả và ghi `ReviewModeration`.

### 3.2.8 Sơ đồ thiết kế cho UC-09 – Quản lý nội dung biên tập (RB-UC09)

```mermaid
flowchart TD
  A_EDT[[Biên tập viên]]
  
  B_EDT([Trang Soạn thảo Bài])
  C_POST((Đăng Bài Nhận Định))
  E_POST[(EditorialPost)]

  A_EDT -->|1. Publish| B_EDT
  B_EDT --> C_POST
  C_POST -->|Save/Update| E_POST
```

**Hình 3.8 – RB-UC09: Quản lý nội dung biên tập**

Nội dung Hình 3.8: Biên tập viên thao tác trên Trang soạn thảo (Boundary). Control đăng bài/biên tập tạo hoặc cập nhật Entity `EditorialPost`.

### 3.2.9 Sơ đồ thiết kế nhóm Đối tác & XML (RB-UC10-11)

```mermaid
flowchart TD
  A_PRO[[NV Cơ sở y tế]]
  A_PRT[[Hệ thống đối tác]]
  
  B_IMP([Giao diện Nhập Catalog])
  B_EXP([Cổng API XML])
  
  C_VAL((Xác thực Định dạng))
  C_MERGE((Hợp nhất Catalog))
  C_GEN((Sinh Schema XML))
  
  E_CAT[(Catalog)]
  E_PARTNER[(Partner)]
  E_PRT[(PartnerCatalogImport)]
  E_EXP[(MiniCatalogExport)]

  A_PRO -->|1. Gửi data| B_IMP
  B_IMP --> C_VAL
  C_VAL -.->|Báo lỗi| B_IMP
  
  C_VAL --> C_MERGE
  C_MERGE -->|2. Log data| E_PRT
  C_MERGE -->|3. Merge/Update ID| E_CAT
  
  A_PRT -->|REST Get| B_EXP
  A_PRO -->|Export click| B_EXP
  
  B_EXP --> C_GEN
  C_GEN -->|Xác nhận quyền| E_PARTNER
  C_GEN -->|Đọc Entity| E_CAT
  C_GEN -->|Log xuất| E_EXP
  C_GEN -->|Response| B_EXP
```

**Hình 3.9 – RB-UC10-11: Nhập catalog đối tác & Xuất mini-catalog XML**

Nội dung Hình 3.9: Nhân viên cơ sở y tế nhập catalog qua Giao diện Nhập (Boundary), Control xác thực định dạng và hợp nhất catalog ghi log `PartnerCatalogImport` và cập nhật `Catalog`. Khi xuất XML, Control sinh schema kiểm tra quyền theo `Partner`, đọc `Catalog`, ghi log `MiniCatalogExport` và trả kết quả qua Cổng API XML.

### 3.2.10 Sơ đồ thiết kế phân đoạn cho UC-12 (RB-UC12)

```mermaid
flowchart TD
  A_CUS[[Khách hàng]]
  A_QR[[Dịch vụ QR/ICS]]
  A_NOT[[Dịch vụ Thông báo]]
  
  B_TKT([Cổng Giao tiếp File])
  B_NOT([Cổng Push Thông báo])
  B_UI([Trang Chi tiết Đơn])
  
  C_TRIG((Xử lý Sự kiện Trạng Thái))
  C_TKT((Xử lý Phát Hành))
  C_NOT((Điều phối Thông Báo))
  
  E_TKT[(AppointmentTicket)]
  E_NOT[(Notification)]

  A_CUS -->|Yêu cầu gửi lại| B_UI
  B_UI --> C_TRIG
  
  C_TRIG -->|Trigger ngầm từ UC-04/05/06| C_TKT
  C_TKT -->|Yêu cầu gen QR| B_TKT
  B_TKT <--> A_QR
  
  B_TKT -->|Lưu mã Ticket| E_TKT
  C_TKT --> C_NOT
  
  C_NOT -->|Push MSG| B_NOT
  B_NOT <--> A_NOT
  
  B_NOT -->|Lưu vết trạng thái gửi| E_NOT
  B_NOT -.->|Báo lỗi / Retry| C_NOT
```

**Hình 3.10 – RB-UC12: Phát hành QR/ICS & gửi thông báo**

Nội dung Hình 3.10: UC-12 được kích hoạt bởi sự kiện trạng thái đơn (từ UC-04/05/06) hoặc yêu cầu gửi lại từ Trang Chi tiết đơn (Boundary). Control phát hành tương tác dịch vụ QR/ICS và lưu `AppointmentTicket`; Control điều phối thông báo gửi qua cổng push, lưu vết `Notification` và xử lý retry khi lỗi.

---
*Ghi chú đánh giá độ bền vững (Robustness Verification)*: 
Quá trình thiết kế hệ thống đồ thị đã nỗ lực tuân thủ ICONIX. Sự nhất quán giữa Use Case (Chương 2) và Domain (Chương 1) được phân tích và trích xuất. Các cặp Entity tổ hợp đã được bóc tách và các Use Case đã được nhóm theo Ma trận truy vết ở Chương 2, tạo nền tảng mang ý nghĩa tham khảo 1:1 cho Sequence Diagram ở Chương 4.
