### nginx上传服务器

一般参与到命令行的文件，不要出现中文

1.先找到nginx文件夹，找到conf,再找到nginx.conf文件.把它拖到编辑器里面。

2.复制里面的server一份，修改listen （端口号）如8888，找到location中找到要上传的文件的根文件路径（能看到index.html那一层），放到root 中，再保存一下。

3.在naginx文件下打开cmd

```js
nginx -t -c nginx文件中conf的路径/nginx.conf  //当验证显示成功则ok，防止配置错误
nginx -s reload //重新加载配置资源
start nginx
然后在浏览器中输入127.0.0.1:8888（在listen中配好的端口号），就可以开启服务器了。
nginx -s stop //关闭nginx
```

#### nginx的使用

1.下载nginx. 地址：http://nginx.org/en/download.html。选择[nginx / Windows-1.18.0 ](http://nginx.org/download/nginx-1.18.0.zip) [pgp](http://nginx.org/download/nginx-1.18.0.zip.asc)

2.修改配置。就是上传服务器哪一快，不过端口号范围在（0-65535）之间。localhost 后面的是我们请求的路径。root 是访问本地资源的目录。index是访问本地资源的文件名。

3.同上面的第三步打开cmd.

4.nginx常用命令

```js
快速停止：nginx -s stop
完全停止：nginx -s quit
查看版本：nginx -v
启动：start nginx 
如果想要在桌面进行启动的话，需要进行配置环境变量
```

