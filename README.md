# CHƯƠNG 5. BIỂU ĐỒ LỚP (CLASS DIAGRAM)

## 5.1 Nền tảng và quy ước trình bày
Trong phương pháp ICONIX và Kỹ thuật phần mềm, **Class Diagram** là bước thiết kế tĩnh cực kỳ quan trọng được xây đắp từ Sơ đồ Tuần tự (Sequence Diagram). Dựa trên yêu cầu đồ án (1M user, Book lịch, Cart, Thanh toán, Review, XML Catalog...), sơ đồ lớp ở chương này được thiết kế và tuân thủ chặt chẽ các chuẩn mực như hình mẫu của Giảng viên:
- **Tính đóng gói (Encapsulation):** Tất cả các thuộc tính (Attributes) đều được bảo vệ ở mức `private` (ký hiệu `-`), ngăn chặn truy cập trực tiếp.
- **Tính hành vi (Operations):** Trang bị đầy đủ các Hàm/Phương thức (Methods) ở mức `public` (ký hiệu `+`) nhằm mô tả các thao tác nghiệp vụ hệ thống.
- **Quan hệ & Bội số (Multiplicity):** Tái sử dụng quan hệ từ Domain Model, bao gồm Composition (`*--`) cho các vòng đời phụ thuộc chặt chẽ (Cart chứa CartItem) và Association (`-->`) cho tham chiếu chéo.

---

## 5.2 Sơ đồ lớp tổng quan
Sơ đồ tổng quan thể hiện bức tranh toàn cảnh về mặt cấu trúc liên kết giữa các nhóm Entity chính trong hệ thống đặt lịch khám bệnh trực tuyến. Ở sơ đồ này, các thuộc tính và hàm được làm ẩn đi để tập trung vào kiến trúc liên kết ở cấp độ vĩ mô.

```mermaid
classDiagram
  direction LR

  class CustomerAccount
  class CustomerProfile
  class InsuranceInfo

  class ProviderClinic
  class Doctor
  class Specialty
  class MedicalService
  class AvailabilitySlot
  class Catalog

  class Wishlist
  class WishlistItem
  class Cart
  class CartItem

  class BookingOrder
  class BookingItem
  class PaymentTransaction
  class RefundRequest
  class CancelPolicy

  class Review
  class ReviewModeration
  class EditorialPost

  class Partner
  class PartnerCatalogImport
  class MiniCatalogExport

  class AppointmentTicket
  class Notification

  CustomerAccount "1" --> "0..1" CustomerProfile
  CustomerProfile "1" --> "0..n" InsuranceInfo

  ProviderClinic "1" --> "0..n" Doctor
  Doctor "0..n" --> "1" Specialty
  ProviderClinic "1" --> "0..n" MedicalService
  ProviderClinic "1" --> "0..n" AvailabilitySlot
  ProviderClinic "1" --> "0..1" CancelPolicy

  Catalog "1" --> "0..n" ProviderClinic
  Catalog "1" --> "0..n" Doctor
  Catalog "1" --> "0..n" MedicalService
  Catalog "1" --> "0..n" AvailabilitySlot

  CustomerAccount "1" --> "0..1" Wishlist
  Wishlist "1" *-- "0..n" WishlistItem

  CustomerAccount "1" --> "0..1" Cart
  Cart "1" *-- "0..n" CartItem
  CartItem "0..n" --> "0..1" AvailabilitySlot
  CartItem "0..n" --> "0..1" MedicalService

  CustomerAccount "1" --> "0..n" BookingOrder
  BookingOrder "1" *-- "1..n" BookingItem
  BookingOrder "1" --> "0..n" PaymentTransaction
  BookingOrder "1" --> "0..n" RefundRequest

  CustomerAccount "1" --> "0..n" Review
  Review "0..n" --> "0..1" Doctor
  Review "0..n" --> "0..1" ProviderClinic
  Review "1" --> "0..1" ReviewModeration

  EditorialPost "0..n" --> "0..1" Doctor
  EditorialPost "0..n" --> "0..1" ProviderClinic

  Partner "1" --> "0..n" PartnerCatalogImport
  Partner "1" --> "0..n" MiniCatalogExport
  Catalog "1" --> "0..n" PartnerCatalogImport
  Catalog "1" --> "0..n" MiniCatalogExport

  BookingOrder "1" --> "0..1" AppointmentTicket
  BookingOrder "1" --> "0..n" Notification
```

