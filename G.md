# G - Triển khai ứng dụng đến End-User
## 1. Tạo tunnel trong Cloudflare 
Bước 1: Đăng nhập Cloudflare Dashboard → Vào Zero Trust.

Bước 2: Vào Networks → Tunnels → Create Tunnel.

<img width="3069" height="1741" alt="image" src="https://github.com/user-attachments/assets/73e927a9-cd08-4f27-8966-1d5d1cd9b053" />

Bước 3: Đặt tên tunnel: nguyentrunghieu-tunnel → Create Tunnel.

<img width="3071" height="1746" alt="image" src="https://github.com/user-attachments/assets/f3a97bc3-bb0f-4972-b331-4c531de0e316" />

Bước 4: Cloudflare hiển thị lệnh docker run ... có chứa token. Sao chép token trong đó.

<img width="3071" height="1737" alt="Screenshot 2026-04-13 161512" src="https://github.com/user-attachments/assets/0aa7ef8b-038e-4f23-8b87-ef3f25b384d8" />

Tạo file .env để lưu token:
```
cd myapp
nano .env
```

<img width="2343" height="284" alt="image" src="https://github.com/user-attachments/assets/66e590f9-4853-4d18-be4a-80dbc8df1243" />

Tạo file .gitignore:

`nano .gitignore`

Nội dung:

<img width="2293" height="344" alt="image" src="https://github.com/user-attachments/assets/741fc733-1a93-40f5-bbf0-0cea276d2569" />

## 2. Khai báo kết quả vào docker-compose.yml
```
mycloudflared:
  image: cloudflare/cloudflared:latest
  container_name: mycloudflared
  restart: unless-stopped
  command: tunnel --no-autoupdate run --token ${CLOUDFLARE_TOKEN}
```
## 3. Chạy lại hệ thống
```
docker compose down
docker compose up -d
```

<img width="2326" height="477" alt="image" src="https://github.com/user-attachments/assets/dc1dd244-06d0-4d1f-b92e-5f07cd3580fe" />

Kiểm tra kết nối:
<img width="3060" height="1739" alt="image" src="https://github.com/user-attachments/assets/3e97759b-8f76-4ec9-8924-5be1f72fb648" />

## 4. Public ứng dụng
Tại giao diện quản lý Tunnel, chọn tab Routes ->  Add a route.

Bảng Add a route hiện ra, chọn Published application:

<img width="3070" height="1735" alt="image" src="https://github.com/user-attachments/assets/87e62836-8aae-40f7-b9f5-419df7e7be45" />

Điền các thông tin:
- subdomain: www
- domain: nguyentrunghiieu.id.vn
- url: http://mynginx:80

<img width="3071" height="1736" alt="image" src="https://github.com/user-attachments/assets/b480873e-654a-4988-b56c-257dac87d52e" />

Bấm Add route:
<img width="3071" height="1737" alt="image" src="https://github.com/user-attachments/assets/e795dfe9-2b86-47e8-9bae-f8d56a20390e" />

#### Khởi động lại hệ thống
```
cd myapp
docker compose down
docker compose up -d
```

Kiểm tra container:
```
docker logs mycloudflared
```
<img width="2351" height="1210" alt="Screenshot 2026-04-13 171221" src="https://github.com/user-attachments/assets/8aeb3c62-d73d-48a9-9144-a24be6e017a0" />

Xuất hiện dòng Registered tunnel connection là OK.

## 5. Kiểm tra url
Trang web : `https://nguyentrunghiieu.id.vn`

<img width="3070" height="1738" alt="image" src="https://github.com/user-attachments/assets/f5489844-1a7e-4e95-b087-0fffb2834c3d" />
