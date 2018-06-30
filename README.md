# php-service
阿里云安全组设置：端口，协议，地址段
域名解析：有说明
安全域名：阿里云免费申请
安装 php
安装 php-fpm
安装 mariadb
安装 nginx
配置 /etc/nginx/nginx.conf
server {
    listen 80;
    listen 443 ssl;
    #server_name localhost;
    #ssl on;
    root /var/www/html;
    index index.html index.php index.htm;
    ssl_certificate   cert/214697929930610.pem;
    ssl_certificate_key  cert/214697929930610.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    location ~ [^/]\.php(/|$) {
        fastcgi_pass 127.0.0.1:9000;
        #fastcgi_pass unix:/dev/shm/php-cgi.sock;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_param PATH_INFO $fastcgi_path_info;
        fastcgi_param  SCRIPT_FILENAME  /etc/nginx/html/$fastcgi_script_name;
        fastcgi_index index.php;
        include fastcgi.conf;
        #try_files \$uri \$uri/ /index.php?q=\$uri&\$args;
        }
    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|ico)$ {
        expires 30d;
        access_log off;
        }
    location ~ .*\.(js|css)?$ {
        expires 7d;
        access_log off;
        }
    location / {
    if (!-e $request_filename) {
        rewrite  ^(.*)$  /index.php?s=/$1  last;
        }
    }
 }
