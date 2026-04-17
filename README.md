# CHƯƠNG 5. ĐÁNH GIÁ KẾT QUẢ VÀ KIẾN NGHỊ

## 5.1. Khái quát kết quả theo dữ liệu Meta Insights

Căn cứ bộ dữ liệu xuất từ **Meta Business Suite/Insights** trong giai đoạn **01/01/2026–16/04/2026**, mẫu phân tích gồm **09 bài viết** (07 bài ảnh, 02 bài video). Các chỉ số tổng hợp của toàn bộ mẫu như sau:
- **Tổng reach:** 1.789
- **Tổng lượt xem (views):** 5.583
- **Tổng tương tác (cảm xúc + bình luận + chia sẻ):** 464 (trong đó: cảm xúc 280; bình luận 177; chia sẻ 7)
- **Tổng lượt click:** 380 (trong đó: **link click** 16)

Chỉ số trung bình theo bài:
- **Reach/bài:** 198,78
- **Views/bài:** 620,33
- **Tương tác/bài:** 51,56
- **Tổng click/bài:** 42,22
- **Link click/bài:** 1,78

Tỷ lệ tổng hợp theo reach:

$$
ER_{tổng} = \frac{464}{1789} \times 100\% \approx 25{,}94\%
$$

$$
CTR_{tổng\ click} = \frac{380}{1789} \times 100\% \approx 21{,}25\%
$$

Trong đó, cần lưu ý chỉ số **Tổng lượt click** trong báo cáo Insights bao gồm nhiều loại hành động (ví dụ: click ảnh, click mở bài, click liên kết…), do đó được sử dụng như chỉ báo về mức độ quan tâm/hành động, không đồng nhất với chuyển đổi đặt hàng.

Lưu ý về định dạng thời gian: dữ liệu trong file CSV xuất từ Insights hiển thị thời gian theo dạng **MM/DD/YYYY**; trong các bảng dưới đây, thời gian đã được **quy đổi sang DD/MM/YYYY** để thống nhất với cách trình bày trong báo cáo.

Lưu ý về cấu trúc dữ liệu: trong file CSV xuất từ Insights, trường “Ngày” hiển thị “Trọn đời”, do đó số liệu phản ánh hiệu quả **tích lũy (lifetime)** của từng bài tại thời điểm xuất báo cáo trong giai đoạn nghiên cứu.

### 5.1.1. Bảng tổng hợp chỉ số theo bài viết (n = 9)

**Bảng 5.1. Tổng hợp chỉ số hiệu quả theo bài viết** (Nguồn: Meta Business Suite/Insights – file CSV; xử lý và tính toán của nhóm tác giả)

| Mã | Thời gian đăng (DD/MM/YYYY) | Định dạng | Nội dung (rút gọn) | Reach | Views | Cảm xúc | Bình luận | Chia sẻ | Tương tác | Tổng click | Link click | ER (%) | CTR (%) |
|---|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| P01 | 16/01/2026 03:15 | Ảnh | Bánh chuối chiên giòn rụm – nóng hổi mỗi ngày | 436 | 1.611 | 38 | 44 | 1 | 83 | 134 | 0 | 19,04 | 30,73 |
| P02 | 23/01/2026 17:48 | Ảnh | Bánh chuối vàng giòn + bánh khoai bùi béo | 92 | 384 | 12 | 7 | 0 | 19 | 27 | 0 | 20,65 | 29,35 |
| P03 | 04/02/2026 06:06 | Ảnh | Thông báo còn bánh; đặt số lượng lớn đặt trước | 76 | 463 | 21 | 7 | 0 | 28 | 25 | 0 | 36,84 | 32,89 |
| P04 | 05/02/2026 01:05 | Video | Trời se lạnh – bánh chuối chiên nóng giòn (có giá/ship) | 617 | 1.600 | 74 | 69 | 6 | 149 | 79 | 16 | 24,15 | 12,80 |
| P05 | 09/03/2026 07:46 | Ảnh | Nội dung khai xuân – nhắc liên hệ đặt đồ ăn | 57 | 187 | 11 | 2 | 0 | 13 | 10 | 0 | 22,81 | 17,54 |
| P06 | 10/03/2026 21:03 | Ảnh | Ra lò mẻ mới + ưu đãi mua 5 tặng 1 | 110 | 290 | 16 | 5 | 0 | 21 | 24 | 0 | 19,09 | 21,82 |
| P07 | 10/04/2026 00:21 | Ảnh | Chuối về nguyên kho; nhấn mạnh nguyên liệu tươi | 67 | 199 | 17 | 3 | 0 | 20 | 9 | 0 | 29,85 | 13,43 |
| P08 | 15/04/2026 03:13 | Ảnh | Sẵn bánh rồi; kêu gọi đặt hàng – ship liền | 120 | 316 | 24 | 6 | 0 | 30 | 26 | 0 | 25,00 | 21,67 |
| P09 | 15/04/2026 22:52 | Video | Bài ưu đãi theo số lượng (10 tặng 1; 100 giảm 10%) | 214 | 533 | 67 | 34 | 0 | 101 | 46 | 0 | 47,20 | 21,50 |

