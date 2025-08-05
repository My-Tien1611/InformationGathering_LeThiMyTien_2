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
- **Kiểm tra bảo mật** là phương pháp đánh giá tính bảo mật của hệ thống máy tính hoặc mạng bằng cách xác thực và kiểm chứng một cách có hệ thống tính hiệu quả của các biện pháp kiểm soát bảo mật ứng dụng. Kiểm tra bảo mật ứng dụng web chỉ tập trung vào việc đánh giá tính bảo mật của ứng dụng web. Quy trình này bao gồm việc chủ động phân tích ứng dụng để tìm ra bất kỳ điểm yếu, lỗi kỹ thuật hoặc lỗ hổng bảo mật nào. Bất kỳ vấn đề bảo mật nào được tìm thấy sẽ được trình bày cho chủ sở hữu hệ thống, cùng với đánh giá tác động, đề xuất biện pháp khắc phục hoặc giải pháp kỹ thuật
- Ví dụ, người kiểm tra có thể tìm thấy một trang tại URL sau: https://www.example.com/login/auth_form
Điều này có thể chỉ ra biểu mẫu xác thực trong đó ứng dụng yêu cầu tên người dùng và mật khẩu.
Các tham số sau đây đại diện cho hai điểm truy cập vào ứng dụng:https://www.example.com/appx?a=1&b=1
- Trong trường hợp này, ứng dụng hiển thị hai điểm truy cập (tham số avà b). Tất cả các điểm đầu vào được tìm thấy trong giai đoạn này đều đại diện cho một mục tiêu thử nghiệm. Việc theo dõi thư mục hoặc cây lệnh gọi của ứng dụng và tất cả các điểm truy cập có thể hữu ích trong quá trình thử nghiệm đang diễn ra.


---

## 2. 🛠️ Cách thực hiện kiểm thử thủ công

---

## 2.1 Thực hiện trinh sát phát hiện rò rỉ thông tin bằng công cụ tìm kiếm:

#### 🎯 Mục tiêu
Xác định thông tin thiết kế và cấu hình nhạy cảm nào của ứng dụng, hệ thống hoặc tổ chức được tiết lộ trực tiếp (trên trang web của tổ chức) hoặc gián tiếp (thông qua dịch vụ của bên thứ ba).

---

### 🛠️ Công cụ sử dụng:

- **Shodan**
- **Google Dorks**
- **dnsdumpster**

---

### 🔍 Truy vấn Shodan để tìm host liên quan đến Netflix:

```
hostname:"netflix.com"
org:"Netflix"
ssl.cert.subject.CN:"*.netflix.com"
```

#### 📌 Kết quả:

- **Hạ tầng phân tán:** Các IP đến từ nhiều khu vực (Mỹ, Tây Ban Nha), chứng tỏ Netflix sử dụng CDN và máy chủ toàn cầu.
- **Không có thông tin nhạy cảm:** Máy chủ trả về 404, chỉ tiết lộ thông tin cơ bản như server, chứng chỉ SSL.
- **Bảo mật tốt:** Sử dụng chứng chỉ SSL hợp lệ, hỗ trợ giao thức HTTPS hiện đại (TLS 1.3).
- **Ý nghĩa:** Dùng để xác định hạ tầng, vị trí địa lý máy chủ, không phải tìm lỗ hổng trực tiếp.

---

#### 📁 Một số Google Dorks đã sử dụng:

```
site:netflix.com "network diagram"
site:netflix.com "architecture diagram"
site:netflix.com filetype:pdf "network"
site:netflix.com filetype:vsd
site:netflix.com "internal use only"
```

#### 📌 Kết quả:

- **Netflix sử dụng CDN riêng (Open Connect):**  
  Không hoàn toàn phụ thuộc bên thứ ba, tự quản lý hệ thống CDN.
- **Kiến trúc mạng phân tán:**  
  Đưa nội dung đến gần người dùng thông qua máy chủ chuyên dụng và cơ chế điều phối thông minh.
- **Tài liệu công khai:**  
  Giúp hiểu hệ thống CDN của Netflix, nhưng không tiết lộ thông tin nhạy cảm.

---

### 🌐 Thu thập subdomain → hiểu rõ kiến trúc hệ thống

![Subdomains Netflix](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/09718a7a70eaef5bd2084b686b65942c05b6fd69/SubdomainsNetflix.jpg)

#### 📌 Kết quả phân tích:

- **Hạ tầng hybrid:**  
  Kết hợp giữa hệ thống riêng của Netflix (ASN 40027) và hạ tầng cloud của AWS (CloudFront, EC2).
- **CDN phân tán:**  
  Sử dụng Open Connect để truyền tải video, CloudFront cho web services.
- **Phân tách môi trường rõ ràng:**  
  Có subdomain riêng cho:
  - Production
  - Test/dev
  - B2B
  - Marketing
- **Proxy & bảo mật:**  
  Dùng HTTPS, reverse proxy (Nginx, Envoy), bảo mật nhiều lớp.
