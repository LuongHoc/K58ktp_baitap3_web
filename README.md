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














