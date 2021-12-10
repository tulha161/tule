# Chuẩn bị các thứ để lab iRedMail :D 
## 1 . Chuẩn bị 
- Mail server : 
    - OS : Ubuntu 20.04 LTS
    - Cấu hình : 4c_8g
    - IP LAN : 10.5.90.134
    - IP WAN : 123.31.11.183
    - SSD : 40GB 
   
- Domain : tutrangcubeoxam.online

## 2 . LAB : 

### Dựng mail server :

- Update & Upgrade server của bạn 
- set hostname to `mail.tutrangcubeoxam.online`
```
hostnamectl set-hostname mail.tutrangcubeoxam.online
hostname -f #to verify config
```
- update `/etc/hosts` : 
```
127.0.0.1       mail.tutrangcubeoxam.online localhost
```
- Download bản cài,  giải nén và chạy script cài đặt : 
```
wget https://github.com/iredmail/iRedMail/archive/1.4.2.tar.gz
tar xvf 1.4.2.tar.gz
cd iRedMail-1.4.2/
bash iRedMail.sh
```
- Sau khi chạy script, sẽ có một wizzard giúp bạn dễ dàng lựa chọn các components để cài đặt (chọn nơi lưu trữ mail, db server, web server, setup domain, các loại password, v.v...). Hoàn thành xong các bước, verify lại và bắt đầu cài đặt thui :D 
- Sau khi cài đặt OK, reboot lại server để mail server làm việc ổn nhất. 
- Tại đây, ta đã có thể truy cập vào các site  :  
    - Admin : https://tutrangcubeoxam.online/iredadmin
    - Email : https://tutrangcubeoxam.online/mail/
    - Email ( SOGo interface) : https://tutrangcubeoxam.online/SOGo/
- Giao diện admin : 
<1.png>
### Testing nội miền 
- Tạo user và gửi test mail nội miền : 
- Ở đây mình tạo 2 user tại dashboard admin (Add -> User) để gửi/nhận email nội miền, giao diện tạo user : 
<2.png>
- 2 User đã tạo như sau : 
<3.png>
- Sau đó, đăng nhập 2 user trên url : https://tutrangcubeoxam.online or https://tutrangcubeoxam.online/SOGo để gửi/nhận mail test :
<4.png>
<5.png>
<6.png>

- Các thao tác gửi/nhận mail nội miền từ 2 user đã thành công. 