- **Cấu trúc subdomain phản ánh chức năng dịch vụ:**  
  Ví dụ: quảng cáo (`ads.`), API nội bộ (`api.`), cảnh báo (`alerts.`), DVD, thử nghiệm,...

✅ **Tổng kết:**  
Kiến trúc giúp Netflix đạt hiệu suất cao, dễ mở rộng và đảm bảo an toàn ở quy mô toàn cầu.

---

#### 📁 Truy vấn Google Dork:

```
site:juice-shop.herokuapp.com intext:@
site:juice-shop.herokuapp.com admin
```

#### 📌 Kết quả:

- Tìm được 2 file:
  - `access.log.2025-08-04`
  - `audit.json`

- Các dòng log có `POST /rest/user/login` phản hồi `HTTP 200 OK` → tiết lộ thông tin đăng nhập:

| # | Email                | Vai trò (theo Juice Shop)                 |
|---|----------------------|--------------------------------------------|
| 1 | `admin@juice-sh.op`  | **Admin (quản trị viên chính)**           |
| 2 | `jim@juice-sh.op`    | Nhân viên (customer support)              |
| 3 | `bender@juice-sh.op` | Nhân viên (bot / tester)                  |
| 4 | `ciso@juice-sh.op`   | Chief Information Security Officer (CISO) |
| 5 | `emma@juice-sh.op`   | Nhân viên (customer support)              |

---

### 🛡️ Cách khắc phục đề xuất:

1. **Không đăng công khai tài liệu nhạy cảm:**  
   - Đặc biệt là sơ đồ kiến trúc mạng, tài liệu kỹ thuật, file cấu hình...

2. **Rà soát định kỳ:**  
   - Kiểm tra định kỳ các công cụ tìm kiếm, GitHub, cloud storage… để đảm bảo không lộ thông tin ngoài ý muốn.

---
> ## 2.2 🧬 Máy chủ web vân tay

---

### 📌 2.2.1 Mục tiêu kiểm tra

Xác định **phiên bản và loại máy chủ web** đang chạy nhằm phát hiện các **lỗ hổng bảo mật đã biết**.

---

### ⚙️ 2.2.2 Các bước thực hiện

---

#### 🔍 **Bước 1: Sử dụng `netcat` để thu thập thông tin tiêu đề HTTP**

```bash
# Kiểm tra HEAD request
nc testphp.vulnweb.com 80
HEAD / HTTP/1.0

# Kết quả:
HTTP/1.1 200 OK
Server: nginx/1.19.0
Date: Mon, 04 Aug 2025 07:28:28 GMT
Content-Type: text/html; charset=UTF-8
Connection: close
X-Powered-By: PHP/5.6.40-38+ubuntu20.04.1+deb.sury.org+1
```

```bash
# Kiểm tra phương thức bị chặn
nc testphp.vulnweb.com 80
DELETE / HTTP/1.0

# Kết quả:
HTTP/1.1 405 Not Allowed
...
<center>nginx/1.19.0</center>
```

```bash
# Kiểm tra phiên bản HTTP không được hỗ trợ
nc testphp.vulnweb.com 80
GET / HTTP/3.0

# Kết quả:
HTTP/1.1 505 HTTP Version Not Supported
...
<center>nginx/1.19.0</center>
```

---

#### 📌 Phân tích kết quả

- **Máy chủ:** `nginx/1.19.0` – phiên bản cũ (phát hành từ năm 2020)
- **Nền tảng:** `PHP/5.6.40` – đã hết vòng đời, tiềm ẩn rủi ro bảo mật cao
- **Giao thức hỗ trợ:** HTTP/1.0 và HTTP/1.1 (không hỗ trợ HTTP/3.0)
- **Phương thức DELETE:** bị chặn (405 Not Allowed) → cấu hình hạn chế phương thức nguy hiểm
- ✅ **Nhận định:** Hệ thống dùng công nghệ lỗi thời, cần **nâng cấp phần mềm và cấu hình** để đảm bảo bảo mật và hiệu suất.

---

#### 🧪 **Bước 2: Sử dụng công cụ quét tự động – [Netcraft](https://www.netcraft.com)**

---

##### 🌐 **Máy chủ & hạ tầng lưu trữ**

- **Hệ điều hành (OS):** Debian Linux
- **Cloud Provider:** AWS EC2
- **IP:** `44.228.249.3`
- **ASN:** `AS16509 (Amazon.com, Inc.)`
- **Reverse DNS:** `ec2-44-228-249-3.us-west-2.compute.amazonaws.com`
- **Vị trí server:** US West (Oregon)

---

##### 🧩 **Công nghệ máy chủ (Server-side)**

| Thành phần             | Thông tin                             |
|------------------------|----------------------------------------|
| PHP                   | Có hỗ trợ và đang chạy                |
| Doctype               | HTML                                  |
| Encoding              | ISO-8859-2 (Eastern European Latin)   |
| HTTP Compression      | Gzip                                  |
| HTTPS                 | ❌ Không hỗ trợ SSL/TLS               |

---

##### 🧠 **Công nghệ phía client (Client-side)**

