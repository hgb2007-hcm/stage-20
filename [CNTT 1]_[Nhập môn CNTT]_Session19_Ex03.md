# Quản lý rủi ro khi mất thiết bị xác thực

## ***Báo cáo Phân tích IPO (Input - Process - Output)***

Mô hình IPO phác họa toàn bộ quá trình khẩn cấp để xử lý sự cố mất thiết bị xác thực 2FA:

***INPUT***

 • Tập hợp các Mã dự phòng (Backup/Recovery Codes) dạng văn bản đã được tạo và lưu trữ ngoại tuyến từ trước khi cấu hình AWS 2FA.

 • Tài khoản IAM / Root Account của AWS cần khôi phục.

 ***PROCESS***

 Bước 1 (Xác thực): Sử dụng một trong các Backup Codes để nhập thay thế cho mã 6 chữ số từ Google Authenticator, giúp đăng nhập thành công vào bảng điều khiển AWS.

 Bước 2 (Hủy liên kết cũ): Vào phần bảo mật tài khoản, tiến hành thu hồi/xóa bỏ thiết bị Authenticator cũ đã bị mất.

 Bước 3 (Cấp mới): Quét mã QR mới để liên kết ứng dụng Google Authenticator trên điện thoại thay thế.

 Bước 4 (Tạo lại mã dự phòng): Vô hiệu hóa toàn bộ danh sách Backup Codes cũ và tạo ra một bộ mã dự phòng hoàn toàn mới.

 ***OUTPUT***

 • Quyền quản trị AWS được khôi phục an toàn trên thiết bị mới.

 • Thiết bị cũ bị vô hiệu hóa vĩnh viễn quyền sinh mã 2FA.

 • Hệ thống cập nhật bộ Backup Codes mới, đảm bảo tính sẵn sàng cho các tình huống khẩn cấp tiếp theo.

 ## ***Phần 2: Tại sao tuyệt đối KHÔNG lưu Backup Codes dưới dạng ảnh trong Thư viện Điện thoại?***

 Nhiều người có thói quen chụp màn hình (Screenshot) các mã dự phòng (Backup Codes) và lưu thẳng vào thư viện ảnh (Gallery/Photos) để tiện "phòng thân". Tuy nhiên, đây là một sai lầm chí mạng về bảo mật vì những lý do kỹ thuật sau:

 ***Nguy cơ tự động đồng bộ hóa đám mây (Cloud Sync Risk)***

 - Các ứng dụng như Google Photos, iCloud Photos, OneDrive hay Dropbox thường có tính năng tự động sao lưu (Auto-backup) ảnh chụp màn hình lên đám mây.

 - Nếu tài khoản đám mây của bạn bị lộ mật khẩu hoặc dính mã độc, toàn bộ các bức ảnh chứa Backup Codes sẽ bị kẻ tấn công quét được ngay lập tức, vô hiệu hóa hoàn toàn lớp bảo vệ 2FA.

 ***Quyền truy cập của ứng dụng bên thứ ba (Broad App Permissions):***

 - Khi bạn cài đặt các ứng dụng chỉnh sửa ảnh, mạng xã hội hoặc game trên điện thoại và cấp quyền "Truy cập tất cả ảnh" (Full Access to Photos), các mã độc trá hình hoặc SDK quảng cáo độc hại có thể âm thầm quét toàn bộ thư viện ảnh để tìm kiếm các từ khóa nhạy cảm như "Backup code", "Recovery", "2FA", sau đó trích xuất về máy chủ C2 của hacker.

 ***Lỗ hổng khi thiết bị rơi vào tay kẻ gian (Physical Extraction):***

 - Nếu điện thoại bị mất và kẻ nhặt được tìm cách vượt qua màn hình khóa (hoặc điện thoại đang mở khóa khi rơi), Thư viện ảnh là nơi đầu tiên họ kiểm tra. Khác với các tệp văn bản mã hóa nằm trong Password Manager, ảnh chụp màn hình nằm ở dạng văn bản thô (Plain-text) có thể đọc bằng mắt thường hoặc qua công cụ OCR (Nhận diện ký tự quang học) trong chưa đầy một giây.