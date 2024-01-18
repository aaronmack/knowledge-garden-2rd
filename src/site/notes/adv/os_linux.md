---
{"dg-publish":true,"permalink":"/adv/os-linux/","title":"OS - Linux","noteIcon":""}
---

# Frp 快速反向代理

如果你有一台独立主机，同时你想暴露自己本地的服务到外部，可以在独立主机上部署frp的服务端，本地部署frp的客户端，这样当外部访问你独立主机的frp就会转发到本地。

## Install

[GitHub - fatedier/frp: A fast reverse proxy to help you expose a local server behind a NAT or firewall to the internet.](https://github.com/fatedier/frp)

在Release页面下载对应的版本。
## Config

服务端

```bash
[common]

bind_port = 12031  // 客户端和服务端连接的端口
token = <your-token: number>

dashboard_port = 12032
dashboard_user = <username>
dashboard_pwd = <password>
enable_prometheus = true  // 普罗米修斯运维服务,可以关闭

log_file = /var/log/frps.log
log_level = info
log_max_days = 3

vhost_http_port = 13040  // 是内部映射到外部的对外端口
vhost_https_port = 13043 // 暂时未用，而是通过nginx配置ssl转发走http

;Bidirectional verification  // 双向验证
tls_only = true
tls_enable = true
tls_cert_file = /root/frp/ca2/server.crt
tls_key_file = /root/frp/ca2/server.key
tls_trusted_ca_file = /root/frp/ca2/ca.crt
```

客户端

```bash
[common]

server_addr = <your-address-ip>
server_port = 12031
token = <your-token: number>  // 与服务端一致

;Bidirectional verification  // 双向验证
tls_enable = true
tls_cert_file = C:/src/ub/frp/ca1/client.crt
tls_key_file = C:/src/ub/frp/ca1/client.key
tls_trusted_ca_file = C:/src/ub/frp/ca1/ca.crt

// 一个配置示例，转发本地的15060端口
//   这样当外部访问域名`sub.xyzzyxwz.top`时就会转发到本地这个端口。
[httpname1-pre]  
type = http
local_port = 15060
local_ip = 127.0.0.1
custom_domains = sub.xyzzyxwz.top
```

双向验证密钥生成

文件 `my-openssl.cnf`，这文件我是不懂的，也不知道从哪拿来的了(●ˇ∀ˇ●)

```bash
[ ca ]
default_ca = CA_default
[ CA_default ]
x509_extensions = usr_cert
[ req ]
default_bits        = 2048
default_md          = sha256
default_keyfile     = privkey.pem
distinguished_name  = req_distinguished_name
attributes          = req_attributes
x509_extensions     = v3_ca
string_mask         = utf8only
[ req_distinguished_name ]
[ req_attributes ]
[ usr_cert ]
basicConstraints       = CA:FALSE
nsComment              = "OpenSSL Generated Certificate"
subjectKeyIdentifier   = hash
authorityKeyIdentifier = keyid,issuer
[ v3_ca ]
subjectKeyIdentifier   = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints       = CA:true
```

进入到与`my-openssl.cnf`文件的同一目录下，执行下方命令，一条一条的执行，注意同时把域名和IP改成自己的。

> [!WARNING] 注意如果在Windows下需要用cygwin的shell去执行。

```bash
# TODO Frp TLS https://www.bookstack.cn/read/frp-0.36-zh/cf470b39f615dd74.md

# 1.Generating a ca Certificate
## Generate the ca key file
openssl genrsa -out ca.key 2048
## Generating a ca Certificate
openssl req -x509 -new -nodes -key ca.key -subj "/CN=xyzzyxwz.top" -days 5000 -out ca.crt

# 2.Generate a server certificate
## Generate the server key file
openssl genrsa -out server.key 2048
## Generate the server CSR file
openssl req -new -sha256 -key server.key \
    -subj "/C=XX/ST=DEFAULT/L=DEFAULT/O=DEFAULT/CN=xyzzyxwz.top" \
    -reqexts SAN \
    -config <(cat my-openssl.cnf <(printf "\n[SAN]\nsubjectAltName=DNS:localhost,IP:127.0.0.1,IP:80.251.217.246,DNS:xyzzyxwz.top")) \
    -out server.csr
## Generate a server certificate
openssl x509 -req -days 365 -sha256 \
	-in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial \
	-extfile <(printf "subjectAltName=DNS:localhost,IP:127.0.0.1,IP:80.251.217.246,DNS:xyzzyxwz.top") \
	-out server.crt

# 3.Generate a client certificate
## Generate the client key file
openssl genrsa -out client.key 2048
## Generate the client CSR file
openssl req -new -sha256 -key client.key \
    -subj "/C=XX/ST=DEFAULT/L=DEFAULT/O=DEFAULT/CN=client.com" \
    -reqexts SAN \
    -config <(cat my-openssl.cnf <(printf "\n[SAN]\nsubjectAltName=DNS:localhost,DNS:example.client.com")) \
    -out client.csr
## Generate a client certificate
openssl x509 -req -days 365 -sha256 \
    -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial \
	-extfile <(printf "subjectAltName=DNS:localhost,DNS:example.client.com") \
	-out client.crt

```

如果成功了，应该是以下几个文件。然后配置文件里会指向这几个文件。

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.3dc6baa8low0.webp" alt="image" width=500/>

# 申请免费 SSL

## Install

使用Let's Encrypt免费的SSL证书。

Linux下安装使用certbot命令行工具，`sudo apt install certbot` 。

我这里的服务机器配置已经有了一个caddy，所以nginx没有去占用80个443端口，所以cerbot我使用的是独立模式。

还有其它的选项比如--nginx，会自动配置好nginx相关的。

```bash
// 有多个域名和子域名使用-d添加多个
certbot certonly --standalone -d xyzzyxwz.top -d server1.xyzzyxwz.top

// 再根据提示一步一步操作，这里等DNS解析需要一段时间
...

//成功后重启nginx服务
systemctl restart nginx
```

## Renew

```bash
crontab -e

0 0,12 * * * python -c 'import random; import time; time.sleep(random.random() * 3600)' && systemctl stop caddy && certbot renew && systemctl start caddy

//有时候这个定时任务不起作用，需要在到期前手动执行一下上述命令
```

## Config

因为在申请SSL时我们用的standalone模式。所以这里我们手动配置SSL。

```
// 第一个服务 （用于frp反向代理）
//   暴露12030端口给外部访问
//   代理内部的端口13040，这个端口是设置在frp服务端的配置文件中的vhost-http
// -----
// `listen 12034 ssl http2;` 同时暴露了12034端口
//     可以给内部的ub-api服务做转发

server {
    listen       12030 default_server;
    listen       [::]:12030 default_server;
    server_name  xyzzyxwz.top;

    underscores_in_headers on;

    listen 12034 ssl http2;
    ssl on;
    ssl_certificate /etc/letsencrypt/live/xyzzyxwz.top/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/xyzzyxwz.top/privkey.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers "ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384";     
    #ssl_prefer_server_ciphers on;
    ssl_session_cache shared:le_nginx_SSL:10m;
    ssl_session_timeout 1440m;
    ssl_session_tickets off;
    ssl_prefer_server_ciphers off;

    error_page 497  https://$host$request_uri;

    #limit_conn perserver 300;
    #limit_conn perip 25;
    #limit_rate 512k;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
        proxy_redirect off;
        #proxy_set_header Host $host;
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Protocol $scheme;
        proxy_set_header X-Url-Scheme $scheme;

        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        proxy_pass http://127.0.0.1:13040;

            add_header Access-Control-Allow-Origin *;
            add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS';
            add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization';
    }

    # If the page is static and does not contain a redirection, this method is OK, or how to change the redirection url with this prefix?
    location ^~ /n1/ {
        proxy_pass http://localhost:8001/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # …

    error_page 404 /404.html;
        location = /40x.html {
    }

    error_page 500 502 503 504 /50x.html;
        location = /50x.html {
    }
}

// 第二个服务（自己serve一些的服务，也可以走ssl），
//   像下面就是暴露9001给外部，监听转发内部8001端口

server{

    listen       9001 default_server;
    listen       [::]:9001 default_server;
    server_name  xyzzyxw.top;

    ssl on;
    ssl_certificate /etc/letsencrypt/live/xyzzyxw.top/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/xyzzyxw.top/privkey.pem;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers "ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384";
    ssl_session_timeout 1440m;
    ssl_session_tickets off;
    ssl_prefer_server_ciphers off;

    location / {
            proxy_pass http://localhost:8001/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection keep-alive;
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```


# 常用命令

有时候我们SSH远程连接到主机后，想执行一个命令，并且在终端意外退出后仍然继续运行。这就需要到screen啦。
```bash
# screen run
yum install screen
screen -S ssl  # start new
screen -r [id] # attach one
  # Ctrl+A D  - will detach
  # Ctrl+D    - will terminate
```

mysql相关。😂🤣😄

```bash
# mysql
mysql -u root -p

-- Re-Increment id
// table_bak是备份出的表
create table table_bak like <table_name>;
insert into table_bak select * from <table_name>;
alter table table_bak drop id;
alter table table_bak add id int(11) primary key auto_increment first;

-- Change Password
ALTER USER 'root'@'%' IDENTIFIED BY '<yourpasswd>'; 
flush privileges;

-- Query users
SELECT User, Host, Grant_priv, Super_priv
FROM mysql.user;


-- Create user with super grant
CREATE USER 'username'@'%' IDENTIFIED BY 'password';
GRANT ALL ON *.* TO 'username'@'%' WITH GRANT OPTION;
#GRANT ALL PRIVILEGES ON *.* TO 'aaron'@'%' WITH GRANT OPTION; 
FLUSH PRIVILEGES;


-- Remove user
DROP USER 'username'@'%';
FLUSH PRIVILEGES;


-- Create database
CREATE DATABASE testdb;
```

# Ubuntu22-04 安装Mongodb

[How to Install MongoDB on Ubuntu 22.04 | Cherry Servers](https://www.cherryservers.com/blog/install-mongodb-ubuntu-22-04)

```bash
# 安装安装过程中所需的先决条件包。
sudo apt install software-properties-common gnupg apt-transport-https ca-certificates -y

# 要安装最新的 MongoDB 软件包，需要将 MongoDB 软件包仓库添加到 Ubuntu 的源代码列表文件中。在此之前，你需要使用 wget 命令在系统中导入 MongoDB 的公钥
curl -fsSL https://pgp.mongodb.com/server-7.0.asc |  sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor

# 在 /etc/apt/sources.list.d 目录中添加 MongoDB 7.0 APT 代码库
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

# 检查它 cat /etc/apt/sources.list.d/mongodb-org-7.0.list
sudo apt update	

# 安装Mongodb
sudo apt install mongodb-org -y

# 启用MongoDB在启动时启动
mongod --version
sudo systemctl status mongod
sudo systemctl start mongod
sudo systemctl enable mongod

# 检查端口是否监听
sudo ss -pnltu | grep 27017

# 创建账号 (这个时候还不需要密码访问)
mongosh
show dbs
use admin
---
db.createUser(
  {
    user: "root",
    pwd: passwordPrompt(),
    roles: [ { role: "userAdminAnyDatabase", db: "admin" }, "readWriteAnyDatabase" ]
 }
)
---
exit

# 配置外部访问 （这个时候就需要密码访问了）

sudo nano /etc/mongod.conf

Change `bindIp: 127.0.0.1` to `bindIp: 0.0.0.0`
Change `port: 27017` to `port: 27070`
Add security:
    authorization: enabled

sudo systemctl restart mongod

mongosh -u <username> -p <password>
```

# Ubuntu22-04 安装postgresql


```bash
# INSTALL postgresql

sudo apt install postgresql
systemctl status postgresql

nano /etc/postgresql/<version>/main/postgresql.conf

: listen_addresses = '*'
: port = <new_port>

sudo -u postgres psql template1
ALTER USER postgres with encrypted password '<password>';

nano /etc/postgresql/<version>/main/pg_hba.conf

: host  all  all 0.0.0.0/0 scram-sha-256

su - postgres
psql

\l   # list database
\du  # list users
drop user IF EXISTS <uasername>;
create database ayon;
create user <username> WITH PASSWORD '<password>';
grant ALL PRIVILEGES ON DATABASE <database> TO <username>;
alter USER <username> WITH SUPERUSER;
alter USER <username> WITH NOSUPERUSER;
alter USER <username> CREATEDB;
```

# Ubuntu22-04 安装redis

```bash
# INSTALL redis

sudo apt install lsb-release curl gpg

curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list

sudo apt-get update
sudo apt-get install redis
systemctl status redis-server

nano /etc/redis/redis.conf

: supervised systemd
: #bind 127.0.0.1 -::1
: port <new-port>

# test
-----
redis-cli
ping
set test "It's working!"
get test
exit
-----

systemctl restart redis-server

# Generate a password (with `openssl rand 60 | openssl base64 -A`)

nano /etc/redis/redis.conf

: requirepass <password>

systemctl restart redis-server

# test
-----
redis-cli
set key1 10
auth <password>
set key1 10
get key1
quit
-----

# more secure info: https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-redis-on-ubuntu-20-04

# ACL
touch /root/users.acl
nano /etc/redis/redis.conf
: aclfile /root/users.acl
: protected-mode no
systemctl restart redis-server

-----
redis-cli
auth <password>

acl users
acl whoami
acl setuser <username>     # 创建
acl setuser <username> on  # 启用
acl list
acl seruser <username> ><password> # 设置密码
acl setuser <username> <<password> # 取消密码
acl deluser <username>
acl cat   # 查看集合
acl setuser <username> on ><password> allkeys allcommands
acl setuser <username> on ><password> ~* &* +@all

acl save  # 持久化
-----
```
# SSH禁用密码登录

发现Mongodb数据库被勒索了，虽然里面没有特别重要的数据，但那些关于Pipeline配置就都没有了~ 如果这是生产环境，后果不堪设想，要定期备份，加强安全性~

```bash
nano /etc/ssh/sshd_config

# 将下面这句话移到sshd_config文件的开头（如果不起作用的话）
PasswordAuthentication no

# 再开启这两句话
PubkeyAuthentication yes
AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2

#重启SSH服务
systemctl restart sshd.service
```