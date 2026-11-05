# Xử lý cảnh báo đăng nhập giả mạo GitHub

## ***Phần 1: Phân tích Ký tự Lạ và Bản chất Tấn công Homograph***  

Ký tự lạ trong URL: [https://githúb.com/login](https://githúb.com/login) chứa ký tự chữ ú (chữ u có dấu sắc, mã Unicode: U+00FA) thay vì chữ u thường chuẩn trong tên miền chính thức của GitHub (github.com).

***Tại sao nó đánh lừa được người dùng?***

- Tấn công Homograph IDN (Internationalized Domain Name Homograph Attack): Kẻ tấn công lợi dụng việc hệ thống tên miền quốc tế cho phép sử dụng các ký tự Unicode. Do chữ ú có hình dáng cực kỳ giống chữ u thông thường (đặc biệt khi hiển thị trên các phông chữ không có chân hoặc khi người dùng đọc lướt qua với tâm lý hoảng hốt trước thông báo "Security Alert"), não bộ con người tự động "nhìn nhận" đó là trang web quen thuộc.

- Hệ quả kỹ thuật: Trình duyệt khi phân giải tên miền này sẽ chuyển đổi chuỗi ký tự chứa Unicode thành mã Punycode (có dạng xn--...). Do đó, đây là một địa chỉ IP / máy chủ hoàn toàn khác do hacker kiểm soát, sẵn sàng đánh cắp thông tin đăng nhập hoặc Token ngay khi bạn bấm nút Submit.


## ***Phần 2: Quy trình Kiểm tra URL Nhanh (Bằng mắt thường & Công cụ)***

Trước khi nhập bất kỳ Master Password hoặc thông tin nhạy cảm nào, bạn cần tuân thủ quy trình kiểm tra nhanh sau đây:

Bước 1: Kiểm tra nhanh bằng mắt thường (Manual Scan)

- Soi kỹ từng ký tự đặc biệt: Cảnh giác cao độ với các ký tự có dấu (á, é, í, ú, ñ...), các biến thể chữ cái (như chữ rn sát nhau trông giống chữ m), hoặc việc thay thế ký tự bằng số (chữ o thành số 0, chữ l thành số 1).

- Kiểm tra đuôi tên miền gốc: Luôn nhìn vào phần gốc trước dấu / đầu tiên (ví dụ: trong [githúb.com/login](https://githúb.com/login), tên miền thực tế là githúb.com, không phải github.com).

Bước 2: Tận dụng Trợ lý Công nghệ (Auto-fill của Password Manager)

- Nguyên tắc bảo vệ: Hãy để các công cụ như Bitwarden làm "bộ lọc" đầu tiên.

- Cách thực hiện: Bấm vào nút Auto-fill. Nếu trang web là giả mạo (githúb.com), Bitwarden sẽ từ chối điền thông tin vì tên miền không khớp chính xác với bản ghi github.com đã lưu trong két sắt. Nếu tool không tự điền, tuyệt đối dừng lại ngay lập tức.

 Bước 3: Kiểm tra mã Punycode hoặc quét nhanh qua Công cụ (Tool-assisted)

 - Nếu nghi ngờ một đường link lạ, bạn có thể sao chép URL và dán vào các trang web kiểm tra chuyển đổi Punycode hoặc các công cụ phân tích an ninh mạng uy tín:

 - VirusTotal (virustotal.com): Dán URL vào mục URL để kiểm tra xem hệ thống bảo mật toàn cầu có đánh dấu đây là trang web lừa đảo (Phishing) hay không.

 - Công cụ kiểm tra IDN Homograph: Giúp bóc tách xem tên miền có chứa ký tự Unicode ẩn danh hay không.