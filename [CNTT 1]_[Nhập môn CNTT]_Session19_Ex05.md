# Xây dựng cơ chế xác thực an toàn cho Crypto Wallet

## ***Thiết kế Kiến trúc: Sơ đồ Luồng Dữ liệu 3 Lớp Bảo vệ***

Để vô hiệu hóa hoàn toàn mối đe dọa khi hacker đã đánh cắp được mật khẩu, hệ thống áp dụng mô hình phòng thủ theo chiều sâu (Defense-in-Depth) gồm 3 lớp bảo vệ độc lập phối hợp chặt chẽ với nhau:

***Chi tiết cơ chế phối hợp ngăn chặn hacker:***

1. Lớp 1 (Xác thực Sinh trắc học & Tri thức): Khi hacker dùng mật khẩu đánh cắp để đăng nhập, chúng sẽ ngay lập tức bị chặn lại ở bước kiểm tra sinh trắc học (khuôn mặt hoặc vân tay của chủ tài khoản thực tế được quét qua cảm biến phần cứng trên điện thoại). Mật khẩu lúc này trở nên vô dụng.

2. Lớp 2 (Thuật toán sở hữu TOTP / Khóa phần cứng): Ngay cả khi hacker vượt qua được thiết bị có sinh trắc học (trường hợp cướp được cả điện thoại nhưng bẻ khóa được), chúng vẫn không thể tự sinh ra mã TOTP (Time-based One-Time Password) vì mã này yêu cầu Khóa bí mật (Secret Key) được liên kết độc quyền với ứng dụng Authenticator hoặc Khóa phần cứng (Hardware Token như YubiKey) nằm trong vùng kiểm soát vật lý của người dùng.

3. Lớp 3 (Vùng đệm phần cứng Secure Enclave & Timeout): Dữ liệu mã hóa ví (Private Key) được bảo vệ bên trong chip phần cứng biệt lập (Secure Enclave). Hơn nữa, cơ chế Immediate Session Timeout đảm bảo phiên làm việc sẽ tự động hủy ngay lập tức khi ngắt kết nối, không cho phép kẻ gian "ăn ké" phiên đăng nhập cũ.

## ***Phần 2: Xử lý Tình huống (Edge Case): Lấy mã OTP khi ở trên máy bay (Không có Internet/Sóng)***

Khi người dùng ở trên máy bay, hoàn toàn không có kết nối Internet hoặc sóng di động, họ vẫn có thể lấy mã OTP bình thường nhờ bản chất kỹ thuật của thuật toán TOTP (RFC 6238):

***Nguyên lý hoạt động ngoại tuyến (Offline Operation):***

Thuật toán TOTP không yêu cầu mạng Internet để tạo mã. Mã OTP được tính toán cục bộ trên thiết bị dựa trên công thức toán học kết hợp giữa hai yếu tố:

         OTP = HMAC-SHA1(Secret key,current time)

***Quy trình thực hiện khi trên máy bay:***

1. Người dùng mở ứng dụng Authenticator (Google Authenticator, Authy,...) hoặc ví crypto có tích hợp bộ tạo OTP trên điện thoại (đang bật chế độ máy bay).  

2. Ứng dụng tự động dùng thời gian thực của máy kết hợp với Secret Key để sinh ra mã 6 chữ số mới có hiệu lực trong 30 giây.

3. Người dùng nhập mã này vào ứng dụng ví để xác thực giao dịch hoặc đăng nhập ngoại tuyến. Hệ thống máy chủ của ví (khi có mạng trở lại hoặc qua cơ chế đồng bộ thời gian blockchain/local) sẽ xác thực tính hợp lệ của mã dựa trên khoảng thời gian lệch cho phép (Drift window).