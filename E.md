# E- TRIỂN KHAI (LEVEL TEST) ỨNG DỤNG
## 1. Khởi chạy hệ thống
Di chuyển vào thư mục ~/myapp : `cd ~/myapp`

Chạy lệnh `docker compose up -d` để run tất cả các service đã khai báo trong docker-compose.yml

## 2. Kiểm tra các container
Chạy lệnh `docker ps` để kiểm tra:

<img width="2330" height="259" alt="image" src="https://github.com/user-attachments/assets/7377feec-b444-4aee-adcc-c164c8843599" />

Kết quả kiểm tra cho thấy cả nginx và nodered đang hoạt động.

## 3. Kiểm thử các service
Dựa trên IP máy ảo của em là 192.168.164.129:

#### Kiểm tra Web: Mở trình duyệt gõ http://192.168.164.129 phải thấy trang HTML cá nhân đã tạo ở Bước C.

<img width="3071" height="1736" alt="image" src="https://github.com/user-attachments/assets/db17cc34-866a-4e49-8b12-d19d7e2c0772" />

#### Kiểm tra Node-RED: Gõ http://192.168.164.129:1880. Đăng nhập thành công với user admin và mật khẩu:

<img width="3071" height="1743" alt="image" src="https://github.com/user-attachments/assets/a3acd476-643a-4911-9149-9a130b5f44a8" />

## 4. Sử dụng nodered tạo api get đơn giản
