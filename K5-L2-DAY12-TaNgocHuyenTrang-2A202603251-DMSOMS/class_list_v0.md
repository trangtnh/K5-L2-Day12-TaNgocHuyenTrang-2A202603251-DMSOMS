# Danh sách lớp v0 — Tạ Ngọc Huyền Trang · G09

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | eye_state | open / closed / unknown | Phát hiện buồn ngủ (tính chỉ số PERCLOS), phát hiện ngủ gật tức thời (micro-sleep) để kích hoạt chuông cảnh báo | Có, trừ khi bị đốm phản xạ LED trên kính hoặc kính râm tối |
| 2 | gaze_zone | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Phát hiện mất tập trung (distraction alert) khi tài xế rời mắt khỏi đường quá 2–3 giây (ví dụ nhìn xuống đùi xem điện thoại) | Có, kết hợp giữa hướng đồng tử và góc xoay đầu (head pose) |
| 3 | hand_on_wheel | yes / no / unknown | Giám sát tài xế duy trì kiểm soát vô lăng trong chế độ lái tự động cấp độ 2/3 (Hands-on Detection), phát cảnh báo Takeover Request | Có, viền phản xạ ánh sáng giữa bàn tay và vành vô lăng rất rõ ràng |
| 4 | phone_state | held / mounted / lap | Phát hiện và cảnh báo hành vi vi phạm sử dụng điện thoại khi đang điều khiển phương tiện | Có, nếu thấy một phần cạnh máy/màn hình; khó nhận biết màn hình bật/tắt nếu không có phản xạ |
| 5 | glasses | none / clear / sun / unknown | Tự động điều chỉnh thuật toán theo dõi mắt phù hợp (bật chế độ bù trừ biến dạng khúc xạ kính hoặc lọc phản xạ) | Có, kính cận thấy rõ tròng; kính râm có thể trông trong suốt dưới tia NIR |
| 6 | face_occlusion | cờ occluded trên face13 (0 / 1) | Đo lường độ tin cậy của thuật toán trích xuất mốc mặt; phát hiện tài xế đeo khẩu trang hoặc dùng tay che mặt | Có, NIR phản xạ khác biệt rõ trên chất liệu vải khẩu trang hoặc ngón tay |
| 7 | mouth_opening (yawn) | open / closed | Nhận diện hành vi ngáp liên tục để cảnh báo trạng thái mệt mỏi sớm trước khi tài xế thực sự nhắm mắt | Có, 4 điểm mốc môi (lip_upper, lip_lower, mouth_left, mouth_right) thể hiện rõ độ mở |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | seat_occupancy | person (rear_left / rear_middle / rear_right / front_passenger) | Nhắc nhở thắt dây an toàn đúng vị trí ghế đang có người ngồi; định tuyến âm thanh cảnh báo tới đúng người | Có, hình thể người nổi bật rõ rệt trên ảnh IR |
| 2 | infant_seat | facing: rear / front (theo ghế) | Tự động ngắt túi khí phía trước nếu nôi em bé quay ngược (tránh chấn thương do lực nổ túi khí); cảnh báo chống bỏ quên trẻ (CPD) | Có, nhận diện chính xác qua kiểu dáng vòm nôi và quai xách |
| 3 | child_seat | occupied: no / yes / unknown (theo ghế) | Kích hoạt hệ thống giám sát trẻ em và nhắc nhở thắt đai an toàn chuyên dụng cho ghế trẻ em | Có, nhận biết qua khung ghế booster/child seat |
| 4 | seatbelt_status | worn_correct / worn_incorrect / not_worn / unknown | Cảnh báo thắt dây sai quy cách (luồn dưới nách hoặc sau lưng) làm giảm hiệu quả bảo vệ khi xảy ra va chạm | Khó hơn trên IR nếu màu áo và dây cùng tối; nhận biết tốt nếu dây có sợi phản quang hoặc áo sáng |
| 5 | leftover_object | bag / box / cup / other (theo ghế) | Hệ thống nhắc nhở hành khách không để quên đồ đạc, balo, laptop có giá trị khi tắt máy rời xe | Có, nhận dạng hình khối không gian tốt trên ảnh IR |
| 6 | occupant_size | adult / child / empty | Điều chỉnh lực bung túi khí thông minh nhiều giai đoạn (Smart Airbag Deployment) tương ứng với trọng lượng/kích cỡ hành khách | Có, dựa trên tỷ lệ chiều cao thân trên và kích thước bounding box |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. **Câu hỏi:** Khi tài xế áp sát bàn tay lên tai trong tư thế nghe điện thoại nhưng bàn tay che kín hoàn toàn thiết bị, mắt thường và detector không nhìn thấy cạnh hay màn hình máy. Guideline quy định không vẽ `phone` nếu không thấy viền máy, nhưng về mặt nghiệp vụ an toàn xe thì đây là hành vi mất tập trung nghiêm trọng. Trong phiên bản guideline tiếp theo, chúng ta nên bổ sung nhãn phụ `suspected_phone_call` ở thuộc tính tay hay vẫn giữ nguyên tắc chỉ gán khi thấy vật thể?  
   **Khung ví dụ:** `D05.png`  
   **Bạn tạm gán thế nào và vì sao:** Tạm thời tuân thủ nghiêm ngặt mục 4.7 của Guideline v1.0: không vẽ nhãn `phone` mà chỉ vẽ box `hand` với thuộc tính `whose=driver`, `on_wheel=no` để tránh gán nhãn giả định (false positive) làm sai lệch mô hình phát hiện vật thể.

2. **Câu hỏi:** Tập dữ liệu SVIRO sử dụng ảnh đồ họa tổng hợp (IR-sim) trong đó không có bất kỳ hành khách nào thắt dây an toàn (tất cả đều là `not_worn`). Nếu huấn luyện mô hình OMS chỉ trên dữ liệu này, mô hình sẽ không bao giờ học được đặc trưng trực quan của lớp `worn_correct` trên ảnh cabin xe BMW X5. Ban dự án có kế hoạch bổ sung dữ liệu cabin thật có người thắt dây hay dùng kỹ thuật tổng hợp đồ họa có dây thắt cho phiên bản v2.0 không?  
   **Khung ví dụ:** `S01_rgb.png`, `S04_ir.png`  
   **Bạn tạm gán thế nào và vì sao:** Tạm gán `seatbelt=not_worn` khi nhìn thấy rõ ngực áo không có dây vắt qua và gán `unknown` khi vùng ngực bị che khuất; đồng thời ghi nhận hạn chế này vào `ir_log.csv` để đội ngũ thiết kế dữ liệu lưu ý.
