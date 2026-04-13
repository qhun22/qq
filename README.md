# CHƯƠNG 4. BIỂU ĐỒ TRÌNH TỰ (SEQUENCE DIAGRAM)

## 4.1 Nền tảng và quy ước trình bày
Trong ICONIX, Sequence Diagram là bước “thiết kế động” (dynamic design) được phát triển dựa trên kịch bản Use Case (Chương 2) và các đối tượng BCE trong Robustness Diagram (Chương 3). Nếu Robustness trả lời câu hỏi “ai tương tác với ai” (Actor–Boundary–Control–Entity), thì Sequence Diagram trả lời câu hỏi “tương tác theo thứ tự nào” (trình tự message theo thời gian) để hoàn thành mục tiêu use case.

Mục tiêu của các sơ đồ trình tự trong chương này:
- Chuẩn hóa luồng xử lý nghiệp vụ theo đúng BASIC/ALTERNATE COURSE đã mô tả.
- Làm rõ vai trò điều phối của lớp Control (điểm đặt logic nghiệp vụ/kiểm tra hợp lệ), và phạm vi đọc/ghi của các Entity liên quan.
- Tạo nền tảng trực tiếp cho việc triển khai (API/service), kiểm thử (test case theo nhánh alt), và đối chiếu tính nhất quán giữa Use Case ↔ Robustness ↔ Domain.

Nguyên tắc ánh xạ từ Robustness (Chương 3) sang Sequence (Chương 4):
- Actor trong Robustness trở thành tác nhân khởi phát message vào Boundary.
- Boundary đại diện cho màn hình/UI hoặc gateway/endpoint; Boundary chỉ gửi/nhận request/response mức nghiệp vụ.
- Control là nơi điều phối: nhận request từ Boundary, thực hiện kiểm tra/ra quyết định, gọi Entity để truy xuất/cập nhật dữ liệu.
- Entity thể hiện trạng thái dữ liệu nghiệp vụ (đọc/ghi). Các tương tác Entity được thể hiện ở mức “read/save/update” thay vì đi sâu chi tiết truy vấn.

Quy ước trình bày:
- Mỗi use case có 1 sơ đồ trình tự mô tả luồng chính; các nhánh ngoại lệ quan trọng được gom trong khối `alt` (rẽ nhánh loại trừ) hoặc `opt` (tùy chọn).
- Chỉ thể hiện thông điệp ở mức nghiệp vụ (không mô tả chi tiết kỹ thuật như HTTP status, SQL, cache…), nhưng vẫn đủ rõ để truy vết từng bước trong BASIC/ALTERNATE COURSE.
- Tên lifeline ưu tiên bám theo stereotype ICONIX: `<<boundary>>`, `<<control>>`, `<<entity>>` như ở Chương 3 để đối chiếu nhanh.
- Thông điệp dạng gọi xử lý dùng mũi tên liền (`->>`), phản hồi/hiển thị/nhắc lỗi dùng mũi tên nét đứt (`-->>`) để nhấn mạnh đây là kết quả trả về cho Boundary/Actor.
- Với các dịch vụ ngoài hệ thống (cổng thanh toán, dịch vụ QR/ICS, dịch vụ thông báo…), lifeline được biểu diễn như tác nhân/hệ thống bên ngoài để làm rõ ranh giới hệ thống.
- Các bước có điều hướng UI (Boundary→Boundary) chỉ dùng để biểu diễn chuyển màn hình hoặc render/refresh; không dùng để thay thế logic nghiệp vụ vốn phải nằm ở Control.

Lưu ý đánh giá: các sơ đồ trong chương này được thiết kế để bám sát Robustness đã trình bày và tuân thủ quy tắc ICONIX (Actor chỉ tương tác Boundary; Boundary không gọi Entity trực tiếp; Control là trung tâm điều phối nghiệp vụ).

---

## 4.2 Các sơ đồ Sequence Diagram chi tiết cho từng Use Case

### 4.2.1 Sơ đồ trình tự cho UC-00 – Đăng ký/Đăng nhập & quản lý hồ sơ (SD-UC00)

