vsftpd相关配置文件及目录
```
/etc/vsftpd/vsftpd.conf		// vsftpd主配置文件
/etc/vsftpd/ftpusers		// 黑名单
/etc/vsftpd/user_list		// 控制名单(可作为白名单或黑名单使用由配置文件定义)
/var/ftp					// ftp共享目录
/var/log/xferlog			// vsftpd日志
```

配置文件参数定义：

```
anonymous_enable=YES
```
允许匿名用户访问，即使用anonymous账号访问FTP访问

```
local_enable=YES
```
允许使用本机用户账号登录

```
write_enable=YES
```
允许用户读写操作

```
local_umask=022
```
默认掩码为 `002` 即创建的文件权限为 `775`

```
anon_upload_enable=YES
```
是否允许匿名用户上传。默认被注释掉，基于安全考虑

```
anon_mkdir_write_enable=YES
```
是否允许匿名用创建文件夹。默认被注释掉，基于安全考虑

```
dirmessage_enable=YES
```
若目录下存在 `.messages` 文件，则显示该文件内容

```
xferlog_enable=YES
```
默认上传/下载的日志记录到 `/var/log/xferlog`

```
connect_from_port_20=YES
```
从20端口连接，默认状态下20端口为FTP的数据传输端口

```
chown_uploads=YES
chown_username=whoever
```
成对出现。允许新上传文件的拥有者为 `whoever` 默认注被释掉

```
xferlog_file=/var/log/xferlog
```
VSFTPD的日志位置设定，默认被注释掉

```
xferlog_std_format=YES
```
使用标准格式记录上传/下载日志

```
idle_session_timeout=600
```
空闲会话超时时间

```
data_connection_timeout=120
```
传输延时时间，单位为秒

```
nopriv_user=ftpsecure
```
使用特殊用户 `ftpsecure` 这里把 `ftpsecure` 帐户作为一般访问用户，即所有链接FTP服务器的用户都具有 `ftpsecure` 用户名。
为了安全起见，可以在 `/etc/passwd` 中将 `ftpsecure` 的用户登录Shell改成 `/sbin/nologin` 默认未创建此用户

```
async_abor_enable=YES
```
取消下载后客户端不挂起

```
ascii_upload_enable=YES
ascii_download_enable=YES
```
是否启用ASCII方式传输文件

```
ftpd_banner=Welcome to blah FTP service.
```
登录提示信息

```
deny_email_enable=YES
banned_email_file=/etc/vsftpd/banned_emails
```
若启用上述两个设定，则可在 `/etc/vsftpd/banned_emails` 中建立黑名单

```
chroot_local_user=YES
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd/chroot_list
```
在 `/etc/vsftpd/chroot_list` 中定义的用户不可访问它们FTP根目录的再上一层目录

```
ls_recurse_enable=YES
```
是否可以使用 `ls -R` 命令

```
listen=YES
```
当选项为 `YES` 时，VSFTPD运行于 `standalone` 模式下

```
listen_ipv6=YES
```
是否使用IPv6

```
pam_service_name=vsftpd
```
列出与**VSFTPD**相关的**PAM**文件

```
userlist_enable=YES
```
当此选项设定为 `YES` 时，启用配置文件 `/etc/vsftpd/user_list` 
此时有两种情况：
* 若没有 `userlist_deny=NO` 则 `/etc/vsftpd/user_list` 中的用户不可访问VSFTPD服务器
* 若存在 `userlist_deny=NO` 则接受 `/etc/vsftpd/user_list` 中存在的的用的户登请求
（同时这些用户不存在于 `/etc/vsftpd/ftpusers`）

将此选项设为 `NO` 时，不会启用配置文件 `/etc/vsftpd/user_list`

```
tcp_wrappers=YES
```
启用**TCP Wrapper**支持

以上是默认安装后，VSFTPD主配置文件中出现的选项。除此之外还有些没出现的选项

```
guest_enable=YES
guest_username=ftp
```
`guest` 用户名，即所有非匿名用户都将具有 `guest` 用户身份

```
local_roor=/var/ftp
anon_root=/var/ftp
```
设定本机用户和匿名用户的FTP根目录

```
pasv_enable=YES
#port_enable=YES
```
这两个参数只能出现一个，另一个必须注释掉。这将决定服务器的工作状态：PORT(主动模式)或PASV(被动模式)

```
pasv_min_port=9001
pasv_max_port=9200
```
若设定了 `pasv_enable=YES` 则上面两项将设定被动模式的数据端口范围。这里从 `9001` 到 `9200` 端口

```
use_localtime=YES
```
时候使用本机时间 若设为 `NO` 则采用格林尼治时间

```
accept_timeout=60
```
被动模式时，服务器等待客户端回应的延时时间，单位为秒

```
max_clients=0
```
最大客户连接数 (standalone 模式下) `0` 为无限制

```
max_per_ip=0
```
每个客户端的最大连接数

```
local_max_rate=0
```
本地帐户的最大传输速率，单位为字节/秒

```
anon_max_rate=0
```
匿名用户的最大传输速率
