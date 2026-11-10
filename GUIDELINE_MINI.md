# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con, sedan, SUV, taxi, xe bán tải. | Không gán xe buýt, xe tải, mô tô/xe máy; xe bán tải thuộc `car`. |
| `truck` | Xe tải chở hàng, xe đầu kéo. | Không gán xe bán tải hoặc cabin nhỏ thuộc `car`. |
| `bus` | Xe buýt, minibus chở khách. | Không gán van chở hàng/chở người thuộc `van`. |
| `van` | Xe van, minivan chở hàng hoặc chở người. | Không gán xe buýt lớn thuộc `bus` hoặc xe con thông thường thuộc `car`. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: box ôm sát phần thân xe nhìn thấy được, không lấy thêm mặt đường, trời hay xe bên cạnh.
- Vật thể bị che khuất: vẫn gán box quanh phần nhìn thấy được nếu còn đủ để nhận diện lớp.
- Vật thể bị cắt bởi rìa ảnh: box chạm đúng rìa ảnh tại phần vật thể còn nhìn thấy, không đoán phần nằm ngoài ảnh.
- Vật thể rất nhỏ/mơ hồ: nếu chỉ còn một phần rất nhỏ và không thể xác định chắc lớp thì không tự đoán; ghi lại ca này và hỏi giảng viên.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_008.jpg` | Một số xe ở xa, kích thước rất nhỏ nên khó xác định chắc lớp. | Chỉ gán box khi phần nhìn thấy còn đủ để nhận diện lớp; trường hợp quá nhỏ/mơ hồ thì không tự đoán. | Theo guideline, vật thể rất nhỏ nếu không thể xác định chắc lớp thì không tự đoán và cần ghi lại ca mơ hồ. |
| `drive_009.jpg` | Nhiều xe buýt và xe khác chồng lấp, che khuất nhau trong cảnh đông xe. | Vẽ box riêng cho từng xe trên phần nhìn thấy được, không gộp nhiều xe vào một box. | Theo guideline, hai xe chồng lên nhau vẫn phải vẽ hai box riêng trên phần nhìn thấy của từng xe. |
| `drive_012.jpg` | Có xe bị cắt bởi rìa ảnh, chỉ còn một phần thân xe xuất hiện trong khung hình. | Vẽ box đến đúng rìa ảnh trên phần xe còn nhìn thấy, không đoán phần nằm ngoài ảnh. | Theo guideline, vật thể bị cắt khung thì box chạm đúng rìa ảnh và không suy đoán phần ngoài ảnh.|
