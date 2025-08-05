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
- Ví dụ, người kiểm tra có thể tìm thấy một trang tại URL sau: https://www.example.com/login/auth_form
Điều này có thể chỉ ra biểu mẫu xác thực trong đó ứng dụng yêu cầu tên người dùng và mật khẩu.
Các tham số sau đây đại diện cho hai điểm truy cập vào ứng dụng:https://www.example.com/appx?a=1&b=1
- Trong trường hợp này, ứng dụng hiển thị hai điểm truy cập (tham số a và b). Tất cả các điểm đầu vào được tìm thấy trong giai đoạn này đều đại diện cho một mục tiêu thử nghiệm. Việc theo dõi thư mục hoặc cây lệnh gọi của ứng dụng và tất cả các điểm truy cập có thể hữu ích trong quá trình thử nghiệm đang diễn ra.


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

---

> ## 2.4 🌐 Liệt kê các ứng dụng trên máy chủ web

---

### 📌 2.4.1 Mục tiêu kiểm tra

- Liệt kê các ứng dụng đang tồn tại và hoạt động trên **máy chủ web mục tiêu**.

---

### ⚙️ 2.4.2 Các bước kiểm tra

---

#### 🛰️ **Bước 1: Quét dịch vụ bằng Nmap**

🔧 **Lệnh:**  
```bash
nmap testphp.vulnweb.com
```

🖥️ **Kết quả:**
```
PORT   STATE SERVICE
80/tcp open  http
```

📌 **Phân tích:**

- Chỉ có cổng **80 (HTTP)** được mở → Hệ thống cố tình **giới hạn bề mặt tấn công**.
- **Không sử dụng HTTPS** → Có thể bị tấn công **sniffing / MITM** trên môi trường không an toàn.

---

#### 🔎 **Hiển thị banner dịch vụ**

🔧 **Lệnh:**  
```bash
nmap -sV testphp.vulnweb.com
```

🖥️ **Kết quả:**
```
80/tcp open  tcpwrapped
```

📌 **Phân tích:**

- Kết quả là `tcpwrapped` → Máy chủ **từ chối hoặc che giấu banner**, không cho phép Nmap lấy thông tin dịch vụ.

---

#### 📡 **Bước 2: Liệt kê ứng dụng bằng Netcat**

🔧 **Lệnh:**
```bash
nc testphp.vulnweb.com 80
GET / HTTP/1.1
Host: testphp.vulnweb.com
```

🖥️ **Kết quả:**
```
HTTP/1.1 200 OK
Server: nginx/1.19.0
Date: Mon, 04 Aug 2025 16:36:06 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
X-Powered-By: PHP/5.6.40-38+ubuntu20.04.1+deb.sury.org+1
```

📌 **Phân tích:**

- ✅ **Phản hồi HTTP 200 OK** → Ứng dụng hoạt động bình thường.
- 🌐 **Web Server:** `nginx/1.19.0`
- 💻 **Ngôn ngữ máy chủ:** `PHP/5.6.40`
- 🐧 **Hệ điều hành:** `Ubuntu 20.04` (suy ra từ chuỗi `.deb.sury.org`)
- 🔁 **Transfer-Encoding:** `chunked` → Truyền dữ liệu theo từng phần.

---

#### 📬 **Bước 3: Tra cứu DNS**

🔧 **Lệnh:**  
```bash
nslookup testphp.vulnweb.com
```

🖥️ **Kết quả:**
```
Name:   testphp.vulnweb.com
Address: 44.228.249.3
```

📌 **Phân tích:**

- Tên miền `testphp.vulnweb.com` ánh xạ tới địa chỉ IP `44.228.249.3`.
- Truy vấn DNS đi qua **DNS nội bộ** (`192.168.198.2`) và là **non-authoritative**.

---

#### 🔁 **Bước 4: Tra cứu tên miền ngược**

🌐 **Dùng công cụ:** `viewdns.info`

📌 **Kết quả:**

- Chỉ duy nhất 1 tên miền được host trên máy chủ này:  
  → `vulnweb.com`

---

#### 🧭 **Bước 5: Xác định máy chủ định danh (Name Server)**

🔧 **Lệnh:**
```bash
host -t ns vulnweb.com
```

🖥️ **Kết quả:**
```
vulnweb.com name server ns1.eurodns.com.
vulnweb.com name server ns2.eurodns.com.
vulnweb.com name server ns3.eurodns.com.
vulnweb.com name server ns4.eurodns.com.
```

📌 **Phân tích:**

- Hệ thống sử dụng dịch vụ **DNS phân tán** từ **EuroDNS** với 4 máy chủ định danh:
  - `ns1.eurodns.com`
  - `ns2.eurodns.com`
  - `ns3.eurodns.com`
  - `ns4.eurodns.com`

