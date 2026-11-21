# Tự đánh giá — Tạ Ngọc Huyền Trang · G09

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S04_rgb.png | infant_seat | `seat_flip` | Thuộc tính seat bị nhận giá trị mặc định `driver` của CVAT, đã chỉnh lại thành `rear_left` theo xe | Đã sửa |
| S02_ir.png | child_seat | `ok` | Ghế trẻ em có túi bên trong, gán đúng 2 box riêng biệt (`child_seat` và `object`) cùng vị trí ghế | Đã sửa |
| D01.png | hand | `lr_flip` | Ban đầu cả 2 bàn tay đều để `side=right`, đã chỉnh lại 1 tay thành `side=left` theo người | Đã sửa |
| D02.png | face13 | `flag` | Mắt phải bị bóng phản xạ đèn NIR che khuất, đã bật cờ `occluded=1` cho 4 điểm mốc mắt phải | Đã sửa |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Bị dính giá trị thuộc tính mặc định của CVAT (đặc biệt là `seat = driver` khi vẽ các đối tượng hàng ghế sau trong SVIRO).

Vì sao mình mắc: Khi vẽ hình chữ nhật mới trong CVAT, hệ thống tự động điền giá trị đầu tiên của danh sách lựa chọn (`driver`). Nếu chỉ tập trung căn chỉnh toạ độ hộp mà không mở ngay tab **DETAILS** bên phải thì rất dễ bỏ sót.

Lần sau tránh bằng cách: Hình thành quy trình thao tác chuẩn: vẽ xong box $\rightarrow$ mở tab DETAILS kiểm tra và chọn lại thuộc tính ngay lập tức trước khi chuyển sang vẽ đối tượng tiếp theo; đồng thời luôn chạy script `cabin_check.py` để rà soát cảnh báo trước khi xuất dữ liệu.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó:
- Tại ảnh `S01_ir.png` (dòng 22), mình gán nhãn vật thể với thuộc tính `type=other`, trong khi bạn gán là `type=bag`. Khi hai bạn cùng đối chiếu lại trên ảnh ghép tăng sáng, đối tượng có quai xách và phom dáng của một chiếc ba lô/túi xách rất rõ ràng nên gán `bag` là chính xác và cụ thể hơn.
- Bài học: Khi gán nhãn trên ảnh hồng ngoại IR thiếu sáng, cần chủ động điều chỉnh công cụ Brightness/Contrast trên canvas CVAT để quan sát rõ viền khối và chi tiết vật thể trước khi quyết định chọn giá trị dự phòng `other`.
