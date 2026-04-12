# B - CÀI ĐẶT UBUNTU + DOCKER
## 1. Cài đặt Ubuntu
Tải file ISO: truy cập trang `ubuntu.com/download/server` để tải file .iso của Ubuntu 24.04.4 LTS

Công cụ giả lập: VMWare
### Tạo máy ảo
- Mở VMware, chọn Create a New Virtual Machine.
- Chọn nguồn cài đặt: * Chọn dòng Installer disc image file (iso).
- Nhấn Browse và tìm đến file ISO Ubuntu Server vừa tải về.
- Đặt tên và lưu trữ:
  - Virtual machine name: Đặt tên cho máy ảo (VD: Ubuntu_Server).
  - Location: Chọn nơi lưu trữ ổ cứng ảo (nên chọn ổ đĩa còn trống ít nhất 20-30GB).
- Cấu hình ổ cứng (Disk Capacity):
  - Maximum disk size: Tối thiểu 20GB.
  - Chọn Split virtual disk into multiple files để dễ dàng di chuyển máy ảo sau này.
- Tùy chỉnh phần cứng (Customize Hardware):
  - Memory (RAM): Tối thiểu 1GB, nhưng nên để 2GB (2048MB) để chạy mượt mà.
  - Processors: Chọn 1 hoặc 2 nhân tùy vào cấu hình máy thật của bạn.
- Nhấn Finish để hoàn tất việc tạo khung máy ảo.
### Cài đặt Ubuntu
Sau khi nhấn Power on this virtual machine, màn hình cài đặt của Ubuntu sẽ hiện ra. Thực hiện các bước sau:
- Language: Chọn English.
- Keyboard Configuration: Chọn English (US).
- Choose type of install: Chọn Ubuntu Server (bản mặc định).
- Networking: Trình cài đặt sẽ tự nhận IP từ VMware qua DHCP. Nhấn Done.
- Storage Configuration: Chọn Use an entire disk.
- Profile Setup:
  - Your name: Hieu
  - Your server's name: hieuserver.
  - Pick a username: nguyentrunghieu
  - Password: (Đặt mật khẩu cho user).
- SSH Setup: Tích chọn Install OpenSSH server.
- Hệ thống sẽ bắt đầu cài đặt. Sau khi hoàn tất chọn Reboot Now.

<img width="1281" height="793" alt="image" src="https://github.com/user-attachments/assets/b6a5bc8e-5e94-4824-9451-27f9153b19d5" />

Sau khi hoàn thành cài đặt, đăng nhập với username và password vừa tạo:

<img width="1297" height="814" alt="image" src="https://github.com/user-attachments/assets/95ffd4ba-04e4-42f7-b3ed-b9ba0523a411" />

Màn hình đăng nhập thành công!
### Cấu hình mạng trong Ubuntu và VMWare để cho phép truy cập SSH vào Ubuntu từ CMD của Windows.
#### Khởi động dịch vụ SSH: 
Thực hiện lần lượt:

`sudo systemctl start ssh`

`sudo systemctl enable ssh`

`sudo systemctl status ssh`

<img width="899" height="421" alt="image" src="https://github.com/user-attachments/assets/9e79bd61-142c-42bc-97e7-3c81302542f7" />

#### Kiểm tra địa chỉ ip ubuntu: `ip -4 addr`
<img width="850" height="159" alt="Screenshot 2026-04-12 231150" src="https://github.com/user-attachments/assets/5f77be0d-8253-49f1-8ee7-3bff34695556" />

#### Kiểm tra SSH từ Windows
Mở cmd từ Windows, gõ lệnh : `ssh nguyentrunghieu@192.168.164.129`

<img width="2346" height="1223" alt="image" src="https://github.com/user-attachments/assets/f3a52839-a8ef-4875-8327-763d87671a4b" />

Hệ thống sẽ yêu cầu nhập password (không hiển thị). Nếu thành công sẽ ubuntu hiển thị chào mừng.

## 2. Tìm hiểu các lệnh cơ bản của Ubuntu
- Liệt kê các file trong thư mục: `ls` , `ls -la` (hiển thị cả file ẩn)
<img width="2348" height="1211" alt="image" src="https://github.com/user-attachments/assets/7b4dfbc2-45eb-4ef4-bd10-5a504e27d5ae" />

- Tạo thư mục: `mkdir nameFolder`
<img width="2358" height="83" alt="image" src="https://github.com/user-attachments/assets/77dab28a-ed78-4b70-adcf-793732afe2d6" />

