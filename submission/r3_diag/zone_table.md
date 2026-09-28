# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 9 | 1 | 1 | 3 | 7 | BOX_GEOMETRY (1) |
| mid | 7 | 0 | 0 | 3 | 1 | — |
| edge | 3 | 1 | 0 | 2 | 4 | MISSING (1) |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên:
  * Người (L): Hoàn thành xuất sắc ở vùng `mid` (7/7 ref matched, 0 missing, 0 spurious). Vùng `center` có 1 missing và 1 spurious (chủ yếu là lỗi hình học BOX_GEOMETRY ở xe máy/ô tô xa). Vùng `edge` gãy nhiều nhất theo tỷ lệ khi bỏ sót 1/3 ref (33.3% missing, ca Pedestrian bị che sát rìa).
  * Model (M): Gãy nặng nhất ở vùng `center` về số lượng tuyệt đối (thừa 7 box spurious và thiếu 3 box). Ở vùng `edge`, model gãy nặng nhất về tỷ lệ khi thiếu 2/3 ref (66.7% missing) và sinh ra 4 box spurious ngoài rìa.
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame:
  * Nguyên nhân: Vùng `edge` chịu độ méo quang học fisheye cực đại làm biến dạng tỷ lệ khung hình (aspect ratio) của vật thể, đồng thời độ phân giải góc giảm mạnh khiến model pre-trained (vốn huấn luyện trên ảnh phẳng phối cảnh pinhole) không nhận diện được biên dạng hoặc bắt nhầm chi tiết nền thành vật thể. Ở vùng `center`, mật độ phương tiện đông đúc và che khuất lẫn nhau (occlusion) khiến model dễ bị lẫn lộn giữa các xe đi sát nhau.
  * Giới hạn: Slice chỉ gồm 3 frame ADASIND đại diện cho một camera trước duy nhất trong điều kiện ban ngày; chưa thể bao quát được biến dạng ở các góc nhìn khác (camera hông/sau) hoặc các điều kiện ánh sáng phức tạp (ngược sáng/ban đêm) trong hệ thống SVM 4 camera thực tế.

