# 📚 Tìm hiểu Information Gathering trong Web Application Security Testing

- **Tên tài liệu:** Information Gathering  
- **Thực hiện:** Lê Thị Mỹ Tiên  
- **Cập nhật:** 05/08/2025

---

## 📑 Mục lục

1. [Mục tiêu thu thập thông tin](#1-mục-tiêu-thu-thập-thông-tin)
2. [Cách thực hiện kiểm thử thủ công](#2-cách-thực-hiện-kiểm-thử-thủ-công)
    - [2.1 Trinh sát với công cụ tìm kiếm](#21-trinh-sát-với-công-cụ-tìm-kiếm)
    - [2.2 Web server fingerprinting](#22-web-server-fingerprinting)
    - [2.3 Phân tích tệp metadata](#23-phân-tích-tệp-metadata)
    - [2.4 Liệt kê ứng dụng trên máy chủ web](#24-liệt-kê-ứng-dụng-trên-máy-chủ-web)
    - [2.5 Kiểm tra nội dung website](#25-kiểm-tra-nội-dung-website)
    - [2.6 Xác định điểm vào ứng dụng](#26-xác-định-điểm-vào-ứng-dụng)
    - [2.7 Thực thi đường dẫn ứng dụng](#27-thực-thi-đường-dẫn-ứng-dụng)
    - [2.8 Web Application Framework Fingerprinting](#28-web-application-framework-fingerprinting)
    - [2.9 Lập bản đồ kiến trúc ứng dụng](#29-lập-bản-đồ-kiến-trúc-ứng-dụng)

---

## 1. 🎯 Mục tiêu Thu thập thông tin

- **Định nghĩa:** Kiểm tra bảo mật là phương pháp đánh giá tính bảo mật của hệ thống máy tính hoặc mạng bằng cách xác thực và kiểm chứng một cách có hệ thống tính hiệu quả của các biện pháp kiểm soát bảo mật ứng dụng. Kiểm tra bảo mật ứng dụng web chỉ tập trung vào việc đánh giá tính bảo mật của ứng dụng web. Quy trình này bao gồm việc chủ động phân tích ứng dụng để tìm ra bất kỳ điểm yếu, lỗi kỹ thuật hoặc lỗ hổng bảo mật nào. Bất kỳ vấn đề bảo mật nào được tìm thấy sẽ được trình bày cho chủ sở hữu hệ thống, cùng với đánh giá tác động, đề xuất biện pháp khắc phục hoặc giải pháp kỹ thuật
- **Kiểm tra bảo mật là phương pháp đánh giá tính bảo mật của hệ thống máy tính hoặc mạng bằng cách xác thực và kiểm chứng một cách có hệ thống tính hiệu quả của các biện pháp kiểm soát bảo mật ứng dụng. Kiểm tra bảo mật ứng dụng web chỉ tập trung vào việc đánh giá tính bảo mật của ứng dụng web. Quy trình này bao gồm việc chủ động phân tích ứng dụng để tìm ra bất kỳ điểm yếu, lỗi kỹ thuật hoặc lỗ hổng bảo mật nào. Bất kỳ vấn đề bảo mật nào được tìm thấy sẽ được trình bày cho chủ sở hữu hệ thống, cùng với đánh giá tác động, đề xuất biện pháp khắc phục hoặc giải pháp kỹ thuật**
- **Ví dụ, người kiểm tra có thể tìm thấy một trang tại URL sau:https://www.example.com/login/auth_form
Điều này có thể chỉ ra biểu mẫu xác thực trong đó ứng dụng yêu cầu tên người dùng và mật khẩu.
Các tham số sau đây đại diện cho hai điểm truy cập vào ứng dụng:https://www.example.com/appx?a=1&b=1**
- **Trong trường hợp này, ứng dụng hiển thị hai điểm truy cập (tham số avà b). Tất cả các điểm đầu vào được tìm thấy trong giai đoạn này đều đại diện cho một mục tiêu thử nghiệm. Việc theo dõi thư mục hoặc cây lệnh gọi của ứng dụng và tất cả các điểm truy cập có thể hữu ích trong quá trình thử nghiệm đang diễn ra.**


---

## 2. 🛠️ Cách thực hiện kiểm thử thủ công

---

### 2.1 Trinh sát với công cụ tìm kiếm

#### 🎯 Mục tiêu
Xác định thông tin nhạy cảm lộ qua website chính hoặc bên thứ ba.

#### 🧪 Công cụ & Truy vấn

- **Công cụ:** Shodan, Google Dorks, dnsdumpster
- **Truy vấn ví dụ với Netflix:**