| Use Case: UC-00 – Đăng ký/Đăng nhập & quản lý hồ sơ |
|---|
| **BASIC COURSE**<br>1. Khách hàng chọn đăng ký hoặc đăng nhập.<br>2. Hệ thống hiển thị form và yêu cầu nhập email/số điện thoại + mật khẩu.<br>3. Với đăng ký: kiểm tra trùng; tạo tài khoản; khởi tạo hồ sơ.<br>4. Với đăng nhập: đối chiếu mật khẩu; tạo phiên đăng nhập.<br>5. Khách hàng cập nhật hồ sơ/bảo hiểm (nếu có).<br>6. Hệ thống lưu và thông báo thành công.<br><br>**ALTERNATE COURSE**<br>- Trùng email/số điện thoại / sai thông tin đăng nhập.<br>- Dữ liệu hồ sơ/bảo hiểm không hợp lệ. |

Để sơ đồ dễ đọc (không quá tải lifeline), UC-00 được tách thành 3 sơ đồ tương ứng 3 nhóm thao tác: đăng ký, đăng nhập và quản lý hồ sơ.

```mermaid
sequenceDiagram
  actor CUS as Khách hàng
  participant UI as <<boundary>> Màn hình Đăng ký
  participant AUTH as <<control>> Xử lý Đăng ký
  participant ACC as <<entity>> CustomerAccount
  participant PROF as <<entity>> CustomerProfile
  participant NOTICE as <<boundary>> Thông báo KQ

  CUS->>UI: Nhập email/sđt + mật khẩu
  UI->>AUTH: register(data)
  AUTH->>ACC: checkDuplicate(email/phone)
  alt Trùng email/sđt
    AUTH-->>NOTICE: error(duplicate)
    NOTICE-->>CUS: hiển thị lỗi
  else Hợp lệ
    AUTH->>ACC: createAccount()
    AUTH->>PROF: initProfile()
    AUTH-->>NOTICE: ok()
    NOTICE-->>CUS: thông báo thành công
  end
```

**Hình 4.1a – SD-UC00: Đăng ký tài khoản**

Nội dung Hình 4.1a: Khách hàng thao tác trên Boundary đăng ký; Control kiểm tra trùng và tạo tài khoản/khởi tạo hồ sơ, kèm nhánh ngoại lệ khi trùng email/số điện thoại.

```mermaid
sequenceDiagram
  actor CUS as Khách hàng
  participant UI as <<boundary>> Màn hình Đăng nhập
  participant AUTH as <<control>> Xử lý Đăng nhập
  participant ACC as <<entity>> CustomerAccount
  participant NOTICE as <<boundary>> Thông báo KQ

  CUS->>UI: Nhập email/sđt + mật khẩu
  UI->>AUTH: login(data)
  AUTH->>ACC: verifyCredential()
  alt Sai thông tin
    AUTH-->>NOTICE: error(invalid)
    NOTICE-->>CUS: hiển thị lỗi
  else OK
    AUTH-->>NOTICE: ok(session)
    NOTICE-->>CUS: đăng nhập thành công
  end
```

**Hình 4.1b – SD-UC00: Đăng nhập**

Nội dung Hình 4.1b: Control xác thực thông tin đăng nhập và trả kết quả về Boundary để hiển thị, kèm nhánh ngoại lệ khi sai thông tin.

```mermaid
sequenceDiagram
  actor CUS as Khách hàng
  participant PROF_UI as <<boundary>> Màn hình Hồ sơ
  participant PROF_C as <<control>> Cập nhật Hồ sơ
  participant PROF as <<entity>> CustomerProfile
  participant INS as <<entity>> InsuranceInfo
  participant NOTICE as <<boundary>> Thông báo KQ

  CUS->>PROF_UI: Cập nhật hồ sơ/bảo hiểm
  PROF_UI->>PROF_C: updateProfile(data)
  alt Dữ liệu không hợp lệ
    PROF_C-->>NOTICE: error(validation)
    NOTICE-->>CUS: hiển thị lỗi
  else OK
    PROF_C->>PROF: saveProfile()
    opt Có thông tin bảo hiểm
      PROF_C->>INS: saveInsurance()
    end
    PROF_C-->>NOTICE: ok()
    NOTICE-->>CUS: thông báo thành công
  end
```

**Hình 4.1c – SD-UC00: Quản lý hồ sơ**

