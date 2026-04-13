# Deployweb_ubuntu
### 1. Cấu hình Domain với Cloudflare
Trước tiên, e sử dụng một tên miền để thực hành tên "divu.click"
<img width="1890" height="894" alt="Screenshot 2026-04-11 175740" src="https://github.com/user-attachments/assets/68005d80-7b8b-4238-89f8-c4a3a7227dda" />
- Tại giao diện Cloudflare truy cập qua (https://dash.cloudflare.com/{userid}/domains/overview)
- Chọn Import DNS records automatically > 
<img width="1056" height="778" alt="Screenshot 2026-04-11 180902" src="https://github.com/user-attachments/assets/5002d3f2-713a-4eec-b79a-929e64a9f37c" />
<img width="379" height="242" alt="Screenshot 2026-04-11 182028" src="https://github.com/user-attachments/assets/75d93f07-eabd-410a-9e3b-02a79aff6b8b" />
- Cập nhật Namesever cho tên miền này để quản lý miền này trên CF, ta được 2 NS. Vào trang NCC tên miền để update NS này. Đợi 5-10p để nó update.
<img width="1102" height="692" alt="Screenshot 2026-04-11 182058" src="https://github.com/user-attachments/assets/d415e5f4-be2b-4b45-ad48-f47ff2f1cbf8" />

### 2.B – Cài Ubuntu + Docker
Dùng phiên bản Ubuntu 24.04.4 LTS (ISO)
Dùng VMwave > Tạo máy ảo mới > Chọn file Iso. Cấu hình ổn định 2c-2r-20gb
Đến đây ta đặt thông tin login, nameserver, password
<img width="1075" height="252" alt="Screenshot 2026-04-13 191941" src="https://github.com/user-attachments/assets/77f538bc-7505-4cca-a202-8f1e654d5d7e" />
Di chuyển tới đây, Space chọn để cài Install OpenSSH server
<img width="622" height="217" alt="Screenshot 2026-04-13 192126" src="https://github.com/user-attachments/assets/68178562-f251-4bba-a132-6ac86803055f" /> 

Sau khi cài đặt xong, đăng nhập vào Ubuntu với thông tin đăng nhập trước đó để chuyển sang bước tiếp theo
#### B2 – Cài SSH
Lệnh: sudo apt update : update hdh
      sudo apt install openssh-server -y  : Cài SSH.
      <img width="686" height="131" alt="image" src="https://github.com/user-attachments/assets/034f473c-62e0-4b2d-a39b-63cdf3af32e9" /> 
      
      ip a : lấy IP máy ta có 192.168.243.131 => ssh anhtu@192.168.243.131
      <img width="826" height="230" alt="image" src="https://github.com/user-attachments/assets/3b931daf-9513-43a0-ae98-9767db5d5291" />
      ssh anhtu@192.168.243.131  : Tại máy kết nối (Windows) join nó vào Ubuntu
#### B3 – Cài Docker
Lệnh cài: sudo apt install docker.io -y
      sudo usermod -aG docker $USER  : để tiện không cần phải sudo khi dùng Docker
      exit để reboot áp dụng thay đổi
      sudo apt install docker-compose -y  : Cài docker compose
  sudo ufw allow 80 / 1880 / 9630 - Mở các cổng cần thiết
  sudo ufw disable -> tắt để login dễ hơn
<img width="565" height="214" alt="image" src="https://github.com/user-attachments/assets/00f8bee9-d688-408b-8de5-8470da969d8d" />

### 3.C – Tạo thư mục project
- Tạo /my-app : mkdir -p ~/myapp
- join nó: cd ~/myapp
- Tạo nginx, myweb, nodered.
myapp/
 ├── docker-compose.yml
 ├── nginx/
 │    └── nginx.conf
 ├── myweb/
 │    └── index.html
 └── nodered/.
Tạo: nano docker-compose.yml.


### E- Triển khai test
Chạy lại containner: docker-compose up -d
Check: docker-compose ps
<img width="976" height="100" alt="image" src="https://github.com/user-attachments/assets/1d62df5e-75ef-4c02-ad3d-1953fc4462e4" />
<img width="917" height="262" alt="image" src="https://github.com/user-attachments/assets/f94a2522-cc8a-4f49-854d-3a3c4c5b3e2f" />

Chỉnh file index.html:
<img width="735" height="312" alt="image" src="https://github.com/user-attachments/assets/2e56cfeb-6a16-4695-bac6-f16c56696816" />
Ví dụ gọi api 
<img width="970" height="379" alt="image" src="https://github.com/user-attachments/assets/dcc6d3a6-6ff0-4ba4-b501-2651cd40b9a9" />

### G - Triển khai ứng dụng đến End-user
Vào Zero Trust
vào Networks → Tunnels
bấm Create a tunnel
chọn Cloudflared, Đặt tên myapp-tunnel
<img width="882" height="564" alt="image" src="https://github.com/user-attachments/assets/61902c82-f83b-45da-8292-dd8d6dc62060" />
 Chọn subdomain: anhtu.divu.click
 Cấu hình URL: http://nginx:80
Chú ý: + kiểm tra container cùng network
docker inspect nginx | grep Network
docker inspect cloudflared | grep Network
- Giai thích: Trường hợp e rằng cloudflared chạy trong docker nên sử dụng nginx:80

👉 phải thấy cùng network

Kết quả:
<img width="1606" height="724" alt="image" src="https://github.com/user-attachments/assets/076368df-429e-4925-b6ca-de94a1095982" />


#### Đúc kết
1. Tại sao dùng Nginx làm Reverse Proxy?
Nginx đóng vai trò gateway, giúp gom toàn bộ traffic vào một điểm duy nhất rồi phân phối (web, API), tăng bảo mật và tránh phải expose trực tiếp Node-RED ra Internet. Để tránh bị quét dò cổng thì không mở port ra ngoài Internet, mà dùng Cloudflare Tunnel: server chỉ tạo kết nối outbound, nên bên ngoài không scan thấy port nào cả.

2. Mount file vs mount thư mục trong Docker
Mount file dùng cho cấu hình cụ thể (ví dụ nginx.conf), còn mount thư mục dùng cho dữ liệu hoặc source code; thư mục linh hoạt hơn vì chứa nhiều file.

3. Sửa index.html có cập nhật ngay không?
Có . Vì container đọc trực tiếp file từ host thông qua mount, nên thay đổi trên Ubuntu sẽ phản ánh ngay mà không cần rebuild.

4. restart: always / unless-stopped dùng để làm gì?
Giúp container tự khởi động lại khi bị crash hoặc khi hệ thống reboot, đảm bảo dịch vụ luôn chạy ổn định. Ban đầu vì chính không có dòng này nên dịch vụ không tự khởi động khi lỗi văng

5. Dùng chung network + lợi ích
Khai báo chung network trong docker-compose.yml giúp các container giao tiếp bằng tên (ví dụ nodered:1880) thay vì IP, dễ quản lý và mở rộng hệ thống.

6. Đưa Cloudflare Token vào .env + .gitignore
Token là thông tin nhạy cảm, nên lưu trong .env và không commit lên GitHub để tránh bị lộ và bị người khác chiếm quyền tunnel.

7. Tại sao dùng :ro khi mount Nginx config
:ro (read-only) giúp container chỉ đọc file cấu hình, không thể sửa từ bên trong, tránh lỗi hoặc bị ghi đè ngoài ý muốn.

8. Dùng Cloudflare Tunnel có cần mở port không?
Không, vì tunnel tạo kết nối outbound tới Cloudflare, từ đó người dùng truy cập vào mà không phải mở cổng trực tiếp trên server, tăng bảo mật
