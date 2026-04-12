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

### Cấu hình mạng trong Ubuntu và VMWare để cho phép truy cập SSH vào Ubuntu từ CMD của Windows.


## 2. Tìm hiểu các lệnh cơ bản của Ubuntu
- Liệt kê các file trong thư mục: `ls`

- Tạo thư mục: `mkdir nameFolder`

- Chuyển thư mục làm việc: `cd path`

- Copy file: `cp file_nguồn path/file_đích`

- Thay đổi quyền thao tác file: `sudo chmod xxx filename`

- Edit file: `sudo nano tenfile` ; `CTRL+ O` : lưu nội dung sau khi edit ; `CTRL+ X` : thoát edit file

- Xem ip của máy ubuntu: `ip -4 addr`

## 3. Cài đặt docker cho Ubuntu

## 4. Kiểm tra phiên bản docker vừa cài đặt, kiểm tra phiên bản của docker compose

## 5. Cấu hình để docker chạy mà không cần tiền tố sudo

## 6. Tìm hiểu tập lệnh của docker và docker compose
Lệnh Docker cơ bản:

- `docker ps`: Xem các container đang chạy. (Thêm docker ps -a để xem tất cả).

- `docker images`: Xem các image đã tải về máy.

- `docker run -d -p 8080:80 nginx`: Tải và chạy container Nginx ngầm (-d), map cổng 8080 của máy chủ vào cổng 80 của container.

- `docker stop <container_id>`: Dừng một container đang chạy.

Lệnh Docker Compose cơ bản: Dùng để quản lý nhiều container cùng lúc qua file docker-compose.yml

- `docker compose up -d`: Khởi chạy các dịch vụ.

- `docker compose down`: Dừng và xóa các container trong compose.

## 7. Đảm bảo tường lửa trên Ubuntu đã cho phép các cổng 80, 1880, 9630
