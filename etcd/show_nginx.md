##  查看生产的nginx配置文件 

```shell
[root@test ~]# more /usr/local/nginx/conf/vhost/app1.conf 
upstream www_a.com {
    
         server 192.168.1.118:8081;
    
         server 192.168.1.118:8082;
    
}

upstream www_c.com {
    
         server 1.1.1.1;
    
         server 1.1.1.2;
    
}
server {
    server_name         a.com;
    location / {
        proxy_pass        http://www_a.com;
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
    server_name         c.com;
    location / {
        proxy_pass        http://www_c.com;
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

