# OpenStack - Identify service 
Cài đặt Openstack Indentify Service - Keystone 

Dịch vụ OpenStack Identity cung cấp một điểm tích hợp duy nhất để quản lý xác thực, ủy quyền và danh mục dịch vụ. Đây là dịch vụ đầu tiên được sử dụng để tương tác với User.

# LAB - Install & Config 

## 1. Chuẩn bị DB Mysql 

- Tạo database, user và grant các quyền .

```
MariaDB [(none)]> CREATE DATABASE keystone;
MariaDB [(none)]> GRANT ALL PRIVILEGES ON keystone.* TO 'keystone'@'localhost' IDENTIFIED BY 'password';
MariaDB [(none)]> GRANT ALL PRIVILEGES ON keystone.* TO 'keystone'@'%' IDENTIFIED BY 'password';
```

## 2. Cài đặt và cáu hình Keystone : 
- Install keystone : 
```
apt install keystone
```
- Chỉnh sửa cấu hình tại `/etc/keystone/keystone.conf` :
```
[database]
connection = mysql+pymysql://keystone:password@controller/keystone
[token]
provider = fernet
```

- db sync : 

```
/bin/sh -c "keystone-manage db_sync" keystone
```

- kiểm tra log file tại `/var/log/keystone/keystone-manage.log`

### Khởi tạo Fernet key repo : 
```
# keystone-manage fernet_setup --keystone-user keystone --keystone-group keystone
# keystone-manage credential_setup --keystone-user keystone --keystone-group keystone
```

### Bootstrap the Identity service : 
```
# keystone-manage bootstrap --bootstrap-password password \
  --bootstrap-admin-url http://controller:5000/v3/ \
  --bootstrap-internal-url http://controller:5000/v3/ \
  --bootstrap-public-url http://controller:5000/v3/ \
  --bootstrap-region-id RegionOne
```
- password : password of admin user 

## 3. Cấu hình apache :

- chỉnh sửa file cấ hình tại `/etc/apache2/apache2.conf`, thêm dòng : 
```
ServerName controller
```
- Restart service :
```
systemctl restart apache2
```
## 4. Cấu hình admin account : 
- Tạo script environtment cho Admin, nội dung : 
```
export OS_USERNAME=admin
export OS_PASSWORD=password
export OS_PROJECT_NAME=admin
export OS_USER_DOMAIN_NAME=Default
export OS_PROJECT_DOMAIN_NAME=Default
export OS_AUTH_URL=http://controller:5000/v3
export OS_IDENTITY_API_VERSION=3
export OS_IMAGE_API_VERSION=2
```
- Chạy scrips : `. admin`
- Kiểm tra hoạt động : `openstack token issue`

## 5. Tạo Domain, projects, users, roles. 
- Dịch vụ Identify cung cấp authen services cho mỗi OpenStack service. Authen service sử dụng sự kết hợp giữa : domains, projects, users và roles.
- Tạo domain mới với `name = test, description = test domain` : 
```
root@controller:/home/tule# openstack domain create --description "test domain" test 
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description | test domain                      |
| enabled     | True                             |
| id          | 712888c798754d6ba6811fc94246a811 |
| name        | test                             |
| options     | {}                               |
| tags        | []                               |
+-------------+----------------------------------+
  
```
- Show các domain mới tạo với `openstack domain list`: 
```
root@controller:/home/tule# openstack domain list
+----------------------------------+---------+---------+--------------------+
| ID                               | Name    | Enabled | Description        |
+----------------------------------+---------+---------+--------------------+
| 712888c798754d6ba6811fc94246a811 | test    | True    | test domain        |
| default                          | Default | True    | The default domain |
+----------------------------------+---------+---------+--------------------+
```

- Tạo project mới `tu-project`, thuộc domain `default` : 
```
root@controller:~# openstack project create --domain default --description "project of tulha" tu-project
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description | project of tulha                 |
| domain_id   | default                          |
| enabled     | True                             |
| id          | 6d007c5fac42413689fd453d895c52b8 |
| is_domain   | False                            |
| name        | tu-project                       |
| options     | {}                               |
| parent_id   | default                          |
| tags        | []                               |
+-------------+----------------------------------+

```
- Tạo user mới `tulha` thuộc domain `default` : 
```
root@controller:~# openstack user create --domain default --password-prompt tulha
User Password:
Repeat User Password:
+---------------------+----------------------------------+
| Field               | Value                            |
+---------------------+----------------------------------+
| domain_id           | default                          |
| enabled             | True                             |
| id                  | 499de361940e441cbbca9b276a8de2be |
| name                | tulha                            |
| options             | {}                               |
| password_expires_at | None                             |
+---------------------+----------------------------------+
```
- Tạo role mới `myrole` :
```
root@controller:~# openstack role create myrole
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description | None                             |
| domain_id   | None                             |
| id          | 6daff73bea8b4337ad041e1baadcdc03 |
| name        | myrole                           |
| options     | {}                               |
+-------------+----------------------------------+
```
- Add `myrole` cho project `tu-project` và user `tulha` vừa tạo ở trên : 
```
openstack role add --project tu-project --user tulha myrole
```

## 6. Kiểm tra  
- Request authentication token from user **admin** : 
```


```
