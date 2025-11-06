//////# Lương Văn Học - K225480106025
# K58ktp - Môn phát triển ứng dụng trên nền web
# Nội dung bài tập 3:
Yêu cầu     : LẬP TRÌNH ỨNG DỤNG WEB trên nền linux
1. Cài đặt môi trường linux: SV chọn 1 trong các phương án
 - enable wsl: cài đặt docker desktop
 - enable wsl: cài đặt ubuntu
 - sử dụng Hyper-V: cài đặt ubuntu
 - sử dụng VMware : cài đặt ubuntu
 - sử dụng Virtual Box: cài đặt ubuntu
2. Cài đặt Docker (nếu dùng docker desktop trên windows thì nó có ngay)
3. Sử dụng 1 file docker-compose.yml để cài đặt các docker container sau: 
   mariadb (3306), phpmyadmin (8080), nodered/node-red (1880), influxdb (8086), grafana/grafana (3000), nginx (80,443)
4. Lập trình web frontend+backend:
 SV chọn 1 trong các web sau:
 4.1 Web thương mại điện tử
 - Tạo web dạng Single Page Application (SPA), chỉ gồm 1 file index.html, toàn bộ giao diện do javascript sinh động.
 - Có tính năng login, lưu phiên đăng nhập vào cookie và session
   Thông tin login lưu trong cơ sở dữ liệu của mariadb, được dev quản trị bằng phpmyadmin, yêu cầu sử dụng mã hoá khi gửi login.
   Chỉ cần login 1 lần, bao giờ logout thì mới phải login lại.
 - Có tính năng liệt kê các sản phẩm bán chạy ra trang chủ
 - Có tính năng liệt kê các nhóm sản phẩm
 - Có tính năng liệt kê sản phẩm theo nhóm
 - Có tính năng tìm kiếm sản phẩm
 - Có tính năng chọn sản phẩm (đưa sản phẩm vào giỏ hàng, thay đổi số lượng sản phẩm trong giỏ, cập nhật tổng tiền)
 - Có tính năng đặt hàng, nhập thông tin giao hàng => được 1 đơn hàng.
 - Có tính năng dành cho admin: Thống kê xem có bao nhiêu đơn hàng, call để xác nhận và cập nhật thông tin đơn hàng. chuyển cho bộ phận đóng gói, gửi bưu điện, cập nhật mã COD, tình trạng giao hàng, huỷ hàng,...
 - Có tính năng dành cho admin: biểu đồ thống kê số lượng mặt hàng bán được trong từng ngày. (sử dụng grafana)
 - backend: sử dụng nodered xử lý request gửi lên từ javascript, phản hồi về json.
 4.2 Web IOT: Giám sát dữ liệu IOT.
 - Tạo web dạng Single Page Application (SPA), chỉ gồm 1 file index.html, toàn bộ giao diện do javascript sinh động.
 - Có tính năng login, lưu phiên đăng nhập vào cookie và session
   Thông tin login lưu trong cơ sở dữ liệu của mariadb, được dev quản trị bằng phpmyadmin, yêu cầu sử dụng mã hoá khi gửi login.
   Chỉ cần login 1 lần, bao giờ logout thì mới phải login lại.
 - hiển thị giá trị mới nhất của các thông số đang giám sát, khi click vào thì hiển thị đồ thị lịch sử quá trình thay đổi (gọi grafana iframe để hiển thị)
 - backend: Sử dụng nodered để đọc dữ liệu từ các cảm biến (có thể dùng api online để lấy dữ liệu theo giời gian thực), 
   nodered sẽ lưu dữ liệu mới nhất (dạng update) vào cơ sở dữ liệu mariadb (sử dụng phpmyadmin để tạp table và quản trị lần đầu)
   nodered sẽ lưu dữ liệu (insert) vào influxdb để lưu giá trị lịch sử, để cho grafana dùng để hiển thị biểu đồ.
