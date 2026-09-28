# Sensor context

- Rig: Camera mắt cá (fisheye camera) góc cực rộng (FoV ~180°-195°) gắn ở vị trí phía trước xe thử nghiệm ADASIND (khu vực cản trước hoặc nắp capo nhìn chúc nhẹ xuống mặt đường), quan sát luồng giao thông phía trước và bề mặt làn đường.
- `ego_body`: Thân xe của xe thử nghiệm (ego vehicle) xuất hiện rõ rệt ở vùng đáy trung tâm khung hình (cản trước/mép capo xe) trên 46/48 frame; ngoại trừ 2 frame đặc thù (adasind_006840.jpg và adasind_271039.jpg) không nhìn thấy thân xe do góc nghiêng/vị trí camera.
- Vòng kính (lens circle): Vòng tròn thị trường quang học tròn nằm chính giữa khung hình, tâm quang học xấp xỉ tâm ảnh; vùng rìa ngoài vòng tròn quang học là vành đen không có thông tin hình ảnh (chiếm khoảng 25-30% diện tích khung hình tại 4 góc), được bao bởi polygon `lens_border`.
