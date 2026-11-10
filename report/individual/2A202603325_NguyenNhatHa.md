# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Nguyen Nhat Ha
- MSSV: 2A202603325
- Email: hgnguyen47347714@gmail.com
- Lớp: 2A
- Link repo bài làm: https://github.com/hanguyen1406/K5-L2B-DAY02-NguyenNhatHa-2A202603325-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1N_CQZOITOo8GX0WnrmSA02L65l5sTrCC#scrollTo=3e0508d8

## 2. Schema và mini guideline

Chi tiết nằm ở [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Schema áp dụng đúng thứ tự
class trong `data.yaml`: `car=0`, `truck=1`, `bus=2`, `van=3`.

- `car`: xe con, sedan, SUV, taxi hoặc xe bán tải/cabin nhỏ.
- `truck`: xe tải chở hàng hoặc xe đầu kéo có thùng/cabin hàng hóa rõ ràng.
- `bus`: xe buýt hoặc minibus có khoang chở nhiều hành khách, thân cao và nhiều cửa sổ.
- `van`: xe van/minivan chở người hoặc hàng, không có đặc trưng của xe buýt.

Box ôm sát phần vật thể nhìn thấy, mỗi xe một box; xe bị che vẫn gán nếu còn đủ
thông tin nhận diện và chỉ vẽ phần nhìn thấy. Xe bị cắt bởi rìa ảnh được gán nếu
phần còn lại đủ nhận diện và box dừng tại biên ảnh. Vật thể quá nhỏ hoặc không
đủ cơ sở phân biệt class thì không tự đoán, ghi lại để hỏi giảng viên.

Ba ca mơ hồ đã quyết định:

1. `drive_002`: xe thân cao, nhiều cửa sổ được gán `bus` (class 2), vì có khoang
   hành khách lớn và đặc trưng xe buýt, không phải van.
2. `drive_005`: xe màu xanh dạng thùng kín được gán `truck` (class 1), vì có
   thùng hàng lớn, vuông và tách biệt với cabin.
3. `drive_009`: xe buýt đỏ bị che một phần vẫn được gán `bus` (class 2), vì
   vẫn nhận ra thân dài, nhiều cửa sổ và khoang hành khách; box chỉ theo phần thấy.

## 3. Annotation và self-QC

- Số file nhãn train: 50/50, cùng basename với 50 ảnh train.
- Kết quả checker: **ĐẠT cấu trúc: 50/50 file nhãn hợp lệ** bằng
  `python tools/check_yolo_labels.py --labels dataset/labels/train --images dataset/images/train`.
  Checker không phát hiện lỗi định dạng, class ID, normalized coordinates hoặc box vượt biên.
- Danh sách ảnh cần đối chiếu nhật ký rà bằng mắt: `drive_001`–`drive_010`.
  Repo chưa lưu xác nhận riêng rằng bước rà bằng mắt đã hoàn tất.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: chưa có nhật ký self-QC trong repo.
- Commit khóa nhãn ban đầu: `210ce314958f3ad2b16660e342c2b9c40e28d312`
  (`upload train label`, 10/11/2026 12:23 UTC+7).

## 4. Gold QC và rework

Kết quả đầy đủ nằm ở [outputs/annotation_evaluation.json](../../outputs/annotation_evaluation.json).
Đây là kết quả đang có trong repo; không có kết quả “trước/sau rework” thứ hai.

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 354 | — |
| FP | 290 | — |
| FN | 299 | — |
| Precision | 0.5497 | — |
| Recall | 0.5421 | — |
| F1 | 0.5459 | — |
| mean_iou_of_matches | 0.8483 | — |
| passed | true* | — |

`passed=true` theo evaluator với ngưỡng `F1 >= 0.50` và `mean IoU >= 0.70`.
Kết quả đạt cả hai ngưỡng của evaluator. Không suy ra nguyên nhân cụ thể chỉ từ
FP/FN; nếu có rework thì cần kiểm tra trực tiếp ảnh và box tương ứng.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: đã chạy notebook Colab với
  checkpoint `yolo26n.pt`; log runtime/phiên bản chi tiết không được lưu trong
  output tải về.
- Các tham số theo notebook: `yolo26n.pt`, 50 epochs, `imgsz=640`, `batch=-1`,
  seed `20260909`; notebook dùng GPU nếu có, nếu không dùng CPU. Chưa có bằng
  chứng về tham số đã thay đổi.
- Đường dẫn best checkpoint: notebook tạo `outputs/yolo26_runs/vehicle_annotation/weights/best.pt`;
  checkpoint không được commit và chưa có bản output Colab trong repo.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4071 |
| Recall | 0.2340 |
| mAP50 | 0.2737 |
| mAP50–95 | 0.2018 |

Hai thư mục [assets/eval_predictions/](assets/eval_predictions/) và
[assets/yolo26_runs/](assets/yolo26_runs/) đã được giải nén và bổ sung vào repo.
Ảnh prediction của 10 ảnh eval nằm trong
[assets/eval_predictions/visuals/](assets/eval_predictions/visuals/); log/biểu đồ
train và eval nằm trong [assets/yolo26_runs/](assets/yolo26_runs/).

### Trả lời bốn câu hỏi cuối notebook

1. **Lớp recall thấp nhất:** `truck`, với recall bằng 0 trong confusion matrix
   của lần eval. Ảnh [drive_059.jpg](assets/eval_predictions/visuals/drive_059.jpg)
   minh họa một xe truck nhỏ ở làn đường; model chỉ dự đoán yếu (confidence
   khoảng 0.55), cho thấy khó nhận diện lớp ít mẫu.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, model thường phát hiện đúng vật thể và
   đúng lớp nhưng box chưa bám sát: tọa độ tâm, chiều rộng/chiều cao hoặc các
   cạnh box còn lệch. Cần kiểm tra trực quan các box ở nhiều ngưỡng IoU.
3. Lỗi class có liên quan guideline rõ nhất là `truck` bị nhận thành `car`
   hoặc bị bỏ sót. Confusion matrix trong
   [vehicle_eval/confusion_matrix.png](assets/yolo26_runs/vehicle_eval/confusion_matrix.png)
   cho thấy cột `truck` không có true positive; ảnh `drive_059` cũng cho thấy
   truck nhỏ và khó phân biệt. Điều này phù hợp với guideline phải phân biệt xe
   tải có thùng hàng với car, đồng thời có thể chịu ảnh hưởng class imbalance.
4. Không dùng 10 ảnh eval để train để tránh rò rỉ dữ liệu và đánh giá lạc quan.
   Trong notebook, eval vừa là tập validation để theo dõi/chọn `best.pt`, vừa
   là tập đánh giá cuối; vì vậy không phải test độc lập hoàn toàn.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Gold QC | F1 0.5459 | Đọc JSON evaluator với ngưỡng F1 0.50 và mean IoU 0.70 | Đạt ngưỡng evaluator; mean IoU đạt 0.8483 |
| YOLO train/eval | Cần đưa output Colab vào đúng cấu trúc nộp | Tải `day2_colab_outputs`, loại checkpoint `.pt`, chép hai folder assets | Đã có 10 ảnh prediction, log/biểu đồ train-eval và 4 metric model |

## 7. Tự đánh giá theo rubric

- Định dạng YOLO: đã có 50/50 file và checker đạt.
- Schema/guideline: [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md) đã có định nghĩa
  bốn class, quy tắc box và ba ca mơ hồ có lý do.
- Gold QC: đã có JSON, F1 đạt ngưỡng evaluator 0.50 và mean IoU đạt 0.70.
- Pipeline YOLO: đã có link Colab, 4 metric model, 10 ảnh prediction và hai
  thư mục assets theo yêu cầu.
- Vì các mục trên chưa đủ bằng chứng, chưa tự nhận hoàn thành toàn bộ rubric.

## 8. Công cụ AI và tài liệu tham khảo

- Dùng AI assistant để đọc cấu trúc repo, kiểm tra checker/JSON, đối chiếu
  notebook và hỗ trợ soạn báo cáo; các số liệu trong báo cáo được kiểm tra lại
  từ file trong repo.
- Tài liệu tham khảo nội bộ: [README.md](../../README.md),
  [GUIDE.md](../../GUIDE.md), [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md),
  [RUBRIC.md](../../RUBRIC.md), [SUBMISSION.md](../../SUBMISSION.md) và
  notebook [day2_annotation_to_yolo.ipynb](../../notebooks/day2_annotation_to_yolo.ipynb).
- Không ghi mật khẩu, token, gold train hoặc checkpoint `.pt`.
