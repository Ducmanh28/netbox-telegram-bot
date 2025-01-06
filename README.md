# Hướng dẫn cài đặt và sử dụng Bot Tele NetBox
Vui lòng tham khảo file [Intro.md](https://github.com/hocchudong/netbox-telegram-bot/blob/main/Intro.md) trước khi sử dụng chương trình
## I. Chuẩn bị
Trước khi tiến tới cài đặt và sử dụng, bạn sẽ cần:
- Ứng dụng ***Telegram*** và tài khoản
- Thiết bị chạy hệ điều hành **Linux**(*Centos*/*Ubuntu*)
- NetBox(*v4.0.5* hoặc các phiên bản khác)
## II. Cài đặt
### Bước 1: Tạo Bot Telegram
Để tạo được Bot, hãy tham khảo tại [đây](https://core.telegram.org/bots#how-do-i-create-a-bot).

Sau khi đã có **Bot Chat Link** và **Bot Token** là bạn đã hoàn thành quá trình tạo Bot

### Bước 2. Tải xuống File Code
Để tải xuống, bạn có thể tải xuống từng file bởi các lệnh sau:
- Tạo nơi chứa các file:
```
# Tạo thư mục chứa 
mkdir /opt/netbox-telegram

# Truy cập vào thư mục
cd /opt/netbox-telegram
```
- Tải xuống ***Bot_Tele_NetBox.py***:
```
curl -O https://raw.githubusercontent.com/hocchudong/netbox-telegram-bot/refs/heads/main/Bot_Tele_NetBox.py
```
- Tải xuống ***config.py***:  
```
curl -O https://raw.githubusercontent.com/hocchudong/netbox-telegram-bot/refs/heads/main/config.py
```

### Bước 3. Tải xuống các mục cần thiết
Bạn sẽ cần cài đặt các gói sau:
- **python3**
```
# Ubuntu
sudo apt install -y python3

# CentOS
sudo yum install -y python3
```
- Tải xuống gói **venv** để tạo môi trường ảo:
```
# Ubuntu
sudo apt install python3.10-venv

# CentOS
sudo yum install python3.10-venv
```
- Thiết lập môi trường ảo với **python3** trong thư mục `netbox-telegram`:
```
python3 -m venv venv
```

- Kích hoạt môi trường ảo:
```
source venv/bin/activate
```
- Cài đặt các mục cần thiết sử dụng `pip install`
```
pip install pynetbox
pip install python-telegram-bot
```

### Bước 4. Cấu hình trước khi sử dụng

Cấu hình file ***config.py*** như sau:

Các bạn có thể sử dụng ***vim*** để chính sửa file:  `vim /opt/netbox-telegram/config.py` 

- `ADMIN_IDS = [’@example’, Nhập thêm vào đây]` : tại đây, các bạn nhập những user telegram có thể nhận phản hồi từ Bot
- `URLNETBOX = “Nhập tại đây”` : Tại đây, các bạn nhập đường link dẫn tới trang web NetBox của mình
- `TOKENNETBOX = “Nhập tại đây”` : Tại đây, các bạn nhập vào Token API của NetBox. Có thể tạo hoặc lấy ở mục ***ADMIN —> API Tokens*** ở NetBox
- `TOKENTELEGRAM = "Nhập tại đây”` : Tại đây, các bạn nhập vào Token của Bot Telegram mà đã tạo ở trên

Vậy là đã hoàn thành cấu hình
### Bước 5. Cấu hình Bot thành 1 dịch vụ của system
Biến Bot_Tele_NetBox thành 1 dịch vụ và để khởi chạy.

- Cấp quyền khởi chạy cho chương trình
```
chmod +x Bot_Tele_NetBox.py
chmod +x config.py
```
- Tạo 1 file dịch vụ cho bot
```
vim /etc/systemd/system/netboxinfo.service
```
- Thêm vào nội dung sau:
```
[Unit]
Description= Get data on netbox
After=network.target

[Service]
PermissionsStartOnly=True
User=root
Group=root
ExecStart=/opt/netbox-telegram/venv/bin/python3 /opt/netbox-telegram/Bot_Tele_NetBox.py
Restart=always
WorkingDirectory=/opt/netbox-telegram

[Install]
WantedBy=multi-user.target
```
- Lưu file vào bắt đầu kiểm tra service:
```
systemctl daemon-reload
systemctl start netboxinfo
systemctl enable netboxinfo
systemctl status netboxinfo
```
## III. Một vài hình ảnh sử dụng
Trước tiên, hãy truy cập vào Bot Chat ở ứng dụng Telegram của bạn
- Khởi đầu, hãy nhập câu lệnh `/start` 

![](/Anh/Screenshot_1003.png)

- Truy cập Menu hiển thị các câu lệnh với lệnh `/help`:
![](/Anh/Screenshot_967.png)

Sử dụng một số chức năng của Bot

- Tìm kiếm Device theo tên:

![](/Anh/Screenshot_968.png)

- Hiển thị danh sách máy ảo theo hệ điều hành
  - Xem báo cáo tên hệ điều hành
  - Tìm kiếm các máy ảo theo tên hệ điều hành

![](/Anh/Screenshot_976.png)

- Xem báo cáo Virtual Machine theo Platform và hiển thị thông vin Virtual Machine:

![](/Anh/Screenshot_973.png)

- Xem báo cáo tổng quát:

![](/Anh/Screenshot_969.png)

**Lưu ý:** Hãy lưu ý khi xem danh sách các tên thiết bị vì để sử dụng được định dạng Markdown, một số tên thiết bị đã bị thay đổi để phù hợp hơn cho việc hiển thị dưới dạng Markdown. Việc copy và sử dụng nguyên tên từ cách danh sách có thể sẽ khiến việc tìm kiếm tên thiết bị không cho ra kết quả vì bị sai tên.

Và còn nhiều chức năng khác nữa, hãy tự khám phá nhé!
## IV. Tham khảo
Nếu bạn quan tâm chi tiết hơn tool này, thãy tham khảo file [Intro](https://github.com/hocchudong/netbox-telegram-bot/blob/main/Intro.md)