5. Nginx làm web-server
 - Cấu hình nginx để chạy được website qua url http://fullname.com  (thay fullname bằng chuỗi ko dấu viết liền tên của bạn)
 - Cấu hình nginx để http://fullname.com/nodered truy cập vào nodered qua cổng 80, (dù nodered đang chạy ở port 1880)
 - Cấu hình nginx để http://fullname.com/grafana truy cập vào grafana qua cổng 80, (dù grafana đang chạy ở port 3000)
# -----BÀI LÀM-----

## 1. Chọn phương án Docker Desktop + WSL2

1.1.  Bật WSL2 (Windows Subsystem for Linux)

- Mở PowerShell (Admin) → chạy: wsl --install

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2f5bd8a2-b835-4e31-ae2c-d875501efdbc" />

- Kiểm tra: wsl -l -v


Thấy Ubuntu có “Version 2” là OK.

<img width="1916" height="1028" alt="image" src="https://github.com/user-attachments/assets/4ba9361f-3e07-4d07-98c1-08ab29c7c17f" />

2. Cài Ubuntu
- Nếu WSL chưa cài Ubuntu: wsl --install -d Ubuntu-22.04
- Sau khi cài xong:

Chạy Ubuntu (gõ “Ubuntu” trong Start)

Tạo username hoc và password 123456(nhập 2 lần)

<img width="1893" height="1032" alt="image" src="https://github.com/user-attachments/assets/a1c08ee9-1409-41dc-bd91-20c86ad70e80" />

## 2.Cài đặt Docker

2.1 Tải Docker Desktop

- Vào link chính thức: https://www.docker.com/

Chọn: Tải xuống cho Windows – AMD64

<img width="1903" height="1020" alt="image" src="https://github.com/user-attachments/assets/1028b673-ffbe-4aad-8932-6fc49caf7726" />

2.2 Chạy cài đặt Docker Desktop

- Mở file .exe

<img width="1839" height="959" alt="image" src="https://github.com/user-attachments/assets/d7259166-4ce9-43df-906b-56a732bbf9b2" />

Sau đó bấm OK

2.3 Bật tích hợp WSL 

- Mở Docker Desktop → Settings → Resources → WSL Integration

- Bật:

“Enable integration with my default WSL distro”

“Ubuntu”

Sau đó bấm Apply & Restart

<img width="1916" height="1080" alt="image" src="https://github.com/user-attachments/assets/c6a33091-5bfd-4714-80f9-c77e75f65f4a" />

2.4 Kiểm tra Docker trong Ubuntu

- Mở lại terminal Ubuntu (WSL2) và gõ: docker version

<img width="1892" height="969" alt="image" src="https://github.com/user-attachments/assets/74b19b6c-206c-4868-9d64-85b4227b97e3" />

→ Docker đã hoạt động thành công 🎉

## 3. DỰNG HỆ THỐNG DOCKER BẰNG FILE docker-compose.yml

3.1 Tạo thư mục dự án

- Trong Ubuntu (WSL2), gõ:

cd /mnt/d

mkdir bt3-web-iot

cd bt3-web-iot

<img width="1880" height="975" alt="image" src="https://github.com/user-attachments/assets/516e09b9-b284-44bd-baba-0381843da71d" />

3.2 Tạo file docker-compose.yml

nano docker-compose.yml

<img width="1920" height="1048" alt="image" src="https://github.com/user-attachments/assets/a4f4ac7d-09b8-4f81-b9ab-09929db4893d" />

