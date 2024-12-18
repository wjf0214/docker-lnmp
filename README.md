# docker-lnmp

## 项目介绍

1. docker-lnmp 项目帮助开发者快速构建本地开发环境，包括Nginx、PHP、MySQL、Redis 服务镜像，支持配置文件和日志文件映射，不限操作系统；
2. 此项目适合个人开发者本机部署，可以快速切换服务版本满足学习服务新版本的需求； 也适合团队中统一开发环境，设定好配置后一键部署， 便于提高团队开发效率；
3. PHP 支持多版本 包括php5.6、 php7.1、php7.2、php7.3、php7.4、php8.0、php8.1、php8.3 版本；
4. MySQL 支持 5.7 、8.0 版本；
5. Redis 支持 4.0 、5.0 、6.0 版本；
6. PHP 扩展包括了gd、grpc、redis、protobuf、memcached、swoole等；

### 一. [install docker](https://docs.docker.com/get-started/get-docker/)

```shell
$ docker -v
Docker version 27.3.1, build ce12230

$ docker-compose -v
Docker Compose version v2.29.2-desktop.2

```

### 二. download

```shell
$ pwd
/e/data
$ git clone https://github.com/wjf0214/docker-lnmp.git
```

### 三. init

```shell
// 初始化
$ cd docker-lnmp
$ cp .env.example .env
```

### 四. run

```shell
#创建网络，指定子网与.env中配置一致
$ docker network create backend --subnet=172.19.0.0/16
63f425499fcbf7f2cae29f36753b002b10cda12a691f19012a0bf6b8c5839866

$ docker network ls | grep backend
63f425499fcb   backend   bridge    local

#首次执行耗时较久，耐心等待
$ docker-compose up -d nginx php83 mysql redis
$ docker ps
CONTAINER ID   IMAGE               COMMAND                   CREATED        STATUS                   PORTS                                                      NAMES
2d18277b1af5   docker-lnmp-php83   "docker-php-entrypoi…"   3 months ago   Up 6 minutes             0.0.0.0:8883->8883/tcp, 0.0.0.0:9083->9083/tcp, 9000/tcp   php83
f27ab4fd69eb   docker-lnmp-redis   "docker-entrypoint.s…"   3 months ago   Up 6 minutes             0.0.0.0:6379->6379/tcp                                     redis
815d03a77c19   docker-lnmp-mysql   "/entrypoint.sh --de…"   3 months ago   Up 6 minutes (healthy)   0.0.0.0:3306->3306/tcp, 33060/tcp                          mysql
67b87172c42c   docker-lnmp-nginx   "nginx -g 'daemon of…"   3 months ago   Up 6 minutes             0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp                   nginx

```

### 五. test

``` shell
$ cp nginx/conf.d/default.conf.example nginx/conf.d/default.conf
$ docker-compose restart nginx

#绑定本机hosts
127.0.0.1 default.dev.com

```

访问 <http://default.dev.com/> 得到响应 Hello wjf0214! 表示运行成功。


### 六. note

```markdown
默认版本为：
MySQL 5.7
Redis 5.0
可以通过修改 env 文件的 MYSQL_VERSION 、REDIS_VERSION 来选择其他版本
MySQL 和 Redis 切换版本时，注意切换配置文件

项目目录默认为 docker-lnmp/../www 目录
可以通过修改 env 文件的 WEB_ROOT_PATH 来指定其他目录

nginx 虚拟主机配置文件在 docker-lnmp/nginx/conf.d 目录内， 可以参考 default 项目配置。
    
```

### 七. restart | down | rebuild

```shell

#修改配置文件后重启即可
$ docker-compose restart nginx php83 mysql redis
Restarting nginx ... done
Restarting php83 ... done
Restarting mysql ... done
Restarting redis ... done

# 修改 dockerfile 或者 env 文件之后 rebuild 可生效
$ docker-compose up -d --build php83 nginx mysql

# 停止
$ docker-compose stop

# 停止并删除容器
$ docker-compose down

# 停止并删除容器+镜像
$ docker-compose down --rmi all

```

### 目录结构

