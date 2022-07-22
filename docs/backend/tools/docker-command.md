# docker

## 服务

### paperless

docker run --name paperless -m 400m --cpus="0.5" -p 8000:8000 -v /root/wudiguang/private/paperless/config:/config -v /root/wudiguang/private/paperless/data:/data linuxserver/paperless-ng