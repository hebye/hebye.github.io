## 启动Confd监听

```shell
confd -watch -backend etcd -node http://127.0.0.1:2379
当你启动后，confd就会从etcd获取key的值并填充到Nginx配置文件模板,并更新到/usr/local/nginx/conf/vhost/app01.conf，并nginx reload
```
