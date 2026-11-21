# Ghi chú gán nhãn Lab12

Bộ G10, project Day12, task Day12-G10, job 23 trên CVAT local 2.81.0 đã được coach chấp nhận. Bản mới nhất là **export_final_v2.zip**: người học đã tự sửa các điểm mắt/mũi và bổ sung ghế, sau đó Codex rà soát thuộc tính và export lại. Bản export_final.zip giữ nguyên để lưu lịch sử. Khai báo hỗ trợ trong prompts.md.

## Trẻ em và các ghế được bổ sung

Theo yêu cầu người học, đối tượng trẻ em vẫn không được vẽ person/face. Người học đã bổ sung bảy box ghế vào các cụm dưới đây; Codex bổ sung box nôi còn thiếu trên S03 RGB, sửa các seat mặc định driver và hướng front/rear. Không có bản review của Trần Anh Văn trong các thay đổi này.

| Cảnh | Vị trí theo xe | Trạng thái ghế trên cả RGB và IR |
|---|---|---|
| S02 | rear_middle | child_seat; occupied=unknown giữ theo bản người học, chờ coach xử lý thiếu giá trị yes |
| S03 | rear_middle | infant_seat, facing=front |
| S05 | rear_right | infant_seat, facing=front |
| S07 | rear_right | infant_seat, facing=rear |

Schema và GUIDE mô tả ghế trống, không có child_seat.occupied=yes. Giá trị unknown ở S02 không có nghĩa là trẻ không nhìn thấy; đây là ngoại lệ dữ liệu/schema chưa được coach phân xử. Câu hỏi này được đưa vào class_list_v0.md. 0 ERROR của script không giải quyết mâu thuẫn đó.

## Các quyết định cần xem lại khi review

- D01: tài xế đang thao tác với dây; seatbelt=unknown vì trạng thái chưa xác định. Đầu quay xa camera, không xác định được mắt/mũi nên không vẽ face hoặc face13 theo GUIDE 4.5.
- D02: gán hai mắt closed theo đường mí quan sát được khi phóng lớn; cần đối chiếu lại xem có khe mắt mở nhỏ hay không. Tay phải bị vô lăng che nên không đặt box cho phần không thấy. gaze_zone=other vì không xác định chắc vùng nhìn trong khung mắt khép.
- D03: phía sau/nghiêng đầu và vật cầm che mặt; không thấy đủ mắt/mũi để đặt face/face13. Box phone có phần bị bàn tay che; cần review lại đường viền và loại vật.
- D04: người học đã định vị lại bốn điểm mắt phải và các mốc khác. Giữ eye_right=unknown vì không xác định chắc trạng thái mắt; các điểm mắt phải ở vị trí ước lượng có occluded=1. Mắt trái open. Giữ box phone người học đã chỉnh; cần review thêm biên phần máy bị che.
- D05: kính đang được cầm trên tay, glasses=none. Người học đã chỉnh mốc mắt/mũi và một số cờ môi; giữ nguyên các chỉnh sửa đó. Mắt bên xa và khóe miệng bên xa còn outside; gaze_zone=lap cần đối chiếu khi peer review.
- D06: chai và bàn tay che mũi/miệng nên các điểm đó outside; không vẽ phone hoặc object cho chai trong DMS. Hai mắt gán open theo khe mắt quan sát được.
- DMS: trái/phải theo người lái; mắt trái có y lớn hơn mắt phải. Mọi light=unknown vì không kết luận ngày/đêm chỉ từ NIR.
- OMS: face chỉ là box; các thuộc tính face giữ mặc định unknown/other theo schema, không suy vùng nhìn/mắt từ ảnh dựng. S01 RGB seatbelt=unknown vì tối; S01 IR-sim not_worn vì thấy rõ thân người không có dải chéo.

## Ghế thực sự trống

Đếm ghế không có người hoặc vật mang theo; không đếm ghế trẻ em/nôi đã đặt lên ghế xe là ghế trống.

| Cảnh | Số ghế trống | Ghi chú |
|---|---|---|
| S01 | 0 | Child seat, cup và người lớn chiếm ba chỗ |
| S02 | 1 | rear_left trống; rear_middle có child seat |
| S03 | 0 | Bag, infant seat và child seat |
| S04 | 2 | rear_middle và rear_left |
| S05 | 1 | rear_middle trống; rear_right có infant seat |
| S06 | 2 | rear_middle và rear_left |
| S07 | 2 | rear_middle và rear_left; rear_right có infant seat |
| S08 | 1 | rear_middle |

## Kiểm tra và review

Hai export thực tế từ CVAT chỉ chứa `annotations.xml`. Bản hiện tại `export_final_v2.zip` được kiểm với expect=22: **22 ảnh, 22 frame_meta, 0 ERROR và 0 WARN**. Có 61 đối tượng gồm child_seat=8, infant_seat=12, person=10, face=8, face13=4, hand=11, phone=2, object=6. Bản người học trước QC (8 WARN) được giữ trong _local_work, không nộp thay bản mới.

QC sau phần sửa của người học chỉ thay thuộc tính ghế/hướng nôi và cờ che khuất mắt phải D04, bổ sung một box nôi S03 RGB; giữ các tọa độ mắt/mũi người học đã chỉnh. Các diff giữa hai bản của chính mình lưu ở _local_work, không dùng làm disagreement_log của peer review.

Bản gửi Văn: `C:\VinUni\lab12\DinhHuuMinh-2A202603129-G10-CVAT.zip`, là bản sao nguyên vẹn của export_final_v2.zip. Tên file trao đổi không bắt buộc; các file nộp chính vẫn theo SUBMISSION.

Hai log `check_cpA.txt` và `check_cpB.txt` là lượt kiểm lại phạm vi S01–S02 và D01–D02 **trên cùng export cuối**, không phải các export chụp tại hai thời điểm giữa buổi. Cả hai phạm vi đều báo 0 ERROR và 0 WARN. Trạng thái này không thay thế bằng chứng tiến trình trong lớp nếu coach yêu cầu.

Chưa có export độc lập từ Trần Anh Văn để chạy peer diff; người học sẽ bổ sung sau. Không tạo disagreement_log hoặc lời nhận xét thay cho bạn review. ir_log.csv và class_list_v0.md đã có bản nháp; self_review dựa trên sửa chữa và phản hồi thật của người học. exit_ticket có đáp án tham khảo hỗ trợ AI, chưa xác nhận người học đã tự hoàn thành.
