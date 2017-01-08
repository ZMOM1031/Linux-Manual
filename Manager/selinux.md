SELinux (Secure Enhanced Linux) 安全增强Linux，由美国安全局(NSA)开发
SELinux是一个内核级的安全机制，从2.6内核之后集成在内核中，主流的Linux发行版本都会集成SELinux机制
SELinux允许管理员更加灵活的定义安全策略

如果你搞不懂SELinux可以直接关闭掉
```
setenforce 0
```

编辑配置文件 `/etc/selinux/config` 把 `SELINUX=enforcing` 改成 `SELINUX=disabled` 
这样就不会影响你干事情了

SELinux针对进程和系统资源这两个类型定义了两个基本概念：域 (domain) 和上下文 (context)
域用来对进程限制，上下文用来对系统资源进行限制

使用 `ps -Z` 查看进程的域信息
使用 `ls -Z` 查看文件的上下文信息

SELinux通过策略来控制哪些域可以访问哪些上下文，SELiunx有很多预置策略，CentOS/RHEL使用预置的目标策略(target)策略
目标 (Target) 策略定义只有目标进程受SELinux限制，其他进程运行在非限制模式下。目标策略只影响网络应用程序

SELinux有3种工作模式，默认为强制(Enforcing)模式
* 强制(Enforcing)：违反策略的行动都被禁止，并作为内核信息记录
* 许可(Permissive)：违反策略的行动都不被禁止，但会产生警告信息
* 禁用(Disabled)：禁用SELinux 与不带SELinux的系统一样

SELinux模式的配置文件为 `/etc/selinux/config`
```
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=enforcing
# SELINUXTYPE= can take one of these two values:
#     targeted - Targeted processes are protected,
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```

使用 `getenforce` 可以查看当前SELinux工作模式
使用 `setenforce` 可以设置当前SELinux工作模式
```
setenforce 0		// 设为许可模式(Permissive)
setenforce 1		// 设为强制模式(Enforcing)
```

要想永久禁用修改配置文件 `/etc/selinux/config` 把 `SELINUX=enforcing` 改成 `SELINUX=disabled` 

命令 `ps` `ls` 加上 `-Z` 参数就可以显示对应的SELinux信息

|用户 (User)|角色 (role)|类型 (type)|MLS、MCS|
|:---------:|:---------:|:---------:|:------:|
|unconfined_u:|object_r:|admin_home_t:|s0|
|system_u:|object_r:|httpd_exec-t:|s0|
|省略|省略|省略|省略|

SELinux策略规定哪些域(进程)可以访问哪些上下文(文件)
在对系统进行管理时，对文件操作有时会改变文件的上下文，导致一些进程无法访问某些文件，
比如复制、移动了某些文件到某地，这样的文件会与所在目录的的上下文不一样
所以我们一般需要检查、修改文件的上下文

使用 `restorecon` 可以恢复文件默认的上下文
```
restorecon -R -v /var/www		// 这里举例用HTTPD的目录
```

参数解释：
* -R 改变文件和目录的文件标签递归
* -v 显示变化的文件标签

使用 `chcon` 可以用以改变文件的上下文
```
chon --reference=/etc/named.conf.orlg /etc/named.conf
```

参数解释：
* --reference=参照文件 被修改文件 

查看SELinux状态

使用 `sestatus` 可以查看SELinux目前的状态
```
SELinux status:                 enabled			// SELinux的状态
SELinuxfs mount:                /selinux		// SELinux挂载点
Current mode:                   enforcing		// 模式
Mode from config file:          enforcing		// 模式
Policy version:                 24				// 版本
Policy from config file:        targeted		// 模式
```

使用 `sestatus -v` 显示更多包含了文件环境的信息
```
SELinux status:                 enabled
SELinuxfs mount:                /selinux
Current mode:                   enforcing
Mode from config file:          enforcing
Policy version:                 24
Policy from config file:        targeted

Process contexts:
Current context:                unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
Init context:                   system_u:system_r:init_t:s0
/sbin/mingetty                  system_u:system_r:getty_t:s0
/usr/sbin/sshd                  system_u:system_r:sshd_t:s0-s0:c0.c1023

File contexts:
Controlling term:               unconfined_u:object_r:user_devpts_t:s0
/etc/passwd                     system_u:object_r:etc_t:s0
/etc/shadow                     system_u:object_r:shadow_t:s0
/bin/bash                       system_u:object_r:shell_exec_t:s0
/bin/login                      system_u:object_r:login_exec_t:s0
/bin/sh                         system_u:object_r:bin_t:s0 -> system_u:object_r:shell_exec_t:s0
/sbin/agetty                    system_u:object_r:getty_exec_t:s0
/sbin/init                      system_u:object_r:init_exec_t:s0
/sbin/mingetty                  system_u:object_r:getty_exec_t:s0
/usr/sbin/sshd                  system_u:object_r:sshd_exec_t:s0
```

## 查看和修改SELinux对网络服务的设定

以VSFTPD服务为例，在安装了VSFTPD，开启SELinux的情况下会对FTP服务进行保护。

使用 `getsebool` 可以获得某个功能的布尔值(状态)