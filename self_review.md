# Tự đánh giá — Đinh Hữu Minh · G10

MSSV: 2A202603129. Các lỗi được ghi nhận qua tự kiểm và đối chiếu export: điểm mắt/mũi bị lệch nhẹ cần sửa; bản đầu bỏ thiếu nhiều infant seat ở các cảnh cuối. Phần review cập nhật ngày 22/11/2026 dùng hai export v2; chưa diễn giải đề xuất riêng là đồng thuận của hai người.

## 1. Tự rà soát năm ảnh

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S03_rgb | infant_seat ở rear_middle | missing | Bản đầu bỏ cụm có trẻ; sau khi người học bổ sung các ghế, ảnh RGB này vẫn thiếu nôi so với IR. Đã vẽ box nôi riêng, không vẽ trẻ; seat=rear_middle, facing=front | Có, trên CVAT |
| S05_ir | infant_seat ở rear_right | missing | Đã được người học bổ sung; thuộc tính mới còn mặc định driver/rear. Đã sửa seat=rear_right và facing=front, kiểm lại RGB tương ứng | Có, trên CVAT |
| D02 | face13 mắt, mũi và môi | box | Điểm mặt của bản đầu gần vùng đúng nhưng lệch khỏi mép mí/chóp mũi; người học đã kéo lại. Khoảng dịch mũi giữa hai bản khoảng 21 px trên ảnh gốc, cho thấy nhìn thu nhỏ chưa đủ để chốt mốc | Có, người học chỉnh tọa độ |
| D03 | phone vùng console | extra | Gán nhầm chi tiết console xe thành điện thoại; đã xóa box theo GUIDE 4.2 và 4.7 sau đối chiếu ảnh và bản peer | Có, ZIP/XML sau review |
| D04 | face13 và thuộc tính mắt phải | flag | Người học đã định vị lại mốc mắt/mũi. eye_right vẫn unknown nhưng bốn điểm mắt phải đều không có cờ. Giữ tọa độ người học đã chỉnh, thêm occluded cho các điểm ước lượng trong vùng khó thấy | Có, trên CVAT |

## 2. Lỗi lặp lại và cách tránh

Hai lỗi lặp lại được người học xác nhận là lệch nhẹ điểm mắt/mũi và bỏ thiếu infant seat. Ngoài ra, lượt kiểm script phát hiện các box ghế mới còn giữ thuộc tính mặc định seat=driver.

Các mốc ban đầu chưa bám đủ sát mép mí/chóp mũi khi xem phóng lớn; một số nôi đã bị để lại cùng cụm trẻ rồi chưa được bổ sung đầy đủ trên cả hai modality. Đây là mô tả từ các bản nhãn, không suy đoán trạng thái tập trung của người gán.

Lần sau: phóng vùng mặt trước khi chốt từng mốc; rà lần lượt ba vị trí ghế trên từng ảnh; sau khi thêm box mở ngay DETAILS; kiểm cả RGB và IR, rồi chạy script. 0 ERROR không bắt được việc bỏ sót nôi hoặc lệch mốc nhỏ.

## 3. Ghi chú về disagreement_log

Log giữ 47 dòng từ lần đối chiếu hai bản v2 trước sửa để lưu dấu review. Sau khi kiểm ảnh: 20 đề xuất giữ mine, 1 peer và 26 ambiguous. Dòng peer là phone D03: box ôm chi tiết console xe, không phải điện thoại. Đã xóa box này trong export_final_v2.zip và XML ngày 22/11/2026 theo GUIDE 4.2, 4.7. Văn không gán phone ở vị trí đó là đúng. Các đề xuất sửa bài Văn đã được lập thành bản bàn giao; chưa ghi nhận Văn xác nhận hoặc sửa xong. Diff sau sửa còn 46 dòng, vì lỗi phone D03 đã hết.

Một góp ý đúng về bản Minh trước QC là thiếu infant_seat trên S03 RGB: bản export_final.zip ban đầu thiếu, trong khi Văn có; bản v2 hiện tại của Minh đã bổ sung trước lần diff mới. Bài học là phải xác định đúng phiên bản trước khi nhận hay bác một góp ý, và kiểm đủ cả hai modality. Góp ý này không còn là bất đồng giữa hai bản v2, nên không tạo thêm dòng giả trong disagreement_log.

## 4. Phạm vi nhãn và bằng chứng

- Bản nhãn hiện tại là export_final_v2.zip; export_final.zip giữ bản trước khi người học bổ sung ghế và sửa các mốc mặt. Cả hai ZIP chỉ chứa annotations.xml.
- Không vẽ đối tượng trẻ theo yêu cầu người học. Ghế tại S02/S03/S05/S07 đã bổ sung trong v2; child_seat ở S02 giữ occupied=unknown do schema không có yes, không có nghĩa là trẻ không nhìn thấy. Cần coach phân xử ngoại lệ dữ liệu này, như câu hỏi trong class_list_v0.md.
- D01 và D03 không có face/face13 vì mắt và mũi không đủ rõ theo cách áp dụng GUIDE 4.5; bất đồng với Văn được ghi trong disagreement_log.csv. D04 eye_right=unknown và các mốc ước lượng có occluded; mũi/miệng bị che hoàn toàn ở D06 để outside.
- check_cpA.txt và check_cpB.txt là lượt kiểm lại đúng phạm vi checkpoint trên bản export cuối, không phải snapshot tại hai thời điểm giữa buổi.
- source="manual" được giữ trên các nhãn còn lại. Lượt sửa sau review chỉ xóa phone D03; mọi tọa độ, thuộc tính và cờ khác giữ nguyên.
- ZIP/XML được sửa tại máy; nhãn trong CVAT chưa đồng bộ thay đổi xóa phone D03 ở lượt này. Cần dùng bản export đã sửa khi gửi review/nộp bài.
