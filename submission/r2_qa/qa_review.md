# QA review · B1-dense

Mã khóa: 0D8D-E9BE

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_001320.jpg | L1 | R05 | Xe ba bánh ThreeWheeler sát mép trái (x:0-88px) bị cắt cụt bởi viền ảnh/vòng kính, cần kiểm tra đảm bảo đã bật thuộc tính truncated=true theo R05 |
| adasind_012570.jpg | L9 | R02 | Ô tô Car ở mép trái (x:0.1-29.1px) chịu độ méo fisheye cực đại ở vùng edge, box cần bám sát phần nhìn thấy thực tế trên ảnh gốc theo R02, không lấn sâu vào vùng vành đen lens_border |
| adasind_036720.jpg | L4 | R03 | Xe hai bánh Bike ở lề trái (h=180px) có người lái ngồi trên xe; đã gộp đúng rider + xe thành 1 box Bike theo R03, cần soát thêm occluded do phương tiện phía trước che khuất một phần |
| adasind_012570.jpg | L7 | R01 | Xe ô tô Car ở hậu cảnh trung tâm (h=53.6px) gần ngưỡng tối thiểu H=40px, box vẽ tương đối chuẩn nhưng cần kiểm tra mép dưới chạm mặt đường |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.