Nội dung Hình 4.1c: Khách hàng cập nhật thông tin trên Boundary hồ sơ; Control kiểm tra hợp lệ, lưu `CustomerProfile` và (tùy chọn) `InsuranceInfo`, kèm nhánh ngoại lệ khi dữ liệu không hợp lệ.

### 4.2.2 Sơ đồ trình tự cho UC-01 – Tìm kiếm & xem chi tiết (SD-UC01)

| Use Case: UC-01 – Tìm kiếm & xem chi tiết |
|---|
| **BASIC COURSE**<br>1. Khách hàng nhập tiêu chí tìm kiếm.<br>2. Hệ thống truy vấn catalog và hiển thị danh sách kết quả.<br>3. Khách hàng chọn 1 mục.<br>4. Hệ thống hiển thị trang chi tiết và danh sách slot còn trống.<br><br>**ALTERNATE COURSE**<br>- Không có kết quả.<br>- Slot vừa hết: UI cập nhật lại. |

Để sơ đồ dễ đọc và tránh quá dày đối tượng, UC-01 được tách thành 2 sơ đồ: (1) luồng tìm kiếm và hiển thị danh sách; (2) luồng xem chi tiết và tải slot.

```mermaid
sequenceDiagram
  actor CUS as Khách hàng
  participant SEARCH as <<boundary>> Màn hình Tìm kiếm
  participant Q as <<control>> Xử lý Truy vấn
  participant CAT as <<entity>> Catalog

  CUS->>SEARCH: Nhập tiêu chí
  SEARCH->>Q: search(criteria)
  Q->>CAT: query(criteria)
  alt Không có kết quả
    Q-->>SEARCH: showNoResult()
  else Có kết quả
    Q-->>SEARCH: showList(results)
  end
```

**Hình 4.2a – SD-UC01: Tìm kiếm & hiển thị danh sách**

Nội dung Hình 4.2a: Khách hàng nhập tiêu chí trên Boundary, Control truy vấn `Catalog` và trả kết quả để UI hiển thị danh sách hoặc thông báo không có kết quả.

```mermaid
sequenceDiagram
  actor CUS as Khách hàng
  participant SEARCH as <<boundary>> Màn hình Tìm kiếm
  participant DETAIL as <<boundary>> Trang Chi tiết
  participant F as <<control>> Lấy Chi tiết & Slot
  participant CAT as <<entity>> Catalog
  participant SLOT as <<entity>> AvailabilitySlot

  CUS->>SEARCH: Click chọn một mục
  SEARCH-->>DETAIL: navigateToDetail(itemId)
  DETAIL->>F: loadDetail(itemId)
  F->>CAT: readDetail(itemId)
  F->>SLOT: listAvailableSlots()
  alt Slot vừa hết
    F-->>DETAIL: warnSlotExhausted()
  else OK
    F-->>DETAIL: renderDetail(detail+slots)
  end
```

**Hình 4.2b – SD-UC01: Xem chi tiết & tải slot**

Nội dung Hình 4.2b: Từ danh sách kết quả, Boundary điều hướng sang trang chi tiết. Control tải dữ liệu chi tiết từ các Entity và danh sách slot còn trống, kèm nhánh ngoại lệ khi slot vừa hết.

### 4.2.3 Sơ đồ trình tự cho UC-02 – Quản lý Wishlist (SD-UC02)

| Use Case: UC-02 – Quản lý Wishlist |
|---|
| **BASIC COURSE**<br>1. Khách hàng thêm/xóa mục yêu thích.<br>2. Hệ thống cập nhật wishlist và hiển thị danh sách mới.<br><br>**ALTERNATE COURSE**<br>- Chưa đăng nhập: yêu cầu đăng nhập. |

Sơ đồ mô tả luồng thêm/xóa wishlist thông qua Control quản lý wishlist.

```mermaid
sequenceDiagram
  actor CUS as Khách hàng
  participant W_UI as <<boundary>> Trang Wishlist
  participant W_C as <<control>> Quản lý Wishlist
  participant W as <<entity>> Wishlist
  participant WI as <<entity>> WishlistItem

  CUS->>W_UI: Add/Remove mục
  W_UI->>W_C: updateWishlist(action,item)
  alt Chưa đăng nhập
    W_C-->>W_UI: requireLogin()
  else OK
    W_C->>W: readOrCreateWishlist()
    W_C->>WI: addOrRemoveItem()
    W_C-->>W_UI: refreshList()
  end
```

