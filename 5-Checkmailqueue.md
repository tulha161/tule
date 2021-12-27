# 1. Cách check mailqueue :
- SMTP server của iredmail là Postfix, vì vậy việc check mail queue sẽ tiến hành trên bộ phận gửi mail này.
- cú pháp : `mailq` hoặc `postqueue -p`
- Ví dụ : 
```
root@mail:~# mailq
----Queue ID----- --Size-- ---Arrival Time---- --Sender/Recipient------
4JMhxB6T8Fz9nsB*      1910 Mon Dec 27 09:50:38 tulha@tulha.fun
                                               tulha161@gmail.com

-- 1 Kbytes in 1 Request.

```
- Trong nội dung queue sẽ show được queue ID, Size mail, Thời gian đến SMTP server và địa chỉ gửi/nhận.
