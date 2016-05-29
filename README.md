# bt-scheme

CentOS平台，BT下载视频方案介绍。

## transmission
transmission是一个跨平台的BT下载软件，它自带一个web客户端。

transmission的配置文件可以在我的仓库中找到，也可以在网上找找，
总之，你有很多渠道可以解决配置的问题。

建议控制一个连接数和上传速度，保护主机不被BT玩死。


## nginx(openresty)

安装nginx，需要使用它的两个功能，一个是虚拟主机，一个是网页服务。

### nginx虚拟主机

```
server {
    listen      80;
    server_name bt.{domain}.com

    index   index.html index.htm;

    location / {
        auth_basic "Restricted";
        auth_basic_user_file conf.d/transmission_passwd;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        proxy_pass http://127.0.0.1:9091;
   }
}
```

> 使用nginx来做一次转发，可以减少端口扫描的恶意攻击者带来的风险。


### 网页服务(H5视频播放)
查看index.html，使用网页播放BT下载的视频。

### 查看视频是否被浏览器支持
mediainfo，安装epel扩展源就可以使用yum安装啦。
关于浏览器的视频兼容方面的详细信息，可以查看其它书籍。
主要是太复杂啦，需要整整一章的内容来介绍，我不太专业，就不扯淡啦。

## 效果
目前，可以在网页版本的BT客户端进行下载管理，然后使用H5技术进行视频预览，如果需要，使用FTP下载到本地，速度还是杠杠的, 这样就可以充分利用服务器的带宽资源和电源啦，低碳生活。
