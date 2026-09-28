# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao?
   Không thể mặc định coi là lỗi `DUPLICATE`, mà bắt buộc phải có quy tắc xử lý riêng (Cross-Camera Seam Policy). Lý do: Tại vùng chồng lấn (seam) giữa hai camera fisheye (ví dụ camera Trước và camera Gương trái), một vật thể vật lý thực sự xuất hiện đồng thời trong trường nhìn quang học của cả hai thấu kính ở hai góc nhìn phối cảnh khác nhau (trên camera trước là góc nhìn từ sau, trên camera hông là góc nhìn từ sườn). Nếu tầng nhận thức 2D độc lập của từng camera cần phát hiện vật thể thì cả 2 box đều là hợp lệ và có giá trị sử dụng. Việc coi là DUPLICATE chỉ xảy ra nếu người gán nhãn vẽ 2 box đè lên nhau trên CÙNG MỘT camera. Khi chuyển sang biểu diễn hợp nhất 3D Bounding Box hoặc Bird's-Eye View (BEV), hệ thống Sensor Fusion sẽ chịu trách nhiệm hợp nhất 2 góc nhìn này dựa trên ma trận hiệu chuẩn (Extrinsic Calibration) và đồng bộ thời gian (Timestamp Sync). Annotator không được tự ý xóa một trong hai box trên ảnh 2D gốc.

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.
   - Giữ cùng Track ID: Khi vật thể liên tục duy trì nhận dạng (identity) và có thể quan sát/theo dõi được qua các frame kế tiếp nhau trong cùng một chuỗi chuyển động mượt mà.
   - Thêm Keyframe: Khi vật thể có sự biến dạng hình học lớn (thay đổi góc nghiêng, đổi hướng di chuyển, từ xa tiến lại gần gây thay đổi tỷ lệ aspect ratio nhanh chóng) cần đặt keyframe để bộ nội suy (interpolation) của công cụ gán nhãn không làm méo box giữa các frame.
   - Trạng thái Outside: Khi vật thể di chuyển hoàn toàn ra khỏi trường nhìn của camera hoặc bị che khuất hoàn toàn 100% trong một khoảng thời gian dài, phải gán trạng thái Outside để thông báo kết thúc một chuỗi quan sát, tránh sinh ra các box nội suy ảo lơ lửng.
   - Bằng chứng cần thiết trước khi nối track qua hai camera: (1) Timestamp đồng bộ chính xác mức mili-giây giữa 2 camera; (2) Ma trận hiệu chuẩn ngoại tại (Extrinsic calibration) và phép biến đổi tọa độ 3D giữa 2 camera; (3) Chính sách đầu ra (Output Policy) quy định rõ việc hợp nhất track ở tầng camera hay tầng tracking trung tâm (Centralized 3D MOT).

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?
   Chỗ bất đồng tiêu biểu: Tại frame adasind_012570.jpg, đối tượng Bike L3 (tọa độ ban đầu x:505-545, y:950-1006). Tôi vẽ box bám sát thân xe máy thấy được và mô hình YOLO cũng phát hiện ra xe máy này (box M12), nhưng so với Reference R10 thì ban đầu bị tính là lệch hình học (IoU=0.467 < 0.50 do Reference kéo rộng ra x:573 để bao trọn cả bánh sau).
   Cách xử lý: Ở vòng QA và Diagnostic, tôi đã phân tích kỹ và escalate ca này, đồng thời tại pha P5 Rework tôi đã căn chỉnh kéo rộng cạnh phải của box ra x:570 để đạt IoU cao hơn và ôm sát toàn bộ phương tiện theo ground truth thực tế, đưa kết quả delta.md đạt 100% matched (19/19 đối tượng).
   Nếu làm lại slice này từ đầu: Tôi sẽ chú ý quan sát kỹ các vật thể ở làn trung tâm hậu cảnh, zoom lớn hơn để kiểm tra toàn bộ chi tiết bánh sau và bóng đổ của phương tiện, đồng thời đo kích thước pixel thực tế ngay từ bước self-QC để tránh bị hẹp box.

