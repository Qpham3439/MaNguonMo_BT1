# E. Triển khai (level test) ứng dụng
Chuyển vào trong thư mục ~/myapp

Gõ lệnh để docker compose chạy: sẽ run tất cả các service khai báo trong file docker-compose.yml

Lợi ích: Chỉ cần docker-compose up -d là toàn bộ hệ thống (Web + Node-RED + Tunnel) tự chạy

Trước hết, ta thêm cloudflared trong docker-compose.yml:

```
version: "3.8"

services:
  nodered:
    image: nodered/node-red
    container_name: mynodered
    ports:
      - "1880:1880"
    volumes:
      - ./nodered:/data

 cloudflared:
    image: cloudflare/cloudflared
    container_name: cloudflared
    command: tunnel run mytunnel
    restart: always
    volumes:
      - ./cloudflared:/etc/cloudflared
    depends_on:
      - nginx

  nginx:
    image: nginx:latest
    container_name: mynginx
    ports:
      - "80:80"
    volumes:
      - ./myweb:/myweb
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - nodered
```

Kiểm tra các container đang chạy trong docker, nếu có cái nào bị restart cần tìm lỗi rồi edit lại docker-compose.yml

Kiểm tra kiểm thử các service đang chạy độc lập thông qua ip và port của nó: ví dụ mở trình duyệt 192.168.56.10:1880 để check nodered đã chạy chưa
<img width="1914" height="323" alt="image" src="https://github.com/user-attachments/assets/e36c94d8-fd5b-46d4-9a07-70776162324e" />

Sử dụng nodered: kéo nodered http_in , http_response, function : để tạo api get đơn giản (dùng cho /api proxy_pass của nginx)
