# DevOps Hackathon DE002 – Vũ Đình Kiên

## Thông tin sinh viên

| Trường          | Giá trị                             |
|-----------------|-------------------------------------|
| Họ tên          | Vu Dinh Kien                        |
| Linux username  | `kienvu-k24cntt2`                   |
| Lớp             | K24 CNTT2                           |
| Trường          | PTIT                                |
| Repository      | `Devops-Hackathon-De004-VuDinhKien` |
| Nhánh           | `main`                              |
| PORT cá nhân    | **8081**                            |

---

## Cấu trúc repository

```
devops-hackathon-de004-vudinhkien/
├── src/
│   └── index.html                  ← Trang web thông tin cá nhân
├── nginx/
│   └── kienvu-k24cntt2.conf  ← Server block Nginx
├── screenshots/
│   ├── 01-user.png
│   ├── 02-nginx.png
│   ├── 03-ufw.png
│   ├── 04-website.png
│   ├── 05-git-log.png
│   └── 06-update.png
├── .gitignore
└── README.md
```

---

## Thông số triển khai

| Thông số            | Giá trị                                          |
|---------------------|--------------------------------------------------|
| Hệ điều hành        | Ubuntu 26.04                                     |
| Thư mục triển khai  | `/var/www/devops-hackathon-de004/vudinhkien`     |
| Web root (Nginx)    | `/var/www/devops-hackathon-de004/vudinhkien/src` |
| Cổng Nginx (PORT)   | `8081`                                           |
| `server_name`       | Địa chỉ IP máy chủ (xem `hostname -I`)           |
| File server block   | `kienvu-k24cntt2.conf`                           |
| UFW rule            | `sudo ufw allow 8081/tcp`                        |
| URL truy cập        | `http://<IP_máy_chủ>:8081`                       |

---

## Hướng dẫn triển khai

### Phần 1 – Chuẩn bị môi trường

#### 1.1 Tạo tài khoản người dùng

```bash
# Tạo user với home dir và shell /bin/bash
sudo useradd -m -s /bin/bash kienvu-k24cntt2

# Đặt mật khẩu
sudo passwd Cocailon10206

# Thêm vào group sudo (secondary group)
sudo usermod -aG sudo kienvu-k24cntt2

# Kiểm tra
id kienvu-k24cntt2
```

#### 1.2 Cài đặt phần mềm

```bash
sudo apt update && sudo apt install -y nginx git ufw curl

# Bật và khởi động Nginx
sudo systemctl enable nginx
sudo systemctl start nginx

# Cấu hình Git
git config --global user.name "KienVu102"
git config --global user.email "vudinhkien1002@gmail.com"
```

---

### Phần 2 – Xây dựng website & quản lý mã nguồn bằng git

```bash
# Clone repository về máy chủ
cd /var/www/Devops-Hackathon-De004-VuDinhKien
git clone https://github.com/KienVu102/Devops-Hackathon-De004-VuDinhKien

# Hoặc pull khi đã clone rồi
git pull origin main
```

---

### Phần 3 – Triển khai website với Nginx & tường lửa

```bash
# Copy server block vào sites-available
sudo cp nginx/kienvu-k24cntt2.conf /etc/nginx/sites-available/kienvu-k24cntt2.conf

# Tạo symlink sang sites-enabled
sudo ln -s /etc/nginx/sites-available/kienvu-k24cntt2.conf \
           /etc/nginx/sites-enabled/kienvu-k24cntt2.conf

# Xoá site default để tránh xung đột cổng 80
sudo rm -f /etc/nginx/sites-enabled/default

# Kiểm tra cú pháp
sudo nginx -t

# Reload Nginx
sudo systemctl reload nginx
```

---

### Phần 4 – Cấu hình UFW Firewall

```bash
# Cho phép SSH (giữ kết nối)
sudo ufw allow OpenSSH

# Cho phép cổng cá nhân
sudo ufw allow 8083/tcp

# Bật UFW
sudo ufw enable

# Kiểm tra
sudo ufw status verbose
```

---

### Kết quả

Truy cập website tại: `http://<IP_máy_chủ>:8081`

---
