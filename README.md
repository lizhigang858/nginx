# nginx
打算跟从https://www.linode.com/docs/web-servers/nginx/nginx-installation-and-basic-setup/
的教程进行，也打算采用docker的方式。所以我先运行了官方的镜像，然后将/etc/nginx目录的内容拷贝出来
，然后再次运行镜像，并将拷贝出来内容作为volume，这样是为了方便编辑配置文件和提交git
```bash
docker pull nginx
docker run -d --name mynginx
docker cp mynginx:/etc/nginx /Users/lizhigang/IdeaProjects/lzg/nginx/etc
docker rm -f mynginx
docker run -d --rm --name mynginx -v /Users/lizhigang/IdeaProjects/lzg/nginx/etc/nginx:/etc/nginx nginx
docker exec -it mynginx bash
``` 

## 设置根目录
```bash
docker run -d --rm --name mynginx -v /Users/lizhigang/IdeaProjects/lzg/nginx/etc/nginx:/etc/nginx -v  /Users/lizhigang/IdeaProjects/lzg/nginx/var/www/example.com:/var/www/example.com   nginx
```

## 服务IPv4和IPv6
## 压缩静态内容
## 最终版
```bash
docker run -d  --name mynginx -v /Users/lizhigang/IdeaProjects/lzg/nginx/etc/nginx:/etc/nginx -v  /Users/lizhigang/IdeaProjects/lzg/nginx/var/www/example.com:/var/www/example.com -p 8081:80  nginx
curl -H 'host:www.example' localhost:8081
```

# 第二章 一点点高级配置
备份
```bash
cp etc/nginx/nginx.conf etc/nginx/nginx.conf.backup-pt2
```

## 多个站点
每个站点是一个server，应该放在自己的配置文件中，如果要禁用某个站点，在文件后面加.disabled就可以
```bash
docker run -d  --name mynginx -v /Users/lizhigang/IdeaProjects/lzg/nginx/etc/nginx:/etc/nginx -v  /Users/lizhigang/IdeaProjects/lzg/nginx/var/www:/var/www -p 8081:80  nginx
curl -H 'host:www.example2.org' localhost:8081
```

## 缓存
```bash
#删除缓存
find /var/www/example.com/cache/ -type f -delete
```

## 第二章最终版

# SSL TSL
## 生成自签名证书
这个是一般方法
```bash
su - root
mkdir /root/certs && cd /root/certs
openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out MyCertificate.crt -keyout MyKey.key
chmod 400 root/certs/MyKey.key
```
为example生成证书
```bash
openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out example.com.crt -keyout example.com.key
chmod 400 example.com.key
```
## 全局配置ssl

## 一个站点配置
```
docker run -d  --name mynginx -v /Users/lizhigang/IdeaProjects/lzg/nginx/etc/nginx:/etc/nginx -v  /Users/lizhigang/IdeaProjects/lzg/nginx/var/www:/var/www -v /Users/lizhigang/IdeaProjects/lzg/nginx/root/certs:/root/certs  -p 8081:80 -p 443:443  nginx
curl -k  -H 'host:www.example.com' https://localhost
```
## 多个站点使用同一个配置
```bash
curl -k -H 'host:example2.org' https://localhost
```

## 配置每个站点用不通的证书
```bash
mkdir example2.org
cd example2.org
openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out example2.org.crt -keyout example2.org.key
docker exec mynginx nginx -s reload
curl -k -H 'host:example2.org' https://localhost
```

# TLS 最佳实践
```bash
cp etc/nginx/nginx.conf etc/nginx/nginx.conf.backup-pt4
cp -r etc/nginx/conf.d/ etc/nginx/conf.d-backup-pt4
```