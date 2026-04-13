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
- URL: /api/student-info
- Bấm Done

<img width="3069" height="1742" alt="image" src="https://github.com/user-attachments/assets/88ef654e-967b-4ab3-85d3-6e14c4c46e34" />

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

Kết quả hiển thị thành công các thông tin cá nhân.

#### 