Trong bảng, $ER = \frac{Engagements}{Reach} \times 100\%$ và $CTR = \frac{Clicks}{Reach} \times 100\%$; số liệu được làm tròn ở mức 2 chữ số thập phân.

### 5.1.2. Vị trí chèn hình minh họa từ dữ liệu Insights

**Hình 5.1. Biểu đồ Reach theo từng bài viết** (Nguồn: Insights CSV; xử lý từ Excel/Google Sheets)

![Hình 5.1. Biểu đồ Reach theo từng bài viết (Nguồn: Insights CSV)](img/hinh5_1_reach.png)

**Hình 5.2. Biểu đồ tổng tương tác theo từng bài viết** (Nguồn: Insights CSV; xử lý từ Excel/Google Sheets)

![Hình 5.2. Biểu đồ tổng tương tác theo từng bài viết (Nguồn: Insights CSV)](img/hinh5_2_engagements.png)

## 5.2. Đánh giá theo hệ thống KPI (đối chiếu Chương 4)

Chương 4 đã đặt KPI theo hướng tăng trưởng so với mức trung bình giai đoạn theo dõi. Bảng dưới đây đối chiếu **mức hiện tại** (kết quả giai đoạn 01/01/2026–16/04/2026) với **mục tiêu kế hoạch**.

| Nhóm KPI | Chỉ số | Mức hiện tại | Mục tiêu kế hoạch | Đánh giá |
|---|---|---:|---:|---|
| Nhận diện | Reach/bài | 198,78 | ≥ 240 | Chưa đạt |
| Nhận diện | Views/bài | 620,33 | ≥ 750 | Chưa đạt |
| Tương tác | Tương tác/bài | 51,56 | ≥ 60 | Chưa đạt |
| Tương tác | $ER = \frac{Engagements}{Reach}$ | 25,94% | ≥ 20% | Đạt |
| Hành động | Tổng click/bài | 42,22 | ≥ 50 | Chưa đạt |
| Hành động | Link click/bài | 1,78 | ≥ 3 | Chưa đạt |

Nhìn chung, mức hiện tại cho thấy Fanpage có **tỷ lệ tương tác theo reach** tương đối tốt (ER đạt ngưỡng mục tiêu), nhưng còn hạn chế ở các chỉ số thuộc nhóm **phân phối nội dung** (reach, views) và **hành động** (click, link click). Trong báo cáo này, các KPI ở Chương 4 được xem là mục tiêu cho giai đoạn triển khai kế hoạch tiếp theo; vì vậy, kết quả giai đoạn 01/01/2026–16/04/2026 đóng vai trò **mức nền (baseline)** để xác định khoảng cách cần cải thiện. Do đó, các mục “chưa đạt” được hiểu là các điểm cần ưu tiên tối ưu trong kế hoạch triển khai, không phải tiêu chí “đạt/không đạt” của toàn bộ đề tài.

## 5.3. Phân tích hiệu quả theo định dạng nội dung

Khi phân tích theo định dạng, kết quả trung bình cho thấy **video** có ưu thế rõ rệt so với **ảnh**:

| Định dạng | Số bài | Reach TB | Views TB | Tương tác TB | Click TB |
|---|---:|---:|---:|---:|---:|
| Video | 2 | 415,50 | 1.066,50 | 125,00 | 62,50 |
| Ảnh | 7 | 136,86 | 492,86 | 30,57 | 36,43 |

