# Peer Review Response - Lab 3

## Thông tin nhóm
- Thành viên 1: Nguyễn Trung Kiên
- Thành viên 2: Hoàng Nhật Anh

## Thành viên 1 góp ý cho thành viên 2
Receiver nên mô tả rõ hơn từng bước xử lý: nhận header, tách key/IV/độ dài, đọc đủ ciphertext rồi mới giải mã. Ngoài ra, phần output nên thống nhất định dạng log để khi demo dễ đối chiếu với sender.

## Thành viên 2 góp ý cho thành viên 1
Sender cần hỗ trợ demo nhanh bằng biến môi trường và in ra thông tin packet rõ ràng hơn. Phần mô tả packet format cũng nên viết sát với code để người chấm dễ kiểm tra.

## Nhóm đã sửa gì sau góp ý
Nhóm đã chuẩn hoá format log của sender và receiver để dễ trình diễn và kiểm tra. Nhóm cũng bổ sung test padding/header, test roundtrip local, test tamper và test wrong key để chứng minh hệ thống chạy đúng luồng và nhận diện được lỗi cơ bản. Ngoài ra, README, report và threat model đã được hoàn thiện để mô tả rõ vai trò của từng người, quy trình demo và các rủi ro bảo mật của thiết kế.