**Hình 4.3 – SD-UC02: Quản lý Wishlist**

Nội dung Hình 4.3: Boundary nhận thao tác thêm/xóa, Control kiểm tra điều kiện và cập nhật `Wishlist`/`WishlistItem`, sau đó trả danh sách mới hoặc yêu cầu đăng nhập.

### 4.2.4 Sơ đồ trình tự cho UC-03 – Quản lý Cart (SD-UC03)

| Use Case: UC-03 – Quản lý Cart |
|---|
| **BASIC COURSE**<br>1. Khách hàng thêm/xóa mục trong giỏ.<br>2. Hệ thống kiểm tra slot và cập nhật giỏ.<br><br>**ALTERNATE COURSE**<br>- Slot không còn trống.<br>- Xung đột thời gian. |

Sơ đồ mô tả kiểm tra khả dụng slot và cập nhật các Entity giỏ đặt lịch.

```mermaid
sequenceDiagram
  actor CUS as Khách hàng
  participant CART_UI as <<boundary>> Trang Giỏ Hàng
  participant CART_C as <<control>> Quản lý Cart
  participant SLOT as <<entity>> AvailabilitySlot
  participant CART as <<entity>> Cart

  CUS->>CART_UI: Thêm/Xóa mục
  CART_UI->>CART_C: updateCart(action,slotId)
  CART_C->>SLOT: checkAvailability(slotId)
  alt Slot không còn trống
    CART_C-->>CART_UI: reject(slotNotAvailable)
  else OK
    CART_C->>CART: readCart()
    CART_C->>CART: addOrRemoveItem(slotId)
    alt Xung đột thời gian
      CART_C-->>CART_UI: warn(conflict)
    else OK
      CART_C-->>CART_UI: refreshCart()
    end
  end
```

**Hình 4.4 – SD-UC03: Quản lý Cart**

Nội dung Hình 4.4: Control kiểm tra slot, cập nhật `Cart`, và trả thông báo lỗi/xung đột hoặc danh sách giỏ mới.

### 4.2.5 Sơ đồ trình tự cho UC-04 – Đặt lịch & Thanh toán (SD-UC04)

| Use Case: UC-04 – Đặt lịch & Thanh toán |
|---|
| **BASIC COURSE**<br>1. Khách hàng xác nhận giỏ.<br>2. Hệ thống kiểm tra từng mục và khởi tạo đơn chờ thanh toán.<br>3. Khách hàng chọn phương thức thanh toán, hệ thống gọi cổng thanh toán.<br>4. Thanh toán thành công, hệ thống cập nhật Paid/Confirmed và gọi UC-12.<br><br>**ALTERNATE COURSE**<br>- Hết slot / nhiều cơ sở y tế.<br>- Thanh toán thất bại/hủy.<br>- Lỗi phát hành QR/ICS hoặc gửi thông báo: ghi nhận & retry. |

Sơ đồ mô tả luồng checkout và thanh toán, đồng thời thể hiện các nhánh ngoại lệ chính đúng theo Robustness UC-04.

```mermaid
sequenceDiagram
  actor CUS as Khách hàng
  participant CHK_UI as <<boundary>> Màn hình Checkout
  participant CHK as <<control>> Khởi tạo Đơn đặt lịch
  participant SLOT as <<entity>> AvailabilitySlot
  participant ORDER as <<entity>> BookingOrder
  participant PAY as <<control>> Xử lý Thanh Toán
  participant GW as <<boundary>> Gateway Giao tiếp API
  participant PAYG as Cổng thanh toán
  participant TRANS as <<entity>> PaymentTransaction
  participant UC12 as <<control>> Gọi UC-12 phát hành QR

  CUS->>CHK_UI: Xác nhận giỏ
  CHK_UI->>CHK: initOrder(cart)
  CHK->>SLOT: check/lockSlots()
  alt Hết slot
    CHK-->>CHK_UI: reject(slotExhausted)
  else OK
    CHK->>ORDER: createOrder(PendingPayment)
    alt Nhiều cơ sở y tế
      CHK-->>CHK_UI: reject(multiClinic)
    else OK
      CUS->>CHK_UI: Chọn phương thức & thanh toán
      CHK_UI->>PAY: pay(order)
      PAY->>GW: callPaymentAPI(payload)
      GW->>PAYG: processPayment()
      PAYG-->>GW: paymentResult()
      GW-->>PAY: result()
      alt Thanh toán thất bại/hủy
        PAY-->>CHK_UI: showPaymentFailed()
      else Thành công
        PAY->>TRANS: saveTransaction()
        PAY->>ORDER: updateStatus(Paid/Confirmed)
        PAY->>UC12: triggerIssueTicket(orderId)
        alt Lỗi QR/Thông báo
          UC12-->>ORDER: recordRetryableError()
        else OK
          UC12-->>CHK_UI: confirmIssued()
        end
      end
    end
  end
```

