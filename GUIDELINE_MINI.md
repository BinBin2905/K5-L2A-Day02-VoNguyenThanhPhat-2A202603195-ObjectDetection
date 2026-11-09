# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp     | Định nghĩa của bạn                                                                                           | Ví dụ/điểm phân biệt                                                                                                      |
| ------- | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| `car`   | Xe con chở người, thường nhỏ và có 4-7 chỗ                                                                   | Có khoang động cơ riêng và mui thấp. Khác van ở chỗ không có thân hộp cao liền khối, khác truck ở chỗ không có thùng hàng |
| `truck` | Xe tải/xe chuyên dụng chở hàng: có cabin riêng và thùng hàng hoặc kết cấu chuyên dụng phía sau               | Đặc trưng là cabin tách khỏi thùng hàng                                                                                   |
| `bus`   | Xe khách/buýt cỡ lớn, chở nhiều người: buýt thành phố, xe khách đường dài, xe đưa đón cỡ lớn, buýt hai tầng. | Thân dài, cao, nhiều cửa sổ dọc thân, thường có số tuyến hoặc biển điểm đến.                                              |
| `van`   | Xe thân hộp liền khối, cỡ giữa xe con và xe buýt                                                             | Cabin và khoang hàng/khách liền một khối, thường không có đầu xe nhô ra rõ.                                               |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Là hình chữ nhật nhỏ nhất ôm sát thân xe(bánh xe, gương chiếu hậu, phần nhô của rơ móc nếu là một cụm). Phần bóng đổ, phản chiếu, ăng ten không tính vào box

- Vật thể bị che khuất: Gán nhãn khi còn thấy khoảng 30–40% thân xe trở lên và nhận ra được lớp. Box chỉ bao phần nhìn thấy, không đoán phần bị che. Nếu bị che quá nhiều hoặc không phân biệt được lớp thì bỏ qua, không gán

- Vật thể bị cắt bởi rìa ảnh: Gán nhãn nếu phần còn lại đủ để xác định lớp. Box kéo đến sát rìa ảnh, không vượt ra ngoài. Phần bị cắt quá nhỏ (chỉ thấy một góc, một bánh) thì bỏ

- Vật thể rất nhỏ/mơ hồ: Nếu vật thể đủ để nhận thấy được thì gán nhãn. Không đoán bừa, ca khó thì ghi vào bảng bên dưới.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh         | Tình huống                                                                                                                                                                 | Quyết định cuối | Lý do theo schema                                                                                     |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- | ----------------------------------------------------------------------------------------------------- |
| `drive_001` | xe trong ảnh nhỏ, góc trên bên phải, tầm 11 pixel không phân biệt được van hay car                                                                                         | car             | Thân liền khối, mui cao, không có khoang động cơ nhô rõ. Khá khớp với car và van, chưa phân biệt được |
| `drive_002` | từ bên trái đếm đến lane thứ 5 có chiếc xe bị che khuất màu trắng, khó phân biệt giữa van và car                                                                           | van             | có thân liền khối, mui cao.                                                                           |
| `drive_003` | một chiếc nằm ngay cạnh bên phải ảnh kế bên đuôi xe bus, chưa phân biệt được truck hay van, vì bị che khuất phần sau nên không phân biệt được cabin tách rời hay liền thân | truck           | có cabin phía sau xe                                                                                  |
| `drive_006` | một chiếc xe ở góc trên trái của ảnh, nhìn khá giống bus, vì ảnh khá mờ nên chưa quyết định được                                                                           | bus             | Thân dài, cao                                                                                         |
| `drive_013` | một chiếc nằm góc trêb bên phải nhìn khá giống van nhưng bị khuất phần sau nên chưa phân biệt được                                                                         | van             | Mui cao                                                                                               |
