# F - GỠ LỖI
## 1. Nếu có lỗi xẩy ra trong quá trình triển khai docker compose up -d
Kiểm tra nhanh: `docker compose ps` giúp biết container nào đang chạy.

<img width="2329" height="234" alt="image" src="https://github.com/user-attachments/assets/5df7eb8b-9a96-438f-86ce-4f03729c389d" />

Nếu có container nào bị restarting cần xem log để tìm lỗi.

Các lệnh xem log từng service:
- Xem log nginx

```docker logs mynginx```

- Xem log Nodered

```docker logs mynodered```

## 2. Giới hạn resource cho một service (tránh việc một service chiếm quá nhiều RAM)
Thêm giới hạn RAM cho nodered trong docker-compose.yml:

```
  mynodered:
    ...
    deploy:
      resources:
        limits:
          memory: 512M
```

Theo dõi lượng RAM sử dụng: 
```
docker compose stats
```

<img width="2347" height="322" alt="Screenshot 2026-04-13 155606" src="https://github.com/user-attachments/assets/f629820a-f315-409c-98ed-5ead96c29083" />
