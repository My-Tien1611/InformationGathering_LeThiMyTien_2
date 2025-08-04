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
- Công cụ thực hiện: Shodan, Google Dorks, dnsdumpster
- Gõ các truy vấn như để tìm host liên quan đến Neflix: 
                        hostname:"netflix.com"
                        org:"Netflix"
                        ssl.cert.subject.CN:"*.netflix.com"
- Kết quả nhận được: 
      + Hạ tầng phân tán: Các IP xuất hiện ở nhiều khu vực (Mỹ, Tây Ban Nha) cho thấy Netflix dùng CDN và máy chủ toàn cầu.
      + Không có thông tin nhạy cảm: Máy chủ trả về 404 và chỉ lộ thông tin cơ bản (server, chứng chỉ), không có nội dung hay dịch vụ đặc biệt mở.
      + Bảo mật tốt: Chứng chỉ SSL hợp lệ, giao thức HTTPS hỗ trợ chuẩn mới (TLS 1.3).
      + Ý nghĩa: Kết quả chủ yếu để xác định vị trí và trạng thái máy chủ chứ không phải lỗ hổng.
- Google có thể index nhầm tài liệu nội bộ (pdf, ppt, visio).
- Một số truy vấn: site:
                netflix.com "network diagram"
                site:netflix.com "architecture diagram"
                site:netflix.com filetype:pdf "network"
                site:netflix.com filetype:vsd
                site:netflix.com "internal use only"
- Kết quả nhận được: 
      + Netflix không phụ thuộc hoàn toàn vào các CDN bên thứ ba mà chủ yếu dựa trên Open Connect – một hệ thống CDN riêng.
      + Kiến trúc mạng của họ dựa vào phân tán nội dung đến gần người dùng, sử dụng máy chủ chuyên dụng và cơ chế điều phối thông minh.
      + Các tài liệu public giúp hiểu cách thức triển khai và vận hành CDN của Netflix, nhưng không tiết lộ thông tin nhạy cảm.
- Thu thập subdomain → giúp hình dung cấu trúc hệ thống.
![image alt](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/09718a7a70eaef5bd2084b686b65942c05b6fd69/SubdomainsNetflix.jpg)
- Kết quả nhận được:
      + Hạ tầng hybrid: Kết hợp giữa hệ thống riêng của Netflix (ASN 40027) và dịch vụ cloud của AWS (CloudFront, EC2).
      + CDN phân tán: Sử dụng Open Connect (CDN riêng) để tối ưu truyền tải phim và CloudFront để phân phối các dịch vụ web công khai.
      + Phân tách rõ môi trường: Có các subdomain riêng cho production, test/dev, B2B và marketing.
      + Bảo mật & Proxy: Dùng HTTPS, reverse proxy (Nginx, Envoy) và các lớp bảo mật nhiều tầng.
      + Cấu trúc domain phản ánh dịch vụ: Các subdomain cho thấy dịch vụ quảng cáo, API nội bộ, cảnh báo, DVD, thử nghiệm,…
=> Kiến trúc này giúp Netflix đạt hiệu suất cao, khả năng mở rộng và an toàn toàn cầu.
Bước 2: Tìm các bài đăng và email được lưu trữ của quản trị viên hoặc nhân viên chủ chốt khác
- Công cụ thực hiện : Google Dorking
- Các truy vấn để tìm thông tin rò rỉ trên web: 
                  site:juice-shop.herokuapp.com intext:@
                  site:juice-shop.herokuapp.com admin
- Kết quả ta thu được được file access.log.2025-08-04 và audit.json:
      + Các dòng log POST /rest/user/login trả HTTP 200 OK cho ta biết được
| # | Email                | Vai trò (theo mặc định Juice Shop)        |
|---|----------------------|--------------------------------------------|
| 1 | `admin@juice-sh.op`  | **Admin (quản trị viên chính)**           |
| 2 | `jim@juice-sh.op`    | Nhân viên (customer support)              |
| 3 | `bender@juice-sh.op` | Nhân viên (bot / tester)                  |
| 4 | `ciso@juice-sh.op`   | Chief Information Security Officer (CISO) |
| 5 | `emma@juice-sh.op`   | Nhân viên (customer support)              |
      + 
