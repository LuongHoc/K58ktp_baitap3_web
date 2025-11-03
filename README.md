# Lương Văn Học - K225480106025
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

mkdir baitap3_web

cd baitap3_web

<img width="1895" height="1029" alt="image" src="https://github.com/user-attachments/assets/2f1b2599-7b63-4e8f-b327-5b43f3bd056b" />

3.2 Tạo file docker-compose.yml

nano docker-compose.yml

<img width="1918" height="983" alt="image" src="https://github.com/user-attachments/assets/b707b9e8-5db5-4862-9430-0b6caa2eb861" />

- Sao chép toàn bộ nội dung bên dưới
```
version: "3.8"

services:
  mariadb:
    image: mariadb:10.6
    container_name: mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: webdb
    ports:
      - "3306:3306"
    volumes:
      - mariadb_data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin
    restart: always
    environment:
      PMA_HOST: mariadb
      PMA_USER: root
      PMA_PASSWORD: root
    ports:
      - "8080:80"
    depends_on:
      - mariadb

  nodered:
    image: nodered/node-red
    container_name: nodered
    restart: always
    ports:
      - "1880:1880"
    volumes:
      - nodered_data:/data

  influxdb:
    image: influxdb:1.8
    container_name: influxdb
    restart: always
    ports:
      - "8086:8086"
    volumes:
      - influxdb_data:/var/lib/influxdb

  grafana:
    image: grafana/grafana
    container_name: grafana
    restart: always
    ports:
      - "3000:3000"
    depends_on:
      - influxdb
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin

  nginx:
    image: nginx:latest
    container_name: nginx
    restart: always
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./frontend:/usr/share/nginx/html

volumes:
  mariadb_data:
  influxdb_data:
  nodered_data:
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

phpMyAdmin	8080	http://localhost:8080

Node-RED	1880	http://localhost:1880

Grafana	3000	http://localhost:3000

<img width="1847" height="966" alt="image" src="https://github.com/user-attachments/assets/ec53cd6d-0a40-4689-a777-4616e065f16d" />

<img width="1910" height="968" alt="image" src="https://github.com/user-attachments/assets/c6a88bcb-6cee-4cdb-aded-5d4761fcd558" />

<img width="1903" height="972" alt="image" src="https://github.com/user-attachments/assets/28e047fc-4278-42d9-8517-05d32afcf994" />

<img width="1879" height="990" alt="image" src="https://github.com/user-attachments/assets/fd017a9f-aadc-4ba5-9990-8748c8c2763a" />

## 4. LẬP TRÌNH WEB FRONTEND + BACKEND (WEB IoT)

Mục tiêu: 

- Tạo một web IoT giám sát nhiệt độ – độ ẩm realtime:

- Node-RED sinh dữ liệu cảm biến (giả lập).

- Node-RED lưu vào InfluxDB để hiển thị biểu đồ.

- Frontend index.html gọi API từ Node-RED, hiển thị thông tin hiện tại.

- Grafana vẽ biểu đồ trực quan từ dữ liệu InfluxDB.

4.1 Cấu hình Node-RED (Backend API)
- Mở Node-RED

Truy cập: http://localhost:1880

- Cài thêm các node cần thiết

Vào menu → Manage palette → Install

Tìm và cài 3 gói:

node-red-contrib-influxdb

node-red-dashboard

node-red-node-random

<img width="1917" height="1021" alt="image" src="https://github.com/user-attachments/assets/5b85bc0c-2fc5-4d16-addf-1afaa7c578bc" />

4.3 Tạo Flow mới

Chọn tab mới và tạo các node như sau:

🔹 Flow mô tả:
[Inject] → [Function: Sinh dữ liệu]
     ↘
     [InfluxDB out] (ghi vào DB)
     ↘
     [HTTP In] → [InfluxDB in] → [HTTP Response]

4️⃣ Cấu hình từng node
🟩 Inject node

Name: Cập nhật cảm biến

Interval: every 10 seconds

Output: timestamp

🟧 Function node (sinh dữ liệu)

Double-click và dán:

msg.payload = [
  {
    measurement: "sensors",
    fields: {
      temperature: Math.round(Math.random() * 5 + 25),
      humidity: Math.round(Math.random() * 20 + 50)
    },
    tags: {
      device: "sensor_A1"
    }
  }
];
return msg;

🟪 InfluxDB out

Server: influxdb

Database: iot_data

Measurement: sensors

👉 Sau đó Deploy — dữ liệu sẽ bắt đầu ghi vào InfluxDB 🎉

5️⃣ Tạo API trả JSON cho frontend

Kéo thêm 3 node:

[HTTP In] → [InfluxDB in] → [HTTP Response]

Cấu hình:

HTTP In

Method: GET

URL: /api/sensor

InfluxDB in

Query:

SELECT * FROM sensors ORDER BY time DESC LIMIT 1


HTTP Response

Giữ mặc định

👉 Deploy lại
Mở trình duyệt và thử:
http://localhost:1880/api/sensor

➡️ Nếu hiện ra JSON như:

[
  {
    "time": "2025-11-03T02:20:30Z",
    "temperature": 28,
    "humidity": 61,
    "device": "sensor_A1"
  }
]


→ Là Node-RED backend đã hoạt động OK ✅

🌐 Bước 2 – Tạo Frontend (index.html)

Tạo file trong thư mục /frontend/index.html
(Nó sẽ được Nginx serve qua http://localhost
)

✳️ Nội dung mẫu:
<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>Giám sát IoT - Lương Văn Học</title>
  <style>
    body { font-family: Arial; text-align: center; background: #f7f9fb; }
    h1 { color: #333; }
    .sensor {
      display: inline-block;
      padding: 20px;
      margin: 20px;
      background: #fff;
      border-radius: 10px;
      box-shadow: 0 2px 8px rgba(0,0,0,.1);
    }
  </style>
</head>
<body>
  <h1>🌡️ Giám sát cảm biến IoT (Docker + Node-RED + Grafana)</h1>
  <div id="data">
    <div class="sensor">
      <h3>Nhiệt độ: <span id="temp">--</span> °C</h3>
      <h3>Độ ẩm: <span id="hum">--</span> %</h3>
    </div>
  </div>
  <script>
    async function updateData() {
      const res = await fetch("http://localhost:1880/api/sensor");
      const data = await res.json();
      if (data && data[0]) {
        document.getElementById("temp").innerText = data[0].temperature;
        document.getElementById("hum").innerText = data[0].humidity;
      }
    }
    setInterval(updateData, 5000);
    updateData();
  </script>
</body>
</html>

📊 Bước 3 – Hiển thị biểu đồ trong Grafana

Truy cập 👉 http://localhost:3000

Add Data Source → chọn InfluxDB

URL: http://influxdb:8086

Database: iot_data

Save & Test ✅

Tạo Dashboard → Add Panel → Query:

SELECT mean("temperature") FROM "sensors" WHERE $timeFilter GROUP BY time(10s)


Bạn sẽ thấy biểu đồ realtime chạy rất đẹp 🎉