``` plaintext
├── LICENSE
├── README.md
├── compose.yml
├── mysql
         ├── Dockerfile
         └── docker.cnf
├── nginx
         ├── Dockerfile
         ├── conf.d
                  ├── default.conf
                  ├── fpm
                           ├── php56-fpm
                           ├── php71-fpm
                           ├── php72-fpm
                           ├── php73-fpm
                           ├── php74-fpm
                           ├── php80-fpm
                           └── php81-fpm
                           └── php83-fpm
         ├── nginx.conf
├── php56
         ├── Dockerfile
         └── config
             ├── php-fpm.conf
             ├── php-fpm.d
                      ├── www.conf
                      └── zz-docker.conf
             └── php.ini
├── php71
         ├── Dockerfile
         └── config
             ├── php-fpm.conf
             ├── php-fpm.d
                      ├── www.conf
                      └── zz-docker.conf
             └── php.ini
├── php72
         ├── Dockerfile
         └── config
             ├── php-fpm.conf
             ├── php-fpm.d
                      ├── www.conf
                      └── zz-docker.conf
             └── php.ini
├── php73
         ├── Dockerfile
         └── config
             ├── php-fpm.conf
             ├── php-fpm.d
                      ├── www.conf
                      └── zz-docker.conf
             └── php.ini
├── php74
         ├── Dockerfile
         └── config
             ├── php-fpm.conf
             ├── php-fpm.d
                      ├── www.conf
                      └── zz-docker.conf
             └── php.ini
├── php80
         ├── Dockerfile
         └── config
             ├── php-fpm.conf
             ├── php-fpm.d
                      ├── www.conf
                      └── zz-docker.conf
             └── php.ini
├── php81
         ├── Dockerfile
         └── config
             ├── php-fpm.conf
             ├── php-fpm.d
                      ├── www.conf
                      └── zz-docker.conf
             └── php.ini
├── php83
         ├── Dockerfile
         └── config
             ├── php-fpm.conf
             ├── php-fpm.d
                      ├── www.conf
                      └── zz-docker.conf
             └── php.ini
└── redis
├── Dockerfile
├── redis4.conf
├── redis5.conf
└── redis6.conf

```

### Certbot 申请免费的ssl证书

1. 先配置http可访问， 以 test.wjf0214.vip 为例

```shell
[root@wjf0214 docker-lnmp]# pwd
/data/docker-lnmp
[root@wjf0214 docker-lnmp]# vim nginx/conf.d/test.conf
server {
    listen 80;
    listen [::]:80;

    server_name test.wjf0214.vip;

    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }

    location / {
        charset utf-8;
        default_type text/html;
        return 200 'Hello wjf0214 Test!';
    }
}

[root@wjf0214 docker-lnmp]# docker-compose restart nginx
[+] Running 1/1
 ⠿ Container nginx  Started
[root@wjf0214 docker-lnmp]# curl test.wjf0214.vip
Hello wjf0214 Test!
```

2. 申请ssl证书

```shell
[root@wjf0214 docker-lnmp]# docker-compose run --rm  certbot certonly --webroot --webroot-path /var/www/certbot/ -d test.wjf0214.vip
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Requesting a certificate for test.wjf0214.vip

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/test.wjf0214.vip/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/test.wjf0214.vip/privkey.pem
This certificate expires on 2024-12-18.
These files will be updated when the certificate renews.

NEXT STEPS:
- The certificate will need to be renewed before it expires. Certbot can automatically renew the certificate in the background, but you may need to take steps to enable that functionality. See https://certbot.org/renewal-setup for instructions.
We were unable to subscribe you the EFF mailing list because your e-mail address appears to be invalid. You can try again later by visiting https://act.eff.org.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
[root@wjf0214 docker-lnmp]#
```

3. 修改nginx配置，支持https

```shell
[root@wjf0214 docker-lnmp]# vim nginx/conf.d/test.conf
server {
    listen 80;
    listen [::]:80;

    server_name test.wjf0214.vip;

    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }

    location / {
        return 301 https://test.wjf0214.vip$request_uri;
    }
}

server {
    listen 443 ssl;
    listen [::]:443 ssl;

    server_name test.wjf0214.vip;

    ssl_certificate /etc/nginx/ssl/live/test.wjf0214.vip/fullchain.pem;
    ssl_certificate_key /etc/nginx/ssl/live/test.wjf0214.vip/privkey.pem;

    location / {
        charset utf-8;
        default_type text/html;
        return 200 'Hello wjf0214 Test Https!';
    }
}
[root@wjf0214 docker-lnmp]# docker-compose restart nginx
[+] Running 1/1
 ⠿ Container nginx  Started
[root@wjf0214 docker-lnmp]# curl https://test.wjf0214.vip
Hello wjf0214 Test Https!
```


4. 配置计划任务，每个月月初自动刷新

```shell
#更新https证书
1 1 1 * * cd /e/data/docker-lnmp && docker-compose run --rm certbot renew >> /dev/null 2>&1
```
