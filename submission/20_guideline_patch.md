# Guideline patch

- **Rule mới đề xuất:** R12 — Quy chuẩn gán nhãn cho xe chở hàng cồng kềnh và tải trọng vượt kích thước xe (Overhanging Cargo / Extended Load).
- **Áp dụng cho:** Các class phương tiện chở hàng gồm `ThreeWheeler`, `Bike`, `Truck` tại tất cả các zone trên ảnh fisheye.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** Luật hiện tại v1.0.0 chỉ quy định về người lái xe hai bánh (R03) và phân loại phương tiện (R04), nhưng hoàn toàn thiếu quy định xử lý khi xe ba bánh (auto-rickshaw) hoặc xe máy chở theo hàng hóa cồng kềnh (thùng hàng, sọt hàng, thanh kim loại nhô dài ra phía sau hoặc sang hai bên). Thực tế dẫn đến sự bất đồng giữa các annotator: người thì vẽ box ôm cả phần hàng cồng kềnh, người thì chỉ vẽ khung xe cơ giới, làm sai lệch IoU và gây tranh cãi trong vòng QA.
- **`rules_version` mới:** v1.1.0
- **Hiệu lực từ:** Bắt đầu có hiệu lực từ Round tiếp theo (Round 2 / Production Batch 2). Quy định cụ thể: Bounding box phải bao trọn toàn bộ phần hàng hóa gắn liền với phương tiện đang di chuyển nếu phần hàng đó tạo thành một khối vật lý tiềm ẩn nguy cơ va chạm giao thông.

