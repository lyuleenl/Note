switch network
手机有shadowrocket小火箭的话可以。
手机和ns连接同一个局域网wifi，
小火箭 设置 代理 代理共享 开启
ns wifi设置 开启代理 输入小火箭代理共享界面的ip和端口就可以了 

阿里云远程连接密码
iyY3cR

解压缩报错tar: Error is not recoverable: exiting now
[root@Gris-11140 FMIS2600bak]# tar -zxvf /home/oradata/FMIS2600DMP.tar.gz
gzip: stdin: not in gzip format
tar: Child returned status 1
tar: Error is not recoverable: exiting now

解决方法：
去掉z参数，使用 tar -xvf 解压正常

如果没有必要，端口均可使用默认值，token、user和password项请自行设置。

“bind_port”表示用于客户端和服务端连接的端口，这个端口号我们之后在配置客户端的时候要用到。
“dashboard_port”是服务端仪表板的端口，若使用7500端口，在配置完成服务启动后可以通过浏览器访问 x.x.x.x:7500 （其中x.x.x.x为VPS的IP）查看frp服务运行信息。
“token”是用于客户端和服务端连接的口令，请自行设置并记录，稍后会用到。
“dashboard_user”和“dashboard_pwd”表示打开仪表板页面登录的用户名和密码，自行设置即可。
“vhost_http_port”和“vhost_https_port”用于反向代理HTTP主机时使用，本文不涉及HTTP协议，因而照抄或者删除这两条均可。