Tìm hiểu Information Gathering trong Web Application Security Testing với trang 
- Tên tài liệu: Information Gathering
- Thực hiện: Lê Thị Mỹ Tiên
- Cập nhật: 1/8/2025
1. Mục lục
2. Nội dung
2.1 Mục tiêu Thu thập thông tin trong kiểm tra bảo mật ứng dụng web:
- Kiểm tra bảo mật là phương pháp đánh giá tính bảo mật của hệ thống máy tính hoặc mạng bằng cách xác thực và kiểm chứng một cách có hệ thống tính hiệu quả của các biện pháp kiểm soát bảo mật ứng dụng. Kiểm tra bảo mật ứng dụng web chỉ tập trung vào việc đánh giá tính bảo mật của ứng dụng web. Quy trình này bao gồm việc chủ động phân tích ứng dụng để tìm ra bất kỳ điểm yếu, lỗi kỹ thuật hoặc lỗ hổng bảo mật nào. Bất kỳ vấn đề bảo mật nào được tìm thấy sẽ được trình bày cho chủ sở hữu hệ thống, cùng với đánh giá tác động, đề xuất biện pháp khắc phục hoặc giải pháp kỹ thuật
- Trong quá trình kiểm thử thụ động, người kiểm thử cố gắng hiểu logic của ứng dụng và khám phá ứng dụng với tư cách người dùng. Các công cụ có thể được sử dụng để thu thập thông tin. Ví dụ: proxy HTTP có thể được sử dụng để quan sát tất cả các yêu cầu và phản hồi HTTP. Cuối giai đoạn này, người kiểm thử thường sẽ hiểu rõ tất cả các điểm truy cập và chức năng của hệ thống (ví dụ: tiêu đề HTTP, tham số, cookie, API, cách sử dụng/mô hình công nghệ, v.v.). Phần Thu thập Thông tin giải thích cách thực hiện kiểm thử thụ động.
Ví dụ, người kiểm tra có thể tìm thấy một trang tại URL sau:https://www.example.com/login/auth_form
Điều này có thể chỉ ra biểu mẫu xác thực trong đó ứng dụng yêu cầu tên người dùng và mật khẩu.
Các tham số sau đây đại diện cho hai điểm truy cập vào ứng dụng:https://www.example.com/appx?a=1&b=1
Trong trường hợp này, ứng dụng hiển thị hai điểm truy cập (tham số avà b). Tất cả các điểm đầu vào được tìm thấy trong giai đoạn này đều đại diện cho một mục tiêu thử nghiệm. Việc theo dõi thư mục hoặc cây lệnh gọi của ứng dụng và tất cả các điểm truy cập có thể hữu ích trong quá trình thử nghiệm đang diễn ra.
2.2 Cách thực hiện kiểm thử thủ động
2.2.1 Thực hiện trinh sát phát hiện rò rỉ thông tin bằng công cụ tìm kiếm:
2.2.1.1 Mục tiêu kiểm tra: Xác định thông tin thiết kế và cấu hình nhạy cảm nào của ứng dụng, hệ thống hoặc tổ chức được tiết lộ trực tiếp (trên trang web của tổ chức) hoặc gián tiếp (thông qua dịch vụ của bên thứ ba).
2.2.1.2 Các bước thực hiện:
Bước 1: Tìm sơ đồ và cấu hình mạng
- Công cụ thực hiện: Shodan - quét IP/dịch vụ