---
> ## 2.5 🕵️‍♂️ Kiểm tra nội dung trang web để phát hiện rò rỉ thông tin

---

### 📌 2.5.1 Mục tiêu kiểm tra

- Xem xét **bình luận trong mã nguồn HTML** và **siêu dữ liệu** để tìm các thông tin rò rỉ.
- Phân tích các **tệp JavaScript** để tìm các thông tin nhạy cảm, chức năng ẩn hoặc endpoint API nội bộ.
- Kiểm tra sự tồn tại của **tệp bản đồ nguồn** (`.map`) hoặc **tệp gỡ lỗi**, giúp tái cấu trúc mã gốc hoặc hiểu sâu logic ứng dụng.

---

### ⚙️ 2.5.2 Các bước thực hiện: Sử dụng BurpSuite

---

#### 🧪 **Phát hiện tài khoản mặc định**

- Trang web **công khai username và password mặc định**:
  ```
  Username: test
  Password: test
  ```
![Tài khoản công khai](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/ca53c7ed3b6bc641fd20b4908ec541c052617c85/Thongttindn.jpg)
- 📌 **Rủi ro:** Thông tin dễ bị khai thác bởi attacker thông qua brute-force hoặc login tự động.
---

#### 🧵 **Phân tích comment HTML**

- Mã nguồn HTML có chứa **comment nội bộ tiết lộ cấu trúc template** hoặc tên biến sử dụng bên trong server.

![Comment rò rỉ thông tin](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/26b8836a962545277d5f1a1a029aba30142e050a/Screenshot%20(1298).png)

📌 **Rủi ro:** Những thông tin này giúp attacker hiểu rõ cấu trúc nội bộ → dễ khai thác tấn công chính xác (targeted injection, template injection, SSRF, etc.).

---

#### 📮 **Phân tích Form và Endpoint**

- Mã HTML chứa form đăng nhập như sau:
  ```html
  <form name="loginform" method="post" action="userinfo.php">
  ```

📌 **Phân tích:**

- 🛠️ **Endpoint:** `userinfo.php` là đích đến của dữ liệu đăng nhập → cần kiểm tra kỹ cho các lỗ hổng như:
  - **Brute-force login**
  - **SQL injection (SQLi)**
  - **Credential Stuffing**
- 🧩 **Biến thể POST + JS:** Khi kết hợp với các tệp JavaScript được tải, attacker có thể tái cấu trúc logic xác thực và viết tool tự động đăng nhập/thử tài khoản.

---

> ## 2.6 🎯 Xác định điểm vào ứng dụng

---

### 📌 2.6.1 Mục tiêu kiểm tra

- Xác định các điểm xâm nhập và điểm tiêm nhiễm có thể xảy ra thông qua phân tích yêu cầu và phản hồi.

---

### 🧪 2.6.2 Các bước thực hiện

---

#### 🛠️ **Xác định các phương thức HTTP**

1. **Phương thức POST** – khi người dùng **gửi dữ liệu** đến máy chủ:
   - Dữ liệu nằm trong **body** của request:
     ```
     searchFor=hello&goButton=go
     ```
   - 🔐 POST thường được dùng khi:
     - Gửi form (đăng nhập, tìm kiếm, đăng ký, phản hồi, v.v.)
     - Tránh hiển thị dữ liệu nhạy cảm trên URL
     - Truyền dữ liệu lớn hoặc chứa thông tin riêng tư

   ![Phương thức POST](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/d84204b6e8d3a136f5f614319c41ecc13af8ebe9/POST.jpg)

---

2. **Phương thức GET** – khi người dùng **yêu cầu dữ liệu** từ server (thường là xem nội dung trang web):
   - Không có dữ liệu trong body.
   - Dữ liệu (nếu có) được truyền thông qua **URL query string**:
     ```
     /page.php?id=3
     ```
   - 🔎 GET thường được dùng khi:
     - Truy cập dữ liệu hoặc tải nội dung tĩnh
     - Duyệt trang, theo liên kết
     - Không thay đổi trạng thái hệ thống

   ![Phương thức GET](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/530206f8fdf7aeece16b83d65a8a3e46aea021bd/GET.png)

---

### ✅ Tổng kết

> Việc xác định điểm vào của ứng dụng (entry points) là bước **cốt lõi trong quá trình đánh giá bảo mật web**. Các điểm sử dụng phương thức GET và POST thường là **nơi có thể thử các payload khai thác**, như:
>
> - **XSS (Cross-Site Scripting)**
> - **SQLi (SQL Injection)**
> - **Command Injection**
> - **LFI/RFI (File Inclusion)**
> - **Privilege Escalation**