**Hình 5.3. Biểu đồ so sánh hiệu quả trung bình theo định dạng (Ảnh vs Video)** (Nguồn: Insights CSV; xử lý từ Excel/Google Sheets)

![Hình 5.3. Biểu đồ so sánh hiệu quả trung bình theo định dạng (Nguồn: Insights CSV)](img/hinh5_3_format_compare.png)

Kết quả này phù hợp với đặc thù ngành đồ ăn vặt: video/Reels thể hiện tốt hơn các yếu tố “nóng – giòn – đã mắt”, từ đó tăng khả năng phân phối và tạo tương tác. Do tỷ trọng video trong mẫu còn thấp (2/9), việc tăng tỷ lệ video là hướng trọng tâm để cải thiện nhóm KPI nhận diện và tương tác.

## 5.4. Phân tích theo trụ nội dung (content pillars)

Phân tích theo trụ nội dung được sử dụng để kiểm tra cơ cấu nội dung trong giai đoạn theo dõi và liên hệ với hiệu quả theo từng nhóm bài. Việc phân loại trong mục này dựa trên nội dung caption/chủ đề thể hiện rõ nhất trong từng bài; do số lượng bài trong mẫu còn ít (n = 9), kết quả được dùng để định hướng triển khai kế hoạch ở Chương 4.

### 5.4.1. Phân loại bài theo trụ nội dung

**Bảng 5.2. Phân loại bài theo trụ nội dung** (Nguồn: Meta Business Suite/Insights – file CSV; tổng hợp của nhóm tác giả)

| Mã | Thời gian đăng | Định dạng | Trụ nội dung | Tầng phễu | Ghi chú ngắn |
|---|---|---|---|---|---|
| P01 | 16/01/2026 03:15 | Ảnh | Sản phẩm – cận cảnh | TOFU/MOFU | Nêu lợi ích “giòn – nóng” kèm thông tin liên hệ |
| P02 | 23/01/2026 17:48 | Ảnh | Sản phẩm – cận cảnh | TOFU | Giới thiệu cả chuối và khoai; nhấn vị/đặc điểm |
| P03 | 04/02/2026 06:06 | Ảnh | Thông báo bán/nhận đơn | BOFU | Nhấn mạnh “đủ bánh”, khuyến khích đặt trước số lượng lớn |
| P04 | 05/02/2026 01:05 | Video | Sản phẩm – cận cảnh | TOFU/MOFU | Gắn bối cảnh thời tiết + có giá/ship + CTA đặt ngay |
| P05 | 09/03/2026 07:46 | Ảnh | Ưu đãi/nhắc đặt theo dịp | BOFU | Nội dung theo bối cảnh “liên hoan/khai xuân” |
| P06 | 10/03/2026 21:03 | Ảnh | Ưu đãi/Combo | BOFU | Vừa “ra lò” vừa có ưu đãi mua 5 tặng 1 |
| P07 | 10/04/2026 00:21 | Ảnh | Hậu trường – nguyên liệu | MOFU | Nêu nguyên liệu “chuối tươi nguyên quả” nhằm tăng tin cậy |
| P08 | 15/04/2026 03:13 | Ảnh | Thông báo bán/nhận đơn | BOFU | Nội dung ngắn, tập trung kêu gọi đặt – ship liền |
| P09 | 15/04/2026 22:52 | Video | Ưu đãi/Combo | BOFU | Ưu đãi theo số lượng, phù hợp bài chốt đơn |

Từ cơ cấu trên có thể thấy nhóm bài tập trung nhiều vào **sản phẩm** và **ưu đãi/thông báo bán**, trong khi trụ **feedback/UGC** (bằng chứng xã hội) hầu như chưa xuất hiện rõ trong mẫu. Đây là một khoảng trống nội dung quan trọng vì feedback thường hỗ trợ tăng tin cậy và thúc đẩy chuyển đổi qua inbox/bình luận.

**Hình 5.4. Biểu đồ cơ cấu trụ nội dung (số bài theo trụ)** (Nguồn: tổng hợp từ Bảng 5.2)

![Hình 5.4. Biểu đồ cơ cấu trụ nội dung (Nguồn: tổng hợp từ Bảng 5.2)](img/hinh5_4_pillars.png)

### 5.4.2. So sánh chỉ số theo trụ nội dung (tham khảo)

