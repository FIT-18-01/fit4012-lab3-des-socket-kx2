# Report 1 Page - Lab 3: DES Socket Implementation

## Thông tin nhóm
- **Thành viên 1**: Nguyễn Trung Kiên 
- **Thành viên 2**: Hoàng Nhật Anh

## Mục tiêu
Bài lab hướng tới việc xây dựng một hệ thống gửi và nhận dữ liệu qua TCP socket với dữ liệu được mã hoá bằng DES-CBC trước khi truyền đi. Nhóm luyện cách tạo bản tin, gắn key/IV/header, đóng gói ciphertext và giải mã ở phía nhận. Bên cạnh phần kỹ thuật, bài lab giúp hiểu rõ các khái niệm padding PKCS#7, framing của packet và kiểm thử luồng end-to-end. Quan trọng hơn, bài lab còn cho thấy vì sao thiết kế “gửi key cùng packet” là một điểm yếu bảo mật nếu áp dụng ngoài thực tế.

## Phân công thực hiện
Nguyễn Trung Kiên phụ trách phần sender, format gói tin, và các test liên quan đến padding/header/tamper, receiver, kiểm tra luồng nhận qua socket, và log minh chứng phía nhận. Hoàng Nhật Anh rà soát tài liệu, test. Phần tài liệu, threat model, peer review và rà soát kết quả được làm chung để thống nhất cách trình bày và bảo đảm đúng yêu cầu nộp bài.

## Cách làm
Sender tạo key 8 byte và IV 8 byte, sau đó mã hoá dữ liệu bằng DES-CBC với padding PKCS#7 rồi ghép theo thứ tự `key + iv + length + ciphertext`. Receiver đọc đúng số byte của header, tách key/IV/độ dài, nhận đủ ciphertext rồi giải mã để lấy lại bản rõ. Nhóm dùng biến môi trường để demo nhanh, lưu log ra thư mục `logs/`, và viết test cho padding, header, contract packet, roundtrip local, tamper và wrong key.

## Kết quả
Khi chạy local, sender gửi thành công và receiver khôi phục đúng bản tin gốc. Các test tự động đều qua, bao gồm test kiểm tra thứ tự packet, kiểm tra header độ dài, test roundtrip socket và hai test âm cho tamper/wrong key. Nhóm cũng lưu log chạy thật để làm minh chứng nộp bài và đối chiếu khi demo.

## Kết luận
Bài lab giúp nhóm hiểu rõ hơn cách thiết kế giao tiếp qua socket, cách đóng gói dữ liệu nhị phân và cách kiểm thử một hệ thống nhỏ có cả gửi, nhận và giải mã. Về mặt bảo mật, bài học lớn nhất là không nên truyền key cùng kênh dữ liệu dưới dạng plaintext; thay vào đó cần cơ chế trao đổi khoá an toàn và xác thực toàn vẹn bằng MAC hoặc authenticated encryption.