- Chuyển thư mục làm việc: `cd path` hoặc `cd ~` để quay về home directory
<img width="2340" height="142" alt="image" src="https://github.com/user-attachments/assets/46d0fe24-462c-4727-b054-e8415f1ce878" />

- Copy file: `cp file_nguồn path/file_đích`
<img width="2335" height="153" alt="image" src="https://github.com/user-attachments/assets/92d4e7ef-0058-4c35-acca-cbc0c62f99ac" />

- Thay đổi quyền thao tác file: `sudo chmod xxx filename`
<img width="2336" height="307" alt="image" src="https://github.com/user-attachments/assets/96e41c4e-e9fb-4a81-9213-087893909a46" />

- Edit file: `sudo nano tenfile` ; `CTRL+ O` : lưu nội dung sau khi edit ; `CTRL+ X` : thoát edit file
<img width="2346" height="1222" alt="image" src="https://github.com/user-attachments/assets/5e58a40b-1d6f-43bb-9c71-031b35dc4688" />

- Xem ip của máy ubuntu: `ip -4 addr`
<img width="850" height="159" alt="Screenshot 2026-04-12 231150" src="https://github.com/user-attachments/assets/c45d2cd8-163f-4d2d-bdb8-0196a9b173a6" />

## 3. Cài đặt docker cho Ubuntu
Gõ và chạy lệnh `sudo apt update`

<img width="2341" height="641" alt="image" src="https://github.com/user-attachments/assets/40dd1fa8-59ee-41a2-bc29-044635b344a3" />

Chạy lệnh `sudo apt install -y docker-ce docker-ce-cli containerd.io \
  docker-buildx-plugin docker-compose-pluginy`

## 4. Kiểm tra phiên bản docker vừa cài đặt, kiểm tra phiên bản của docker compose
Kiểm tra phiên bản: 

`docker --version`

`docker compose version`

<img width="2325" height="154" alt="image" src="https://github.com/user-attachments/assets/ea43672e-82fd-4c6d-8580-f95e78c05602" />

## 5. Cấu hình để docker chạy mà không cần tiền tố sudo
Gõ lệnh `sudo usermod -aG docker $USER`

Sau khi chạy lệnh này, đăng xuất khỏi SSH (gõ lệnh exit) và kết nối ssh lại từ Windows thì cấu hình mới có hiệu lực.

<img width="2337" height="746" alt="image" src="https://github.com/user-attachments/assets/f807e38a-dd0d-461d-b1c7-2948e672733e" />

Test thử `docker ps` mà không cần sudo:

<img width="2335" height="117" alt="image" src="https://github.com/user-attachments/assets/c76d7040-6e4e-45fc-87db-c7280ef2e078" />

## 6. Tìm hiểu tập lệnh của docker và docker compose
Lệnh Docker cơ bản:

- `docker ps`: Xem các container đang chạy. (Thêm docker ps -a để xem tất cả).

- `docker images`: Xem các image đã tải về máy.

- `docker run -d -p 8080:80 nginx`: Tải và chạy container Nginx ngầm (-d), map cổng 8080 của máy chủ vào cổng 80 của container.

- `docker stop <container_id>`: Dừng một container đang chạy.

Lệnh Docker Compose cơ bản: Dùng để quản lý nhiều container cùng lúc qua file docker-compose.yml

- `docker compose up -d`: Khởi chạy các dịch vụ.

- `docker compose down`: Dừng và xóa các container trong compose.

## 7. Cấu hình tường lửa UFW: Đảm bảo tường lửa trên Ubuntu đã cho phép các cổng 80, 1880, 9630
Trước khi bật UFW, BẮT BUỘC phải cho phép cổng SSH, nếu không sẽ bị đá văng ra khỏi phiên SSH và không thể kết nối lại được từ Windows.

Thực hiện lần lượt các lệnh:
- `sudo ufw allow 22`       # Cho phép SSH
- `sudo ufw allow 80`        # Cho phép cổng 80 (thường dùng cho Web HTTP)
- `sudo ufw allow 1880`      # Cho phép cổng 1880 (thường dùng cho Node-RED)
- `sudo ufw allow 9630`      # Cho phép cổng 9630 
- `sudo ufw enable`          # Bật tường lửa
- `sudo ufw status`          # Kiểm tra lại trạng thái các cổng đã mở

<img width="2339" height="1222" alt="image" src="https://github.com/user-attachments/assets/86f9151a-cc90-410e-ba62-54755d04bd34" />
