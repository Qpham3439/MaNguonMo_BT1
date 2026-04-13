# G. Triển khai ứng dụng đến End-user
## Tạo tunnel (đường hầm)

Trước hết cần cài cloudflared:( do lệnh sudo apt install cloudflared -y của em thông báo không tìm thấy package nên e dùng lệnh khác.)
<img width="1639" height="904" alt="image" src="https://github.com/user-attachments/assets/d311445f-c4e1-42a9-a97a-f8ac39ebfdb6" />

Kiểm tra xem cloudflared đã cài đặt thành công chưa:
<img width="926" height="79" alt="image" src="https://github.com/user-attachments/assets/918c23e3-b3ce-4407-a70f-823b800d8e98" />

Tiếp theo là tạo Tunnel:(dùng Remote-Managed)

Cách này thì đơn giản vì tạo tunnel bằng Web cloudflared có UI và hướng dẫn, chỉ cần dùng token, không cần config.yml và file (tunnelID).json

Bước 1: Tạo tunnel. Vào Networking==>Tunnels==>Create Tunnel:
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/2aaf62f3-3ff2-4970-94f7-578f53910aca" />
<img width="1919" height="1038" alt="image" src="https://github.com/user-attachments/assets/aaaa1724-96b0-4f80-ba2a-a2ab760905f4" />

Bước 2: Copy dòng lệnh màu cam, paste vào Ubuntu (tại vị trí ~/myapp)
<img width="1917" height="695" alt="image" src="https://github.com/user-attachments/assets/fe62b23f-ba22-4b02-b5e5-b8d5ebf53303" />

Bước 3: Convert sang docker-compose

```
cloudflared:
  image: cloudflare/cloudflared:latest
  container_name: cloudflared
  command: tunnel --no-autoupdate run --token <TOKEN>
  restart: always
  depends_on:
    - nginx
```

như này là ok.
<img width="1010" height="514" alt="image" src="https://github.com/user-attachments/assets/82b5f222-4129-4255-acfb-be8e59c769c0" />

Bước 4: Cấu hình Public Hostname
<img width="1900" height="1036" alt="Screenshot 2026-04-13 162311" src="https://github.com/user-attachments/assets/cc8553dd-f492-445d-b0e4-c388dabcc160" />
<img width="616" height="526" alt="Screenshot 2026-04-13 162320" src="https://github.com/user-attachments/assets/a251dbce-cbf5-47a9-a195-00dbabbb4a93" />

Bước 5: Cấu hình lại nginx.conf 
<img width="1521" height="736" alt="image" src="https://github.com/user-attachments/assets/c81d78a2-703f-45d6-82c0-05ff251444a5" />

```
events {}

http {
    server {
        listen 80;
        server_name www.qpham3439.io.vn;

        location / {
            root /myweb;
            index index.html;
        }

        location /api/ {
            proxy_pass http://nodered:1880/;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }
}
```
## Chạy lại docker compose

<img width="1893" height="331" alt="image" src="https://github.com/user-attachments/assets/0aab84fb-4952-4575-9b21-dbff5ea384a3" />

## Kiểm tra url sub-domain đã hoạt động public cho mọi end-user

![z7721222710160_c3ac7377ae88c1e4b3ad68ab0e17fee2](https://github.com/user-attachments/assets/6624248f-82df-4983-8d69-210c8f34ff2d)
