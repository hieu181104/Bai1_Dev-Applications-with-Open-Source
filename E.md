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
#### Tạo flow gồm ba node:
```[http in] ──► [function] ──► [http response]```

#### Cấu hình `http in`:

- Method: GET
- URL: /student-info
- Bấm Done

<img width="3071" height="1738" alt="image" src="https://github.com/user-attachments/assets/8fab4bf0-bbf1-4949-ae74-e38623c24ebe" />

#### Cấu hình `funcion`:
```
// Tạo một object chứa toàn bộ thông tin cá nhân
var information = {
    sinhvien: "Nguyễn Trung Hiếu",
    mssv: "K225480106019",
    lop: "K58KTPM",
    chuyennganh: "Công nghệ phần mềm",
    monhoc: "Phát triển ứng dụng với mã nguồn mở",
    domain: "nguyentrunghiieu.id.vn"
};

// Đưa object này vào msg.payload để trả về
msg.payload = information;
return msg;
```

#### Cấu hình `http-response`:
- Status code: `200`
- Bấm Done

#### Bấm nút Deploy
<img width="3070" height="1740" alt="image" src="https://github.com/user-attachments/assets/b41b69f8-feb7-4d9d-9662-304316ba60f2" />

#### Test API Nodered
Nhập địa chỉ: 

```192.168.164.129:1880/api/student-info```

<img width="3071" height="1744" alt="image" src="https://github.com/user-attachments/assets/ec8c17c6-525d-448d-a5b3-72b1cd2f1d6e" />

Kết quả hiển thị thành công các thông tin cá nhân dưới dạng json.

#### Cập nhật nginx.conf
Chạy `nano nginx/nginx.conf`
Sửa:
```
        location /nodered/ {
            proxy_pass http://mynodered:1880/;
            proxy_set_header Host              $host;
            proxy_set_header X-Real-IP         $remote_addr;
            proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
        }
```
<img width="2329" height="348" alt="image" src="https://github.com/user-attachments/assets/90bc8bc3-b947-408e-8a0f-a28a63c07d81" />

## 5. Sửa file index.html
Chạy lệnh để thực hiện sửa đổi file

```
cd ~/myapp
nano myweb/index.html
```

Nội dung file:
```
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Trang cá nhân - Nguyễn Trung Hiếu</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f7f6;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }

        .profile-card {
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            width: 450px;
            overflow: hidden;
            border: 1px solid #eee;
        }

        .card-header {
            background-color: #2c3e50;
            color: white;
            padding: 30px;
            text-align: center;
        }

        .card-header h1 {
            margin: 0;
            font-size: 24px;
        }

        .card-header p {
            margin: 5px 0 0;
            opacity: 0.8;
            font-size: 14px;
        }

        .card-content {
            padding: 30px;
        }

        .info-group {
            display: flex;
            margin-bottom: 15px;
            align-items: center;
        }

        .info-label {
            font-weight: bold;
            color: #7f8c8d;
            width: 140px;
            font-size: 14px;
            text-transform: uppercase;
        }

        .info-value {
            flex: 1;
            color: #2c3e50;
            font-size: 16px;
        }

        .loading {
            color: #7f8c8d;
            font-style: italic;
        }
    </style>
</head>
<body>
    <div class="profile-card">
        <div class="card-header">
            <h1>Thông Tin Sinh Viên</h1>
            <p>Bài tập Mã nguồn mở</p>
        </div>
        <div class="card-content" id="info-container">
            <div class="loading">Đang tải dữ liệu...</div>
        </div>
    </div>

    <script>
        // JS tự động gọi API khi tải trang
        document.addEventListener("DOMContentLoaded", function() {
            // Đường dẫn proxy Nginx đã cấu hình
            fetch('/student-info')
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Mạng bị lỗi, không kết nối được API!');
                    }
                    return response.json();
                })
                .then(data => {
                    // Tạo HTML từ dữ liệu nhận được
                    let container = document.getElementById('info-container');
                    let html = `
                        <div class="info-group">
                            <span class="info-label">Sinh viên</span>
                            <span class="info-value">${data.sinhvien}</span>
                        </div>
                        <div class="info-group">
                            <span class="info-label">MSSV</span>
                            <span class="info-value">${data.mssv}</span>
                        </div>
                        <div class="info-group">
                            <span class="info-label">Lớp</span>
                            <span class="info-value">${data.lop}</span>
                        </div>
                        <div class="info-group">
                            <span class="info-label">Chuyên ngành</span>
                            <span class="info-value">${data.chuyennganh}</span>
                        </div>
                        <div class="info-group">
                            <span class="info-label">Môn học</span>
                            <span class="info-value">${data.monhoc}</span>
                        </div>
                        <div class="info-group">
                            <span class="info-label">Domain</span>
                            <span class="info-value">${data.domain}</span>
                        </div>
                    `;
                    container.innerHTML = html;
                })
                .catch(err => {
                    document.getElementById('info-container').innerText = "Lỗi kết nối API: " + err.message;
                    console.error(err);
                });
        });
    </script>
</body>
</html>
```
Lưu file và thoát: `Ctrl + O` -> `Enter` -> `Ctrl + X`

Reload nginx:

```docker compose exec mynginx nginx -s reload```

## 6. Kiểm tra trang web
