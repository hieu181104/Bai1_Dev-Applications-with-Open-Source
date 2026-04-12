# B - CÀI ĐẶT UBUNTU + DOCKER
## 1. Cài đặt Ubuntu
Tải file ISO: truy cập trang `ubuntu.com/download/server` để tải file .iso của Ubuntu 24.04.4 LTS

Công cụ giả lập: VMWare


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
