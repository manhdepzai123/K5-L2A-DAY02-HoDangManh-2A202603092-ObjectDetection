# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Hồ Đăng Mạnh
- MSSV:2A202603092
- Email: mho60471@gmail.com
- Lớp: L2A
- Link repo bài làm: https://github.com/manhdepzai123/K5-L2A-DAY02-HoDangManh-2A202603092-ObjectDetection.git
- Link Colab đã chạy:https://colab.research.google.com/drive/15sfMHARVWwSZ-i7HcKPgMlZPYZT3KtjP?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

Schema sử dụng trong bài:
0 = car: xe con, sedan, SUV, taxi, xe bán tải.
1 = truck: xe tải chở hàng, xe đầu kéo.
2 = bus: xe buýt, minibus chở khách.
3 = van: xe van, minivan chở hàng hoặc chở người.

Quy tắc chung: mỗi xe hợp lệ được gán một bounding box riêng. Box bám sát phần thân xe nhìn thấy, không lấy thêm mặt đường, trời hoặc xe bên cạnh. Xe bị che khuất vẫn được gán nếu còn đủ thông tin để nhận diện lớp. Xe bị cắt bởi rìa ảnh được vẽ box đến đúng rìa ảnh, không suy đoán phần nằm ngoài ảnh. Với vật thể quá nhỏ hoặc không xác định chắc lớp, không tự đoán.

Ba ca mơ hồ đã ghi nhận:
drive_008.jpg: một số xe ở xa, kích thước rất nhỏ nên khó xác định chắc lớp. Quyết định chỉ gán khi phần nhìn thấy còn đủ để nhận diện; trường hợp quá nhỏ/mơ hồ thì không tự đoán.
drive_009.jpg: nhiều xe buýt và xe khác chồng lấp, che khuất nhau. Quyết định vẽ box riêng cho từng xe trên phần nhìn thấy được, không gộp nhiều xe vào một box.
drive_012.jpg: có xe bị cắt bởi rìa ảnh. Quyết định vẽ box đến đúng rìa ảnh trên phần xe còn nhìn thấy, không suy đoán phần ngoài ảnh.

Phần cần giảng viên làm rõ khi gặp trường hợp khó là các xe quá nhỏ hoặc các trường hợp kiểu van/minivan khó phân biệt chắc chắn với bus hoặc car.

## 3. Annotation và self-QC

- Số file nhãn train:
- Kết quả checker và lỗi đã xử lý:
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
- Commit khóa nhãn ban đầu:

  Số file nhãn train: 50 file YOLO.
Kết quả checker: 50/50 file nhãn hợp lệ.
Danh sách 10 ảnh đã rà bằng mắt:
drive_001.jpg
drive_006.jpg
drive_011.jpg
drive_016.jpg
drive_021.jpg
drive_026.jpg
drive_031.jpg
drive_036.jpg
drive_041.jpg
drive_046.jpg

Khi self-QC ban đầu, tôi kiểm tra các lỗi bỏ sót xe, sai class, box quá rộng/quá hẹp và trường hợp gộp nhiều xe vào một box. Ở lần kiểm này tôi không phát hiện lỗi cần sửa.
Sau Gold QC, tôi tiếp tục kiểm tra và chỉnh sửa annotation trên CVAT, sau đó export lại định dạng Ultralytics YOLO Detection, thay thế bộ nhãn và chạy lại checker/evaluator.
Commit khóa nhãn ban đầu: 2780ac5.

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa |
|---|---:|---:|
| TP | 418 | 430 |
| FP | 360 | 411 |
| FN | 235 | 223 |
| Precision | 0.5373 | 0.5113 |
| Recall | 0.6401 | 0.6585 |
| F1 | 0.5842 | 0.5756 |
| mean_iou_of_matches | 0.8454 | 0.8430 |
| passed | False | False |

