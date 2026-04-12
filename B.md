<img width="1919" height="903" alt="Screenshot 2026-04-09 201445" src="https://github.com/user-attachments/assets/e45cd8b1-db67-4114-8261-385030a878cb" /># B. Cài đặt Ubuntu + Docker
## Cài đặt hệ điều hành Ubuntu 24.04.4 LTS
### Sử dụng một trong các công cụ để giả lập: VirutualBox (Miễn phí).
- Truy cập trang oracle virtualbox downloads Chọn bản dành cho window.
<img width="1919" height="1079" alt="Screenshot 2026-04-12 154600" src="https://github.com/user-attachments/assets/25fdcce8-c98b-4d9a-b6b2-9b98b8ede898" />

- Tải về và cài đặt.
### Download file ubuntu
- Tải ubuntu-24.04.4-live-server-amd64.iso để cài đặt.
### Cấu hình mạng trong Ubuntu (và công cụ giả lập) để cho phép truy cập SSH vào Ubuntu từ cmd của windows
- Cấu hình network Virtualbox Chọn Network ở giao diện chính của Virtualbox. Tab host-only được mở, chọn DHCP ở dưới, bỏ tích enable server để cấu hình ip tĩnh cho các máy ảo.
<img width="1919" height="1079" alt="Screenshot 2026-04-12 154746" src="https://github.com/user-attachments/assets/ac496df4-8eb9-47e2-aa49-8bb79ec0296e" />

- Sang tab Nat network, chuột phải vào khoảng trắng chọn Create, đặt IPv4 Prefix là 10.0.2.0
<img width="1904" height="1062" alt="Screenshot 2026-04-12 154835" src="https://github.com/user-attachments/assets/f088456c-2fc7-4261-8d0d-6f0b72ff70b0" />

- Xong cấu hình mạng Virtualbox.
#### Cấu hình network Ubuntu
  Bước 1: Trước hết cần tạo máy ảo Ubuntu.
- Tạo 1 máy ảo, bấm Create a new virtual machine
<img width="1205" height="493" alt="Screenshot 2026-04-12 154857" src="https://github.com/user-attachments/assets/3f59ebc9-ebb4-49d8-989a-062ba99d49ef" />

- Đặt tên máy ảo là Ubuntu, chọn file iso là file ubuntu-24.04.4-live-server-amd64.iso vừa tải về. Tới bước tiếp theo, đặt mật khẩu và tên user name tuỳ ý(trong bài này em đặt là quynh). Bước tiếp theo là cấu hình phần cứng tuỳ ý, để phù hợp em chọn: Bộ xử lý 4 nhân, Bộ nhớ 4GB, Disk 25GB. Sau khi tạo ta được máy ảo có cấu hình như hình:
<img width="1046" height="884" alt="Screenshot 2026-04-12 154930" src="https://github.com/user-attachments/assets/5067d382-917e-4bd6-8cab-7cd7ed296c99" />

Bước 2: Cấu hình network cho Ubuntu.
- Chọn Setting (bánh răng màu vàng cam). Chọn Network. Ở Adapter 1: Chọn Attached to: Nat network, Name: NatNetwork vừa tạo.
<img width="987" height="693" alt="Screenshot 2026-04-12 155032" src="https://github.com/user-attachments/assets/496e96f6-81b5-4ad8-887a-cc5f606ff447" />

- Ở Adapter 2: Chọn Attached to: Host-only Adapter, Name mặc định
<img width="998" height="788" alt="Screenshot 2026-04-12 155117" src="https://github.com/user-attachments/assets/b73f466f-aada-458a-a468-5578099d32ea" />

Bước 3: Cấu hình ip tĩnh cho Ubuntu.
- Trước hết, strart Ubuntu. Chờ khoảng 10p cho nó chạy các file setup. Sau đó gõ reboot để khởi động lại.
- Sau khi reboot, thì sẽ thiết lập login và password.
- Tiếp theo gõ ```sudo nano /etc/netplan/*.yaml``` Để mở file 50-cloud-init.yaml (đây là file cấu hình mạng mặc định được tạo khi mà đã tạo thành công máy ảo ubuntu); Sau đó gõ đoạn code sau.
```
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: true

    enp0s8:
      dhcp4: no
      addresses:
        - 192.168.56.10/24
```
- Lưu file lại dùng tổ hợp phím Ctrl+O, enter để lưu, Crtl+X để thoát.
- Cuối cùng gõ lệnh sudo ```netplan apply``` để đưa cấu hình mạng mới thiết lập đi vào hoạt động.

Bước 4: Check ip Ubuntu.
- Gõ lệnh ```ip -4 addr``` để kiểm tra:
  + Ta thấy có ip 192.168.56.10/24 trong enp0s8 là ip Ubuntu đã đáp ứng điều kiện cần cho ssh.
  + Ta thấy có ip 10.0.2.x/24 trong enp0s3 là đã sẵn sàng kết nối internet. Ta có thể kiểm tra bằng cách ping tới google.com bằng lệnh ```ping 8.8.8.8```. Các gói tin gửi đi như hình là ok.
<img width="1919" height="1079" alt="Screenshot 2026-04-12 155928" src="https://github.com/user-attachments/assets/47c53472-7515-47f1-8ebc-c151f818d79d" />