**Hình 4.5 – SD-UC04: Đặt lịch & Thanh toán**

Nội dung Hình 4.5: Control khởi tạo đơn kiểm tra/lock slot và tạo `BookingOrder` (PendingPayment). Control thanh toán giao tiếp cổng thanh toán qua gateway, ghi `PaymentTransaction`, cập nhật trạng thái đơn; sau đó kích hoạt UC-12 phát hành QR/ICS và gửi thông báo, kèm nhánh ngoại lệ khi lỗi hoặc thanh toán thất bại.

### 4.2.6 Sơ đồ trình tự cho UC-05 & UC-06 – Hủy lịch & Hoàn tiền (SD-UC05-06)

| Use Case: UC-05 – Hủy lịch; UC-06 – Yêu cầu hoàn tiền |
|---|
| **UC-05 – BASIC COURSE**<br>1. Khách hàng yêu cầu hủy đơn.<br>2. Hệ thống kiểm tra cut-off và cập nhật Cancelled.<br>3. Hệ thống gọi UC-12 để thông báo kết quả.<br><br>**UC-06 – BASIC COURSE**<br>1. Khách hàng yêu cầu hoàn tiền.<br>2. Hệ thống kiểm tra chính sách, tạo `RefundRequest`, gửi cổng thanh toán.<br>3. Cập nhật trạng thái và gọi UC-12 thông báo.<br><br>**ALTERNATE COURSE**<br>- Quá cut-off / không đủ điều kiện / gateway lỗi. |

Để trình bày gọn theo từng chức năng, mục này được tách thành 2 sơ đồ: UC-05 (hủy lịch) và UC-06 (hoàn tiền).

```mermaid
sequenceDiagram
  actor CUS as Khách hàng
  participant ORD_UI as <<boundary>> Trang Lịch Khám
  participant CANCEL as <<control>> Xử lý Hủy Đơn
  participant POL as <<entity>> CancelPolicy
  participant ORD as <<entity>> BookingOrder
  participant NOTICE as <<boundary>> Thông báo KQ

  CUS->>ORD_UI: Yêu cầu hủy đơn
  ORD_UI->>CANCEL: cancel(orderId)
  CANCEL->>POL: checkCutoff(orderId)
  CANCEL->>ORD: readOrder()
  alt Quá cut-off/không hợp lệ
    CANCEL-->>NOTICE: reject(cancelNotAllowed)
    NOTICE-->>CUS: hiển thị lý do
  else OK
    CANCEL->>ORD: updateStatus(Cancelled)
    CANCEL-->>NOTICE: ok()
    NOTICE-->>CUS: thông báo hủy thành công
  end
```

**Hình 4.6a – SD-UC05: Hủy lịch**

Nội dung Hình 4.6a: Khách hàng gửi yêu cầu hủy; Control kiểm tra chính sách cut-off, cập nhật trạng thái `BookingOrder` và trả kết quả để UI thông báo.

