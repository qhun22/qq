<!doctype html>
<html lang="vi">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Chương 2 - Mô hình ca sử dụng</title>
  <style>
    body { font-family: "Times New Roman", serif; line-height: 1.4; margin: 40px; }
    h1 { text-align: center; font-size: 20px; letter-spacing: 0.5px; margin: 0 0 14px; }
    h2 { font-size: 16px; margin: 18px 0 10px; }
    h3 { font-size: 14px; margin: 14px 0 8px; }
    p { margin: 8px 0; text-align: justify; }
    ul { margin: 6px 0 10px 22px; }
    table { width: 100%; border-collapse: collapse; margin: 10px 0 14px; }
    th, td { border: 1px solid #000; padding: 6px 8px; vertical-align: top; }
    th { text-align: left; }
    pre { border: 1px solid #000; padding: 10px; overflow: auto; }
    code { font-family: Consolas, "Courier New", monospace; }
    .note { font-style: italic; }
  </style>
</head>
<body>
  <h1>CHƯƠNG 2. MÔ HÌNH CA SỬ DỤNG (USE CASE)</h1>

  <h2>2.1 Sơ đồ gói</h2>
  <p>Mục này phân rã hệ thống thành các gói (package) ở mức khái niệm để hỗ trợ tổ chức lớp và phân công trách nhiệm.</p>
  <p>Nguyên tắc phân rã:</p>
  <ul>
    <li>Mỗi gói gom các ca sử dụng/chức năng gần nhau theo “mục tiêu nghiệp vụ”.</li>
    <li>Quan hệ phụ thuộc giữa các gói thể hiện luồng nghiệp vụ chính (gói nào cần dữ liệu/dịch vụ của gói nào).</li>
    <li>Các hệ thống bên ngoài (cổng thanh toán, QR/ICS, nhà cung cấp thông báo) không nằm trong gói nội bộ; chúng xuất hiện ở sơ đồ use-case tổng quát tại mục 2.2.</li>
  </ul>
  <pre><code>flowchart TB
  subgraph SYS[Hệ thống đặt lịch khám trực tuyến]
    ID[Tài khoản/Hồ sơ]
    CAT[Catalog/Tìm kiếm]
    PLAN[Cart/Wishlist]
    BK[Đặt lịch]
    PAY[Thanh toán/Hoàn]
    REV[Review/Biên tập]
    PART[Đối tác/XML]
    TKT[QR/ICS]
    NOTI[Thông báo]

    ID --&gt; BK
    CAT --&gt; BK
    PLAN --&gt; BK

    BK --&gt; PAY
    BK --&gt; TKT --&gt; NOTI

    PART --&gt; CAT
    REV --&gt; BK
  end</code></pre>
  <p><b>Hình 2.1 – Sơ đồ gói</b></p>
  <p>Diễn giải các gói:</p>
  <ul>
    <li><b>Tài khoản/Hồ sơ</b>: đăng ký/đăng nhập, quản lý hồ sơ khách hàng (CustomerAccount, CustomerProfile, InsuranceInfo).</li>
    <li><b>Catalog/Tìm kiếm</b>: duyệt/tìm kiếm cơ sở khám, bác sĩ, dịch vụ và slot; nhận dữ liệu đồng bộ từ đối tác nếu có (ProviderClinic, Doctor, MedicalService, AvailabilitySlot).</li>
    <li><b>Cart/Wishlist</b>: lưu “ý định đặt” trước khi chốt; chọn slot/dịch vụ vào giỏ và danh sách yêu thích (Cart, Wishlist).</li>
    <li><b>Đặt lịch</b>: tạo BookingOrder/BookingItem, giữ slot, xác nhận đặt (BookingOrder, BookingItem).</li>
    <li><b>Thanh toán/Hoàn</b>: ghi nhận thanh toán, yêu cầu hoàn tiền theo chính sách hủy/hoàn (PaymentTransaction, RefundRequest, CancelPolicy).</li>
    <li><b>Review/Biên tập</b>: tạo review, kiểm duyệt và nội dung biên tập (Review, ReviewModeration, EditorialPost).</li>
    <li><b>Đối tác/XML</b>: nhập/xuất mini-catalog qua XML phục vụ tích hợp (PartnerCatalogImport, MiniCatalogExport).</li>
    <li><b>QR/ICS</b>: phát hành mã QR hoặc lịch ICS cho lượt khám sau khi đơn ở trạng thái phù hợp (AppointmentTicket).</li>
    <li><b>Thông báo</b>: gửi thông báo trạng thái qua email/SMS (Notification).</li>
  </ul>
  <p>Ý nghĩa mũi tên trong Hình 2.1: gói ở phía sau thường phụ thuộc gói phía trước về dữ liệu hoặc dịch vụ (ví dụ: “Đặt lịch” cần thông tin từ “Tài khoản/Hồ sơ”, “Catalog/Tìm kiếm”, “Cart/Wishlist”).</p>
  <p class="note">Ghi chú: Các sơ đồ trong chương được trình bày gọn để thuận tiện đưa vào báo cáo và in A4.</p>

  <h2>2.2 Sơ đồ use-case tổng quát</h2>
  <h3>2.2.1 Sơ đồ use-case cho gói “Chung” (khách hàng)</h3>
  <pre><code>flowchart TB
  CUS([Khách hàng])
  PAY([Cổng thanh toán])
  TKT([Dịch vụ QR/ICS])
  NOTI([Dịch vụ thông báo])

  subgraph U[Hệ thống đặt lịch khám trực tuyến]
    direction TB
    UC13((UC-13 ĐK/ĐN &amp; Hồ sơ))
    UC01((UC-01 Tìm kiếm &amp; xem chi tiết))
    UC02((UC-02 Wishlist))
    UC03((UC-03 Cart))
    UC04((UC-04 Đặt lịch &amp; Thanh toán))
    UC05((UC-05 Hủy lịch))
    UC06((UC-06 Yêu cầu hoàn tiền))
    UC07((UC-07 Gửi review))
    UC12((UC-12 QR/ICS &amp; Thông báo))
  end

  CUS --- UC13
  CUS --- UC01
  CUS --- UC02
  CUS --- UC03
  CUS --- UC04
  CUS --- UC05
  CUS --- UC06
  CUS --- UC07

  PAY --- UC04
  PAY --- UC06
  TKT --- UC12
  NOTI --- UC12

  UC04 -.-&gt; UC12 : &lt;&lt;include&gt;&gt;
  UC05 -.-&gt; UC12 : &lt;&lt;include&gt;&gt;
  UC06 -.-&gt; UC12 : &lt;&lt;include&gt;&gt;</code></pre>

  <p><b>Hình 2.2a – Sơ đồ use-case cho gói “Chung” (khách hàng)</b></p>
  <p>Hình 2.2a mô tả các ca sử dụng mà Khách hàng tương tác trực tiếp và các hệ thống ngoài liên quan (cổng thanh toán, dịch vụ QR/ICS, dịch vụ thông báo). UC-12 được mô hình hóa theo quan hệ &lt;&lt;include&gt;&gt; từ các ca sử dụng phát sinh thông báo/phiếu hẹn.</p>

  <h3>2.2.2 Sơ đồ use-case cho gói “Quản trị nội bộ” (admin)</h3>
  <pre><code>flowchart TB
  MOD([Nhân viên kiểm duyệt])
  EDT([Biên tập viên])

  subgraph U[Hệ thống đặt lịch khám trực tuyến]
    direction TB
    UC08((UC-08 Kiểm duyệt review))
    UC09((UC-09 Quản lý editorial review))
  end

  MOD --- UC08
  EDT --- UC09</code></pre>

  <p><b>Hình 2.2b – Sơ đồ use-case cho gói “Quản trị nội bộ” (admin)</b></p>
  <p>Hình 2.2b mô tả các Use Case nội bộ phục vụ kiểm duyệt review và quản lý nội dung biên tập.</p>

  <h3>2.2.3 Sơ đồ use-case cho gói “Đối tác/Catalog”</h3>
  <pre><code>flowchart TB
    PRO([Nhân viên cơ sở y tế])
  PRT([Hệ thống đối tác])

  subgraph U[Hệ thống đặt lịch khám trực tuyến]
    direction TB
    UC10((UC-10 Nhập catalog đối tác))
    UC11((UC-11 Xuất mini-catalog XML))
  end

  PRO --- UC10
  PRO --- UC11
  PRT --- UC11</code></pre>

  <p><b>Hình 2.2c – Sơ đồ use-case cho gói “Đối tác/Catalog”</b></p>
  <p>Hình 2.2c mô tả các Use Case liên quan tích hợp catalog với đối tác (nhập catalog, xuất mini-catalog XML).</p>

  <p class="note">Ghi chú: Các sơ đồ use-case được chia theo gói (chung/admin/đối tác) theo mẫu trình bày.</p>

  <h2>2.3 Kịch bản ca sử dụng (luồng chính/luồng thay thế)</h2>
  <p>Mỗi Use Case trong mục này được mô tả theo <b>mẫu thống nhất</b> để thuận tiện cho việc dựng sơ đồ Robustness/Sequence ở các chương sau:</p>
  <ul>
    <li><b>Tác nhân chính</b></li>
    <li><b>Mục tiêu</b></li>
    <li><b>Tiền điều kiện</b> / <b>Hậu điều kiện</b></li>
    <li><b>LUỒNG CHÍNH</b></li>
    <li><b>LUỒNG THAY THẾ/NGOẠI LỆ</b></li>
  </ul>

  <h3>2.3.1 UC-13 – Đăng ký/Đăng nhập &amp; quản lý hồ sơ</h3>
  <ul>
    <li><b>Tác nhân chính</b>: Khách hàng</li>
    <li><b>Mục tiêu</b>: Đăng ký/đăng nhập và quản lý hồ sơ cá nhân (kèm bảo hiểm nếu có).</li>
    <li><b>Tiền điều kiện</b>: Không (đăng ký) hoặc đã có tài khoản (đăng nhập).</li>
    <li><b>Hậu điều kiện</b>: Tạo tài khoản/phiên đăng nhập; hồ sơ được lưu nếu cập nhật thành công.</li>
  </ul>
  <p><b>LUỒNG CHÍNH</b></p>
  <ol>
    <li>Khách hàng chọn đăng ký hoặc đăng nhập.</li>
    <li>Hệ thống hiển thị form và yêu cầu nhập email/số điện thoại + mật khẩu.</li>
    <li>Với đăng ký: hệ thống kiểm tra trùng; tạo tài khoản; khởi tạo hồ sơ.</li>
    <li>Với đăng nhập: hệ thống đối chiếu mật khẩu với tài khoản gốc trong CSDL trung tâm; tạo phiên đăng nhập.</li>
    <li>Khách hàng cập nhật hồ sơ (họ tên, ngày sinh, thông tin liên hệ) và thông tin bảo hiểm (nếu có).</li>
    <li>Hệ thống lưu thay đổi và thông báo thành công.</li>
  </ol>
  <p><b>LUỒNG THAY THẾ/NGOẠI LỆ</b></p>
  <ul>
    <li>Email/số điện thoại đã tồn tại: hệ thống yêu cầu dùng thông tin khác.</li>
    <li>Thông tin đăng nhập sai: hệ thống thông báo và yêu cầu nhập lại.</li>
    <li>Dữ liệu hồ sơ/bảo hiểm không hợp lệ: hệ thống từ chối và nêu rõ trường lỗi.</li>
  </ul>

  <h3>2.3.2 UC-01 – Tìm kiếm &amp; xem chi tiết</h3>
  <ul>
    <li><b>Tác nhân chính</b>: Khách hàng</li>
    <li><b>Mục tiêu</b>: Tìm kiếm và xem thông tin chi tiết bác sĩ/cơ sở/dịch vụ.</li>
    <li><b>Tiền điều kiện</b>: Catalog tổng có dữ liệu.</li>
    <li><b>Hậu điều kiện</b>: Hiển thị danh sách kết quả và/hoặc trang chi tiết.</li>
  </ul>
  <p><b>LUỒNG CHÍNH</b></p>
  <ol>
    <li>Khách hàng nhập tiêu chí tìm kiếm (tên bác sĩ, chuyên khoa, từ khóa, địa điểm, khung giờ).</li>
    <li>Hệ thống truy vấn catalog tổng và hiển thị danh sách kết quả.</li>
    <li>Khách hàng chọn một bác sĩ/cơ sở/dịch vụ.</li>
    <li>Hệ thống hiển thị trang chi tiết (hồ sơ, giá, review rút gọn tối đa 200 ký tự) và danh sách slot còn trống.</li>
  </ol>
  <p><b>LUỒNG THAY THẾ/NGOẠI LỆ</b></p>
  <ul>
    <li>Không có kết quả: hệ thống thông báo và gợi ý điều chỉnh tiêu chí.</li>
    <li>Slot vừa hết: hệ thống cập nhật lại danh sách slot khi người dùng thao tác.</li>
  </ul>

  <h3>2.3.3 UC-02 – Quản lý Wishlist</h3>
  <ul>
    <li><b>Tác nhân chính</b>: Khách hàng</li>
    <li><b>Mục tiêu</b>: Lưu và quản lý danh sách bác sĩ/cơ sở/dịch vụ yêu thích để đặt sau.</li>
    <li><b>Tiền điều kiện</b>: Khách hàng đã đăng nhập (để lưu/xóa).</li>
    <li><b>Hậu điều kiện</b>: Wishlist được cập nhật.</li>
  </ul>
  <p><b>LUỒNG CHÍNH</b></p>
  <ol>
    <li>Khách hàng nhấn “Yêu thích/Lưu” tại trang chi tiết bác sĩ/cơ sở/dịch vụ.</li>
    <li>Hệ thống thêm mục vào wishlist của khách hàng.</li>
    <li>Khách hàng mở trang wishlist để xem danh sách.</li>
    <li>Khách hàng xóa một mục khỏi wishlist.</li>
    <li>Hệ thống cập nhật wishlist và hiển thị danh sách mới.</li>
  </ol>
  <p><b>LUỒNG THAY THẾ/NGOẠI LỆ</b></p>
  <ul>
    <li>Chưa đăng nhập: hệ thống yêu cầu đăng nhập trước khi lưu/xóa.</li>
  </ul>

  <h3>2.3.4 UC-03 – Quản lý Cart (giỏ đặt lịch)</h3>
  <ul>
    <li><b>Tác nhân chính</b>: Khách hàng</li>
    <li><b>Mục tiêu</b>: Tạo và quản lý giỏ đặt lịch trước khi thanh toán.</li>
    <li><b>Tiền điều kiện</b>: Có thể truy cập trang chi tiết bác sĩ/cơ sở/dịch vụ và chọn slot.</li>
    <li><b>Hậu điều kiện</b>: Giỏ được cập nhật; sẵn sàng chuyển sang UC-04 khi người dùng thanh toán.</li>
  </ul>
  <p><b>LUỒNG CHÍNH</b></p>
  <ol>
    <li>Khách hàng chọn slot/dịch vụ trên trang chi tiết.</li>
    <li>Khách hàng nhấn “Thêm vào giỏ”.</li>
    <li>Hệ thống thêm mục vào giỏ và hiển thị tổng quan giỏ.</li>
    <li>Khách hàng xóa mục khỏi giỏ.</li>
    <li>Khách hàng nhấn “Thanh toán/Đặt lịch”.</li>
  </ol>
  <p><b>LUỒNG THAY THẾ/NGOẠI LỆ</b></p>
  <ul>
    <li>Slot không còn trống: hệ thống từ chối thêm vào giỏ và yêu cầu chọn slot khác.</li>
    <li>Xung đột thời gian giữa các mục trong giỏ: hệ thống cảnh báo và yêu cầu điều chỉnh.</li>
  </ul>

  <h3>2.3.5 UC-04 – Đặt lịch &amp; Thanh toán</h3>
  <ul>
    <li><b>Tác nhân chính</b>: Khách hàng, Cổng thanh toán</li>
    <li><b>Mục tiêu</b>: Tạo đơn đặt lịch và hoàn tất thanh toán.</li>
    <li><b>Tiền điều kiện</b>: Giỏ có ít nhất một mục hợp lệ; slot còn trống; thông tin hồ sơ cần thiết đã có/được cung cấp.</li>
    <li><b>Hậu điều kiện</b>: Ghi nhận giao dịch; đơn đặt lịch cập nhật trạng thái (Paid/Confirmed); phát hành QR/ICS và gửi thông báo khi đủ điều kiện.</li>
  </ul>
  <p><b>LUỒNG CHÍNH</b></p>
  <ol>
    <li>Khách hàng xác nhận thông tin giỏ và thông tin hồ sơ cần thiết.</li>
    <li>Hệ thống kiểm tra tính hợp lệ từng mục (slot còn trống, giá hiện hành) và đảm bảo các mục thanh toán thuộc cùng một cơ sở y tế.</li>
    <li>Khách hàng chọn phương thức thanh toán (thẻ/ ví điện tử).</li>
    <li>Hệ thống tạo yêu cầu thanh toán và chuyển sang cổng thanh toán.</li>
    <li>Cổng thanh toán xử lý và trả kết quả thành công.</li>
    <li>Hệ thống ghi nhận giao dịch, cập nhật trạng thái đơn đặt lịch sang “Paid” và (nếu tự xác nhận) “Confirmed”.</li>
    <li>Khi đơn ở trạng thái “Confirmed”, hệ thống phát hành QR/ICS và gửi thông báo xác nhận (email/SMS).</li>
  </ol>
  <p><b>LUỒNG THAY THẾ/NGOẠI LỆ</b></p>
  <ul>
    <li>Slot không còn trống: hệ thống thông báo và yêu cầu chọn slot khác hoặc xóa mục.</li>
    <li>Giỏ có mục thuộc nhiều cơ sở y tế: hệ thống yêu cầu tách thành nhiều đơn (mỗi đơn thuộc một cơ sở y tế) trước khi thanh toán.</li>
    <li>Thanh toán thất bại/hủy: hệ thống ghi nhận trạng thái và cho phép retry theo chính sách.</li>
    <li>Lỗi phát hành QR/ICS hoặc gửi thông báo: đơn vẫn hợp lệ; hệ thống ghi nhận lỗi và retry.</li>
  </ul>

  <h3>2.3.6 UC-05 – Hủy lịch</h3>
  <ul>
    <li><b>Tác nhân chính</b>: Khách hàng</li>
    <li><b>Mục tiêu</b>: Hủy đơn đặt lịch theo chính sách cut-off.</li>
    <li><b>Tiền điều kiện</b>: Khách hàng đã đăng nhập và có đơn đặt lịch thuộc sở hữu.</li>
    <li><b>Hậu điều kiện</b>: Đơn được cập nhật trạng thái hủy; khách hàng nhận thông báo kết quả.</li>
  </ul>
  <p><b>LUỒNG CHÍNH</b></p>
  <ol>
    <li>Khách hàng mở danh sách đơn đặt lịch của mình.</li>
    <li>Khách hàng chọn một đơn và nhấn “Hủy”.</li>
    <li>Hệ thống kiểm tra điều kiện hủy theo cut-off của cơ sở y tế (thời điểm cut-off = thời điểm bắt đầu slot - <code>cutoffMinutes</code>).</li>
    <li>Hệ thống cập nhật trạng thái đơn sang “Cancelled”.</li>
    <li>Hệ thống gửi thông báo kết quả hủy cho khách hàng.</li>
  </ol>
  <p><b>LUỒNG THAY THẾ/NGOẠI LỆ</b></p>
  <ul>
    <li>Quá cut-off: hệ thống từ chối hủy và nêu lý do.</li>
    <li>Đơn không tồn tại/không thuộc sở hữu khách hàng: hệ thống từ chối.</li>
    <li>Khách hàng chọn “Đổi lịch” (trước cut-off): hệ thống hủy đơn hiện tại theo chính sách và chuyển khách hàng sang UC-04 để tạo đơn mới với slot khác.</li>
  </ul>

  <h3>2.3.7 UC-06 – Yêu cầu hoàn tiền</h3>
  <ul>
    <li><b>Tác nhân chính</b>: Khách hàng, Cổng thanh toán</li>
    <li><b>Mục tiêu</b>: Tạo yêu cầu hoàn tiền theo chính sách hoàn/hủy.</li>
    <li><b>Tiền điều kiện</b>: Đơn đã thanh toán; thỏa điều kiện hoàn theo <code>CancelPolicy</code>.</li>
    <li><b>Hậu điều kiện</b>: Tạo <code>RefundRequest</code>; cập nhật trạng thái hoàn và gửi thông báo.</li>
  </ul>
  <p><b>LUỒNG CHÍNH</b></p>
  <ol>
    <li>Khách hàng chọn đơn đã thanh toán và yêu cầu hoàn tiền.</li>
    <li>Hệ thống kiểm tra chính sách hoàn (điều kiện theo cut-off, mức hoàn theo % cấu hình trong <code>CancelPolicy</code>, thời gian xử lý).</li>
    <li>Hệ thống tạo <code>RefundRequest</code> và gửi yêu cầu hoàn sang cổng thanh toán.</li>
    <li>Cổng thanh toán phản hồi kết quả.</li>
    <li>Hệ thống cập nhật trạng thái hoàn và gửi thông báo cho khách hàng.</li>
  </ol>
  <p><b>LUỒNG THAY THẾ/NGOẠI LỆ</b></p>
  <ul>
    <li>Không đủ điều kiện hoàn: hệ thống từ chối và nêu lý do.</li>
    <li>Cổng thanh toán lỗi/tạm thời không khả dụng: hệ thống ghi nhận trạng thái và retry theo chính sách.</li>
  </ul>

  <h3>2.3.8 UC-07 – Gửi review</h3>
  <ul>
    <li><b>Tác nhân chính</b>: Khách hàng</li>
    <li><b>Mục tiêu</b>: Gửi đánh giá (review) cho bác sĩ/cơ sở.</li>
    <li><b>Tiền điều kiện</b>: Khách hàng đã đăng nhập.</li>
    <li><b>Hậu điều kiện</b>: Review được tạo ở trạng thái “Pending” để chờ kiểm duyệt.</li>
  </ul>
  <p><b>LUỒNG CHÍNH</b></p>
  <ol>
    <li>Khách hàng mở trang chi tiết bác sĩ/cơ sở.</li>
    <li>Khách hàng nhập tiêu đề, nội dung, rating 1–5 và gửi.</li>
    <li>Hệ thống lưu review ở trạng thái “Pending” và thông báo đã tiếp nhận.</li>
  </ol>
  <p><b>LUỒNG THAY THẾ/NGOẠI LỆ</b></p>
  <ul>
    <li>Chưa đăng nhập: hệ thống yêu cầu đăng nhập.</li>
    <li>Nội dung quá dài/quá ngắn/điểm không hợp lệ: hệ thống từ chối và nêu lý do.</li>
  </ul>

  <h3>2.3.9 UC-08 – Kiểm duyệt review</h3>
  <ul>
    <li><b>Tác nhân chính</b>: Nhân viên kiểm duyệt</li>
    <li><b>Mục tiêu</b>: Phê duyệt/từ chối review trước khi xuất bản.</li>
    <li><b>Tiền điều kiện</b>: Có review ở trạng thái chờ duyệt.</li>
    <li><b>Hậu điều kiện</b>: Review được cập nhật trạng thái (phê duyệt/từ chối) và hiển thị theo quy định.</li>
  </ul>
  <p><b>LUỒNG CHÍNH</b></p>
  <ol>
    <li>Nhân viên kiểm duyệt mở danh sách review chờ duyệt.</li>
    <li>Nhân viên kiểm duyệt xem chi tiết review.</li>
    <li>Nhân viên kiểm duyệt phê duyệt hoặc từ chối.</li>
    <li>Hệ thống cập nhật trạng thái; nếu phê duyệt thì review xuất bản (hiển thị rút gọn tối đa 200 ký tự trên trang chi tiết, có nút xem đầy đủ).</li>
  </ol>
  <p><b>LUỒNG THAY THẾ/NGOẠI LỆ</b></p>
  <ul>
    <li>Review không còn hợp lệ (đối tượng bị xóa/ẩn): hệ thống từ chối thao tác và yêu cầu làm mới danh sách.</li>
  </ul>

  <h3>2.3.10 UC-09 – Quản lý editorial review</h3>
  <ul>
    <li><b>Tác nhân chính</b>: Biên tập viên</li>
    <li><b>Mục tiêu</b>: Tạo và xuất bản bài nội dung biên tập (editorial review).</li>
    <li><b>Tiền điều kiện</b>: Biên tập viên đã đăng nhập và có quyền tạo bài.</li>
    <li><b>Hậu điều kiện</b>: Bài editorial được lưu bản nháp hoặc được xuất bản và hiển thị.</li>
  </ul>
  <p><b>LUỒNG CHÍNH</b></p>
  <ol>
    <li>Biên tập viên tạo bài nội dung biên tập và gắn với bác sĩ/cơ sở.</li>
    <li>Hệ thống lưu bản nháp.</li>
    <li>Biên tập viên xuất bản bài.</li>
    <li>Hệ thống hiển thị bài editorial trên trang chi tiết.</li>
  </ol>
  <p><b>LUỒNG THAY THẾ/NGOẠI LỆ</b></p>
  <ul>
    <li>Thiếu thông tin bắt buộc (tiêu đề/nội dung/đối tượng gắn): hệ thống từ chối lưu hoặc xuất bản.</li>
  </ul>

  <h3>2.3.11 UC-10 – Nhập catalog đối tác</h3>
  <ul>
    <li><b>Tác nhân chính</b>: Nhân viên cơ sở y tế</li>
    <li><b>Mục tiêu</b>: Nhập và hợp nhất dữ liệu catalog bác sĩ/dịch vụ vào catalog tổng.</li>
    <li><b>Tiền điều kiện</b>: Có dữ liệu catalog theo quy ước (định dạng/khóa định danh).</li>
    <li><b>Hậu điều kiện</b>: Catalog tổng được cập nhật; có ghi nhận kết quả nhập (thành công/cảnh báo).</li>
  </ul>
  <p><b>LUỒNG CHÍNH</b></p>
  <ol>
    <li>Nhân viên cơ sở y tế cung cấp dữ liệu catalog bác sĩ/dịch vụ theo quy ước.</li>
    <li>Hệ thống nhận dữ liệu và kiểm tra định dạng.</li>
    <li>Hệ thống hợp nhất vào catalog tổng (cập nhật/ghi mới theo khóa định danh: clinicId, doctorId, serviceId, slotId; có thể kèm externalId nếu nhập từ hệ thống bên ngoài).</li>
    <li>Hệ thống ghi nhận kết quả nhập (thành công/cảnh báo).</li>
  </ol>
  <p><b>LUỒNG THAY THẾ/NGOẠI LỆ</b></p>
  <ul>
    <li>Dữ liệu sai định dạng/thiếu trường: hệ thống từ chối và trả lỗi chi tiết.</li>
    <li>Trùng/không khớp định danh: hệ thống ghi nhận cảnh báo và áp dụng chính sách xử lý trùng.</li>
  </ul>

  <h3>2.3.12 UC-11 – Xuất mini-catalog XML</h3>
  <ul>
    <li><b>Tác nhân chính</b>: Hệ thống đối tác</li>
    <li><b>Mục tiêu</b>: Xuất mini-catalog dạng XML để đối tác tiêu thụ/nhúng.</li>
    <li><b>Tiền điều kiện</b>: Catalog tổng có dữ liệu hợp lệ theo phạm vi xuất.</li>
    <li><b>Hậu điều kiện</b>: Hệ thống cung cấp XML đúng schema cho đối tác.</li>
  </ul>
  <p><b>LUỒNG CHÍNH</b></p>
  <ol>
    <li>Hệ thống nhận yêu cầu xuất mini-catalog theo phạm vi (cơ sở, chuyên khoa, gói).</li>
    <li>Hệ thống truy xuất dữ liệu từ catalog tổng.</li>
    <li>Hệ thống tạo XML theo schema thống nhất, bao gồm các khóa định danh tối thiểu (clinicId, doctorId, serviceId, slotId) để đối tác có thể đối chiếu.</li>
    <li>Hệ thống cung cấp XML cho đối tác (tải về hoặc endpoint).</li>
  </ol>
  <p><b>LUỒNG THAY THẾ/NGOẠI LỆ</b></p>
  <ul>
    <li>Thiếu dữ liệu: hệ thống trả XML rỗng kèm thông báo hoặc lỗi theo quy ước.</li>
  </ul>

  <h3>2.3.13 UC-12 – Phát hành QR/ICS &amp; gửi thông báo</h3>
  <p class="note">Ghi chú: Use Case này chủ yếu được kích hoạt tự động bởi hệ thống theo trạng thái đơn; khách hàng chỉ tham gia gián tiếp qua luồng thay thế “Gửi lại vé/ICS”.</p>
  <ul>
    <li><b>Tác nhân chính</b>: Hệ thống (tự động), Dịch vụ QR/ICS, Dịch vụ thông báo</li>
    <li><b>Mục tiêu</b>: Phát hành phiếu hẹn (QR/ICS) và gửi thông báo trạng thái cho khách hàng.</li>
    <li><b>Tiền điều kiện</b>: Đơn đặt lịch thay đổi sang trạng thái phù hợp (ví dụ “Confirmed”) hoặc phát sinh sự kiện hủy/hoàn.</li>
    <li><b>Hậu điều kiện</b>: QR/ICS được phát hành (hoặc dùng lại nếu đã có); thông báo được gửi/ghi log; lỗi được ghi nhận để retry theo chính sách.</li>
  </ul>
  <p><b>LUỒNG CHÍNH</b></p>
  <ol>
    <li>Khi đơn đặt lịch ở trạng thái “Confirmed” (hoặc khi hủy/hoàn thay đổi trạng thái), hệ thống tạo yêu cầu phát hành QR/ICS.</li>
    <li>Dịch vụ QR/ICS trả về payload hoặc tham chiếu phiếu xác nhận.</li>
    <li>Hệ thống gửi thông báo (email/SMS) qua dịch vụ thông báo.</li>
    <li>Hệ thống lưu log trạng thái gửi.</li>
  </ol>
  <p><b>LUỒNG THAY THẾ/NGOẠI LỆ</b></p>
  <ul>
    <li>Dịch vụ QR/ICS hoặc thông báo lỗi: hệ thống ghi nhận lỗi và retry theo chính sách.</li>
    <li>Khách hàng yêu cầu “Gửi lại vé/ICS”: hệ thống dùng vé/ICS đã phát hành (hoặc phát hành lại nếu cần) và gửi lại thông báo.</li>
  </ul>

  <h2>2.4 Tổng quan yêu cầu</h2>
  <h3>2.4.1 Mục tiêu nghiệp vụ</h3>
  <ul>
    <li>Tăng khả năng tiếp cận dịch vụ y tế: người dùng có thể tìm kiếm, so sánh và đặt lịch trực tuyến theo nhiều tiêu chí.</li>
    <li>Chuẩn hóa quy trình đặt lịch–thanh toán–xác nhận, giảm thời gian chờ và giảm tải cho khâu tiếp nhận.</li>
    <li>Hỗ trợ hợp tác đối tác (bảo hiểm/doanh nghiệp) thông qua mini-catalog, nhằm mở rộng kênh phân phối dịch vụ.</li>
    <li>Tăng tính minh bạch và tin cậy thông qua review có kiểm duyệt và nội dung biên tập.</li>
  </ul>

  <h3>2.4.2 Phạm vi yêu cầu trong chương</h3>
  <p>Chương 2 cụ thể hóa các nội dung đã nêu ở Chương 1 thành: tác nhân và phạm vi tương tác; FR/NFR; mô hình ca sử dụng và kịch bản ca sử dụng (luồng chính/luồng thay thế); tiêu chí chấp nhận và ma trận truy vết phục vụ Chương 3–4.</p>

  <h3>2.4.3 Quy ước mô tả yêu cầu</h3>
  <ul>
    <li><b>FR-xx</b>: yêu cầu chức năng.</li>
    <li><b>NFR-xx</b>: yêu cầu phi chức năng.</li>
    <li><b>UC-xx</b>: Use Case.</li>
  </ul>

  <h2>2.5 Tác nhân và các bên liên quan</h2>
  <h3>2.5.1 Danh sách tác nhân chính</h3>
  <ul>
    <li><b>A1 – Khách hàng</b>: tìm kiếm, đặt lịch, thanh toán, hủy/hoàn, wishlist, review.</li>
    <li><b>A2 – Nhân viên kiểm duyệt</b>: duyệt/ẩn review trước khi xuất bản.</li>
    <li><b>A3 – Biên tập viên y tế</b>: đăng bài nội dung biên tập (nhận định/hướng dẫn).</li>
    <li><b>A4 – Nhân viên cơ sở y tế</b>: nhập/cập nhật catalog, cấu hình cut-off, tiếp nhận đơn đặt lịch.</li>
    <li><b>A5 – Hệ thống đối tác</b>: nhúng mini-catalog; trao đổi XML.</li>
    <li><b>A6 – Cổng thanh toán</b>: xử lý thanh toán thẻ/ ví.</li>
    <li><b>A7 – Dịch vụ thông báo</b>: email/SMS.</li>
    <li><b>A8 – Dịch vụ phát hành xác nhận</b>: QR/ICS lịch hẹn.</li>
  </ul>
  <p class="note">Lưu ý: Các tích hợp A6–A8 được mô tả theo hướng không phụ thuộc nhà cung cấp.</p>

  <h3>2.5.2 Mô tả vai trò và quyền hạn (tóm tắt)</h3>
  <ul>
    <li>Khách hàng thao tác trên dữ liệu cá nhân (hồ sơ, wishlist, booking của chính mình) và gửi review.</li>
    <li>Nhân viên kiểm duyệt xem danh sách review chờ duyệt, phê duyệt/từ chối, chỉnh trạng thái hiển thị.</li>
    <li>Biên tập viên tạo/sửa/xuất bản bài editorial review.</li>
    <li>Nhân viên cơ sở y tế cập nhật catalog và thiết lập cut-off; tiếp nhận đơn.</li>
    <li>Hệ thống đối tác tiêu thụ mini-catalog XML và/hoặc nhúng mini-catalog vào website.</li>
  </ul>

  <h3>2.5.3 Ma trận tác nhân ↔ nhóm chức năng</h3>
  <table>
    <thead>
      <tr>
        <th>Nhóm chức năng</th><th>A1 Khách hàng</th><th>A2 Nhân viên kiểm duyệt</th><th>A3 Biên tập viên</th><th>A4 Nhân viên cơ sở y tế</th><th>A5 Hệ thống đối tác</th><th>A6 Cổng thanh toán</th><th>A7 Dịch vụ thông báo</th><th>A8 QR/ICS</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>Tìm kiếm &amp; xem chi tiết</td><td>✔</td><td></td><td>✔ (xem)</td><td>✔ (cập nhật dữ liệu)</td><td></td><td></td><td></td><td></td></tr>
      <tr><td>Wishlist</td><td>✔</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
      <tr><td>Giỏ đặt lịch (Cart)</td><td>✔</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
      <tr><td>Đặt lịch/Booking</td><td>✔</td><td></td><td></td><td>✔ (tiếp nhận)</td><td></td><td></td><td></td><td></td></tr>
      <tr><td>Thanh toán</td><td>✔</td><td></td><td></td><td></td><td></td><td>✔</td><td></td><td></td></tr>
      <tr><td>Hủy &amp; hoàn</td><td>✔</td><td></td><td></td><td>✔ (chính sách/cut-off)</td><td></td><td>✔ (hoàn nếu có)</td><td></td><td></td></tr>
      <tr><td>Review &amp; kiểm duyệt</td><td>✔ (gửi)</td><td>✔ (duyệt)</td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
      <tr><td>Nội dung biên tập</td><td></td><td></td><td>✔</td><td></td><td></td><td></td><td></td><td></td></tr>
      <tr><td>Mini-catalog XML</td><td></td><td></td><td></td><td>✔ (dữ liệu nguồn)</td><td>✔ (tiêu thụ)</td><td></td><td></td><td></td></tr>
      <tr><td>Thông báo &amp; xác nhận</td><td>✔ (nhận)</td><td></td><td></td><td></td><td></td><td></td><td>✔</td><td>✔</td></tr>
    </tbody>
  </table>

  <h2>2.6 Yêu cầu chức năng (FR)</h2>
  <h3>2.6.1 FR – Quản lý tài khoản &amp; hồ sơ khách hàng</h3>
  <ul>
    <li><b>FR-01</b>: Tạo tài khoản và lưu hồ sơ (họ tên, email, SĐT, ngày sinh, bảo hiểm).</li>
    <li><b>FR-02</b>: Danh sách tài khoản nằm trong CSDL trung tâm.</li>
    <li><b>FR-03</b>: Khi đăng nhập, mật khẩu phải được đối chiếu với danh sách tài khoản gốc trong CSDL trung tâm.</li>
    <li><b>FR-04</b>: Cập nhật hồ sơ theo chính sách.</li>
  </ul>

  <h3>2.6.2 FR – Tìm kiếm &amp; xem chi tiết</h3>
  <ul>
    <li><b>FR-05</b>: Tìm theo tên bác sĩ, chuyên khoa, từ khóa, địa điểm, khung giờ.</li>
    <li><b>FR-06</b>: Hiển thị danh sách kết quả kèm thông tin tóm tắt.</li>
    <li><b>FR-07</b>: Xem chi tiết bác sĩ/cơ sở y tế (hồ sơ, giá, lịch trống).</li>
  </ul>

  <h3>2.6.3 FR – Wishlist</h3>
  <ul>
    <li><b>FR-08</b>: Lưu bác sĩ/phòng khám yêu thích.</li>
    <li><b>FR-09</b>: Xóa mục khỏi wishlist.</li>
  </ul>

  <h3>2.6.4 FR – Giỏ đặt lịch (Cart)</h3>
  <ul>
    <li><b>FR-10</b>: Thêm lịch khám/dịch vụ vào giỏ trước thanh toán.</li>
    <li><b>FR-11</b>: Xóa mục khỏi giỏ.</li>
    <li><b>FR-12</b>: Hiển thị tổng quan giỏ.</li>
  </ul>

  <h3>2.6.5 FR – Booking/Order</h3>
  <ul>
    <li><b>FR-13</b>: Tạo một hoặc nhiều đơn đặt lịch từ giỏ; mỗi đơn đặt lịch thuộc một cơ sở y tế.</li>
    <li><b>FR-14</b>: Lưu trạng thái đơn đặt lịch.</li>
    <li><b>FR-15</b>: Cơ sở y tế tiếp nhận đơn đặt lịch trực tuyến.</li>
  </ul>

  <h3>2.6.6 FR – Thanh toán</h3>
  <ul>
    <li><b>FR-16</b>: Thanh toán thẻ hoặc ví điện tử.</li>
    <li><b>FR-17</b>: Ghi nhận kết quả thanh toán và cập nhật trạng thái.</li>
  </ul>

  <h3>2.6.7 FR – Hủy &amp; hoàn tiền</h3>
  <ul>
    <li><b>FR-18</b>: Hủy trước cut-off do cơ sở y tế quy định (thời điểm cut-off = thời điểm bắt đầu slot - <code>cutoffMinutes</code>).</li>
    <li><b>FR-19</b>: Yêu cầu hoàn tiền theo chính sách (mức hoàn theo % cấu hình trong CancelPolicy).</li>
    <li><b>FR-20</b>: Ghi nhận yêu cầu hoàn và trạng thái xử lý.</li>
  </ul>

  <h3>2.6.8 FR – Review &amp; kiểm duyệt</h3>
  <ul>
    <li><b>FR-21</b>: Gửi review cho bác sĩ/cơ sở.</li>
    <li><b>FR-22</b>: Review có rating 1–5 và nội dung/tiêu đề.</li>
    <li><b>FR-23</b>: Review phải kiểm duyệt trước xuất bản.</li>
    <li><b>FR-24</b>: Review dài rút gọn (tối đa 200 ký tự) và có xem đầy đủ.</li>
  </ul>

  <h3>2.6.9 FR – Editorial review</h3>
  <ul>
    <li><b>FR-25</b>: Biên tập viên có thể đăng bài nhận định/hướng dẫn (editorial review) và hiển thị tại trang chi tiết.</li>
  </ul>

  <h3>2.6.10 FR – Catalog đối tác &amp; mini-catalog XML</h3>
  <ul>
    <li><b>FR-26</b>: Provider thêm catalog bác sĩ/dịch vụ riêng.</li>
    <li><b>FR-27</b>: Nhập vào catalog tổng để xuất hiện trong tìm kiếm.</li>
    <li><b>FR-28</b>: Nhúng mini-catalog vào website đối tác.</li>
    <li><b>FR-29</b>: Mini-catalog định nghĩa bằng XML.</li>
  </ul>

  <h3>2.6.11 FR – Thông báo &amp; phát hành xác nhận</h3>
  <ul>
    <li><b>FR-30</b>: Phát hành QR/ICS lịch hẹn khi đơn đặt lịch ở trạng thái “Confirmed”.</li>
    <li><b>FR-31</b>: Gửi thông báo (email/SMS) qua dịch vụ bên thứ ba.</li>
  </ul>

  <h2>2.7 Yêu cầu phi chức năng (Non-functional Requirements – NFR)</h2>
  <h3>2.7.1 NFR – Hiệu năng &amp; khả năng mở rộng</h3>
  <ul>
    <li><b>NFR-01</b>: 100.000 tài khoản (6 tháng đầu) → 1.000.000 (sau 6 tháng).</li>
    <li><b>NFR-02</b>: 1.000 người dùng đồng thời → 10.000.</li>
    <li><b>NFR-03</b>: 100 yêu cầu tìm kiếm/phút → 1.000/phút.</li>
    <li><b>NFR-04</b>: 100 đơn đặt lịch/giờ → 1.000/giờ.</li>
  </ul>

  <h3>2.7.2 NFR – Tính sẵn sàng &amp; độ tin cậy</h3>
  <ul>
    <li><b>NFR-05</b>: Xử lý lỗi/tạm thời không khả dụng của payment/notify (ghi nhận trạng thái, cho phép retry).</li>
    <li><b>NFR-06</b>: Tránh ghi nhận trùng (idempotency) cho thanh toán/cập nhật trạng thái ở mức thiết kế.</li>
  </ul>

  <h3>2.7.3 NFR – Bảo mật &amp; quyền riêng tư</h3>
  <ul>
    <li><b>NFR-07</b>: Mật khẩu lưu dạng băm; không lưu plaintext.</li>
    <li><b>NFR-08</b>: Phân quyền theo vai trò.</li>
    <li><b>NFR-09</b>: Bảo vệ dữ liệu hồ sơ cá nhân và bảo hiểm.</li>
  </ul>

  <h3>2.7.4 NFR – Tính tương chuẩn &amp; tích hợp</h3>
  <ul>
    <li><b>NFR-10</b>: Mini-catalog sử dụng XML để trao đổi liên hệ thống.</li>
    <li><b>NFR-11</b>: Tích hợp theo hướng thay thế nhà cung cấp (adapter), cấu hình bằng tham số.</li>
    <li><b>NFR-12</b>: Trong phạm vi minh họa, có thể sử dụng môi trường thử nghiệm (sandbox) và cấu hình SMTP để mô phỏng luồng nghiệp vụ.</li>
  </ul>

  <h3>2.7.5 NFR – Khả năng bảo trì &amp; mở rộng kênh</h3>
  <ul>
    <li><b>NFR-13</b>: Tách lớp nghiệp vụ khỏi giao diện (API-first/headless).</li>
  </ul>

  <h3>2.7.6 NFR – Ràng buộc dữ liệu</h3>
  <ul>
    <li><b>NFR-14</b>: Tài khoản và catalog tổng quản lý nhất quán tại CSDL trung tâm.</li>
  </ul>

  <h2>2.8 Nhóm Use Case theo nghiệp vụ</h2>
  <ul>
    <li>Nhóm tìm kiếm: UC-01</li>
    <li>Nhóm lập kế hoạch đặt lịch: UC-02, UC-03</li>
    <li>Nhóm đặt lịch &amp; thanh toán: UC-04, UC-12</li>
    <li>Nhóm hủy/hoàn: UC-05, UC-06, UC-12</li>
    <li>Nhóm nội dung &amp; đánh giá: UC-07, UC-08, UC-09</li>
    <li>Nhóm catalog &amp; đối tác: UC-10, UC-11</li>
    <li>Nhóm tài khoản: UC-13</li>
  </ul>

  <h2>2.9 Danh sách Use Case (tóm tắt)</h2>
  <table>
    <thead>
      <tr><th>Mã UC</th><th>Tên Use Case</th><th>Tác nhân chính</th></tr>
    </thead>
    <tbody>
      <tr><td>UC-01</td><td>Tìm kiếm &amp; xem chi tiết</td><td>Khách hàng</td></tr>
      <tr><td>UC-02</td><td>Quản lý Wishlist</td><td>Khách hàng</td></tr>
      <tr><td>UC-03</td><td>Quản lý Cart</td><td>Khách hàng</td></tr>
      <tr><td>UC-04</td><td>Đặt lịch &amp; Thanh toán</td><td>Khách hàng, Cổng thanh toán</td></tr>
      <tr><td>UC-05</td><td>Hủy lịch</td><td>Khách hàng</td></tr>
      <tr><td>UC-06</td><td>Yêu cầu hoàn tiền</td><td>Khách hàng, Cổng thanh toán</td></tr>
      <tr><td>UC-07</td><td>Gửi review</td><td>Khách hàng</td></tr>
      <tr><td>UC-08</td><td>Kiểm duyệt review</td><td>Nhân viên kiểm duyệt</td></tr>
      <tr><td>UC-09</td><td>Quản lý nội dung biên tập</td><td>Biên tập viên</td></tr>
      <tr><td>UC-10</td><td>Nhập catalog đối tác</td><td>Nhân viên cơ sở y tế</td></tr>
      <tr><td>UC-11</td><td>Xuất mini-catalog XML</td><td>Nhân viên cơ sở y tế, Hệ thống đối tác</td></tr>
      <tr><td>UC-12</td><td>Phát hành QR/ICS &amp; gửi thông báo</td><td>Hệ thống (tự động), Dịch vụ QR/ICS, Dịch vụ thông báo</td></tr>
      <tr><td>UC-13</td><td>Đăng ký/Đăng nhập &amp; quản lý hồ sơ</td><td>Khách hàng</td></tr>
    </tbody>
  </table>

  <h2>2.10 Tiêu chí chấp nhận &amp; truy vết</h2>
  <h3>2.10.1 Tiêu chí chấp nhận (Cho trước – Khi – Thì)</h3>
  <p>Các tiêu chí dưới đây hỗ trợ kiểm thử và truy vết.</p>

  <p><b>AC-01 – Đặt lịch &amp; thanh toán</b></p>
  <ul>
    <li><b>Cho trước</b>: Người dùng đã chọn bác sĩ/dịch vụ và khung giờ hợp lệ; giỏ có ít nhất một mục.</li>
    <li><b>Khi</b>: Người dùng xác nhận đặt lịch và thanh toán thành công qua cổng thanh toán.</li>
    <li><b>Thì</b>: Hệ thống tạo đơn đặt lịch, cập nhật trạng thái “Paid/Confirmed”, phát hành QR/ICS và gửi email/SMS xác nhận.</li>
  </ul>

  <p><b>AC-02 – Hủy lịch trước cut-off</b></p>
  <ul>
    <li><b>Cho trước</b>: Đơn đặt lịch còn trước thời điểm cut-off (thời điểm cut-off = thời điểm bắt đầu slot - <code>cutoffMinutes</code>).</li>
    <li><b>Khi</b>: Người dùng yêu cầu hủy.</li>
    <li><b>Thì</b>: Hệ thống cập nhật trạng thái hủy, gửi thông báo; nếu đủ điều kiện hoàn thì tạo yêu cầu hoàn theo % cấu hình trong <code>CancelPolicy</code>.</li>
  </ul>

  <p><b>AC-03 – Review phải kiểm duyệt</b></p>
  <ul>
    <li><b>Cho trước</b>: Người dùng đã đăng nhập.</li>
    <li><b>Khi</b>: Người dùng gửi review.</li>
    <li><b>Thì</b>: Review ở trạng thái Pending và không hiển thị công khai cho đến khi nhân viên kiểm duyệt phê duyệt.</li>
  </ul>

  <p><b>AC-04 – Xuất mini-catalog XML</b></p>
  <ul>
    <li><b>Cho trước</b>: Catalog tổng có dữ liệu bác sĩ/dịch vụ hợp lệ.</li>
    <li><b>Khi</b>: Đối tác yêu cầu xuất mini-catalog.</li>
    <li><b>Thì</b>: Hệ thống trả về XML đúng schema, bao gồm các trường dữ liệu tối thiểu (tối thiểu <code>clinicId</code>, <code>doctorId</code>, <code>serviceId</code>, <code>slotId</code>) và có thể nhúng/tiêu thụ.</li>
  </ul>

  <h3>2.10.2 Ma trận truy vết FR ↔ Use Case ↔ (mô hình ở Chương 3–4)</h3>
  <table>
    <thead>
      <tr><th>Yêu cầu</th><th>Use Case liên quan</th><th>Mô hình miền (Domain)</th><th>Robustness (RB)</th><th>Sequence (SD)</th></tr>
    </thead>
    <tbody>
      <tr><td>FR-01..FR-04 (Tài khoản/hồ sơ)</td><td>UC-13</td><td>CustomerAccount/Credential/CustomerProfile/InsuranceInfo</td><td>RB-UC13</td><td>SD-UC13</td></tr>
      <tr><td>FR-05..FR-07 (Tìm kiếm/chi tiết)</td><td>UC-01</td><td>ProviderClinic/Doctor/Specialty/MedicalService/AvailabilitySlot</td><td>RB-UC01</td><td>SD-UC01</td></tr>
      <tr><td>FR-08..FR-09 (Wishlist)</td><td>UC-02</td><td>Wishlist/WishlistItem</td><td>RB-UC02</td><td>SD-UC02</td></tr>
      <tr><td>FR-10..FR-12 (Cart)</td><td>UC-03</td><td>Cart/CartItem</td><td>RB-UC03</td><td>SD-UC03</td></tr>
      <tr><td>FR-13..FR-17 (Booking/Payment)</td><td>UC-04</td><td>BookingOrder/BookingItem/PaymentTransaction</td><td>RB-UC04</td><td>SD-UC04</td></tr>
      <tr><td>FR-18..FR-20 (Cancel/Refund)</td><td>UC-05, UC-06</td><td>BookingOrder/CancelPolicy/RefundRequest</td><td>RB-UC05-06</td><td>SD-UC05-06</td></tr>
      <tr><td>FR-21..FR-24 (Review/Moderation)</td><td>UC-07, UC-08</td><td>Review/ReviewModeration</td><td>RB-UC07-08</td><td>SD-UC07-08</td></tr>
      <tr><td>FR-25 (Editorial)</td><td>UC-09</td><td>EditorialPost</td><td>RB-UC09</td><td>SD-UC09</td></tr>
      <tr><td>FR-26..FR-29 (Catalog/XML)</td><td>UC-10, UC-11</td><td>Catalog/Partner/PartnerCatalogImport/MiniCatalogExport</td><td>RB-UC10-11</td><td>SD-UC10-11</td></tr>
      <tr><td>FR-30..FR-31 (QR/Notify)</td><td>UC-12</td><td>AppointmentTicket/Notification</td><td>RB-UC12</td><td>SD-UC12</td></tr>
    </tbody>
  </table>
</body>
</html>
