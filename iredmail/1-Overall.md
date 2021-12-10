# 1. iRedMail là gì : 
- iRedMail là một mailserver free. Nó sử dụng shell script để tự động hóa cài đặt và cấu hình các thành phần cho một mail server trên server Linux/BSD. Với iRedMail, việc cài đặt  và cấu hình mail server trở nên dễ dàng.
- Các thành phần :
    - Postfix : SMTP server
    - Dovecot : IMAP server
    - Nginx : Web server 
    - OpenLDAP, MySQL/MariaDB, or PostgreSQL  : Database 
    - amavisd-new : Content Filter 
    - SpamAssassin : Anti-spam
    - ClamAV : Anti-Virus
    - Roundcube : Webmail mặc định
    - SOGo : cũng là webmail nhưng nhiều chức năng hơn :D 
    - Fail2ban : ngăn xâm nhập trái phép 
    - mlmmj : mailing list manager
    - Netdata : monitoring các thông số server
    - iRedAPD : policy server for greylisting

- iRedMail là MIỄN PHÍ, các thành phần sử dụng đều là open-source