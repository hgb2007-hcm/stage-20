#  Ngăn chặn rò rỉ thông tin bí mật trên GitHub

## ***Pre-push Checklist***

Trước khi thực hiện lệnh git push lên Public Repository, toàn bộ thành viên trong team bắt buộc phải hoàn thành các bước kiểm tra sau:

***Quét mã nguồn tìm Hardcoded Secrets***

Sử dụng công cụ quét tự động như TruffleHog, GitGuardian hoặc Gitleaks để quét lịch sử Git và thư mục làm việc hiện tại:

***GitBash:*** gitleaks detect --source . -v

***Kiểm tra tệp .gitignore****

Xác nhận chắc chắn các tệp nhạy cảm (như .env, node_modules/, venv/, các tệp khóa cấu hình cục bộ) đã được đưa vào .gitignore.

***Rà soát tệp cấu hình mẫu***

Kiểm tra xem tệp .env.example đã được tạo và đẩy lên Git hay chưa, đồng thời đảm bảo nội dung bên trong chỉ chứa các khóa mẫu (dummy value), tuyệt đối không chứa dữ liệu thật.

## ***Hướng dẫn Kích hoạt 2FA Bắt buộc cho GitHub Organization***

Để bảo vệ repository của dự án thương mại điện tử, Trưởng nhóm (Admin) cần cấu hình bắt buộc xác thực hai yếu tố (2FA) cho tất cả thành viên tham gia:

- Truy cập GitHub Organization: Vào trang chủ Organization của công ty $\rightarrow$ Chọn tab Settings.

- Cài đặt Bảo mật (Security Settings):

  - Ở menu bên trái, chọn Member privileges (hoặc mục Security tương ứng).

  - Tìm đến phần Two-factor authentication.

- Bật tính năng bắt buộc:

  - Tích chọn ô: "Require two-factor authentication for all members of [Tên_Organization]".

  - Nhấn Save để lưu lại thiết lập.

- Thông báo cho Team: Các thành viên chưa bật 2FA trên tài khoản cá nhân sẽ nhận được thông báo yêu cầu kích hoạt qua ứng dụng Authenticator hoặc SMS trước khi được phép tiếp tục truy cập hoặc commit/push mã nguồn vào các repository của tổ chức.

## ***Báo cáo Đánh giá Rủi ro (Risk Assessment) nếu thiếu tệp .gitignore***

Việc bỏ quên hoặc thiết lập sai tệp .gitignore (dẫn đến việc đưa các tệp chứa thông tin nhạy cảm lên GitHub Public) sẽ gây ra những hậu quả thảm khốc sau:

***Rủi ro rò rỉ Khóa API Thanh toán & Dữ liệu Khách hàng (Stripe / AWS)***

- Hậu quả: Nếu các khóa như STRIPE_SECRET_KEY hoặc AWS_ACCESS_KEY_ID bị lộ trên Public Repo, các bot tự động quét mã nguồn của hacker sẽ lập tức lấy được chúng trong vòng vài giây.

- Thiệt hại tài chính: Kẻ tấn công có thể gọi API thanh toán để thực hiện các giao dịch gian lận, rút sạch tiền từ cổng thanh toán của doanh nghiệp hoặc tạo ra hàng nghìn máy chủ ảo trên AWS để đào tiền ảo, đẩy chi phí hạ tầng lên mức khổng lồ.

***Mất quyền kiểm soát Hạ tầng & Cơ sở dữ liệu***

- Hậu quả: Tệp cấu hình chứa thông tin đăng nhập Database (DB_USER, DB_PASSWORD) trực tiếp cho phép kẻ gian kết nối từ xa vào hệ thống cơ sở dữ liệu của dự án (nếu không cấu hình tường lửa IP chặt chẽ).

- Thiệt hại dữ liệu: Dữ liệu đơn hàng, thông tin cá nhân và lịch sử giao dịch của khách hàng thương mại điện tử sẽ bị đánh cắp, tiêu hủy hoặc mã hóa đòi tiền chuộc (Ransomware), vi phạm nghiêm trọng các tiêu chuẩn bảo mật (như GDPR, PCI-DSS).

***Khó khăn trong việc thu hồi và khắc phục***

- Hậu quả đặc thù của Git: Ngay cả khi bạn phát hiện ra lỗi và thực hiện lệnh git rm để xóa tệp .env ở commit tiếp theo, lịch sử commit (Git History) vẫn lưu giữ toàn bộ nội dung cũ.

- Giải pháp khắc phục phức tạp: Team buộc phải sử dụng các công cụ phức tạp như git-filter-repo hoặc BFG Repo-Cleaner để xóa sạch lịch sử, đồng thời phải tiến hành Rotate (Đổi mới toàn bộ) tất cả các Secret Keys, API Keys và Token đã bị lộ trên mọi hệ thống liên quan.