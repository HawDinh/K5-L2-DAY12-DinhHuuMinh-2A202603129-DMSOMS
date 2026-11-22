# TEAM — Ngày 12

Mã bộ ảnh: **G10** · Hình thức: **cá nhân** theo yêu cầu coach.

**Phân biệt bài nộp:** `export_final.zip`, `export_final_v2.zip` và `disagreement_log.csv` trong repo này thuộc **Đinh Hữu Minh**; bản nhãn mới nhất ghi tài khoản CVAT `ongbin123`. Giữ các tên này theo SUBMISSION. File nhận từ partner (`van48`) đặt ngoài repo ở `C:\VinUni\lab12\partner_trananhvan\export_final_v2_trananhvan.zip` và `disagreement_log_trananhvan.csv`. Trong CSV của partner, mine=Văn và peer=Minh; trong CSV ở repo này, mine=Minh và peer=Văn.

| Họ tên | MSSV | Đã gán đủ 22 ảnh? | Review chéo với (peer diff) | Số dòng disagreement đã xử lý |
|---|---|---|---|---|
| Đinh Hữu Minh | 2A202603129 | Đã xử lý 22 ảnh; sửa mốc mặt, bổ sung ghế và xóa phone gán nhầm ở D03; đối tượng trẻ vẫn bỏ qua theo yêu cầu | Trần Anh Văn — 2A202603058 | 47 dòng lịch sử: 20 đề xuất mine, 1 peer đã sửa (phone D03), 26 ambiguous |

Review hai chiều: Đinh Hữu Minh review bài Trần Anh Văn và ngược lại. Đã đối chiếu hai export v2 cùng 22 tên ảnh G10. Mine trong CSV của Minh là Đinh Hữu Minh; peer là Trần Anh Văn. CSV Văn gửi dùng chiều ngược lại và so với bản Minh cũ, nên không chép trực tiếp kết luận của CSV đó.

Coach đã chấp nhận CVAT hiện tại (2.81.0 tại localhost:8090), theo thông tin người học cung cấp ngày 21/11/2026.

Các ca cần trao đổi, hỏi coach nếu chưa thống nhất: D02 bàn tay gần vô lăng; D03 điều kiện vẽ face; lỗi phone của Minh đã sửa, hand của Văn đã đề nghị sửa; D04 mắt phải, cờ điểm và biên phone; D05 open/closed khi liếc xuống và cờ mốc bên xa; gaze_zone ở D02/D04/D05/D06; seatbelt trên S01 RGB tối. Chưa ghi nhận đã hỏi coach hoặc hai người đã đồng thuận.

## Cá nhân

### DinhHuuMinh 2A202603129

- Phần thực hiện: gán nhãn 22 ảnh, tự kiểm mốc mắt/mũi và nôi; sửa seat mặc định, bổ sung nôi S03 RGB, kiểm cờ điểm mặt và sửa phone D03 sau review. Chi tiết quá trình trong prompts.md và self_review.md.
- Bài học từ lượt rà soát: 0 lỗi script không bảo đảm mốc mặt chính xác hoặc đủ nôi; cần phóng to mặt, rà ba vị trí ghế, mở DETAILS ngay sau khi thêm box.
- Trạng thái review: dùng duy nhất export_final_v2.zip trong gói K5-L2-DAY12-TranAnhVan-2A202603058-DMSOMS.zip mới được cung cấp ngày 22/11/2026; cả hai bản đều 0 ERROR, 0 WARN. Đã xóa phone gán nhầm tại D03 trong ZIP/XML, chạy lại kiểm tra; còn 26 dòng ambiguous. Đề xuất sửa bài Văn được bàn giao riêng, chưa có xác nhận đã sửa. Đã xóa phone D03 trên CVAT, Save và kiểm lại sau tải lại trang ngày 22/11/2026.