Bước 5: Cài SSH cho Ubuntu.
- Ta gõ lần lượt các lệnh: ```sudo apt update``` và ```sudo apt install openssh-server -y```
- Có thể kiểm tra ssh đã cài thành công chưa bằng lệnh ```sudo systemctl status ssh``` ra như hình là ok. Đừng lo nếu thấy inactive(dead), do chưa có client nào kết nối đến nên nó hiện vậy. 
<img width="1512" height="425" alt="Screenshot 2026-04-12 160057" src="https://github.com/user-attachments/assets/a93f477f-866c-401c-b710-70b01eff2b52" />
- Sau khi đã có SSH server rồi thì ta có thể làm việc trên CMD của window sẽ dễ dàng hơn (nó cho phép cuộn chuột, zoom to nhỏ, copy bằng Ctrl+C và pate bằng Chuột phải).
- Mở CMD trên window gõ lệnh ```ssh quynh@192.168.56.10``` (lưu ý, username của Ubuntu đặt là gì thì sẽ thay quynh = cái đó). Sau đó nó bắt chọn yes/no thì gõ yes (Cần tắt gõ tiếng việt), tiếp theo nó bắt nhập mật khẩu, ta nhập mật khẩu đá đặt cho Ubuntu rồi enter là ok. Màn hình chào mừng của Ubuntu hiện ra.
<img width="1919" height="1079" alt="Screenshot 2026-04-12 160227" src="https://github.com/user-attachments/assets/3d0e2946-4ad1-424a-b137-a925f7451cd8" />

- Khi này ta có thể check lại lệnh ```sudo systemctl status ssh``` sẽ thấy nó active ngon lành.
<img width="1428" height="481" alt="image" src="https://github.com/user-attachments/assets/71afcbc9-60b1-4e70-a64b-36f55fe24a25" />

- Ok. Thiết lập mạng xong.
### Tìm hiểu các lệnh cơ bản của ubuntu
#### Các lệnh cần tìm hiểu:
    Liệt kê các file trong thư mục: ls
    Tạo thư mục: mkdir nameFolder
    Chuyển thư mục làm việc: cd path
    Copy file: cp file_nguồn path/file_đích
    Thay đổi quyền thao tác file: sudo chmod xxx filename
    Edit file: sudo nano tenfile
    CTRL+o : lưu nội dung sau khi edit
    CTRL+x : thoát edit file
    Xem ip của máy ubuntu: ip -4 addr
    Bonus: Các lệnh thông dụng khác:
      chmod: Thay đổi quyền truy cập tệp tin/thư mục.
      chown: Thay đổi chủ sở hữu của tệp tin/thư mục.
      cat: Hiển thị nội dung tập tin.
      grep: Tìm kiếm một mẫu văn bản trong file.
      find: Tìm kiếm tệp tin hoặc thư mục dựa trên các tiêu chí
      nano: Trình soạn thảo văn bản trong terminal.
      rm: Xóa tập tin (rm file.txt).
      rmdir: Xóa thư mục rỗng.
      rm -rf: Xóa thư mục và toàn bộ nội dung bên trong (cẩn thận khi sử dụng).
      pwd: Hiển thị đường dẫn đầy đủ của thư mục hiện tại.

### Cài đặt docker cho Ubuntu
Trước hết install docker.io (-y là auto yes), chạy lần lượt:
```
sudo apt update
sudo apt install docker.io -y
```

<img width="1793" height="751" alt="Screenshot 2026-04-09 201440" src="https://github.com/user-attachments/assets/1871839f-646e-4964-bc5d-68206166904b" />

Tiếp theo cài docker-compose. 
<img width="1919" height="903" alt="Screenshot 2026-04-09 201445" src="https://github.com/user-attachments/assets/9b9e6a70-191a-4479-a618-92c552cc7f92" />
Kiểm tra phiên bản docker vừa cài đặt, kiểm tra phiên bản của docker compose

<img width="814" height="120" alt="Screenshot 2026-04-12 160543" src="https://github.com/user-attachments/assets/d7475f33-f839-4d22-9362-f7f86913dcb4" />

Cấu hình để docker chạy mà không cần tiền tố sudo

<img width="948" height="108" alt="Screenshot 2026-04-09 201636" src="https://github.com/user-attachments/assets/13f10c41-989c-4810-8700-b94dad35cbc8" />

Chạy lệnh ```sudo usermod -aG docker $USER``` sau đó SSH lại, chạy thử lệnh không sudo docker ps ra như hình là ok.

<img width="1569" height="319" alt="Screenshot 2026-04-09 201759" src="https://github.com/user-attachments/assets/5ca401e9-6f5d-4ddc-abea-f2884da8fef6" />

Tìm hiểu tập lệnh của docker và docker compose

<img width="1919" height="1079" alt="Screenshot 2026-04-09 201045" src="https://github.com/user-attachments/assets/918fb462-7fd5-45a4-b47b-052a202e8a67" />


Đảm bảo tường lửa trên Ubuntu đã cho phép các cổng 80, 1880, 9630 (Lệnh: sudo ufw allow ...)

<img width="1568" height="459" alt="Screenshot 2026-04-09 202026" src="https://github.com/user-attachments/assets/b16cadec-8b1a-447a-bb45-e2e09623f911" />
