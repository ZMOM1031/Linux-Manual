确保配置文件中有以下配置
```
anonymous_enable=YES
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

```
