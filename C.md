# C - CẤU HÌNH DOCKER COMPOSE
## 1. Tạo các thư mục
### Cấu trúc thư mục
```
myapp/
├── docker-compose.yml
├── nginx/
│   └── nginx.conf
├── myweb/
│   └── index.html
└── nodered/
│   └── (nhiều file tự sinh)
│   └── settings.js 
```
### Tạo thư mục 
Chạy lệnh: `mkdir myapp`để tạo thư mục

Chuyển vào thư mục myapp: `cd myapp`

Tạo thư mục myweb: `mkdir myweb`

Tạo file index: `nano myweb/index.html`

<img width="2326" height="196" alt="image" src="https://github.com/user-attachments/assets/f043025a-7ee1-4886-823f-095da83419a6" />

Nội dung test file index.html là tên của em:

<img width="2344" height="1216" alt="image" src="https://github.com/user-attachments/assets/f4f3756e-351b-45df-97a9-da90c3b20c35" />

### Tạo file cấu hình Nginx
Tạo thư mục nginx và file cấu hình:
```
mkdir nginx
nano nginx/nginx.conf
```
Nội dung file:
```
events {}

http {
    server {
        listen 80;

        # Sub-domain của em 
        server_name nguyentrunghiieu.id.vn;

        # ── Phục vụ file tĩnh
        # location / trỏ root tới thư mục /myweb trong container
        location / {
            root /myweb;
            index index.html index.htm;
            try_files $uri $uri/ =404;
        }

        # ── Reverse Proxy tới Node-RED
        # Mọi request tới /api sẽ được forward sang Node-RED
        # "mynodered" là tên container, docker network tự resolve DNS
        location /api {
            proxy_pass http://mynodered:1880/;
            proxy_set_header Host              $host;
            proxy_set_header X-Real-IP         $remote_addr;
            proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```
<img width="2351" height="1219" alt="image" src="https://github.com/user-attachments/assets/7993243a-f219-4af6-a0db-cc7e48df4646" />

### Tạo file docker-compose.yml
Tạo thư mục nodered và cấp quyền trước để tránh lỗi Permission Denied:
```
mkdir nodered
sudo chmod 777 nodered
```
Tạo file docker-compose
```
nano docker-compose.yml
```
Nội dung file:
```
services:
  # ── Node-RED
  mynodered:
    image: nodered/node-red
    container_name: mynodered
    restart: unless-stopped
    ports:
      - "1880:1880" 
    volumes:
      - ./nodered:/data 

  # ── Nginx
  mynginx:
    image: nginx
    container_name: mynginx
    restart: always
    ports:
      - "80:80"
    volumes:
      # Mount thư mục ./myweb thành /myweb trong container
      - ./myweb:/myweb:ro
      # Mount file cấu hình nginx
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - mynodered
```
### Khởi chạy NODE-RED để sinh file cấu hình
Chạy riêng Node-RED trước để nó tự sinh file settings.js:
```
docker compose up -d mynodered
```
Sau đó gõ lệnh `ls nodered/` để kiểm tra. Nếu thấy file settings.js xuất hiện là thành công:

<img width="2350" height="305" alt="image" src="https://github.com/user-attachments/assets/47395e35-b851-48b8-8c89-d28e303d1230" />

### Chỉnh sửa file settings.js:
Tạo mã hash cho password: Mật khẩu trong Node-RED không được lưu dưới dạng văn bản (ví dụ "123456") mà phải được mã hóa, chạy lệnh:
```
docker exec -it mynodered node-red admin hash-pw
```
Kết quả nhận được là một chuỗi đã được mã hóa: `$2y$08$xoNk2TtSYdvCz9Uq3HYDquv4KL9OHeWpUoHfXskiOOE0wf5KeJZLq`

Chạy lệnh: `nano nodered/settings.js` để thực hiện chỉnh sửa.

<img width="2339" height="111" alt="image" src="https://github.com/user-attachments/assets/c4b1fbe1-544e-41ff-a263-80cf59e0d027" />

Tìm khối mã có tên adminAuth: (nó thường bị comment bằng dấu // ở đầu dòng). Xóa dấu // để kích hoạt nó và dán đoạn mã hash vào mục password:
```
adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "$2y$08$xoNk2TtSYdvCz9Uq3HYDquv4KL9OHeWpUoHfXskiOOE0wf5KeJZLq",
            permissions: "*"
        }]
},
```
<img width="2347" height="1218" alt="image" src="https://github.com/user-attachments/assets/89421205-a0ea-48d7-87ca-e83c68ae19ad" />

Sau khi sửa, restart NODERED: 

`docker compose restart mynodered`

Chạy:

``` docker compose up -d```

Khi truy cập vào `http:192.168.164.129:1880` nodered sẽ bắt login:

<img width="3069" height="1840" alt="image" src="https://github.com/user-attachments/assets/03056aec-805a-419f-b212-c28462bc6eca" />
