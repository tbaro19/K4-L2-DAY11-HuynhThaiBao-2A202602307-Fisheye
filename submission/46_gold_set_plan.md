# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Chói sáng mạnh (ngược nắng trực tiếp/đèn pha ban đêm), người đi bộ băng cắt nhanh ở rìa góc nhìn (edge zone), thời tiết mưa bão làm mờ kính | Tốc độ tương đối lớn gây motion blur; độ phân giải suy giảm ở rìa fisheye làm lẫn Pedestrian với biển báo/cột đèn; lóa sáng làm mất tương phản biên vật thể | Giữ nguyên ảnh raw fisheye 2D (không undistort); lưu trữ ma trận nội tại intrinsic (focal length, distortion polynomial) và ngoại tại extrinsic (tọa độ gắn camera trước so với tâm xe) | 2 annotator cấp cao gán nhãn độc lập (double-blind); đo IoU ≥ 0.85; các ca bất đồng (conflict) bắt buộc do Lead Reviewer thẩm định trên ảnh gốc kết hợp dữ liệu cảm biến trước khi chốt Gold |
| rear | Ống kính bám giọt nước/bụi bẩn cản trở quang học, lùi xe ban đêm thiếu sáng, vật cản thấp sát cản sau (trẻ em, gờ bê tông, cọc tiêu thấp) | Góc khuất cản sau và bóng tối gầm xe làm mất biên dạng đáy vật thể; vệt nước/bụi gây khúc xạ tạo false positives hoặc bỏ sót vật thể thấp nguy hiểm | Giữ nguyên ảnh raw fisheye 2D; bảo toàn thông số extrinsic độ cao và góc chúc (pitch/roll/yaw) của camera lùi để phục vụ tính khoảng cách an toàn xuống mặt phẳng đường | Review chéo độc lập 2 vòng; soi kỹ từng pixel vùng tiếp giáp thân xe (`ego_body`) và vùng sát mặt đất; đối chiếu cảm biến đỗ xe/siêu âm để xác thực ground truth |
| left | Xe máy/xe đạp vượt sát sườn ở tốc độ tương đối cao, vật thể nằm đúng góc chuyển tiếp seam (front-left seam hoặc rear-left seam), bị cắt cụt bởi vành tròn quang học | Độ méo thấu kính fisheye ở rìa trái cực đại làm aspect ratio của xe máy bị kéo giãn cong vênh; người lái và phương tiện dễ bị tách nhầm thành 2 box hoặc gộp sai | Giữ nguyên ảnh raw fisheye 2D; bảo toàn calibration extrinsic góc mở ngang và độ xoay của camera gương chiếu hậu trái phục vụ thuật toán stitching và biến đổi sang Bird's-Eye View (BEV) | Review mù độc lập; kiểm tra kỹ tính nhất quán nhãn rider/bike; đối chiếu đồng thời với frame cùng timestamp của camera trước/sau ở vùng seam để đảm bảo nhãn không bị mâu thuẫn hình học |
| right | Điểm mù góc phụ bên phải rộng, người đi bộ bước từ vỉa hè xuống sát mép đường, xe ô tô rẽ phải cắt đầu, vạch ô đỗ bên phải bị che khuất | Tương tác phức tạp giữa mép đường, vỉa hè và chướng ngại vật tĩnh; góc nhìn chúc và méo quang học làm sai lệch ước lượng ranh giới mặt đường và độ cao vật thể | Giữ nguyên ảnh raw fisheye 2D; bảo toàn calibration extrinsic góc mở và ma trận biến đổi tọa độ camera phải sang hệ tọa độ xe (Vehicle Coordinate System - VCS) | Review chéo mù 2 vòng độc lập; kiểm tra kỹ ranh giới vạch ô đỗ và chướng ngại vật tĩnh sát lề; hội đồng kỹ thuật (Adjudication) phân xử các ca nghi ngờ |

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule):
  1. Khi thay đổi phần cứng camera: Đổi cảm biến, thay đổi độ phân giải (ví dụ từ 1.3MP lên 2MP), thay ống kính fisheye khác FoV hoặc thay đổi vị trí/góc gắn camera trên thân xe.
  2. Khi hiệu chuẩn lại hệ thống (Recalibration): Khi hệ số méo quang học (intrinsic) hoặc ma trận vị trí (extrinsic) bị dịch chuyển sau bảo trì/va chạm, làm thay đổi ánh xạ tọa độ sang mặt phẳng BEV.
  3. Khi cập nhật quy chuẩn nhãn (Guideline Revision): Khi taxonomy thay đổi (ví dụ thay đổi định nghĩa class Bike/Rider, thay đổi ngưỡng chiều cao pixel tối thiểu H=40, hoặc ban hành chính sách mới về xử lý vùng seam).
  4. Khi phát hiện trôi dạt miền dữ liệu (Data Drift): Xuất hiện điều kiện thời tiết mới (mưa tuyết, sương mù dày) hoặc bối cảnh hạ tầng giao thông mới chưa có trong tập gold cũ.

- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box:
  * Ví dụ ca thực tế: Một người đi xe máy đang vượt lên tại góc trước-trái của xe ego, xuất hiện đồng thời trong vùng nhìn chồng lấn (seam) giữa camera trước (Front) và camera gương trái (Left).
  * Hiện tượng: Trên camera Front, xe máy xuất hiện ở vùng méo rìa trái (`edge` zone) với góc nhìn từ phía sau; trên camera Left, xe máy xuất hiện ở vùng rìa phải (`edge` zone) với góc nhìn từ bên sườn.
  * Policy & Evidence bắt buộc trước khi ghép/xử lý:
    1. Bằng chứng đồng bộ thời gian (Timestamp sync): 2 frame từ 2 camera phải có timestamp chênh lệch dưới ngưỡng cho phép (ví dụ < 5ms) bằng hardware trigger để loại trừ sai lệch do vật thể di chuyển.
    2. Bằng chứng hình học (Extrinsic Calibration): Cần ma trận chuyển đổi không gian giữa Front và Left camera để xác thực rằng 2 ray chiếu từ 2 optical center giao nhau tại cùng một tọa độ 3D trên mặt đường.
    3. Quy định đích (Output Policy): Cần phân định rõ mục tiêu của tầng nhận thức (Perception). Nếu hệ thống yêu cầu 2D Bounding Box riêng cho từng camera đầu vào, thì cả 2 box đều là hợp lệ độc lập và KHÔNG ĐƯỢC xóa coi là duplicate. Nếu hệ thống yêu cầu một biểu diễn 3D Bounding Box duy nhất trên không gian BEV/3D, việc hợp nhất (fusion) phải được thực hiện bằng thuật toán tracking 3D, tuyệt đối không để annotator gán nhãn 2D tự ý xóa box của một bên camera bằng cảm tính.

- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:
  1. Giới hạn không gian đơn lẻ: Bộ dữ liệu 1 camera (như ADASIND) chỉ đánh giá được chất lượng gán nhãn 2D cục bộ trên mặt phẳng chiếu méo của duy nhất một thấu kính; nó hoàn toàn không phản ánh được sai số tại các vùng chồng lấn (seams) giữa các camera quanh xe.
  2. Bỏ qua sai số đồng bộ thời gian (Temporal Synchronization): Hệ thống 4 camera thực tế luôn tiềm ẩn độ trễ rolling shutter hoặc lệch khung hình mili-giây, điều mà ảnh 1 camera đơn lập không thể phát hiện hay mô phỏng.
  3. Không kiểm tra được tính nhất quán phối cảnh 3D/BEV: Hai người cùng đồng thuận (high peer agreement) trên một box 2D của camera trước không đảm bảo rằng khi kết hợp với camera hông, vị trí vật thể trên bản đồ toàn cảnh 360 độ (Bird's-Eye View) sẽ ăn khớp và không bị nhân đôi (ghost objects) hay gãy khúc biên dạng.
  4. Khác biệt về điều kiện môi trường quang học giữa các vị trí: Camera sau và hông xe chịu ảnh hưởng bụi bẩn, vệt nước mưa và ánh sáng đèn chiếu hông hoàn toàn khác với camera trước. Đạt độ chính xác cao trên camera trước không đồng nghĩa mô hình hoặc quy chuẩn gán nhãn sẽ hoạt động tốt trên 3 camera còn lại.