**Bảng 5.3. Chỉ số trung bình theo trụ nội dung** (Nguồn: Insights CSV; tính toán của nhóm tác giả)

| Trụ nội dung | Số bài | Reach TB | Views TB | Tương tác TB | Bình luận TB |
|---|---:|---:|---:|---:|---:|
| Sản phẩm – cận cảnh | 3 | 381,67 | 1.198,33 | 83,67 | 40,00 |
| Ưu đãi/Combo/nhắc đặt theo dịp | 3 | 127,00 | 336,67 | 45,00 | 13,67 |
| Thông báo bán/nhận đơn | 2 | 98,00 | 389,50 | 29,00 | 6,50 |
| Hậu trường – nguyên liệu | 1 | 67,00 | 199,00 | 20,00 | 3,00 |
| Feedback/UGC | 0 | – | – | – | – |

Kết quả gợi ý rằng nhóm **sản phẩm – cận cảnh** đang tạo hiệu quả tốt nhất về reach, views và bình luận; nhóm **ưu đãi/Combo** cho thấy tỷ lệ tương tác cao ở một số bài (ví dụ bài ưu đãi theo số lượng), phù hợp triển khai ở tầng chốt đơn. Do trụ **feedback/UGC** chưa có trong mẫu, kế hoạch ở Chương 4 cần bổ sung nhóm nội dung này để tăng tính thuyết phục và hỗ trợ chuyển đổi.

## 5.5. Phân tích bài viết nổi bật và yếu tố tạo hiệu quả

Dựa theo reach, bình luận và tổng tương tác, có thể nhận diện một số bài nổi bật:

| Thời điểm đăng | Định dạng | Reach | Views | Tương tác | Bình luận | Link click | Nhận xét ngắn |
|---|---|---:|---:|---:|---:|---:|---|
| 05/02/2026 | Video | 617 | 1.600 | 149 | 69 | 16 | Nội dung gắn “thời tiết se lạnh” + mô tả lợi ích “nóng giòn” + có giá; tạo nhiều bình luận và hành động |
| 16/01/2026 | Ảnh | 436 | 1.611 | 83 | 44 | 0 | Ảnh sản phẩm rõ ràng, có địa chỉ/điện thoại; tạo nhiều click và bình luận hỏi mua |
| 15/04/2026 | Video | 214 | 533 | 101 | 34 | 0 | Bài ưu đãi (theo số lượng) tạo tỷ lệ tương tác cao; phù hợp tầng chốt đơn |

Các đặc điểm chung của nhóm bài hiệu quả:
- Nội dung mô tả rõ “lợi ích ngay lập tức” của sản phẩm (nóng, giòn, chiên mới).
- CTA hướng về hành động cụ thể (đặt hàng/inbox), kèm thông tin giá/địa chỉ giúp giảm câu hỏi lặp.
- Bối cảnh (thời tiết, khung giờ, ưu đãi) giúp tăng động lực ra quyết định.

## 5.6. Đánh giá tín hiệu chuyển đổi (bình luận, inbox, link click)

Trong bộ dữ liệu CSV được xuất, chỉ số tin nhắn (messages/inbox initiated) không nằm trong nhóm cột sử dụng để tổng hợp. Tuy nhiên, theo ghi nhận thực tế vận hành:
- **Số đơn chốt qua nhắn tin Facebook (Messenger):** 10 đơn
- **Số đơn chốt qua web:** 30 đơn

Như vậy, tổng số đơn hàng trong giai đoạn nghiên cứu là **40 đơn**, trong đó kênh web chiếm tỷ trọng lớn hơn. Điều này cho thấy hành vi khách hàng có thể bắt đầu từ Facebook (xem bài, nhắn tin hỏi) nhưng chuyển sang đặt hàng qua web để thuận tiện xác nhận, hoặc ngược lại. Việc tích hợp và đối chiếu dữ liệu giữa các kênh giúp đánh giá chính xác hơn hiệu quả chuyển đổi tổng thể.

## 5.7. Hạn chế chính trong kết quả hiện tại

