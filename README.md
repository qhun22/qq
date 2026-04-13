# CHƯƠNG 5. BIỂU ĐỒ LỚP (CLASS DIAGRAM)

## 5.1 Nền tảng và quy ước trình bày
Trong ICONIX, **Class Diagram** là bước “thiết kế tĩnh” (static design) nhằm mô tả cấu trúc lớp của hệ thống: thuộc tính cốt lõi, các quan hệ (association/composition), bội số (multiplicity) và kế thừa (generalization) khi cần. Class Diagram ở chương này được xây dựng dựa trên:
- **Domain Model (Chương 1)**: tập lớp nghiệp vụ và quan hệ chính.
- **Use Case/Robustness/Sequence (Chương 2–4)**: xác nhận lớp nào thực sự tham gia luồng nghiệp vụ và phạm vi đọc/ghi dữ liệu.

Quy ước:
- Tập trung vào **lớp nghiệp vụ (entity/domain)** và các cấu trúc dữ liệu cần lưu vết; không đi sâu chi tiết kỹ thuật (ORM/SQL/HTTP…).
- Thuộc tính liệt kê ở mức tối thiểu để hiểu mô hình; kiểu dữ liệu dùng ký hiệu chung (`String`, `UUID`, `DateTime`, `Money`, `Enum`).
- Quan hệ **chứa/thành phần** (composition) dùng `*--` để thể hiện vòng đời phụ thuộc (ví dụ: `BookingOrder` chứa `BookingItem`).
- Bội số thể hiện theo thực tế nghiệp vụ đã mô tả ở Chương 1 và các UC.

---

## 5.2 Sơ đồ lớp tổng quan
Sơ đồ tổng quan thể hiện các nhóm lớp chính: tài khoản & hồ sơ, catalog y tế, wishlist/cart, đặt lịch & thanh toán, nội dung & review, đối tác & XML, và nhóm phát hành vé/ thông báo.

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

**Hình 5.1 – Class Diagram tổng quan hệ thống**

Nội dung Hình 5.1: Mô tả cấu trúc lớp nghiệp vụ chính và quan hệ giữa các nhóm đối tượng: từ tài khoản/hồ sơ, catalog dịch vụ y tế, wishlist/cart, đặt lịch & thanh toán, review/nội dung, đến tích hợp đối tác (import/export XML) và nhóm phát hành vé/ thông báo.

---

## 5.3 Sơ đồ lớp chi tiết theo nhóm chức năng
Để tránh sơ đồ tổng quan quá dày, phần này trình bày các sơ đồ chi tiết theo nhóm UC tương ứng, tập trung vào lớp có liên quan trực tiếp đến luồng nghiệp vụ.

