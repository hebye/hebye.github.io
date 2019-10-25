## 配置Confd

- 创建配置目录

```shell
mkdir -p /etc/confd/{conf.d,templates}
conf.d    # 资源模板，下面文件必须以toml后缀
templates # 配置文件模板，下面文件必须以tmpl后缀
```

- 创建资源模板,支持多个资源站点

```shell
[root@test conf.d]# more /etc/confd/conf.d/app01.conf.toml 
[template]
src = "app01.conf.tmpl" # 默认在/etc/confd/templates目录下
dest = "/usr/local/nginx/conf/vhost/app1.conf" #要更新的配置文件
owner = "root"
mode = "0666"
keys = [
	"/nginx",	#检测的key
	"/nq",		#检测的key
]

check_cmd = "/usr/local/nginx/sbin/nginx -t"
reload_cmd = "/usr/local/nginx/sbin/nginx -s reload"
```
- 创建Nginx配置文件模板

```shell
more /etc/confd/templates/app01.conf.tmpl
upstream www_{{getv "/nginx/www/server/server_name"}} {
    {{range getvs "/nginx/www/upstream/*"}}
         server {{.}};
    {{end}}
}

upstream www_{{getv "/nq/server_name"}} {
    {{range getvs "/nq/upstream/*"}}
         server {{.}};
    {{end}}
}
server {
    server_name         {{getv "/nginx/www/server/server_name"}};
    location / {
        proxy_pass        http://www_{{getv "/nginx/www/server/server_name"}};
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
        proxy_redirect    off;
        proxy_connect_timeout 90; 
        proxy_send_timeout 90; 
        proxy_read_timeout 90; 
        proxy_buffer_size 4k; 
        proxy_buffers 4 32k;
        proxy_busy_buffers_size 64k; 
        proxy_temp_file_write_size 64k;
    }
}
server {
    server_name         {{getv "/nq/server_name"}};
    location / {
        proxy_pass        http://www_{{getv "/nq/server_name"}};
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
        proxy_redirect    off;
	proxy_connect_timeout 90; 
        proxy_send_timeout 90; 
        proxy_read_timeout 90; 
        proxy_buffer_size 4k; 
        proxy_buffers 4 32k;
        proxy_busy_buffers_size 64k; 
        proxy_temp_file_write_size 64k;
    }
}
```