**Hình 5.1 – Class Diagram tổng quan kiến trúc hệ thống dữ liệu**

---

## 5.3 Sơ đồ lớp chi tiết theo nhóm chức năng
Phần này bóc tách sơ đồ tổng quan thành các phân hệ lõi, bổ sung đầy đủ chi tiết **Thuộc tính (private)** và **Phương thức (public)** bám sát template chuẩn mà giảng viên hướng dẫn yêu cầu.

### 5.3.1 Nhóm UC-00/UC-02/UC-03: Tài khoản – Hồ sơ – Wishlist/Cart
```mermaid
classDiagram
  direction LR

  class CustomerAccount {
    -UUID id
    -String email
    -String password
    -String phone
    -AccountStatus status
    -DateTime createdAt
    +login(email, pass) bool
    +register(data) UUID
    +resetPassword(email) bool
  }

  class CustomerProfile {
    -UUID id
    -String fullName
    -Date dateOfBirth
    -String address
    +updateProfile(data) bool
    +getProfile() data
  }

  class InsuranceInfo {
    -UUID id
    -String providerName
    -String policyNumber
    -Date validUntil
    +addInsurance(data) bool
    +verifyInsurance() bool
  }

  class Wishlist {
    -UUID id
    -DateTime updatedAt
    +addFavorite(targetId) bool
    +removeFavorite(targetId) bool
    +viewWishlist() list
  }

  class WishlistItem {
    -UUID id
    -String targetType
    -UUID targetId
    -DateTime addedAt
  }

  class Cart {
    -UUID id
    -DateTime updatedAt
    +addItem(slotId) bool
    +removeItem(itemId) bool
    +clearCart() void
    +checkout() UUID
  }

  class CartItem {
    -UUID id
    -UUID slotId
    -UUID serviceId
    -Money price
  }

  class AvailabilitySlot {
    -UUID id
    -DateTime startAt
    -DateTime endAt
    -SlotStatus status
    +checkAvailability() bool
    +holdSlot() bool
  }

  class MedicalService {
    -UUID id
    -String name
    -Money basePrice
  }

  class AccountStatus {
    <<enumeration>>
    ACTIVE
    SUSPENDED
  }

  class SlotStatus {
    <<enumeration>>
    AVAILABLE
    HELD
    BOOKED
  }

  CustomerAccount "1" --> "0..1" CustomerProfile
  CustomerProfile "1" --> "0..n" InsuranceInfo
  CustomerAccount "1" --> "0..1" Wishlist
  Wishlist "1" *-- "0..n" WishlistItem
  CustomerAccount "1" --> "0..1" Cart
  Cart "1" *-- "0..n" CartItem
  CartItem "0..n" --> "0..1" AvailabilitySlot
  CartItem "0..n" --> "0..1" MedicalService
```

**Hình 5.2 – Class Diagram chi tiết: Module Quản lý Người dùng & Giỏ hàng**