- Sao chép toàn bộ nội dung bên dưới
```
version: "3.9"

services:
  mariadb:
    image: mariadb:10.11
    container_name: mariadb
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_DATABASE: iotdb
      MYSQL_USER: iotuser
      MYSQL_PASSWORD: iot123
    ports:
      - "3306:3306"
    volumes:
      - ./db_data:/var/lib/mysql
    networks:
      - iotnet

  phpmyadmin:
    image: phpmyadmin/phpmyadmin:latest
    container_name: phpmyadmin
    restart: unless-stopped
    environment:
      PMA_HOST: mariadb
      PMA_USER: root
      PMA_PASSWORD: root123
    ports:
      - "8081:80"         
    depends_on:
      - mariadb
    networks:
      - iotnet

  nodered:
    image: nodered/node-red:latest
    container_name: nodered
    restart: unless-stopped
    ports:
      - "1880:1880"
    volumes:
      - ./nodered_data:/data
    depends_on:
      - mariadb
      - influxdb
    networks:
      - iotnet

  influxdb:
    image: influxdb:1.8
    container_name: influxdb
    restart: unless-stopped
    ports:
      - "8086:8086"
    volumes:
      - ./influxdb_data:/var/lib/influxdb
    networks:
      - iotnet

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    restart: unless-stopped
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin123
      - GF_SECURITY_ALLOW_EMBEDDING=true
    volumes:
      - ./grafana_data:/var/lib/grafana
    depends_on:
      - influxdb
    networks:
      - iotnet

  nginx:                  
    image: nginx:latest
    container_name: nginx
    restart: unless-stopped
    ports:
      - "8080:80"          # web IoT SPA qua Nginx
    volumes:
      - ./frontend:/usr/share/nginx/html:ro
      - ./nginx/conf.d/default.conf:/etc/nginx/conf.d/default.conf:ro
    depends_on:
      - nodered
      - grafana
    networks:
      - iotnet

networks:
  iotnet:
    driver: bridge

```

- Nhấn Ctrl + O → Enter để lưu

- Nhấn Ctrl + X để thoát khỏi nano

3.3 Tạo file nginx.conf

- Trong thư mục /mnt/d/baitap3_web, gõ lệnh: nano nginx.conf


Dán nội dung dưới đây:

```
events {}

http {
  server {
    listen 80;
    server_name hocluong.com;

    # Trang web chính (Frontend)
    location / {
      root /usr/share/nginx/html;
      index index.html;
    }

    # Truy cập Node-RED qua http://hocluong.com/nodered
    location /nodered/ {
      proxy_pass http://nodered:1880/;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
    }

    # Truy cập Grafana qua http://hocluong.com/grafana
    location /grafana/ {
      proxy_pass http://grafana:3000/;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
    }
  }
}
````

Nhấn Ctrl + O → Enter để lưu

Nhấn Ctrl + X để thoát

<img width="1893" height="994" alt="image" src="https://github.com/user-attachments/assets/dd2c0651-af9a-4bc0-b16c-8590c4d50e2a" />

3.4 Tạo thư mục giao diện web

- Trong Ubuntu ( ở thư mục /mnt/d/baitap3_web), gõ: mkdir frontend

- Tạo file index.html cơ bản để kiểm tra: nano frontend/index.html

Dán nội dung dưới đây:

```
<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Website Lương Văn Học</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(135deg, #667eea, #764ba2);
      color: white;
      text-align: center;
      padding: 80px;
    }
    h1 {
      font-size: 48px;
      margin-bottom: 20px;
    }
    p {
      font-size: 20px;
    }
    .btn {
      background-color: white;
      color: #764ba2;
      padding: 12px 25px;
      text-decoration: none;
      border-radius: 8px;
      font-weight: bold;
      transition: 0.3s;
    }
    .btn:hover {
      background-color: #ddd;
    }
  </style>
</head>
<body>
  <h1>🌍 Website Lương Văn Học</h1>
  <p>Chào mừng bạn đến với hệ thống web chạy trên Docker + WSL2</p>
  <a href="/nodered/" class="btn">Truy cập Node-RED</a>
  <a href="/grafana/" class="btn">Xem biểu đồ Grafana</a>