Việc sử dụng **BurpSuite, proxy hoặc công cụ phân tích HTTP** giúp kiểm tra rõ ràng và ghi nhận chính xác các endpoint có thể bị khai thác.

---
## 2.7 🗺️ Thực thi đường dẫn thông qua ứng dụng

---

### 🎯 2.7.1 Mục tiêu kiểm tra

- Lập bản đồ (mapping) toàn bộ ứng dụng web mục tiêu.
- Hiểu các **luồng xử lý chính** (workflow) và **điểm điều hướng quan trọng** trong hệ thống.

---

### 🧪 2.7.2 Các bước thực hiện

---

#### 🔍 Bước 1: Thêm vào Scope và khởi động Spider

- Vào **tab Target → Site Map → URL view** của BurpSuite.
- Chọn host `testphp.vulnweb.com` để xem chi tiết cấu trúc và phản hồi từng endpoint.

![Spider Scope](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/115c21743e2d383c4f23d0ade2f3ac7c42e44fad/Screenshot%20(1312).png)

**Thông tin quan sát:**
- **Contents:** Liệt kê các URL, phương thức (GET/POST), tham số, status code, MIME type, tiêu đề,...
- **Request / Response:** Hiển thị toàn bộ nội dung HTTP của yêu cầu và phản hồi.

---

#### 🌲 Bước 2: Xem cấu trúc cây đường dẫn (Site Map)

- Sau khi thu thập URL bằng Spider hoặc duyệt tay (manual browsing), chuyển sang **Crawl paths view**.
- Click vào từng node để xem chi tiết các request, issue, và outlink tương ứng.

![Site Map Tree](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/115c21743e2d383c4f23d0ade2f3ac7c42e44fad/Screenshot%20(1313).png)

**Nhận xét:**
- Burp đã mapping được **toàn bộ cấu trúc ứng dụng**.
- Có các đường dẫn dạng: `/login`, `/signup`, `/profile`, `/category?id=...`
- Từ đây, có thể xác định các điểm chức năng quan trọng để phục vụ khai thác như:
  - Đăng nhập
  - Tìm kiếm sản phẩm
  - Quản lý hồ sơ
  - Lọc danh mục, v.v.
- Quá trình này **rất hữu ích** cho việc xác định điểm tiêm nhiễm sau này (trong giai đoạn kiểm thử).

---

## 2.8 🧬 Khung ứng dụng web – Dấu vân tay

---

### 🎯 2.8.1 Mục tiêu kiểm tra

- Xác định **ngôn ngữ lập trình**, **hệ điều hành**, **framework**, **công nghệ frontend/backend** mà ứng dụng đang sử dụng.
- Dựa vào các thông tin này để suy đoán **lỗ hổng tiềm ẩn**.

---

### 🧪 2.8.2 Các bước thực hiện

---

#### 🔎 Bước 1: Fingerprint Web Application Framework

![Framework Info](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/6a8305baca1438013e24cf531fb67c5c008e37ad/Screenshot%20(1314).png)

**Kết quả thu được:**
- **Ngôn ngữ phía server:** PHP
- **Phiên bản PHP:** 5.6.40
- **Hệ điều hành:** Ubuntu 20.04
- **Nguồn build:** từ kho deb.sury.org

🔴 **Cảnh báo:** Phiên bản PHP 5.6 đã **ngừng hỗ trợ từ lâu**, rất nhiều lỗ hổng nghiêm trọng có thể tồn tại.

---

#### 📂 Bước 2: Fingerprint Web Application

![App Info](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/6a8305baca1438013e24cf531fb67c5c008e37ad/Screenshot%20(1315).png)

**Quan sát:**
- Giao diện lỗi thời, sử dụng HTML 4.01, Flash, JS cổ điển.
- Phát triển bằng phần mềm Adobe Dreamweaver.
- Đây là một **ứng dụng mô phỏng để thực hành pentest**, chứa các lỗi bảo mật phổ biến như:
  - SQLi
  - XSS
  - LFI
  - CSRF
  - Cookie hijacking...

---

#### 🔧 Bước 3: Sử dụng công cụ Wappalyzer

- Sử dụng extension Wappalyzer trên trình duyệt để phân tích công nghệ web đang sử dụng.

