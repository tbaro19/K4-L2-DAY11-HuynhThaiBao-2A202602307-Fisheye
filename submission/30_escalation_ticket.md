# Escalation ticket

## Ticket 1

- **Frame:** adasind_012570.jpg
- **Ảnh chụp:** submission/screenshots/escalation_adasind_012570.jpg
- **Expected impact:** Khi Reference chuẩn dạy học bỏ sót một đối tượng có thật (Ground Truth Defect), việc đánh giá chất lượng mô hình AI và người gán nhãn sẽ bị sai lệch nghiêm trọng: mô hình phát hiện đúng vật thể thật nhưng bị phạt là False Positive / Spurious, kéo tụt chỉ số Precision và mAP thực tế.
- **Owner:** `guideline`
- **Recommendation:** Bổ sung box `Bike` tại vị trí (x: 505-573, y: 945-1006) vào bản Reference chính thức của tập dữ liệu; cập nhật tài liệu đào tạo cho annotator về việc rà soát các phương tiện ở làn trung tâm hậu cảnh; không phạt các model phát hiện được vật thể này.