Bước 3: Tìm quy trình đăng nhập và định dạng tên người dùng:
Bước 4: Tìm tên người dùng, mật khẩu và khóa riêng tư:
Bước 5: Tìm tệp cấu hình dịch vụ đám mây hoặc của bên thứ ba
Bước 6: Tìm tiết lộ nội dung thông báo lỗi và phát triển, thử nghiệm, Kiểm tra chấp nhận của người dùng (UAT) và phiên bản dàn dựng của các trang web.
2.2.2 Máy chủ web vân tay
2.2.2.1 Mục tiêu kiểm tra: Xác định phiên bản và loại máy chủ web đang chạy để có thể phát hiện thêm bất kỳ lỗ hổng bảo mật nào đã biết
2.2.2.2 Các bước thực hiện:
Bước 1: Sử dụng netcat
      + nc testphp.vulnweb.com 80 HEAD / HTTP/1.0
HTTP/1.1 200 OK
Server: nginx/1.19.0
Date: Mon, 04 Aug 2025 07:28:28 GMT
Content-Type: text/html; charset=UTF-8
Connection: close
X-Powered-By: PHP/5.6.40-38+ubuntu20.04.1+deb.sury.org+1
      + nc testphp.vulnweb.com 80 DELETE / HTTP/1.0
HTTP/1.1 405 Not Allowed
Server: nginx/1.19.0
Date: Mon, 04 Aug 2025 07:37:44 GMT
Content-Type: text/html
Content-Length: 157
Connection: close

<html>
<head><title>405 Not Allowed</title></head>
<body>
<center><h1>405 Not Allowed</h1></center>
<hr><center>nginx/1.19.0</center>
</body>
</html>
      + nc testphp.vulnweb.com 80 GET / HTTP/3.0

HTTP/1.1 505 HTTP Version Not Supported
Server: nginx/1.19.0
Date: Mon, 04 Aug 2025 07:43:55 GMT
Content-Type: text/html
Content-Length: 187
Connection: close

<html>
<head><title>505 HTTP Version Not Supported</title></head>
<body>
<center><h1>505 HTTP Version Not Supported</h1></center>
<hr><center>nginx/1.19.0</center>
</body>
</html>
- Từ kết quả đó:
      + Máy chủ web testphp.vulnweb.com đang sử dụng nginx phiên bản 1.19.0, một phiên bản đã cũ được phát hành từ năm 2020. Trang web này chạy trên nền tảng PHP 5.6.40, là phiên bản đã hết vòng đời và không còn được cập nhật bảo mật, tiềm ẩn nhiều rủi ro.
      + Máy chủ hỗ trợ các giao thức HTTP/1.0 và HTTP/1.1, nhưng không hỗ trợ HTTP/3.0, dẫn đến lỗi 505 HTTP Version Not Supported nếu gửi yêu cầu với phiên bản đó. Ngoài ra, phương thức HTTP DELETE bị chặn, phản hồi mã lỗi 405 Not Allowed, cho thấy cấu hình server đã hạn chế các phương thức nguy hiểm.
      + Từ các thông tin này, có thể thấy hệ thống đang sử dụng công nghệ cũ và chưa được cập nhật, cần được xem xét nâng cấp để đảm bảo an toàn và hiệu năng hoạt động.
Bước 2: Sử dụng công cụ quét tự động: Netcraft , một công cụ trực tuyến quét các trang web để tìm thông tin, bao gồm cả máy chủ web.
- Máy chủ & Hạ tầng lưu trữ:
      + Hệ điều hành (OS): Debian Linux
      + Hạ tầng đám mây (Cloud): Amazon Web Services EC2 (Elastic Compute Cloud) 
      + Địa chỉ IP: 44.228.249.3
      + ASN (Autonomous System Number): AS16509 (Amazon.com, Inc.)
      + Reverse DNS: ec2-44-228-249-3.us-west-2.compute.amazonaws.com
      + Vị trí server: US West (Oregon) datacenter