![Wappalyzer](https://github.com/My-Tien1611/InformationGathering_LeThiMyTien_2/blob/cd11cd95739ed8f701b1425df88fc1330033aae2/Screenshot%20(1317).png)

**Kết quả:**
- **Web server:** Nginx
- **Ngôn ngữ backend:** PHP
- **Client-side:** JavaScript cũ
- **Hệ thống quản lý:** Không có CMS (Content Management System)
- Có nhiều điểm dễ khai thác do công nghệ lỗi thời và thông tin rò rỉ rõ ràng.

---
## 2.9 🗺️ Kiến trúc Ứng dụng – Bản đồ

---

### 🎯 2.9.1 Mục tiêu kiểm tra

- Tạo bản đồ kiến trúc của ứng dụng web mục tiêu.
- Dựa trên thông tin thu thập từ header, HTTP methods, phản hồi lỗi và các quan sát hệ thống để xác định thành phần hệ thống.

---

### 🧪 2.9.2 Các bước thực hiện

---

#### 📥 Bước 1: Phân tích tiêu đề HTTP

```bash
curl -I https://juice-shop.herokuapp.com/
```

**Kết quả:**

```http
HTTP/1.1 200 OK
Accept-Ranges: bytes
Access-Control-Allow-Origin: *
Cache-Control: public, max-age=0
Content-Length: 80117
Content-Type: text/html; charset=UTF-8
Date: Tue, 05 Aug 2025 04:20:22 GMT
Etag: W/"138f5-19878707b21"
Feature-Policy: payment 'self'
Last-Modified: Tue, 05 Aug 2025 04:14:54 GMT
Nel: {...}
Server: Heroku
Via: 1.1 heroku-router
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
```

✅ **Phân tích:**
- Ứng dụng đang được triển khai trên nền tảng **Heroku (PaaS)**.
- Header `Via: heroku-router` cho thấy có sử dụng **reverse proxy** (mặc định của Heroku).
- Cho phép truy cập từ mọi domain (`Access-Control-Allow-Origin: *`).
- Header `X-Content-Type-Options`, `X-Frame-Options` thể hiện một số cấu hình bảo mật cơ bản.
- Không có dấu hiệu rõ ràng về **WAF** (Web Application Firewall).

---

#### 🔧 Bước 2: Kiểm tra phương thức HTTP được hỗ trợ

```bash
curl -X OPTIONS https://juice-shop.herokuapp.com/ -i
```

**Kết quả:**

```http
HTTP/1.1 204 No Content
Access-Control-Allow-Methods: GET,HEAD,PUT,PATCH,POST,DELETE
Access-Control-Allow-Origin: *
Server: Heroku
Via: 1.1 heroku-router
```

✅ **Phân tích:**
- Ứng dụng hỗ trợ đầy đủ các phương thức HTTP chuẩn REST: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`.
- Hỗ trợ **CORS toàn cục** (Access-Control-Allow-Origin: *) → có thể gọi API từ bất kỳ domain nào.
- Cho thấy ứng dụng có khả năng là **RESTful API backend**.

---

#### 🛡️ Bước 3: Kiểm tra Reverse Proxy hoặc WAF

```bash
curl "https://juice-shop.herokuapp.com/' OR '1'='1"
```

**Kết quả:**
```
curl: (3) URL rejected: Malformed input to a URL function
```

✅ **Phân tích:**
- Không thấy thông báo lỗi chi tiết từ phía server.
- Không có bằng chứng rõ ràng rằng có sử dụng **WAF** để chặn payload SQLi.
- Heroku có thể đang xử lý một số lỗi đầu vào thông qua reverse proxy.
- Khả năng cao là payload bị lỗi ở client (curl) do URL malformed, chưa đến được app.

---

### 🧱 Tổng kết kiến trúc ứng dụng

| Thành phần          | Chi tiết                                                                           |
|---------------------|------------------------------------------------------------------------------------|
| **Nền tảng triển khai** | Heroku (PaaS)                                                                    |
| **Reverse Proxy**      | Có – `Via: heroku-router`                                                        |
| **WAF**                | Không rõ ràng – Không thấy dấu hiệu chặn payload rõ                               |
| **Giao diện (Frontend)** | Angular Framework                                                              |
| **Backend**            | Node.js + Express (theo tài liệu OWASP Juice Shop)                               |
| **API hỗ trợ**         | RESTful – GET, POST, PUT, PATCH, DELETE                                          |
| **Database**           | Có – Ứng dụng kết nối CSDL phục vụ cho các chức năng CRUD sản phẩm, đánh giá... |
| **CORS**               | Cho phép toàn bộ origin (`*`)                                                    |

---

🔍 **Kết luận:**  
Ứng dụng OWASP Juice Shop có kiến trúc hiện đại với REST API và phân tách rõ frontend/backend. Tuy nhiên:
- Cho phép mọi origin có thể là lỗ hổng CORS.
- Không có biện pháp chặn payload rõ ràng có thể dẫn đến nguy cơ **SQL Injection** nếu không xử lý input đúng cách.
- Là ứng dụng mô phỏng nên có chủ ý chứa nhiều lỗi bảo mật để phục vụ kiểm thử.

---


