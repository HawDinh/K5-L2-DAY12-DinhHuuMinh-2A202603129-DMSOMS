# Ghi chú gán nhãn Lab12

Bộ G10, project Day12, task Day12-G10, job 23 trên CVAT local 2.81.0. Người học xác nhận coach chấp nhận phiên bản này. Nhãn trong lần hỗ trợ này do Codex chuẩn bị từ quan sát canvas CVAT và nhập qua giao diện; cần người học rà soát trước khi nộp. Khai báo hỗ trợ trong prompts.md.

## Các cụm để lại theo yêu cầu người học

GUIDE mục 3 nói không gán nếu thấy hình giống trẻ em. Sau khi được báo về ca S02, người học trả lời: “bỏ qua trẻ em, tôi có thể cập nhật sau đó nếu cần”. Vì schema chỉ mô tả ghế trống và không có occupied=yes, các cụm sau tạm chưa có box; không đổi chúng thành ghế trống hoặc dùng unknown để che việc nhìn thấy trẻ.

| Cảnh | Vị trí theo xe | Cụm chưa gán trên cả RGB và IR |
|---|---|---|
| S02 | rear_middle | Hình giống trẻ em trong child seat |
| S03 | rear_middle | Hình giống em bé trong infant seat |
| S05 | rear_right | Hình giống em bé trong infant seat |
| S07 | rear_right | Hình giống em bé trong infant seat, hướng về sau |

Các ảnh này vẫn có frame_meta và các đối tượng khác rõ ràng. 0 ERROR của script chỉ nói cấu trúc hợp lệ, không xác nhận đủ nhãn tại các vùng đang để lại.

## Các quyết định cần xem lại khi review

- D01: tài xế đang thao tác với dây; seatbelt=unknown vì trạng thái chưa xác định. Đầu quay xa camera, không xác định được mắt/mũi nên không vẽ face hoặc face13 theo GUIDE 4.5.
- D02: gán hai mắt closed theo đường mí quan sát được khi phóng lớn; cần đối chiếu lại xem có khe mắt mở nhỏ hay không. Tay phải bị vô lăng che nên không đặt box cho phần không thấy. gaze_zone=other vì không xác định chắc vùng nhìn trong khung mắt khép.
- D03: phía sau/nghiêng đầu và vật cầm che mặt; không thấy đủ mắt/mũi để đặt face/face13. Box phone có phần bị bàn tay che; cần review lại đường viền và loại vật.
- D04: mắt phải người lái bị bàn tay/vật cầm che, eye_right=unknown; các điểm mắt phải outside vì không xác định chắc vị trí. Mắt trái open. Box phone dựa vào đoạn cạnh/góc tối nhìn thấy, phần còn lại bị che; cần review lại loại vật và đường viền.
- D05: kính đang được cầm trên tay, glasses=none. Mắt bên xa và khóe miệng bên xa không định vị được nên outside; các mốc môi khó thấy dưới râu đánh occluded. Vùng nhìn tạm lap theo hướng đầu về vật đang cầm, cần bạn review đối chiếu.
- D06: chai và bàn tay che mũi/miệng nên các điểm đó outside; không vẽ phone hoặc object cho chai trong DMS. Hai mắt gán open theo khe mắt quan sát được.
- DMS: trái/phải theo người lái; mắt trái có y lớn hơn mắt phải. Mọi light=unknown vì không kết luận ngày/đêm chỉ từ NIR.
- OMS: face chỉ là box; các thuộc tính face giữ mặc định unknown/other theo schema, không suy vùng nhìn/mắt từ ảnh dựng. S01 RGB seatbelt=unknown vì tối; S01 IR-sim not_worn vì thấy rõ thân người không có dải chéo.

## Ghế thực sự trống

Đếm ghế không có người hoặc vật mang theo; không đếm ghế trẻ em/nôi đã đặt lên ghế xe là ghế trống.

| Cảnh | Số ghế trống | Ghi chú |
|---|---|---|
| S01 | 0 | Child seat, cup và người lớn chiếm ba chỗ |
| S02 | 1 | rear_left trống; rear_middle đang để lại |
| S03 | 0 | Bag, cụm em bé đang để lại và child seat |
| S04 | 2 | rear_middle và rear_left |
| S05 | 1 | rear_middle trống; rear_right đang để lại |
| S06 | 2 | rear_middle và rear_left |
| S07 | 2 | rear_middle và rear_left; rear_right đang để lại |
| S08 | 1 | rear_middle |

## Kiểm tra và review

Export thực tế từ CVAT: `export_final.zip`, chỉ chứa `annotations.xml`. Lệnh check với expect=22 báo **22 ảnh, 22 frame_meta, 0 ERROR và 0 WARN**. Đã mở lại cả 22 khung trên canvas sau import; chỉnh nhỏ box face S01 RGB/IR trực tiếp trên CVAT rồi Save trước export.

Hai log `check_cpA.txt` và `check_cpB.txt` là lượt kiểm lại phạm vi S01–S02 và D01–D02 **trên cùng export cuối**, không phải các export chụp tại hai thời điểm giữa buổi. Cả hai phạm vi đều báo 0 ERROR và 0 WARN. Trạng thái này không thay thế bằng chứng tiến trình trong lớp nếu coach yêu cầu.

Chưa có export độc lập từ Trần Anh Văn để chạy peer diff. Không tạo disagreement_log hoặc lời nhận xét thay cho bạn review.