- Công nghệ máy chủ (Server-Side Technology)
      + PHP Enabled: Server hỗ trợ và đang chạy PHP
      + Doctype: HTML (Document Type Declaration)
      + Character Encoding: ISO-8859-2 (Eastern European Latin)
      + HTTP Compression: Gzip Content Encoding
      + SSL/TLS: Không hỗ trợ HTTPS (chỉ HTTP) – có thể bị sniffing, MITM
- Công nghệ phía client (Client-Side Technology)
      + Adobe Flash: Công nghệ cũ, tiềm ẩn nhiều lỗ hổng bảo mật (RCE)
      + JavaScript: Được sử dụng để tạo nội dung động trên website
- Cấu hình bảo mật liên quan (Email & Policy)
      + SPF Record: Không có (dễ bị giả mạo email – phishing)
      + DMARC Record: Không có (không kiểm soát xác thực email)
- Các thông tin khác
      + Hosting company: Amazon - US West (Oregon) datacenter
      + Registrar: eurodns.com
      + Nameserver: ns1.eurodns.com
      + Tổ chức vận hành: Acunetix Limited (Malta)
      + Site title: Home of Acunetix Art
      + Primary Language: English
2.2.2.3 Cách khắc phục
- Che giấu thông tin máy chủ web trong tiêu đề, chẳng hạn như với mô-đun mod_headers của Apache.
- Sử dụng máy chủ proxy ngược được tăng cường để tạo thêm một lớp bảo mật giữa máy chủ web và Internet.
- Đảm bảo máy chủ web luôn được cập nhật phần mềm và bản vá bảo mật mới nhất.
2.2.3 Kiểm tra các tệp siêu dữ liệu của máy chủ web để tìm rò rỉ thông tin
2.2.3.1 Mục tiêu kiểm tra
- Xác định các đường dẫn và chức năng ẩn hoặc bị che khuất thông qua việc phân tích các tệp siêu dữ liệu.
- Trích xuất và lập bản đồ các thông tin khác có thể giúp hiểu rõ hơn về các hệ thống hiện có.
2.2.3.2 Các bước thực hiện:
Bước 1: Dùng Robots.txt: juice-shop.herokuapp.com/robots.txt
 Kết quả: 
          User-agent: *
          Disallow: /ftp
- User-agent: *: Áp dụng cho tất cả bot tìm kiếm.
- Disallow: /ftp: Cấm bot truy cập thư mục /ftp
- /ftp có thể chứa dữ liệu nhạy cảm (backup, mật khẩu, file cấu hình...).
- Là gợi ý tiềm năng cho hacker trong môi trường Juice Shop (mô phỏng lỗ hổng bảo mật).
- Cần kiểm tra truy cập trực tiếp hoặc quét thư mục để tìm file ẩn.
- Truy cập trực tiếp: https://juice-shop.herokuapp.com/ftp
![image alt](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/3ddd45070efcc5d578066607ca075659854f65e3/ftp.jpg)
- Directory Listing được bật:
      + Máy chủ liệt kê toàn bộ nội dung thư mục /ftp.
      + Cho phép kẻ tấn công xem và tải các file tiềm ẩn dữ liệu nhạy cảm.
- Một số file nhạy cảm đáng chú ý:
      + incident-support.kdbx: Có thể là file KeePass chứa mật khẩu (dạng mã hóa).
      + package.json.bak, package-lock.json.bak: File backup có thể tiết lộ thông tin dependency, version, scripts nguy hiểm.
      + announcement_encrypted.md, encrypt.pyc: Gợi ý có mã hóa hoặc thông báo ẩn.
      + coupons_2013.md.bak: Có thể chứa mã giảm giá hoặc thông tin nhạy cảm từ quá khứ.
      + Thư mục quarantine/: Thường dùng chứa file nghi ngờ/phát hiện có vấn đề.



