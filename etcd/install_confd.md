## 安装Confd

```shell
mkdir -p /opt/confd/bin/
wget https://github.com/kelseyhightower/confd/releases/download/v0.16.0/confd-0.
16.0-linux-amd64
mv confd-0.16.0-linux-amd64  /opt/confd/bin/confd
chmod +x /opt/confd/bin/confd
echo "export PATH=$PATH:/opt/confd/bin" >>/etc/profile
source  /etc/profile
which confd
confd  --help
```
