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