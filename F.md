# F - GỠ LỖI
## 1. Nếu có lỗi xẩy ra trong quá trình triển khai docker compose up -d
Kiểm tra nhanh: `docker compose ps` giúp biết container nào đang chạy.

<img width="2329" height="234" alt="image" src="https://github.com/user-attachments/assets/5df7eb8b-9a96-438f-86ce-4f03729c389d" />

Nếu có container nào bị restarting cần xem log để tìm lỗi.

Các lệnh xem log từng service:
# Xem log nginx
```docker logs mynginx```

# Xem log Nodered
```docker logs mynodered```

## 2. 