### 5.3.1 Nhóm UC-00/UC-02/UC-03: Tài khoản – Hồ sơ – Wishlist/Cart
```mermaid
classDiagram
  direction LR

  class CustomerAccount {
    +UUID id
    +String email
    +String phone
    +AccountStatus status
    +DateTime createdAt
  }

  class CustomerProfile {
    +UUID id
    +String fullName
    +Date dateOfBirth
    +String address
  }

  class InsuranceInfo {
    +UUID id
    +String providerName
    +String policyNumber
    +Date validUntil
  }

  class Wishlist {
    +UUID id
    +DateTime updatedAt
  }

  class WishlistItem {
    +UUID id
    +String targetType
    +UUID targetId
    +DateTime addedAt
  }

  class Cart {
    +UUID id
    +DateTime updatedAt
  }

  class CartItem {
    +UUID id
    +UUID slotId
    +UUID serviceId
    +Money price
  }

  class AvailabilitySlot {
    +UUID id
    +DateTime startAt
    +DateTime endAt
    +SlotStatus status
  }

  class MedicalService {
    +UUID id
    +String name
    +Money basePrice
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

**Hình 5.2 – Class Diagram chi tiết: Tài khoản – Hồ sơ – Wishlist/Cart**

Nội dung Hình 5.2: Thể hiện cấu trúc lớp phục vụ đăng ký/đăng nhập và quản lý hồ sơ (UC-00), cùng các lớp wishlist/cart để lưu lựa chọn trước khi đặt lịch (UC-02/UC-03). `WishlistItem` dùng mô hình tham chiếu tổng quát (`targetType/targetId`) để có thể lưu nhiều loại đối tượng quan tâm.

### 5.3.2 Nhóm UC-01/UC-04/UC-05/UC-06/UC-12: Catalog – Đặt lịch – Thanh toán – Hủy/Hoàn – Phát hành vé
```mermaid
classDiagram
  direction LR

  class Catalog {
    +DateTime lastSyncAt
  }

  class ProviderClinic {
    +UUID id
    +String name
    +String address
  }

  class Doctor {
    +UUID id
    +String fullName
  }

  class Specialty {
    +UUID id
    +String name
  }

  class AvailabilitySlot {
    +UUID id
    +DateTime startAt
    +DateTime endAt
    +SlotStatus status
  }

  class BookingOrder {
    +UUID id
    +BookingStatus status
    +Money totalAmount
    +DateTime createdAt
  }

  class BookingItem {
    +UUID id
    +UUID slotId
    +UUID serviceId
    +Money price
  }

  class PaymentTransaction {
    +UUID id
    +String providerRef
    +Money amount
    +PaymentStatus status
    +DateTime createdAt
  }

  class RefundRequest {
    +UUID id
    +Money amount
    +RefundStatus status
    +String reason
  }

  class CancelPolicy {
    +UUID id
    +int cutoffHours
    +String ruleNote
  }

  class AppointmentTicket {
    +UUID id
    +String qrRef
    +String icsRef
    +TicketStatus status
  }

  class BookingStatus {
    <<enumeration>>
    DRAFT
    PENDING_PAYMENT
    PAID
    CONFIRMED
    CANCELLED
    REFUNDED
  }

  class PaymentStatus {
    <<enumeration>>
    INIT
    SUCCESS
    FAILED
  }

  class RefundStatus {
    <<enumeration>>
    REQUESTED
    PROCESSING
    SUCCEEDED
    REJECTED
  }

  class TicketStatus {
    <<enumeration>>
    ISSUED
    FAILED
    RETRYING
  }

  class SlotStatus {
    <<enumeration>>
    AVAILABLE
    HELD
    BOOKED
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

**Hình 5.3 – Class Diagram chi tiết: Catalog – Đặt lịch – Thanh toán – Hủy/Hoàn – Ticket**

Nội dung Hình 5.3: Mô tả cấu trúc dữ liệu phục vụ tìm kiếm/xem chi tiết theo catalog (UC-01), khởi tạo đơn và thanh toán (UC-04), hủy/hoàn theo `CancelPolicy` (UC-05/UC-06), và phát hành `AppointmentTicket` (UC-12).

### 5.3.3 Nhóm UC-07/UC-08/UC-09/UC-10/UC-11: Review – Biên tập – Đối tác XML
```mermaid
classDiagram
  direction LR

  class CustomerAccount {
    +UUID id
    +String email
  }

  class ProviderClinic {
    +UUID id
    +String name
  }

  class Doctor {
    +UUID id
    +String fullName
  }

  class Review {
    +UUID id
    +int rating
    +String content
    +ReviewStatus status
    +DateTime createdAt
  }

  class ReviewModeration {
    +UUID id
    +String decision
    +String note
    +DateTime moderatedAt
  }

  class EditorialPost {
    +UUID id
    +String title
    +String status
    +DateTime publishedAt
  }

  class Partner {
    +UUID id
    +String name
    +String apiKeyRef
  }

  class Catalog {
    +DateTime lastSyncAt
  }

  class PartnerCatalogImport {
    +UUID id
    +String result
    +DateTime importedAt
  }

  class MiniCatalogExport {
    +UUID id
    +String scope
    +DateTime exportedAt
  }

  class ReviewStatus {
    <<enumeration>>
    PENDING
    APPROVED
    REJECTED
  }

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
```

**Hình 5.4 – Class Diagram chi tiết: Review – Biên tập – Đối tác XML**

Nội dung Hình 5.4: Thể hiện cấu trúc lớp phục vụ review và kiểm duyệt (UC-07/UC-08), nội dung biên tập (UC-09), và luồng nhập/xuất mini-catalog cho đối tác (UC-10/UC-11).

### 5.3.4 Nhóm UC-12: Thông báo (Notification)
```mermaid
classDiagram
  direction LR

  class BookingOrder {
    +UUID id
    +String status
  }

  class Notification {
    +UUID id
    +NotificationChannel channel
    +String recipient
    +NotificationStatus status
    +DateTime sentAt
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

**Hình 5.5 – Class Diagram chi tiết: Notification**

Nội dung Hình 5.5: Mô tả lớp `Notification` lưu vết việc gửi email/SMS trong UC-12, gồm kênh gửi, người nhận và trạng thái (queued/delivered/failed) để phục vụ truy vết và retry khi cần.
