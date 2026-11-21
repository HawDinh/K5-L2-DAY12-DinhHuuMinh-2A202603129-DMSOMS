# Tự đánh giá — Đinh Hữu Minh · G10

MSSV: 2A202603129. Nội dung được soạn với Codex từ các thay đổi trong export và phản hồi của người học: điểm mắt/mũi bị lệch nhẹ cần sửa; bản đầu bỏ thiếu nhiều infant seat ở các cảnh cuối. Đây là tự rà soát trước review chéo, chưa phải nhận xét của Trần Anh Văn.

## 1. Tự rà soát bốn ảnh

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S03_rgb | infant_seat ở rear_middle | missing | Bản đầu bỏ cụm có trẻ; sau khi người học bổ sung các ghế, ảnh RGB này vẫn thiếu nôi so với IR. Đã vẽ box nôi riêng, không vẽ trẻ; seat=rear_middle, facing=front | Có, trên CVAT |
| S05_ir | infant_seat ở rear_right | missing | Đã được người học bổ sung; thuộc tính mới còn mặc định driver/rear. Đã sửa seat=rear_right và facing=front, kiểm lại RGB tương ứng | Có, trên CVAT |
| D02 | face13 mắt, mũi và môi | box | Điểm mặt của bản đầu gần vùng đúng nhưng lệch khỏi mép mí/chóp mũi; người học đã kéo lại. Khoảng dịch mũi giữa hai bản khoảng 21 px trên ảnh gốc, cho thấy nhìn thu nhỏ chưa đủ để chốt mốc | Có, người học chỉnh tọa độ |
| D04 | face13 và thuộc tính mắt phải | flag | Người học đã định vị lại mốc mắt/mũi. eye_right vẫn unknown nhưng bốn điểm mắt phải đều không có cờ. Giữ tọa độ người học đã chỉnh, thêm occluded cho các điểm ước lượng trong vùng khó thấy | Có, trên CVAT |

## 2. Lỗi lặp lại và cách tránh

Hai lỗi lặp lại được người học xác nhận là lệch nhẹ điểm mắt/mũi và bỏ thiếu infant seat. Ngoài ra, lượt kiểm script phát hiện các box ghế mới còn giữ thuộc tính mặc định seat=driver.

Các mốc ban đầu chưa bám đủ sát mép mí/chóp mũi khi xem phóng lớn; một số nôi đã bị để lại cùng cụm trẻ rồi chưa được bổ sung đầy đủ trên cả hai modality. Đây là mô tả từ các bản nhãn, không suy đoán trạng thái tập trung của người gán.

Lần sau: phóng vùng mặt trước khi chốt từng mốc; rà lần lượt ba vị trí ghế trên từng ảnh; sau khi thêm box mở ngay DETAILS; kiểm cả RGB và IR, rồi chạy script. 0 ERROR không bắt được việc bỏ sót nôi hoặc lệch mốc nhỏ.

## 3. Ghi chú về disagreement_log

Chưa nhận export của Trần Anh Văn nên chưa có dòng mine/peer để kết luận ai đúng. Cần bổ sung một bất đồng thật, luật áp dụng và điều học được sau review. Không dùng diff giữa hai bản của chính mình thay thế E3.
