# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B1 | BOX_GEOMETRY | 2 |
| center | B1 | MISSING | 4 |
| center | B1 | SPURIOUS | 7 |
| center | C0 | IGNORE_SCOPE | 1 |
| center | C0 | SPURIOUS | 1 |
| edge | B1 | ATTRIBUTE | 2 |
| edge | B1 | MISSING | 4 |
| edge | B1 | SPURIOUS | 4 |
| mid | B1 | BOX_GEOMETRY | 2 |
| mid | B1 | MISSING | 3 |
| mid | B1 | SPURIOUS | 1 |
| mid | C0 | MISSING | 1 |

## Top defects
- SPURIOUS: 13 (ví dụ frame adasind_019560.jpg)
- MISSING: 12 (ví dụ frame adasind_019560.jpg)
- BOX_GEOMETRY: 4 (ví dụ frame adasind_012570.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: Lỗi nổi bật nhất trong bảng thống kê là SPURIOUS của Model AI (M) tại vùng `center` (7 ca) và `edge` (4 ca) trên frame adasind_012570.jpg (các box M7, M8, M10, M11) và adasind_036720.jpg (M1, M2, M4, M6). Nguyên nhân khả dĩ là `E4_model_domain` do mô hình YOLO26m pre-trained được huấn luyện trên ảnh phối cảnh phẳng (pinhole perspective). Khi áp dụng trực tiếp lên ảnh mắt cá fisheye góc rộng (FoV ~180°), sự biến dạng quang học cong méo cùng bóng đổ và chi tiết kiến trúc ven đường khiến mô hình nhận diện nhầm nền thành vật thể (False Positives). Đối với phía Người gán nhãn, lỗi chủ đạo là MISSING do che khuất (occlusion) giữa các phương tiện đi sát nhau (ví dụ Pedestrian R4 bị ThreeWheeler che khuất trên frame adasind_001320.jpg).
- Cách sửa và ai nhận việc (`owner`):
  * Phía Mô hình (`ai_team`): Cần tiến hành domain adaptation, huấn luyện lại (fine-tune) mô hình trên dữ liệu fisheye thực tế có bù méo quang học; áp dụng ngưỡng confidence threshold thích ứng theo từng zone (tăng ngưỡng tin cậy ở vùng `edge` và `center` nhiều nhiễu để triệt tiêu box thừa).
  * Phía Gán nhãn (`annotator`): Tăng cường quy trình zoom-in rà soát kỹ các vùng tiếp giáp giữa các xe lớn để phát hiện người đi bộ hoặc xe đạp bị che khuất một phần; các ca này đã được sửa chữa triệt để ở pha rework P5.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): Ảnh minh chứng tại `submission/screenshots/rework_evidence_001320.jpg`; các dòng findings `r1_craft,B1-dense,adasind_001320.jpg,R4` và `r3_diag,B1-dense,adasind_012570.jpg,M7..M11`; căn cứ theo điều luật R01 (ngưỡng H ≥ 40px) và R02 (gán nhãn bám sát ảnh gốc).