</body>
</html>
```

- Lưu lại file

Ctrl + O → Enter → Ctrl + X

3.5 Chạy toàn bộ hệ thống

- Giờ đã có đủ 3 thành phần:

docker-compose.yml

nginx.conf

frontend/index.html

- Chạy: docker compose up -d


Docker sẽ bắt đầu tải và chạy 6 container:

mariadb, phpmyadmin, nodered, influxdb, grafana, nginx

<img width="1914" height="991" alt="image" src="https://github.com/user-attachments/assets/387be6b8-a53c-4172-8e6b-86d8edb49a1d" />

<img width="1890" height="1000" alt="image" src="https://github.com/user-attachments/assets/ab6427d7-ffc3-404c-9135-656c9aabdbd6" />

- Sau khi chạy xong kiểm tra container: docker ps

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4ae33612-da37-4436-9a5a-7d51f2cc71ae" />

3.6 Kiểm tra trên trình duyệt

Dịch vụ	Cổng	Truy cập
Trang chính (Nginx)	80	http://localhost

phpMyAdmin	8081	http://localhost:8081

Node-RED	1880	http://localhost:1880

Grafana	3000	http://localhost:3000

<img width="1847" height="966" alt="image" src="https://github.com/user-attachments/assets/ec53cd6d-0a40-4689-a777-4616e065f16d" />

<img width="1910" height="987" alt="image" src="https://github.com/user-attachments/assets/69a61d51-406a-4a27-b49a-8a6f2be5f6e8" />

<img width="1903" height="972" alt="image" src="https://github.com/user-attachments/assets/28e047fc-4278-42d9-8517-05d32afcf994" />

<img width="1879" height="990" alt="image" src="https://github.com/user-attachments/assets/fd017a9f-aadc-4ba5-9990-8748c8c2763a" />

## 4. LẬP TRÌNH WEB FRONTEND + BACKEND (WEB IoT)

### 4.1 Thiết kế CSDL MariaDB

Vào http://localhost:8081 → đăng nhập:

•	user: root, pass: root123

•	db: iotdb.

<img width="1920" height="977" alt="image" src="https://github.com/user-attachments/assets/a5c1877a-049e-4e29-aa8a-b3c2ec76ca82" />

Chạy SQL:

```
-- bảng user login
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(50) NOT NULL UNIQUE,
  password_hash VARCHAR(255) NOT NULL
);

-- tạo 1 user: admin / 123456 (base64 là MTIzNDU2)
INSERT INTO users (username, password_hash)
VALUES ('admin', '	MTIzNDU2');