```mermaid
sequenceDiagram
  actor CUS as Khách hàng
  participant ORD_UI as <<boundary>> Trang Lịch Khám
  participant REFUND as <<control>> Xử lý Hoàn Tiền
  participant POL as <<entity>> CancelPolicy
  participant ORD as <<entity>> BookingOrder
  participant GW as <<boundary>> Gateway Refund API
  participant PAYG as Cổng thanh toán
  participant NOTICE as <<boundary>> Thông báo KQ

  CUS->>ORD_UI: Yêu cầu hoàn tiền
  ORD_UI->>REFUND: requestRefund(orderId)
  REFUND->>POL: checkRefundPolicy(orderId)
  REFUND->>ORD: readOrder()
  alt Không đủ điều kiện
    REFUND-->>NOTICE: reject(refundNotAllowed)
    NOTICE-->>CUS: hiển thị lý do
  else OK
    REFUND->>GW: callRefundAPI(payload)
    GW->>PAYG: processRefund()
    PAYG-->>GW: refundResult()
    alt Gateway lỗi/tạm thời
      GW-->>REFUND: error()
      REFUND-->>NOTICE: pendingRetry()
      NOTICE-->>CUS: thông báo đang xử lý
    else OK
      GW-->>REFUND: ok()
      REFUND-->>NOTICE: ok()
      NOTICE-->>CUS: thông báo hoàn tiền thành công
    end
  end
```

**Hình 4.6b – SD-UC06: Yêu cầu hoàn tiền**

Nội dung Hình 4.6b: Control kiểm tra điều kiện hoàn, gọi gateway/cổng thanh toán để xử lý hoàn, và trả trạng thái thành công/đang retry hoặc từ chối theo chính sách.

### 4.2.7 Sơ đồ trình tự cho UC-07 & UC-08 – Đánh giá & Kiểm duyệt (SD-UC07-08)

| Use Case: UC-07 – Gửi review; UC-08 – Kiểm duyệt review |
|---|
| **UC-07 – BASIC COURSE**<br>1. Khách hàng gửi review (rating + nội dung).<br>2. Hệ thống lưu review ở trạng thái Pending và thông báo đã tiếp nhận.<br><br>**UC-08 – BASIC COURSE**<br>1. Nhân viên kiểm duyệt duyệt hoặc từ chối review.<br>2. Hệ thống cập nhật trạng thái và ghi nhận log.<br><br>**ALTERNATE COURSE**<br>- Chưa đăng nhập / nội dung không hợp lệ / spam.<br>- Review không còn hợp lệ khi kiểm duyệt. |

Để dễ theo dõi, mục này được tách thành 2 sơ đồ: UC-07 (gửi review) và UC-08 (kiểm duyệt review).

```mermaid
sequenceDiagram
  actor CUS as Khách hàng
  participant REV_UI as <<boundary>> Form Đánh Giá
  participant SEND as <<control>> Lưu Review
  participant REV as <<entity>> Review
  participant NOTICE as <<boundary>> Thông báo KQ

  CUS->>REV_UI: Nhập review + gửi
  REV_UI->>SEND: submitReview(data)
  alt Chưa đăng nhập
    SEND-->>NOTICE: requireLogin()
    NOTICE-->>CUS: yêu cầu đăng nhập
  else Dữ liệu không hợp lệ
    SEND-->>NOTICE: reject(validation)
    NOTICE-->>CUS: hiển thị lỗi
  else OK
    SEND->>REV: save(Pending)
    opt Cảnh báo spam
      SEND-->>NOTICE: warn(spam)
      NOTICE-->>CUS: hiển thị cảnh báo
    end
    SEND-->>NOTICE: ack(received)
    NOTICE-->>CUS: thông báo đã tiếp nhận
  end
```

**Hình 4.7a – SD-UC07: Gửi review**

Nội dung Hình 4.7a: Khách hàng gửi review qua Boundary; Control kiểm tra điều kiện và lưu `Review` ở trạng thái Pending, kèm nhánh ngoại lệ khi chưa đăng nhập/không hợp lệ và tùy chọn cảnh báo spam.

```mermaid
sequenceDiagram
  actor MOD as NV Kiểm duyệt
  participant MOD_UI as <<boundary>> Trang Kiểm duyệt
  participant MODC as <<control>> Duyệt Review
  participant REV as <<entity>> Review
  participant LOG as <<entity>> ReviewModeration

  MOD->>MOD_UI: Mở danh sách chờ duyệt
  MOD_UI->>MODC: moderate(reviewId,decision)
  MODC->>REV: readReview()
  alt Review không còn hợp lệ
    MODC-->>MOD_UI: reject(stale)
  else OK
    MODC->>REV: updateStatus(Approved/Rejected)
    MODC->>LOG: createModerationLog()
    MODC-->>MOD_UI: refreshList()
  end
```

