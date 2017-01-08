尽允许系统用户登录到家目录

编辑配置文件
```
anonymous_enable=NO
```

启动服务
```
service vsftpd start
```

设置开启启动
```
chconfig --level 2345 vsftpd on
```

设置SELinux
```
setsebool -P ftp_home_dir 1
```

设置IPTables

```
-A INPUT -p tcp -m multiport --dport 20,21  -m state --state NEW -j ACCEPT		// 开启20,21端口
-A INPUT -p tcp -m state --state NEW -m tcp --dport 21 -j ACCEPT		// 开启21主动端口
-A INPUT -p tcp --dport 30000:31000 -j ACCEPT		// 开启被动端口
```