Kết quả chi tiết được lưu tại [`outputs/annotation_evaluation.json`](../../outputs/annotation_evaluation.json).
Sau lần Gold QC đầu tiên, tôi mở lại CVAT để kiểm tra các annotation của mình, tập trung vào các ảnh có chênh lệch số lượng box, các trường hợp bỏ sót, box thừa, sai class hoặc vật thể khó quan sát. Tôi chỉ chỉnh sửa annotation của mình, không thay đổi gold labels.
Sau khi rework, tôi export lại bộ nhãn theo định dạng Ultralytics YOLO Detection, thay thế 50 file nhãn trong `dataset/labels/train`, chạy lại checker và evaluator.
Kết quả sau rework cho thấy số true positive tăng từ 418 lên 430 và false negative giảm từ 235 xuống 223. Mean IoU của các box match vẫn ở mức cao, từ 0.8454 xuống 0.8430.
Precision sau rework là 0.5113. Theo hướng dẫn trực tiếp của giảng viên trong buổi học, Precision > 0.5 được chấp nhận để tiếp tục bước train/eval.
Trường `passed` trong `annotation_evaluation.json` vẫn là `False` vì script đang sử dụng gate mặc định của bài. Tôi ghi nhận kết quả này và tiếp tục bước train/eval theo hướng dẫn trực tiếp của giảng viên.

## 5. Kết quả YOLO

Runtime: Google Colab.
Thiết bị trong lần chạy: CPU — Intel Xeon CPU @ 2.20 GHz.
Python: 3.13.18.
Ultralytics: 8.4.208.
Torch: 2.14.1+cpu.
Các tham số thay đổi so với notebook: Không thay đổi; giữ nguyên cấu hình notebook của bài.
Best checkpoint trong phiên chạy: /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.4524 |
| Recall |0.2464 |
| mAP50 |0.2760 |
| mAP50–95 |0.2022 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.
Kết quả theo từng lớp:

| Lớp | Precision | Recall | mAP50 | mAP50–95 |
|---|---:|---:|---:|---:|
| car | 0.714 | 0.702 | 0.737 | 0.514 |
| truck | 0.903 | 0.200 | 0.274 | 0.233 |
| bus | 0.000 | 0.000 | 0.0165 | 0.0099 |
| van | 0.193 | 0.0833 | 0.0759 | 0.0525 |
Tài sản kết quả:

- [Eval predictions](assets/eval_predictions/)
- [YOLO26 runs](assets/yolo26_runs/)

Nhận xét: `car` là lớp model nhận diện tốt nhất với Recall = 0.702 và mAP50 = 0.737. `truck` có Precision cao 0.903 nhưng Recall chỉ 0.200, cho thấy model còn bỏ sót nhiều đối tượng truck. `bus` và `van` có kết quả thấp rõ rệt. Khi phân tích cần xem xét cả độ phủ dữ liệu, số mẫu của từng lớp và chất lượng annotation thay vì chỉ kết luận từ một chỉ số.