-- bảng giá trị mới nhất của sensor
CREATE TABLE sensor_latest (
  id INT AUTO_INCREMENT PRIMARY KEY,
  sensor_name VARCHAR(50) NOT NULL,
  value DOUBLE NOT NULL,
  updated_at DATETIME NOT NULL,
  UNIQUE KEY uq_sensor (sensor_name)
);
```
<img width="1920" height="974" alt="image" src="https://github.com/user-attachments/assets/f3743ad7-8895-44cd-8b35-1ea275f9e9b7" />

<img width="1917" height="1025" alt="image" src="https://github.com/user-attachments/assets/35c44fe9-9e18-44af-8db8-57783885c11b" />
### 4.2 Kết nối MariaDB & InfluxDB

MySQL:

•	Host: mariadb

•	Port: 3306

•	Database: iotdb

•	User: iotuser

•	Password: iot123

<img width="1871" height="989" alt="image" src="https://github.com/user-attachments/assets/329fe09f-af33-4d21-9341-aef62c7b7dd8" />

InfluxDB config:

•	URL: http://influxdb:8086

•	Database: iotdb.

<img width="1920" height="1006" alt="image" src="https://github.com/user-attachments/assets/29c2f199-f7fb-48a7-b665-3ff2007594c6" />

### 4.2 Cấu hình Node-RED 
- Mở Node-RED

Truy cập: http://localhost:1880

- Cài thêm các node cần thiết

Vào menu → Manage palette → Install

Tìm và cài các gói:

node-red-contrib-influxdb

node-red-node-mysql

node-red-dashboard

node-red-node-random

<img width="1917" height="1021" alt="image" src="https://github.com/user-attachments/assets/5b85bc0c-2fc5-4d16-addf-1afaa7c578bc" />

Hệ thống được thiết kế bằng Node-RED để mô phỏng cảm biến và cung cấp API truy cập dữ liệu. Luồng chính gồm 4 nhóm chức năng:

1️⃣ Sinh dữ liệu cảm biến định kỳ

Node Inject (Tick 5s) → Function (Tạo dữ liệu temp/hum) → InfluxDB out 

Chức năng: mô phỏng dữ liệu nhiệt độ (temp) và độ ẩm (hum) gửi vào cơ sở dữ liệu InfluxDB mỗi 5 giây.

Dữ liệu lưu vào database: iotdb, measurement: sensor_data.

2️⃣ Sinh dữ liệu mẫu khác (tùy chọn)

Node Inject (Tick 10s) → Function (Tạo dữ liệu cảm biến) → InfluxDB out (iotdb)

Dùng để tạo thêm dữ liệu mẫu phục vụ kiểm thử giao diện hiển thị.

3️⃣ API lấy dữ liệu mới nhất

Node HTTP In (GET /api/latest) → Function (Prepare SQL latest) → InfluxDB in (iotdb) → Function (Format JSON) → HTTP Response (200 latest)

Khi truy cập endpoint http://localhost:1880/api/latest, hệ thống trả về giá trị nhiệt độ và độ ẩm mới nhất từ InfluxDB dưới dạng JSON.

4️⃣ API đăng nhập

Node HTTP In (POST /api/login) → Function (Prepare SQL login) → MariaDB (iotdb) → Function (Xử lý kết quả login) → HTTP Response (200 login)

Dùng để xác thực tài khoản từ bảng users trong MariaDB, so sánh username và password_hash (được gửi từ front-end).



### 4.2 Kết nối Grafana và hiển thị biểu đồ

a. Đăng nhập Grafana

- Truy cập:  http://localhost:3000

•	Username: admin

•	Password: admin123 (sau đó nhập mật khẩu mới)

b. Thêm nguồn dữ liệu (Data Source)
- Ở menu bên trái → Connections → Data sources
- Chọn InfluxDB
- Cấu hình như sau:
- URL: http://influxdb:8086
- Database: iotdb
- Query Language: InfluxQL
- User: admin
- Password: admin123
- Nhấn Save & Test 

<img width="1856" height="1041" alt="image" src="https://github.com/user-attachments/assets/939c4fa4-0bc2-4de8-b537-f21c42869b17" />

<img width="1915" height="1027" alt="image" src="https://github.com/user-attachments/assets/4cfd9c82-ea7c-4415-bd6c-22870f498860" />

c. Tạo Dashboard hiển thị dữ liệu
- Vào Dashboards → New → New dashboard
- Add new panel
- Trong phần Query (InfluxQL), nhập lệnh:
```
SELECT mean("temp") AS "Nhiệt độ"
FROM "sensor_data"
WHERE $timeFilter
GROUP BY time($__interval) fill(null)
```
Panel title: Temperature (°C)

<img width="1880" height="964" alt="image" src="https://github.com/user-attachments/assets/11f3148c-9c31-40a8-a929-2e285199ce80" />

Làm tương tự tạo panel thứ hai:
```
SELECT mean("hum") AS "Độ ẩm" 
FROM "sensor_data"
WHERE $timeFilter 
GROUP BY time($__interval) fill(null)
```
Panel title: Humidity (%)

<img width="1920" height="1027" alt="image" src="https://github.com/user-attachments/assets/6378929c-8403-4974-830a-e74eb66b7c1c" />

- Nhấn Apply để lưu panel.



### 4.3 Tạo Frontend (index.html)

a. Trong Ubuntu (WSL), vào thư mục dự án trên ổ D : 

```
cd /mnt/d/bt3-web-iot/frontend
nano index.html

```

<img width="1916" height="1068" alt="image" src="https://github.com/user-attachments/assets/1631a4b6-8d63-4ff2-92a0-978ae8a60ad3" />


b. Tạo file app.js để gọi API Node-RED

Vẫn ở thư mục frontend: nano app.js

<img width="1879" height="984" alt="image" src="https://github.com/user-attachments/assets/9780b2fb-9e53-4591-b265-742a82987287" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cb13d575-b59f-4d8b-b004-18e281f67ac0" />

c. Đảm bảo Nginx đang chạy

Trong Ubuntu (WSL), tại thư mục dự án baitap3_web:

```
cd /mnt/d/baitap3_web
docker compose ps

```

d. Mở web frontend

Trình duyệt ↦ vào:  http://localhost

<img width="1894" height="1017" alt="image" src="https://github.com/user-attachments/assets/ef50fb36-7564-4319-bd04-e18a3736ed06" />