Từ kết quả tổng hợp và so sánh KPI, một số hạn chế đáng chú ý gồm:
- **Tần suất và cơ cấu định dạng:** số bài trong giai đoạn theo dõi còn ít; tỷ trọng video thấp, trong khi video có hiệu quả vượt trội.
- **Nội dung chưa tạo được mức reach/views theo mục tiêu:** mức tiếp cận trung bình/bài chưa đạt mục tiêu kế hoạch, khiến nhóm KPI nhận diện bị hạn chế.
- **Hành động theo liên kết thấp:** link click thấp phản ánh liên kết chưa phải điểm chạm chính; cần tối ưu hướng “inbox/bình luận” và thông tin đặt hàng.

## 5.8. Kiến nghị điều chỉnh để đạt KPI kế hoạch

Để cải thiện các chỉ số chưa đạt và tiệm cận KPI Chương 4, đề xuất tập trung vào các hướng sau:
1) **Tăng tỷ lệ video/Reels** trong lịch đăng (ưu tiên cận cảnh, âm thanh chiên, thành phẩm nóng).
2) **Chuẩn hóa cấu trúc caption**: lợi ích chính (ngon – nóng – giòn) + giá/combo + cách đặt + khu vực giao.
3) **Tăng bài chốt đơn theo bối cảnh**: ưu đãi theo khung giờ hoặc theo số lượng ở các ngày cao điểm.
4) **Tăng nội dung tạo tin cậy**: hậu trường quy trình, đóng gói, feedback khách; dùng làm lớp hỗ trợ cho chuyển đổi qua inbox.
5) **Theo dõi bình luận có ý định mua theo tuần** để xác định mẫu CTA hiệu quả và nhân rộng.

## 5.9. Tiểu kết chương

Chương 5 đã tổng hợp và đánh giá kết quả hoạt động Facebook Marketing của **Bánh Chuối Chiên QT** trong giai đoạn 01/01/2026–16/04/2026, đối chiếu với KPI kế hoạch ở Chương 4. Kết quả cho thấy tỷ lệ tương tác theo reach đạt mức tốt, tuy nhiên reach/views và nhóm chỉ số hành động chưa đạt mục tiêu kế hoạch. Trên cơ sở đó, chương đã đề xuất các hướng điều chỉnh trọng tâm (tăng tỷ trọng video/Reels, chuẩn hóa CTA và tối ưu nội dung chốt đơn) nhằm cải thiện hiệu quả triển khai trong giai đoạn tiếp theo.

# KẾT LUẬN

Qua quá trình nghiên cứu tình huống và phân tích dữ liệu Meta Insights giai đoạn 01/01/2026–16/04/2026, đề tài đã xác định được bức tranh hiện trạng Facebook Marketing của **Bánh Chuối Chiên QT** theo lộ trình Nhận diện → Tương tác → Chuyển đổi, trong đó tỷ lệ tương tác theo reach đạt mức tốt nhưng các chỉ số phân phối (reach, views) và hành động (click, link click) chưa đạt mục tiêu kế hoạch; đồng thời kết quả theo định dạng cho thấy video/Reels có hiệu quả vượt trội so với ảnh, vì vậy hướng triển khai phù hợp trong giai đoạn tiếp theo là tăng tỷ trọng video/Reels, chuẩn hóa cấu trúc nội dung và CTA theo mục tiêu chốt đơn qua bình luận/inbox, kết hợp nội dung tạo tin cậy (hậu trường, feedback) và theo dõi chỉ số theo tuần để tối ưu liên tục, qua đó từng bước nâng hiệu quả truyền thông và hỗ trợ tăng chuyển đổi đặt hàng cho thương hiệu.

# TÀI LIỆU THAM KHẢO

1. Meta for Business. *Meta Business Suite: Manage Facebook and Instagram in one place*. https://www.facebook.com/business/tools/meta-business-suite (Truy cập: 17/04/2026).
2. Meta Business Help Centre. *Meta Business Help Centre: Help, support and troubleshooting*. https://www.facebook.com/business/help (Truy cập: 17/04/2026).
3. Wikipedia contributors. *AIDA (marketing)*. https://en.wikipedia.org/wiki/AIDA_(marketing) (Truy cập: 17/04/2026).
4. Wikipedia contributors. *STP marketing*. https://en.wikipedia.org/wiki/STP_marketing (Truy cập: 17/04/2026).
5. Wikipedia contributors. *Engagement rate*. https://en.wikipedia.org/wiki/Engagement_rate (Truy cập: 17/04/2026).
