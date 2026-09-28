# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Vùng rìa thấu kính (Edge zone) / adasind_001320.jpg & adasind_036720.jpg | 4 ca (MISSING do che khuất, ATTRIBUTE nhầm lẫn truncated/occluded, biến dạng méo fisheye) | Vùng edge zone chịu độ méo quang học fisheye lớn nhất, vật thể bị kéo giãn cong vênh và độ phân giải suy giảm mạnh, khiến cả annotator lẫn mô hình AI dễ bỏ sót người đi bộ hoặc vẽ box sai ranh giới thực tế | Tọa độ box trên ảnh gốc fisheye, thuộc tính truncated=true đối với các đối tượng tiếp giáp viền tròn quang học lens_border, và hình ảnh so sánh với model compare |
| Cụm phương tiện mật độ dày (Center Dense cluster) / adasind_012570.jpg | 8 ca (BOX_GEOMETRY ở xe máy/ô tô xa, SPURIOUS false positives của model) | Mật độ phương tiện hỗn hợp (xe ba bánh, xe máy, ô tô, người đi bộ) đan xen ở trung tâm dẫn đến tình trạng che khuất (occlusion) nghiêm trọng, dễ gây mâu thuẫn gán nhãn rider/bike và làm model sinh ra nhiều box ảo | Bảng ma trận nhầm lẫn confusion matrix, chỉ số IoU phân tầng theo từng ngưỡng từ 0.3 đến 0.7, và ảnh crop chi tiết các đối tượng bị che khuất |

Giới hạn của kết luận từ ba frame ADASIND: Tập dữ liệu slice B1-dense chỉ gồm 3 frame từ một camera mắt cá phía trước đơn lẻ trong điều kiện ban ngày đô thị. Kết luận rút ra không thể khái quát hóa cho toàn bộ hệ thống SVM 360 độ gồm 4 camera (trước, sau, trái, phải) trong các điều kiện thời tiết khắc nghiệt (mưa, ban đêm, ngược sáng), cũng như không thể phản ánh được các lỗi liên camera như ghép nối vùng chồng (seams) hay đồng bộ thời gian.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh
như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:
1. Kiểm soát tính độc lập không gian - thời gian: Khi lấy mẫu 200 frame từ 50.000 frame giả lập, phải áp dụng khoảng cách lấy mẫu tối thiểu (stride ≥ 30-50 frame, tương đương cách nhau ít nhất 3-5 giây di chuyển) để tránh lấy các frame liên tiếp trong cùng một chuỗi cảnh, đảm bảo 200 frame là 200 tình huống giao thông độc lập thực sự.
2. Phân tầng theo metadata điều kiện vận hành (ODD): Đảm bảo độ phủ đủ 4 camera (front, rear, left, right) với tỷ lệ cân đối giữa normal slice (thiết lập baseline) và hard slice (tập trung ca rủi ro: chói sáng, bám bẩn ống kính, xe máy cắt góc vùng seam, người đi bộ trong điểm mù).
3. Giới hạn đo lường: Kế hoạch lấy mẫu 200 frame này là phương pháp kiểm toán mục tiêu (targeted risk-based sampling) nhằm phát hiện các ca lỗi nguy hiểm và khoảng trống trong guideline, không thể dùng trực tiếp để suy luận tỷ lệ lỗi tổng thể (Error Rate) của toàn bộ 50.000 frame vì việc chọn mẫu ưu tiên ca khó đã làm sai lệch phân phối ngẫu nhiên tự nhiên của tập dữ liệu gốc.

