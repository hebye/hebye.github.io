## 安装Nginx

```shell
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel read
line-devel tk-devel gcc make  pcre-devel wget -y
useradd nginx -s /sbin/nologin
cd /usr/local/src
wget http://nginx.org/download/nginx-1.10.3.tar.gz
tar xf nginx-1.10.3.tar.gz
cd nginx-1.10.3
./configure --prefix=/usr/local/nginx --user=nginx --group=nginx --with-http_ssl
_module --with-http_gzip_static_module --with-http_realip_module --with-http_stu
b_status_module
mkdir /usr/local/nginx/conf/vhost  
cd /usr/local/nginx/conf/
egrep -v "#|^$" nginx.conf.default  >nginx.conf
```

```sh
[root@test conf.d]# more /usr/local/nginx/conf/nginx.conf
worker_processes  2;
events {
    worker_connections  102400;
    use epoll;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    sendfile        on;
    tcp_nopush     on;
    server_tokens off;
    server_names_hash_bucket_size 128;
    client_header_buffer_size 32k;
    large_client_header_buffers 4 32k;
    client_max_body_size 200m;
    keepalive_timeout  30;
    tcp_nodelay on;
    open_file_cache max=102400 inactive=30s;
    open_file_cache_min_uses 1;
    open_file_cache_valid 30s;
    gzip  on;
    gzip_min_length  1k;
    gzip_buffers     4 16k;
    gzip_http_version 1.0;
    gzip_comp_level 2;
    gzip_types       text/plain application/x-javascript text/css application/xm
l;
    gzip_vary on;
    fastcgi_connect_timeout 600;
    fastcgi_send_timeout 600;
    fastcgi_read_timeout 600;
    fastcgi_buffer_size 64k;
    fastcgi_buffers  2 256k;
    fastcgi_busy_buffers_size 256k;
    fastcgi_temp_file_write_size 256k;
    fastcgi_intercept_errors on;
    server_name_in_redirect off;

    server {
        listen       80;
        server_name  localhost;
        access_log  /usr/local/nginx/logs/access.log main;
        location / {
            root   html;
            index  index.html index.htm;
        }
	
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
    include vhost/*.conf;
}

```
