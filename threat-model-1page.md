# Threat Model - Lab 3

## Thông tin nhóm
- Thành viên 1: Nguyễn Trung Kiên
- Thành viên 2: Hoàng Nhật Anh

## Assets
Những tài sản cần bảo vệ gồm: bản tin gốc, DES key, IV, ciphertext, tính toàn vẹn của packet, log chạy thử và cấu hình socket. Trong đó key là tài sản nhạy cảm nhất vì ai có key đều có thể giải mã dữ liệu.

## Attacker model
Đối tượng tấn công có thể là người cùng mạng, tiến trình độc hại trên máy local, hoặc một client giả mạo biết địa chỉ/port của receiver. Kẻ tấn công có thể nghe lén traffic, sửa packet, gửi packet rác, giữ kết nối để gây treo, hoặc cố đọc log nếu log bị lưu sai quyền truy cập.

## Threats
1. **Eavesdropping**: Vì key và IV được gửi cùng packet dưới dạng plaintext, attacker bắt được traffic là có thể giải mã toàn bộ bản tin.
2. **Tampering**: Attacker sửa ciphertext hoặc header độ dài để làm hỏng dữ liệu, gây lỗi giải mã hoặc làm receiver xử lý sai.
3. **Replay / impersonation**: Không có MAC, nonce hay xác thực phiên nên attacker có thể phát lại packet cũ hoặc giả mạo sender.
4. **Denial of Service**: Packet sai định dạng, kết nối treo, hoặc length bất thường có thể làm receiver chờ lâu và mất tài nguyên.

## Mitigations
1. **Không gửi key plaintext trong hệ thống thật**; dùng TLS hoặc cơ chế trao đổi khoá an toàn hơn, ví dụ key đã được bảo vệ bởi một kênh tin cậy.
2. **Thêm bảo vệ toàn vẹn** như HMAC hoặc chuyển sang authenticated encryption (ví dụ AES-GCM) để phát hiện sửa đổi dữ liệu.
3. **Ràng buộc kích thước và timeout** cho header/ciphertext, kiểm tra length hợp lệ trước khi đọc dữ liệu.
4. **Giới hạn phạm vi demo** ở localhost hoặc mạng nội bộ, bật `SO_REUSEADDR` và timeout để tránh treo port lâu.
5. **Bảo vệ log và file output** bằng quyền truy cập phù hợp, không ghi dữ liệu nhạy cảm ngoài phạm vi cần thiết.

## Residual risks
Ngay cả khi thêm kiểm tra và timeout, thiết kế hiện tại vẫn không an toàn để triển khai ngoài đời thật vì DES đã lỗi thời và việc gửi key cùng packet làm mất ý nghĩa của mã hoá. Đây chỉ là mô hình học tập để quan sát luồng dữ liệu, không phải giải pháp bảo mật production.
