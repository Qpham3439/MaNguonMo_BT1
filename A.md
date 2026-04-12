# A. Đăng ký tên miền xịn cho cá nhân
## 1. Đăng ký domain xịn
  - Truy cập trang Mắt bão:
  <img width="1920" height="1080" alt="Screenshot 2026-04-12 152725" src="https://github.com/user-attachments/assets/293d1c02-78a6-45ea-b4fd-55e744198f54" />
  - Sau khi đăng ký, đăng nhập thành công.
  <img width="1920" height="1080" alt="Screenshot 2026-04-12 152751" src="https://github.com/user-attachments/assets/fde1d7af-1847-4ac9-82ef-7d22395c0124" />
  - Xác thực tài khoản eKYC trước khi đăng ký domain.
  <img width="1919" height="1079" alt="Screenshot 2026-04-12 152857" src="https://github.com/user-attachments/assets/2e2d80eb-aef4-4c7d-8058-cdcd37f88cbf" />
  - Chọn đăng ký tên miền:
  <img width="1920" height="1080" alt="Screenshot 2026-04-12 152914" src="https://github.com/user-attachments/assets/e852c2eb-8429-4ee7-be11-a794fb08806e" />
  
  - Tìm kiếm tên miền mong muốn:
  <img width="1919" height="1079" alt="Screenshot 2026-04-12 152946" src="https://github.com/user-attachments/assets/e82cd9db-be83-49d6-9c87-163b4ce53f61" />
  
  - Sau khi chọn tên miền, trang đăng ký tên miền mở ra, có thể bỏ chọn bảo vệ dns. Thực hiện tuần tự các bước và thanh toán.
  <img width="1920" height="1080" alt="Screenshot 2026-04-12 153022" src="https://github.com/user-attachments/assets/b42fdff3-c404-4bcb-96a3-5265bcc63ee9" />
  
  - Sau khi ký hợp đồng, Mắt bão sẽ gửi email về máy. Chú ý là sau 10 ngày không kê khai tên miền thì domain sẽ bị tạm khoá.
  <img width="1919" height="1079" alt="Screenshot 2026-04-12 153434" src="https://github.com/user-attachments/assets/30f2bc02-8e30-4306-a1b8-c073fd096cbd" />
  
  - Ta thực hiện khai báo domain:
  <img width="1919" height="1079" alt="Screenshot 2026-04-12 153249" src="https://github.com/user-attachments/assets/a1d7748f-a8f6-413c-bf62-1d9d79b0b5da" />
  
  - Sau khi khai báo thành công thì ta sẽ nhận được các email thông báo từ công ty CP Mắt bão và Bộ khoa học & Công nghệ:
  <img width="1918" height="1079" alt="Screenshot 2026-04-12 153646" src="https://github.com/user-attachments/assets/1acdfe10-71cb-4a96-b382-28fe98650a5a" />
  <img width="1919" height="1079" alt="Screenshot 2026-04-12 153459" src="https://github.com/user-attachments/assets/0cefd4b9-288e-4c46-a98f-491c46e7af1a" />
  
  - Như vậy là đăng ký domain đã hoàn tất.
## 2. Đăng ký tài khoản cloudflare
  - Trước hết đăng ký tài khoảng cloudflare. Thực hiện tuàn tự các bước là ok.
### Thêm domain đã đăng ký vào trong cloudflare: Nhận 2 dòng namespace
  - Chọn domain, sau đó ấn nút Orboard domain. Nhập domain vừa đăng ký vào.
<img width="1919" height="1079" alt="Screenshot 2026-04-12 154025" src="https://github.com/user-attachments/assets/c098fe2a-b8c3-4901-a44c-c1715dd54006" />

  - Next đến chỗ có 2 dòng nameserver.
### Nhập 2 dòng namespace của cloudflare vào trong trang quản lý DNS record của tên miền đăng ký (vd trên mắt bão)
  - Vào lại trang Mắt bão, vào trang quản trị domain muốn kết nối với cloudflare, dán 2 nameserver vào và ấn Lưu thay đổi:
<img width="1916" height="1076" alt="Screenshot 2026-04-12 154238" src="https://github.com/user-attachments/assets/599ec6ec-1fdd-48d9-ae9a-967d94db1c51" />

  - Quay lại trang Cloudflare, ấn update nameserver
  - Ấn vào check nameservers now. Chờ 1 lúc sẽ được.( 2 cái này em quên chụp ảnh)
  - Kết quả:
<img width="1919" height="1079" alt="Screenshot 2026-04-12 154341" src="https://github.com/user-attachments/assets/911e1a21-437d-4db7-af3d-f1f98977f66a" />

### Hoàn tất nhiệm vụ A.
