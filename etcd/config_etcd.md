## 配置Etcd

```
etcdctl set /nginx/www/server/server_name a.com
a.com
etcdctl set /nginx/www/upstream/server01 192.168.1.1.118:8081
192.168.1.1.118:8081
etcdctl set /nginx/www/upstream/server02 192.168.1.1.118:8082
192.168.1.1.118:8082

[root@test ~]# etcdctl set /nq/server_name c.com
c.com
[root@test ~]# etcdctl set /nq/upstream/server01 1.1.1.1
1.1.1.1
[root@test ~]# etcdctl set /nq/upstream/server02 1.1.1.2
1.1.1.2

```
