# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Võ Nguyễn Thành Phát
- MSSV: 2A202603195
- Email: vonguyenthanhphat2905@gmail.com
- Lớp: L2A
- Link repo bài làm: https://github.com/BinBin2905/K5-L2A-Day02-VoNguyenThanhPhat-2A202603195-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1Bp9O_Z_agXHW0noFb_H2djP0DSoAv3-j?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: Kết quả chạy tool eval thấy báo chưa đạt,cụ thể là F1 ~ 0.02, check lại thì thấy ID class của bài làm và gold khác nhau. Xử lý bằng cách xóa các class trên cvat và tạo lại theo đúng thứ tự, sau đó import lại dataset bị fail trước đó về để cập nhật dữ liệu -> F1 > 0.5
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: rà tất cả các ảnh
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
- Commit khóa nhãn ban đầu: 5519721520d49de3aaed37b80829f42cf341f0ab

## 4. Gold QC và rework

- các chỉ số hiện tại sau khi sửa thứ tự class

| Chỉ số              | Trước sửa | Sau sửa nếu có |
| ------------------- | --------: | -------------: |
| TP                  |       486 |                |
| FP                  |       637 |                |
| FN                  |       167 |                |
| Precision           |    0.4328 |                |
| Recall              |    0.7443 |                |
| F1                  |    0.5473 |                |
| mean_iou_of_matches |     0.853 |                |
| passed              |     false |                |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
  - Ultralytics :v.0.0.8
  - GPU : T4 GPU
  - Runtime : Python3
  - Torch: Latest
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy: report\individual\assets\yolo26_runs\vehicle_annotation\weights\best.pt

| Chỉ số trên 10 ảnh eval |             Giá trị |
| ----------------------- | ------------------: |
| Precision               |  0.4492617396851951 |
| Recall                  | 0.24636524822695036 |
| mAP50                   |  0.2719686566170884 |
| mAP50–95                | 0.20411088940950894 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?

- Lớp bus có recall thấp nhất: 0.000, tức bắt được 0/2 xe buýt GT. Tiếp theo là van (0.083, khoảng 1/12) và truck (0.200, 1/5). Recall trung bình 0.246 bị kéo xuống bởi ba lớp này

- Ảnh minh họa: drive_052 và drive_053, nơi có xe buýt lớn, rõ ràng (box trắng). Model có dự đoán bus ở đó (drive_053 có nhãn bus 0.83), nhưng recall bus vẫn bằng 0

2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?

- mAP50 chỉ cần IoU ≥ 0.5, còn mAP50–95 lấy trung bình đến IoU 0.95. Nếu hai số cách xa nhau thì model tìm đúng xe nhưng box không đủ sát
- Cần kiểm tra Box lỏng hoặc chặt, xe bị che, xe bị cắt rìa ảnh, xe nhỏ và xa, xe mờ

3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?

- van: recall 0.083 và precision 0.201, đều rất thấp, dù có 12 instance trong 6 ảnh. Đồng thời model gán van với độ tin cậy cao cho vật thể trông giống xe con: drive_051 (van 0.94) và drive_054 (van 0.89). Còn ở drive_056, xe lớn ở góc dưới phải trông giống van hoặc minivan lại bị gọi car 0.78. Hai chiều nhầm ngược nhau cho thấy ranh giới van/car không nhất quán trong nhãn train.

- bus: GT chỉ có 2 nhưng model dự đoán 14 box bus (drive_058 một mình có 6), nên precision bằng 0.

4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

- Không train trên ảnh eval vì sẽ rò rỉ dữ liệu (data leakage). Nếu model đã thấy các ảnh này, metric sẽ đo khả năng nhớ chứ không đo khả năng tổng quát hóa sang ảnh mới. Số liệu sẽ đẹp hơn thực tế và bạn không biết model thật sự tốt hay chỉ học thuộc.

- Đo P, R, mAP50, mAP50-95 tổng và theo từng lớp trên dữ liệu model chưa train. Phát hiện điểm yếu từng lớp , để quay lại sửa nhãn, guideline hoặc bổ sung dữ liệu. Phân tích định tính qua ảnh dự đoán đã lưu (eval_predictions/visuals)

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
| -------------- | ------------ | ----------- | ----------------- |
|                |              |             |                   |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

- Gold-set annotation QC: phần này ở commit hiện tại đang có F1 ~0.5 ( < 0.8 theo yêu cầu đề ra), sẽ rework để cố gắng cải thiện

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

- Công cụ đã dùng: Claude
- Dùng ở bước đọc các output của colab, phân tích suy luận của claude và ghi chép vào báo cáo