Ảnh minh họa cụ thể: [Ảnh minh họa: drive_057.jpg – trong ảnh có một xe buýt hai tầng lớn ở khu vực trung tâm nhưng model không phát hiện đúng đối tượng này dưới lớp bus. Trường hợp này phù hợp với Recall của lớp bus bằng 0.000 trên tập eval.].
Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
Lớp có recall thấp nhất là `bus` với Recall = 0.000. Điều này cho thấy trong tập eval, model không phát hiện đúng được các instance bus theo tiêu chí đánh giá.
Ảnh minh họa: [drive_057.jpg – trong ảnh có một xe buýt hai tầng lớn ở khu vực trung tâm nhưng model không phát hiện đúng đối tượng này dưới lớp bus.].
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
Cần kiểm tra độ chính xác vị trí của bounding box, đặc biệt các cạnh box có bám sát vật thể hay không. Một box có thể đạt IoU đủ ở ngưỡng 0.50 nhưng không đạt ở các ngưỡng IoU cao hơn nếu box còn rộng, lệch hoặc chưa bám sát vật thể. Vì vậy cần kiểm tra tightness và vị trí của box.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
Từ các metrics tổng hợp hiện tại chưa đủ để khẳng định chắc chắn một cặp class nào đang bị nhầm với nhau; cần đối chiếu thêm confusion matrix và prediction cụ thể.
Tuy nhiên, theo guideline, các trường hợp `van/minivan` có thể mơ hồ với `bus` hoặc `car`, nên nếu confusion matrix hoặc prediction cho thấy các lỗi `van ↔ bus` hoặc `van ↔ car` thì đây là nhóm lỗi có khả năng liên quan đến ranh giới định nghĩa lớp.
Dữ kiện hỗ trợ việc cần kiểm tra thêm là kết quả của `bus` và `van` thấp hơn nhiều so với `car`: bus có Recall = 0.000, mAP50 = 0.0165; van có Recall = 0.0833, mAP50 = 0.0759.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
Không dùng 10 ảnh eval để train vì cần giữ chúng tách khỏi dữ liệu huấn luyện. Trong notebook, 10 ảnh eval được dùng làm validation trong quá trình train và để đánh giá/chọn checkpoint `best.pt`. Nếu đưa các ảnh này vào train thì kết quả validation không còn phản ánh khả năng của model trên dữ liệu chưa được dùng trực tiếp để tối ưu trọng số.


## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Chạy checker bằng PowerShell | Ban đầu nhập cả phần `PS C:\...>` như một lệnh nên PowerShell báo lỗi | Chỉ nhập phần lệnh sau dấu nhắc PowerShell | Chạy checker thành công, 50/50 file hợp lệ |
| Gold QC lần đầu | Precision/Recall/F1 chưa cao; evaluator báo chưa đạt gate mặc định | Mở lại CVAT, kiểm tra và chỉnh sửa annotation, export YOLO lại, chạy checker/evaluator lại | Kết quả cuối Precision = 0.5113; theo hướng dẫn giảng viên Precision > 0.5 được chấp nhận |
| Train/eval YOLO | Model nhận diện `car` tốt hơn rõ rệt so với `bus` và `van` | Giữ nguyên split và cấu hình notebook, đánh giá trên 10 ảnh eval | Model hoàn thành train/eval; cần phân tích class imbalance và prediction cụ thể |
| Hoàn thiện báo cáo | Cần tổng hợp nhiều loại metrics khác nhau | Tách riêng Gold QC annotation và model evaluation | Đã có số liệu chính; còn bổ sung tên ảnh eval minh họa và TP/FP/FN |


## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

Các phần đã có kết quả:

- Đã hoàn thành mini guideline với bốn lớp và ba ca mơ hồ.
- Đã annotation đủ 50 ảnh train và export 50 file YOLO.
- Checker đạt 50/50 file.
- Đã self-QC bằng mắt ít nhất 10 ảnh.
- Đã giữ commit khóa nhãn ban đầu.
- Đã chạy Gold QC và rework annotation.
- Đã lưu `outputs/annotation_evaluation.json`.
- Đã chạy notebook train/eval YOLO.
- Đã có Precision, Recall, mAP50, mAP50–95 và kết quả theo từng lớp.
- Đã có link Colab.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.


Công cụ AI đã sử dụng:

- Tôi tự thực hiện annotation trên CVAT, tự chạy checker/evaluator, tự chạy notebook train/eval và tự kiểm tra lại các kết quả trước khi đưa vào báo cáo.

Tài liệu tham khảo chính:

- `README.md`
- `RULES.md`
- `CHECKPOINTS.md`
- `GUIDE.md`
- `GUIDELINE_MINI.md`
- `RUBRIC.md`
- `SUBMISSION.md`
- Notebook `day2_annotation_to_yolo.ipynb`
