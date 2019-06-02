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