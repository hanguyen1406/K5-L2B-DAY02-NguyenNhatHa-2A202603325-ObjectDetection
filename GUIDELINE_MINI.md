# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con, sedan, SUV, taxi hoặc xe bán tải/cabin nhỏ dùng chủ yếu để chở người hoặc hàng nhẹ. | Xe con và taxi vẫn là `car`, kể cả khi đang đỗ hoặc bị nhìn từ phía sau. |
| `truck` | Xe tải chở hàng hoặc xe đầu kéo, có phần thùng/cabin hàng hóa rõ ràng. | Xe có thùng hàng lớn là `truck`; không dùng `truck` cho xe bán tải nhỏ. |
| `bus` | Xe buýt hoặc minibus có khoang chở nhiều hành khách, thường có thân xe cao và nhiều cửa sổ. | Xe buýt nội đô, xe buýt đường dài và xe khớp nối đều là `bus`. |
| `van` | Xe van hoặc minivan chở người/hàng, thân kín và lớn hơn xe con nhưng không có đặc trưng xe buýt. | Van chở hàng hoặc minivan là `van`; không đổi thành `bus` chỉ vì thân xe cao. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: ôm sát toàn bộ phần vật thể nhìn thấy, không lấy thêm
  mặt đường, hàng rào hoặc khoảng trống; mỗi xe chỉ có một box.
- Vật thể bị che khuất: vẫn gán nếu phần nhìn thấy đủ để nhận ra là xe và phân biệt
  được class; box chỉ bao quanh phần nhìn thấy, không đoán phần bị che.
- Vật thể bị cắt bởi rìa ảnh: vẫn gán nếu phần còn lại đủ nhận diện; box được cắt
  đúng tại biên ảnh.
- Vật thể rất nhỏ/mơ hồ: chỉ gán khi còn thấy được hình dạng xe và có cơ sở chọn
  class; nếu không thể phân biệt đáng tin cậy thì không tự đoán class và ghi lại để
  hỏi giảng viên.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_002` | Ở giữa ảnh có một xe thân cao, nhiều cửa sổ, kích thước lớn; dễ nhầm với van. | Gán `bus` (class 2). | Có khoang hành khách lớn, thân cao và nhiều cửa sổ; theo schema đây là xe buýt, không phải van chở hàng/minivan. |
| `drive_005` | Có xe màu xanh lá dạng thùng kín giữa nhiều xe buýt; phần thùng có thể khiến nhầm với van. | Gán `truck` (class 1). | Thùng hàng vuông, lớn và tách biệt với cabin cho thấy đây là xe tải chở hàng; van không có thùng hàng lớn như vậy. |
| `drive_009` | Xe buýt màu đỏ ở bên trái bị các xe khác che một phần thân và nằm sát vùng đông xe. | Gán `bus` (class 2), box theo phần nhìn thấy. | Vẫn nhận ra thân xe dài, nhiều cửa sổ và khoang hành khách; che khuất không làm đổi loại vật thể, chỉ không vẽ box cho phần bị che. |
