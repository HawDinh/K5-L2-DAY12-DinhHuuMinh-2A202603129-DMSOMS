# Danh sách lớp v0 — Đinh Hữu Minh · G10

MSSV: 2A202603129. Bản nháp có hỗ trợ Codex, dùng các lớp và thuộc tính trong Guideline Cabin v1.0. Cần đối chiếu thêm slide 54 trước khi nộp; file slide chưa có trong tài nguyên local đã kiểm tra. Danh sách này không bổ sung nhãn mới vào project CVAT.

## DMS

| # | Lớp / thuộc tính | Giá trị | Để làm gì | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | face và face13 | Box mặt; 13 điểm với occluded/outside | Xác định vùng mặt và các mốc hình học để phân tích mắt, đầu, miệng | Có nếu mặt đủ lớn và đủ rõ; bỏ/đánh cờ phần không định vị được |
| 2 | eye_left, eye_right | open / closed / unknown, riêng từng mắt | Theo dõi trạng thái mắt; chỉ đánh giá nhắm mắt kéo dài khi có chuỗi thời gian | Có khi nhìn rõ khe mắt; phản xạ, vật che hoặc ảnh mờ có thể làm mất thông tin |
| 3 | glasses | none / clear / sun / unknown | Nhận biết điều kiện ảnh hưởng đến khả năng quan sát mắt | Có thể thấy gọng và tròng, nhưng NIR có thể làm kính râm trông trong; không chắc chọn unknown |
| 4 | gaze_zone | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Phân loại vùng nhìn để hỗ trợ đánh giá việc quan sát đường và các khu vực trong xe | Có thể suy từ mắt và đầu khi rõ; không chắc dùng other và ghi lý do |
| 5 | hand.whose, side, on_wheel | driver/passenger; left/right; yes/no/unknown | Phân biệt tay của ai và tay người lái có đang chạm vô lăng | Thường thấy được hình dạng và vị trí; tay bị che thì không đoán |
| 6 | phone.state | held / mounted / lap | Nhận biết điện thoại và trạng thái sử dụng trong cabin | Có nếu nhìn rõ một phần hình dạng; không dựa riêng vào tư thế tay gần tai |
| 7 | person.seatbelt | worn_correct / worn_incorrect / not_worn / unknown | Mô tả tình trạng dây an toàn của người lái trong khung hình | Phụ thuộc tương phản và phần thân nhìn thấy; đang thắt dở hoặc bị che dùng unknown |

## OMS

| # | Lớp / thuộc tính | Giá trị | Để làm gì | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | person | Box phần cơ thể thấy được | Nhận biết người lớn đang chiếm chỗ ngồi | Có nếu phân biệt được cơ thể với nền ghế |
| 2 | seat | driver / front_passenger / rear_left / rear_middle / rear_right | Gắn người hoặc vật với đúng vị trí theo xe | Vị trí hình học còn dùng được; camera nhìn về sau nên trái ảnh là phải xe |
| 3 | child_seat | Box ghế trẻ em | Phân biệt ghế trẻ em với ghế xe trống, nôi và vật khác | Chủ yếu dựa vào lưng ghế, đỡ đầu và hình dáng tổng thể |
| 4 | child_seat.occupied | no / unknown trong schema hiện tại | Phân biệt ghế trống với trường hợp chưa xác định được bên trong | Chỉ gán no khi thấy rõ ghế trống; schema chưa biểu diễn được trường hợp có trẻ rõ ràng |
| 5 | infant_seat | Box ghế/nôi xách cho em bé | Nhận biết thiết bị chở em bé; riêng sự có mặt của nôi không chứng minh có em bé bên trong | Dựa vào lòng nôi ngả, vành và quai nếu thấy; không dựa vào màu vải |
| 6 | infant_seat.facing | rear / front theo hướng xe | Phân biệt hướng lắp đặt của nôi | Dựa vào hướng đầu/chân và hình dáng nôi; không đồng nhất với bên trái/phải ảnh |
| 7 | object.type | bag / box / cup / other | Phân biệt vật mang theo với người hoặc thiết bị chở trẻ để mô tả ghế đang bị chiếm chỗ | Hình dáng/quai/cạnh còn có ích; màu vật bị mất trên ảnh xám |
| 8 | frame_meta | modality, light, camera_blocked | Tách điều kiện dữ liệu và phát hiện camera bị che để tránh diễn giải sai các nhãn OMS | Modality biết từ nguồn ảnh; NIR không tự chứng minh là ban đêm |

## Hai câu hỏi leo thang

1. **Ghế có trẻ rõ ràng nhưng schema không có occupied=yes thì phải nộp thế nào?**
   Khung ví dụ: G10 S02_rgb/S02_ir; các cụm tương tự ở S03, S05, S07.
   Hiện trạng: người học yêu cầu bỏ qua đối tượng trẻ, sau đó đã bổ sung box ghế. S02 đang giữ occupied=unknown vì schema không có giá trị yes; không diễn giải unknown này là bằng chứng không thấy rõ trẻ. Cần coach xác định có thay ảnh, mở rộng schema, hay chấp nhận ngoại lệ ghi chú. Không gán no cho ghế đang có trẻ.

2. **Khi chỉ thấy góc/cạnh điện thoại, giới hạn ngoại suy box toàn máy là bao nhiêu?**
   Khung ví dụ: G10 D04, điện thoại gần mặt bị bàn tay che phần lớn.
   Hiện trạng: gán phone, state=held, occluded vì nhìn thấy một phần đường viền; người học đã chỉnh lại box. Cần người viết guideline xác định tiêu chí về mức nhìn thấy và biên box của phần bị che để hai người review dùng cùng một cách.