**Hình 4.7b – SD-UC08: Kiểm duyệt review**

Nội dung Hình 4.7b: Nhân viên kiểm duyệt thao tác trên Boundary; Control đọc review, cập nhật trạng thái và ghi log kiểm duyệt, kèm nhánh ngoại lệ khi review không còn hợp lệ.

### 4.2.8 Sơ đồ trình tự cho UC-09 – Quản lý nội dung biên tập (SD-UC09)

| Use Case: UC-09 – Quản lý editorial review |
|---|
| **BASIC COURSE**<br>1. Biên tập viên tạo/lưu nháp.<br>2. Biên tập viên xuất bản bài.<br><br>**ALTERNATE COURSE**<br>- Thiếu thông tin bắt buộc. |

Sơ đồ mô tả luồng soạn thảo và xuất bản bài biên tập.

```mermaid
sequenceDiagram
  actor EDT as Biên tập viên
  participant UI as <<boundary>> Trang Soạn thảo Bài
  participant C as <<control>> Đăng Bài Nhận Định
  participant POST as <<entity>> EditorialPost

  EDT->>UI: Nhập nội dung
  UI->>C: saveDraft(data)
  alt Thiếu thông tin bắt buộc
    C-->>UI: reject(requiredFields)
  else OK
    C->>POST: saveOrUpdate(Draft)
    C-->>UI: saved()
  end

  EDT->>UI: Publish
  UI->>C: publish(postId)
  C->>POST: updateStatus(Published)
  C-->>UI: published()
```

**Hình 4.8 – SD-UC09: Quản lý nội dung biên tập**

Nội dung Hình 4.8: Biên tập viên thao tác trên Boundary, Control lưu nháp/xuất bản và cập nhật `EditorialPost`, kèm nhánh từ chối khi thiếu trường bắt buộc.

### 4.2.9 Sơ đồ trình tự cho UC-10 & UC-11 – Đối tác & XML (SD-UC10-11)

| Use Case: UC-10 – Nhập catalog đối tác; UC-11 – Xuất mini-catalog XML |
|---|
| **UC-10 – BASIC COURSE**<br>1. NV cơ sở y tế gửi dữ liệu catalog.<br>2. Hệ thống xác thực định dạng và hợp nhất vào catalog tổng.<br>3. Ghi log kết quả nhập.<br><br>**UC-11 – BASIC COURSE**<br>1. Đối tác yêu cầu xuất mini-catalog XML.<br>2. Hệ thống kiểm tra quyền, đọc catalog tổng và sinh XML theo schema.<br>3. Ghi log xuất và trả XML.<br><br>**ALTERNATE COURSE**<br>- Dữ liệu sai định dạng/thiếu trường; thiếu dữ liệu; cảnh báo trùng ID. |

Để tránh quá tải, mục này được tách thành 2 sơ đồ: UC-10 (nhập catalog) và UC-11 (xuất mini-catalog XML).

```mermaid
sequenceDiagram
  actor PRO as NV Cơ sở y tế
  participant IMP_UI as <<boundary>> Giao diện Nhập Catalog
  participant IMP as <<control>> Nhập & Hợp nhất Catalog
  participant CAT as <<entity>> Catalog
  participant IMP_LOG as <<entity>> PartnerCatalogImport

  PRO->>IMP_UI: Gửi dữ liệu catalog
  IMP_UI->>IMP: import(payload)
  alt Sai định dạng/thiếu trường
    IMP-->>IMP_UI: reject(formatError)
  else OK
    IMP->>CAT: upsertCatalog()
    IMP->>IMP_LOG: writeImportLog()
    opt Cảnh báo trùng/không khớp ID
      IMP-->>IMP_UI: warn(idConflict)
    end
    IMP-->>IMP_UI: importResult(ok)
  end
```

**Hình 4.9a – SD-UC10: Nhập catalog đối tác**

Nội dung Hình 4.9a: Boundary nhận dữ liệu catalog; Control kiểm tra hợp lệ, hợp nhất vào `Catalog` và ghi `PartnerCatalogImport`, kèm nhánh từ chối khi sai định dạng.

