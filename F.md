# F. Gỡ lỗi:
Nếu có lỗi xẩy ra trong quá trình triển khai docker compose up -d

Kiểm tra nhanh: docker compose ps giúp biết container nào đang chạy xem log, ví dụ: docker logs cloudflared

Thực chất trong quá trình làm xảy ra rất nhiều lỗi, trong đó khi chạy nodered thường bị lỗi về quyền sở hữu thư mục khiến nodered bị restating liên tục; Khi gặp phải trường hợp đó ta cần set lại quyền truy cập thư mục bằng lệnh sudo chown -R 1000:1000 nodered Dưới đây là giải thích chi tiết từng thành phần lệnh:

sudo: Chạy lệnh với quyền quản trị cao nhất (root). Bạn cần quyền này vì việc thay đổi chủ sở hữu các tệp tin hệ thống hoặc của người dùng khác bị hạn chế. chown: Viết tắt của "change owner". Đây là lệnh dùng để thay đổi chủ sở hữu (user) và nhóm sở hữu (group) của tệp tin hoặc thư mục.
-R: Viết tắt của "Recursive" (đệ quy). Tham số này cực kỳ quan trọng: nó áp dụng thay đổi cho thư mục nodered và tất cả các tệp/thư mục con bên trong nó. 1000:1000:
Số 1000 đầu tiên là UID (User ID): Định danh của người dùng. Trong nhiều hệ thống Linux (như Ubuntu, Debian), 1000 thường là ID của người dùng đầu tiên được tạo ra.
Số 1000 thứ hai là GID (Group ID): Định danh của nhóm người dùng.
Dấu : ngăn cách giữa User và Group.
nodered: Tên của thư mục hoặc tệp tin mà bạn muốn áp dụng thay đổi.

Thêm healthcheck cho myapi trong file docker-compose.yml healthcheck: test: ["CMD", "curl", "-f", "http://localhost:9630"]

```
healthcheck:
    test: ["CMD", "curl", "-f", "http://localhost:9630"]   #gọi API
    interval: 30s   #30s check 1 lần
    timeout: 10s   #tối đa 10s
    retries: 3   #fail 3 lần ==> unhealth (không ổn)
```

giới hạn resource cho một service: (tránh việc 1 service chiếm quá nhiều ram)

```
deploy:
  resources:
    limits:
      memory: 512M
```

Sau khi thêm healthcheck và deaploy:

<img width="977" height="502" alt="image" src="https://github.com/user-attachments/assets/df98ef73-5819-47df-8be6-4949a2710506" />

sử dụng lệnh: docker stats để quan sát lượng ram sử dụng bởi mỗi service (các thông số sẽ thay đổi theo thời gian)

<img width="1763" height="302" alt="image" src="https://github.com/user-attachments/assets/73ec983b-0ba4-444d-a924-f73e55a133bc" />