### 5.3.2 Nhóm UC-01/UC-04/UC-05/UC-06/UC-12: Catalog – Đặt lịch – Thanh toán – Hủy/Hoàn
```mermaid
classDiagram
  direction LR

  class Catalog {
    -DateTime lastSyncAt
    +searchDoctor(keyword, filters) list
    +searchClinic(keyword) list
  }

  class ProviderClinic {
    -UUID id
    -String name
    -String address
    +getDetails() data
  }

  class Doctor {
    -UUID id
    -String fullName
    +getProfile() data
    +getSlots() list
  }

  class Specialty {
    -UUID id
    -String name
  }

  class AvailabilitySlot {
    -UUID id
    -DateTime startAt
    -DateTime endAt
    -SlotStatus status
    +bookSlot() bool
    +releaseSlot() bool
  }

  class BookingOrder {
    -UUID id
    -BookingStatus status
    -Money totalAmount
    -DateTime createdAt
    +createOrder(cartData) UUID
    +cancelOrder() bool
    +updateStatus(status) void
  }

  class BookingItem {
    -UUID id
    -UUID slotId
    -UUID serviceId
    -Money price
  }

  class PaymentTransaction {
    -UUID id
    -String providerRef
    -Money amount
    -PaymentStatus status
    -DateTime createdAt
    +processPayment() bool
    +verifyTransaction() bool
  }

  class RefundRequest {
    -UUID id
    -Money amount
    -RefundStatus status
    -String reason
    +createRefund() UUID
    +approveRefund() bool
  }

  class CancelPolicy {
    -UUID id
    -int cutoffHours
    -String ruleNote
    +checkEligibility(orderTime) bool
  }

  class AppointmentTicket {
    -UUID id
    -String qrRef
    -String icsRef
    -TicketStatus status
    +generateQR() string
    +generateICS() file
  }

  class BookingStatus {
    <<enumeration>>
    DRAFT
    PENDING_PAYMENT
    PAID
    CONFIRMED
    CANCELLED
  }

  class PaymentStatus {
    <<enumeration>>
    INIT
    SUCCESS
    FAILED
  }

  Catalog "1" --> "0..n" ProviderClinic
  ProviderClinic "1" --> "0..n" Doctor
  Doctor "0..n" --> "1" Specialty
  ProviderClinic "1" --> "0..n" AvailabilitySlot
  ProviderClinic "1" --> "0..1" CancelPolicy
  BookingOrder "1" *-- "1..n" BookingItem
  BookingOrder "1" --> "0..n" PaymentTransaction
  BookingOrder "1" --> "0..n" RefundRequest
  BookingOrder "1" --> "0..1" AppointmentTicket
  BookingItem "0..n" --> "0..1" AvailabilitySlot
```

**Hình 5.3 – Class Diagram: Module Đặt lịch, Thanh toán, Ticket & Hủy/Hoàn**

### 5.3.3 Nhóm UC-07/UC-08/UC-09/UC-10/UC-11: Review – Biên tập – Đối tác XML
```mermaid
classDiagram
  direction LR

  class Review {
    -UUID id
    -int rating
    -String content
    -ReviewStatus status
    -DateTime createdAt
    +submitReview(data) bool
    +getReviewsByTarget() list
  }

  class ReviewModeration {
    -UUID id
    -String decision
    -String note
    -DateTime moderatedAt
    +moderateReview(action) bool
  }

  class EditorialPost {
    -UUID id
    -String title
    -String status
    -DateTime publishedAt
    +createDraft() UUID
    +publishPost() bool
  }

  class Partner {
    -UUID id
    -String name
    -String apiKeyRef
    +authenticate() bool
  }

  class PartnerCatalogImport {
    -UUID id
    -String result
    -DateTime importedAt
    +parseXML(file) data
    +syncToCatalog() bool
  }

  class MiniCatalogExport {
    -UUID id
    -String scope
    -DateTime exportedAt
    +queryActiveData() data
    +generateXML() file
  }

  class ReviewStatus {
    <<enumeration>>
    PENDING
    APPROVED
    REJECTED
  }

  Review "1" --> "0..1" ReviewModeration
  Partner "1" --> "0..n" PartnerCatalogImport
  Partner "1" --> "0..n" MiniCatalogExport
```

**Hình 5.4 – Class Diagram chi tiết: Module Đánh giá, Nội dung & Tích hợp Đối tác XML**

### 5.3.4 Nhóm UC-12: Thông báo (Notification)
```mermaid
classDiagram
  direction LR

  class BookingOrder {
    -UUID id
    -BookingStatus status
    +triggerNotification() void
  }

  class Notification {
    -UUID id
    -NotificationChannel channel
    -String recipient
    -NotificationStatus status
    -DateTime sentAt
    +sendEmail() bool
    +sendSMS() bool
    +retryFailed() void
  }

  class NotificationChannel {
    <<enumeration>>
    EMAIL
    SMS
  }

  class NotificationStatus {
    <<enumeration>>
    QUEUED
    DELIVERED
    FAILED
  }

  BookingOrder "1" --> "0..n" Notification
```

**Hình 5.5 – Class Diagram chi tiết: Module Thông báo (Notification Alert)**
