# Doveadm command 
- Thao tác với Doveadm -  Dovecot's administration utility

- https://wiki2.dovecot.org/Tools/Doveadm


## 1. Quota
- Get quota của all user : `doveadm quota get -A`
- Get quota  của user : `doveadm quota get -u "<user-mail>" `
```
root@mail:~# doveadm quota get -u "maivantien@tutrangcubeoxam.online"
Quota name Type    Value   Limit                                                                                     %
user       STORAGE    25 1048576                                                                                     0
user       MESSAGE    24       -                                                                                     0
root@mail:~# doveadm quota get -u "tulha@tulha.fun"
Quota name Type    Value  Limit                                                                                      %
user       STORAGE     1 512000                                                                                      0
user       MESSAGE     3      -                                                                                      0

```

## 2. Search 
- Syntax : `doveadm search [-u <user>|-A] <search query>`
    - `-u <user>` : chỉ định user
    - `-A` : search all user
    - ` <search query> ` : có thể search theo rất nhiều lựa chọn như FROM '<sender-email>', TO '<receiver-email>, mailbox '<mailbox>', ..v.v... Tìm hiểu chi tiết tại : https://wiki2.dovecot.org/Tools/Doveadm/SearchQuery 
- Một số ví dụ :
    - Search ALL mail TO 'tulha161@gmail.com'
```
root@mail:~# doveadm -f tab search -A  TO 'tulha161@gmail.com'
Username        mailbox-guid    uid
maivantien@tutrangcubeoxam.online       20ea4b04c01cb8619c8202004c0d6952        7
maivantien@tutrangcubeoxam.online       20ea4b04c01cb8619c8202004c0d6952        8
maivantien@tutrangcubeoxam.online       20ea4b04c01cb8619c8202004c0d6952        9
maivantien@tutrangcubeoxam.online       20ea4b04c01cb8619c8202004c0d6952        10
maivantien@tutrangcubeoxam.online       20ea4b04c01cb8619c8202004c0d6952        11
maivantien@tutrangcubeoxam.online       20ea4b04c01cb8619c8202004c0d6952        12
maivantien@tutrangcubeoxam.online       20ea4b04c01cb8619c8202004c0d6952        13
maivantien@tutrangcubeoxam.online       20ea4b04c01cb8619c8202004c0d6952        14
maivantien@tutrangcubeoxam.online       20ea4b04c01cb8619c8202004c0d6952        15
maivantien@tutrangcubeoxam.online       20ea4b04c01cb8619c8202004c0d6952        16
maivantien@tutrangcubeoxam.online       20ea4b04c01cb8619c8202004c0d6952        18
tulha@tulha.fun a8b6120c3d26c96190da29004c0d6952        2
postmaster@tutrangcubeoxam.online       900bfc172dbfba61e57e00004c0d6952        1
postmaster@tutrangcubeoxam.online       900bfc172dbfba61e57e00004c0d6952        2
postmaster@tutrangcubeoxam.online       900bfc172dbfba61e57e00004c0d6952        3
postmaster@tutrangcubeoxam.online       900bfc172dbfba61e57e00004c0d6952        4
postmaster@tutrangcubeoxam.online       900bfc172dbfba61e57e00004c0d6952        5

```
    - Search ALL mail FROM 'tulha161@gmail.com' : 
```
root@mail:~# doveadm -f tab search -A  FROM 'tulha161@gmail.com'
Username        mailbox-guid    uid
fail@tutrangcubeoxam.online     d309dd0d7029c961ceae2a004c0d6952        1
maivantien@tutrangcubeoxam.online       4d67973abf1cb861938202004c0d6952        5


```

    - Search mail of user 'tulha@tulha.fun' in all mailbox : 
```
root@mail:~# doveadm search -u 'tulha@tulha.fun' mailbox '*'
a8b6120c3d26c96190da29004c0d6952 1
a8b6120c3d26c96190da29004c0d6952 2
a8b6120c3d26c96190da29004c0d6952 3

```
