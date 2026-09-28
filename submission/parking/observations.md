# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh): Các đoạn polyline sơn màu trắng/vàng phân chia rõ rệt các ô đỗ xe riêng lẻ ở tiền cảnh (foreground), kéo dài từ cạnh dưới của bãi lên đến điểm kết thúc vạch phân chia ô (tọa độ y từ ~510px đến ~720px).
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: Không vẽ vạch biên kéo dài chỉ dẫn lối xe chạy ngang bãi và mép ranh giới đường ở xa; lý do vì các vạch này có vai trò định hướng giao thông/ranh giới lưu thông nội bộ bãi xe, không có chức năng tạo ranh giới phân định một ô đỗ xe riêng lẻ theo hướng dẫn tại docs/11-parking-lines-vi.md.
- Polygon `free_space` dừng ở đâu; có phần bị che nào không: Polygon `free_space` bao phủ phần mặt đường trống nhìn thấy được của lối xe chạy giữa các dãy ô đỗ; polygon dừng lại chính xác tại mép vỉa hè và chân các vạch sơn chia ô, không cắt xuyên qua các phương tiện đang đỗ hoặc vật cản tĩnh.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”): Các vạch sơn bị mờ do bóng râm hoặc mài mòn ở hậu cảnh xa phía sau giáp khu vực công trình; đã chủ động không vẽ để tránh tạo ra false positives do thiếu căn cứ hình học rõ ràng.