```mermaid
sequenceDiagram
  actor PRT as Hệ thống đối tác
  participant EXP_API as <<boundary>> Cổng API XML
  participant GEN as <<control>> Sinh XML
  participant PARTNER as <<entity>> Partner
  participant CAT as <<entity>> Catalog
  participant EXP_LOG as <<entity>> MiniCatalogExport

  PRT->>EXP_API: GET /mini-catalog
  EXP_API->>GEN: generateXML(scope)
  GEN->>PARTNER: authorize()
  GEN->>CAT: readCatalog(scope)
  alt Thiếu dữ liệu
    GEN-->>EXP_API: emptyXMLOrError()
  else OK
    GEN->>EXP_LOG: writeExportLog()
    GEN-->>EXP_API: xmlResponse()
  end
```

**Hình 4.9b – SD-UC11: Xuất mini-catalog XML**

Nội dung Hình 4.9b: Đối tác gọi Boundary API; Control kiểm tra quyền theo `Partner`, đọc `Catalog` để sinh XML, ghi log `MiniCatalogExport`, và trả kết quả hoặc thông báo thiếu dữ liệu.

### 4.2.10 Sơ đồ trình tự cho UC-12 – Phát hành QR/ICS & gửi thông báo (SD-UC12)

| Use Case: UC-12 – Phát hành QR/ICS & gửi thông báo |
|---|
| **BASIC COURSE**<br>1. UC-12 được kích hoạt bởi sự kiện trạng thái đơn (Confirmed/Cancelled/Refunded) hoặc yêu cầu gửi lại từ khách hàng.<br>2. Hệ thống phát hành QR/ICS qua dịch vụ ngoài và lưu ticket.<br>3. Hệ thống gửi thông báo email/SMS qua dịch vụ ngoài và lưu log trạng thái gửi.<br><br>**ALTERNATE COURSE**<br>- Dịch vụ QR/ICS hoặc thông báo lỗi: ghi nhận & retry.<br>- Gửi lại: dùng ticket có sẵn (hoặc phát hành lại nếu cần). |

Sơ đồ mô tả phát hành vé và gửi thông báo như một luồng dùng lại bởi các use case thanh toán/hủy/hoàn.

```mermaid
sequenceDiagram
  actor CUS as Khách hàng
  participant UI as <<boundary>> Trang Chi tiết Đơn
  participant TRIG as <<control>> Xử lý Sự kiện Trạng Thái
  participant TKT as <<control>> Xử lý Phát Hành
  participant NOTC as <<control>> Điều phối Thông Báo
  participant FILE_GW as <<boundary>> Cổng Giao tiếp File
  participant QR as Dịch vụ QR/ICS
  participant PUSH_GW as <<boundary>> Cổng Push Thông báo
  participant NOTS as Dịch vụ Thông báo
  participant TKT_E as <<entity>> AppointmentTicket
  participant NOT_E as <<entity>> Notification

  CUS->>UI: Yêu cầu gửi lại vé/ICS (nếu có)
  UI->>TRIG: requestResend(orderId)
  TRIG->>TKT: issueOrReuseTicket(orderId)
  TKT->>FILE_GW: requestQRICS(orderId)
  FILE_GW->>QR: generate()
  alt QR/ICS lỗi
    QR-->>FILE_GW: error()
    TKT-->>NOTC: recordRetry(QR)
  else OK
    QR-->>FILE_GW: payload()
    FILE_GW-->>TKT: payload()
    TKT->>TKT_E: saveTicket()
    TKT->>NOTC: notify(orderId)
  end

  NOTC->>PUSH_GW: push(message)
  PUSH_GW->>NOTS: send(email/sms)
  alt Thông báo lỗi
    NOTS-->>PUSH_GW: error(status)
    PUSH_GW-->>NOTC: failed(status)
    NOTC->>NOT_E: saveNotification(failed)
  else OK
    NOTS-->>PUSH_GW: ok(status)
    PUSH_GW-->>NOTC: delivered(status)
    NOTC->>NOT_E: saveNotification(delivered)
  end
```

**Hình 4.10 – SD-UC12: Phát hành QR/ICS & gửi thông báo**

Nội dung Hình 4.10: UC-12 nhận kích hoạt từ UI hoặc sự kiện trạng thái, Control phát hành tương tác dịch vụ QR/ICS và lưu `AppointmentTicket`, sau đó Control điều phối gửi thông báo và lưu `Notification`, kèm nhánh lỗi để retry.
