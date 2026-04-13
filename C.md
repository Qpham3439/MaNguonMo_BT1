# C. Cấu hình docker compose:
- Tạo thư mục: ~/myapp
- Chuyển vào trong thư mục ~/myapp
- Tạo thư mục: ./myweb
- Tạo file ./myweb/index.html (với nội dung là thông tin cá nhân của em)
<img width="1475" height="756" alt="Screenshot 2026-04-12 093413" src="https://github.com/user-attachments/assets/88329023-282b-4ab4-9e1c-2aba6209d505" />

Tạo file docker-compose.yml để nó sẽ có các dịch vụ sau:
- Khai báo sử dụng nodered/node-red, cổng 1880, dữ liệu nằm tại thư mục ./nodered
- Khai báo sử dụng nginx, cổng 80, cấu hình trong file ./nginx/nginx.conf
- Mount thư mục ./myweb thành thư mục /myweb trong nginx
- Mount file ./nginx/nginx.conf vào file /etc/nginx/nginx.conf trong nginx

<img width="1919" height="905" alt="image" src="https://github.com/user-attachments/assets/c2af7150-42a0-4f22-82a9-33a453fcb5b6" />

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

  myapi:
    build: ./myapi
    container_name: myapi
    ports:
      - "9630:9630"

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

Edit file ./nginx/nginx.conf để: Cấu hình web server cổng 80 server_name là sub-domain (sub-domain tuỳ ý của em) location / trỏ tới root là thư mục /myweb location /api dùng proxy_pass trỏ tới 1 (hoặc nhiều) node http_in của nodered
<img width="1527" height="539" alt="image" src="https://github.com/user-attachments/assets/aa4dabd7-35b2-4dd0-a159-c38aee0b32d2" />

```
events {}

http {
    server {
        listen 80;
        server_name api.qpham3439.io.vn;

        location / {
            root /myweb;
            index index.html;
        }

        location /api {
            proxy_pass http://myapi:9630/;
        }
    }
}

```

Edit file ./nodered/settings.js để nodered bắt buộc đăng nhập
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/8e9342a4-4999-411a-b490-7cbce8cc3589" />

Chạy docker-compose lần đầu để Node-RED tự sinh file cấu hình trong thư mục ./nodered, sau đó mới tiến hành sửa settings.js và restart lại container Tiếp theo sinh hash-pw bằng lệnh ```docker exec -it nodered node-red admin hash-pw```.

Nó sẽ hỏi password, nhập password bạn muốn (lưu ý: nó không hiện chữ khi nhập)
Sau đó copy paste hash-pw đó vào setting.js, nên chú thích bên cạnh để không quên
<img width="1550" height="581" alt="Screenshot 2026-04-13 143510" src="https://github.com/user-attachments/assets/bd720973-89b0-4e59-9511-cfe3c439f03a" />
<img width="1211" height="573" alt="image" src="https://github.com/user-attachments/assets/5770c02a-413f-49cd-a04a-035037a3f18a" />

Sau khi xong hết phần C ta được kết quả:
<img width="1919" height="298" alt="image" src="https://github.com/user-attachments/assets/756fb28c-9cc9-4e13-abad-c20f1d59a51f" />
<img width="1919" height="1079" alt="Screenshot 2026-04-13 143845" src="https://github.com/user-attachments/assets/68d3aeb0-f443-4245-a3c2-5a1f5262a050" />