- **Adobe Flash:** Có sử dụng (nguy cơ RCE cao)
- **JavaScript:** Được sử dụng để tạo nội dung động trên website

---

##### 🔐 **Cấu hình bảo mật liên quan đến email**

| Chính sách           | Trạng thái  |
|----------------------|-------------|
| SPF Record           | ❌ Không có |
| DMARC Record         | ❌ Không có |

---

##### 📝 **Thông tin khác**

- **Hosting:** Amazon (US West)
- **Registrar:** eurodns.com
- **Nameserver:** `ns1.eurodns.com`
- **Tổ chức:** Acunetix Limited (Malta)
- **Tiêu đề trang:** *Home of Acunetix Art*
- **Ngôn ngữ chính:** English

---

### 🛡️ 2.2.3 Cách khắc phục đề xuất

- 🕵️‍♂️ **Che giấu thông tin máy chủ web** (sử dụng mô-đun như `mod_headers` của Apache)
- 🔁 **Sử dụng proxy ngược (reverse proxy)** để thêm lớp bảo mật giữa Internet và web server
- ⏫ **Thường xuyên cập nhật** phần mềm, bản vá bảo mật của web server và PHP
---
> ## 2.3 🗂️ Kiểm tra các tệp siêu dữ liệu để tìm rò rỉ thông tin

---

### 📌 2.3.1 Mục tiêu kiểm tra

- Xác định các **đường dẫn ẩn** hoặc bị che khuất thông qua việc phân tích các tệp siêu dữ liệu.
- Trích xuất và lập bản đồ các thông tin có thể giúp hiểu rõ hơn về **hệ thống nội bộ** của máy chủ.

---

### ⚙️ 2.3.2 Các bước thực hiện

---

#### 🔍 **Bước 1: Phân tích tệp `robots.txt`**

📄 **URL kiểm tra:**  
`https://juice-shop.herokuapp.com/robots.txt`

```txt
User-agent: *
Disallow: /ftp
```

**Ý nghĩa:**

- `User-agent: *` → Áp dụng cho tất cả các bot tìm kiếm.
- `Disallow: /ftp` → Cấm bot truy cập vào thư mục `/ftp`, **gợi ý về vùng nhạy cảm**.

➡️ **Tiến hành kiểm tra trực tiếp:**  
🔗 [`https://juice-shop.herokuapp.com/ftp`](https://juice-shop.herokuapp.com/ftp)  
![image alt](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/3ddd45070efcc5d578066607ca075659854f65e3/ftp.jpg)

📂 **Kết quả: Directory Listing đang bật**

- Cho phép xem **toàn bộ nội dung thư mục /ftp**.
- Có thể tải xuống các file chứa **dữ liệu nhạy cảm** như:
  - `incident-support.kdbx`: Có thể là file KeePass (mật khẩu được mã hóa).
  - `package.json.bak`, `package-lock.json.bak`: File backup – dễ lộ **dependency & scripts**.
  - `announcement_encrypted.md`, `encrypt.pyc`: Gợi ý nội dung **bị mã hóa hoặc ẩn**.
  - `coupons_2013.md.bak`: Có thể chứa **mã giảm giá cũ** hoặc thông tin marketing.
  - Thư mục `quarantine/`: Có thể chứa **file bị nghi ngờ/phát hiện rủi ro**.

---

#### 🧪 **Bước 2: Phân tích với Burp Suite**

✅ Trang thử nghiệm: **About Us**

📌 **Phát hiện endpoint tiềm năng:** `/ftp/legal.md`  
→ Có thể thấy khi duyệt web hoặc thông qua Burp Suite (HTTP history).

![image alt](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/c6e39f53d053deeafeb9042377ca67a9acb029df/Screenshot%20(1305).png)

- Nhấn forward. Tất cả các tiến trình yêu cầu từ trình duyệt máy khách đến máy chủ sẽ được ghi lại trên HTTP history. Tìm địa chỉ trong cột URL và chọn Send to Repeater

![image alt](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/d88811f7310e36694ba372eaa0530389870f8b4a/Screenshot%20(1308).png)

📎 **Kết quả:**  
Directory Listing hiển thị các file:

- `acquisitions.md`
- `announcement_encrypted.md`
- `encrypt.pyc`
- `coupons_2013.md.bak`
- ...

🛑 → Đây là một lỗ hổng bảo mật dạng **Directory Browsing / Insecure Direct Object Reference (IDOR)**  
→ Attacker có thể thấy **toàn bộ file** mà **không cần xác thực**.

![Directory Listing trong Burp](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/3eb1fa42df50ef264326e5abcb48e2054636ffbc/Screenshot%20(1310).png)

---

📥 **Gửi yêu cầu GET đến file cụ thể:** `/ftp/acquisitions.md`

🧾 Phản hồi:  
```txt
** This document is confidential! Do not distribute! **
```

✅ Đã truy xuất thành công **tài liệu mật** nhờ vào lỗ hổng Directory Listing.

![Khai thác file nhạy cảm](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/4af444353f4a4c8641b506612edf5551b5cfbf24/Screenshot%20(1311).png)

