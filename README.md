# Delopweb_ubuntu
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



