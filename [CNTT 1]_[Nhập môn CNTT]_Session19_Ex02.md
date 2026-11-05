# Chuỗi tấn công từ mật khẩu bị rò rỉ

## ***Phần 1: Cơ chế Tấn công "Credential Stuffing" & Hiệu ứng Domino***

Khi một diễn đàn công nghệ nhỏ bị tấn công và rò rỉ cơ sở dữ liệu chứa thông tin người dùng (Email và Mật khẩu ở dạng văn bản thô hoặc băm yếu), kẻ tấn công sẽ kích hoạt hiệu ứng domino thông qua cơ chế Credential Stuffing (Nhồi thông tin đăng nhập):

- Tự động hóa hàng loạt: Hacker không tự tay đăng nhập từng tài khoản. Chúng sử dụng các công cụ tự động hóa (Botnets/Script) nạp danh sách hàng nghìn cặp [Email - Mật khẩu "123456"] vừa trộm được từ diễn đàn nhỏ.

- Tận dụng thói quen dùng chung mật khẩu: Vì người dùng có xu hướng tái sử dụng một mật khẩu duy nhất cho nhiều dịch vụ (Gmail, Facebook, GitHub), các bot tự động này sẽ "thử" đồng loạt trên các nền tảng lớn.

- Hệ quả chiếm đoạt (Domino Effect): Chỉ trong vài giây, nếu tài khoản Gmail hoặc Facebook của bạn dùng chung mật khẩu "123456", hệ thống của các nền tảng lớn sẽ chấp nhận thông tin đăng nhập đó. Kẻ tấn công ngay lập tức chiếm quyền kiểm soát Gmail (tài khoản mẹ), từ đó dùng tính năng "Quên mật khẩu" để chiếm đoạt toàn bộ các dịch vụ liên kết khác.

## ***Phần 2 : So sánh: Mật khẩu Cảm tính (Con người) vs. Mật khẩu Sinh học/Máy tính***

***Tính ngẫu nhiên***

- Thấp. Dựa trên từ ngữ có nghĩa, ngày tháng năm sinh, tên người yêu, thú cưng hoặc các biến thể đơn giản (P@ssw0rd123).

- Tuyệt đối ngẫu nhiên, không tuân theo bất kỳ quy luật ngôn ngữ hay ngữ nghĩa nào của con người.

***Không gian mẫu (Key Space)***

- Hẹp. Hacker dễ dàng quét qua các từ điển phổ biến (Dictionary Attack) kết hợp quy tắc thay thế ký tự (@ thay cho a, ! thay cho i).

- Cực kỳ khổng lồ. Với độ dài 32 ký tự bao gồm chữ hoa, chữ thường, số và ký tự đặc biệt, không gian mẫu đạt tới hàng triệu tỷ tỷ biến thể.

***Khả năng chống Brute-force***

- Kém. Các cỗ máy tính hiện đại hoặc GPU có thể dò ra các mật khẩu ngắn, dễ đoán chỉ trong vài giây đến vài phút.

- Bất khả thi. Việc vét cạn (Brute-force) một mật khẩu ngẫu nhiên dài 24-32 ký tự đòi hỏi thời gian tính bằng hàng triệu năm bằng công nghệ siêu máy tính hiện đại.

## ***Phần 3: Tại sao Password Manager (dùng chuẩn AES-256) an toàn hơn Ghi chép Thủ công?***

Nhiều người dùng chọn ghi chép mật khẩu ra sổ tay, tệp Notepad (passwords.txt) hoặc lưu trên điện thoại vì sợ quên. Tuy nhiên, các phương pháp thủ công này cực kỳ rủi ro so với việc sử dụng Password Manager ứng dụng chuẩn mã hóa AES-256 (Advanced Encryption Standard - 256 bit):

***Rủi ro chết người của việc ghi chép thủ công***

- Sổ tay vật lý: Dễ bị mất cắp, bị người khác đọc lén khi để trên bàn làm việc, hoặc bị hủy hoại bởi hỏa hoạn/nước.

- Tệp văn bản (Notepad, Word, Excel): Khi máy tính nhiễm mã độc Infostealer, các tệp này là mục tiêu đầu tiên bị quét và tự động trích xuất gửi về máy chủ của hacker dưới dạng văn bản thô (Clear-text) hoàn toàn không được bảo vệ.

***Sức mạnh bảo mật của AES-256 trong Password Manage***

- Bản chất chuẩn quân sự: AES-256 là tiêu chuẩn mã hóa đối xứng được Chính phủ Hoa Kỳ và các tổ chức tài chính toàn cầu sử dụng để bảo vệ thông tin tối mật.

***Cơ chế hoạt động:***

- Khi bạn lưu mật khẩu vào Password Manager, dữ liệu được mã hóa ngay lập tức trên thiết bị của bạn (Client-side Encryption) thành một chuỗi ký tự hỗn độn trước khi truyền đi hoặc lưu trữ.

- Khóa mã hóa sử dụng độ dài 256-bit. Theo định luật vật lý lượng tử và năng lượng tính toán hiện tại, để phá mã AES-256 bằng phương pháp thử tất cả các khóa (Brute-force), các siêu máy tính mạnh nhất thế giới sẽ cần nhiều thời gian hơn cả tuổi thọ của vũ trụ ($2^{256}$ khả năng).

- Kiến trúc Zero-Knowledge: Nhà cung cấp dịch vụ Password Manager cũng không thể đọc được mật khẩu của bạn vì họ không lưu khóa giải mã trên máy chủ. Do đó, ngay cả khi máy chủ đám mây của dịch vụ bị hack hoàn toàn, kẻ tấn công cũng chỉ thu được các tệp mã hóa vô dụng không thể giải mã.