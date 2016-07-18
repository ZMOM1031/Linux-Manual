.. _gpg:

==========
GunPG
==========

安装
==========

* Debian / Ubuntu

::

    apt-get install gnupg

* CentOS / Fedora

::

    yum install gnupg

第一次使用
==========

首先你要有一对自己的密钥，才能开始使用。

生成密钥对
----------

::

    gpg --gen-key

回车以后，会跳出一大段文字：
::

    gpg (GnuPG) 1.4.12; Copyright (C) 2012 Free Software Foundation, Inc.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.
    
    gpg: directory `/home/zero/.gnupg' created
    gpg: new configuration file `/home/zero/.gnupg/gpg.conf' created
    gpg: WARNING: options in `/home/zero/.gnupg/gpg.conf' are not yet active during this run
    gpg: keyring `/home/zero/.gnupg/secring.gpg' created
    gpg: keyring `/home/zero/.gnupg/pubring.gpg' created
    Please select what kind of key you want:
       (1) RSA and RSA (default)
       (2) DSA and Elgamal
       (3) DSA (sign only)
       (4) RSA (sign only)
    Your selection?		// 选择加密算法，默认选择 (1) RSA 加密，直接回车
    RSA keys may be between 1024 and 4096 bits long.
    What keysize do you want? (2048)		// 选择加密长度，默认 (2048)
    Requested keysize is 2048 bits
    Please specify how long the key should be valid.
             0 = key does not expire
          <n>  = key expires in n days
          <n>w = key expires in n weeks
          <n>m = key expires in n months
          <n>y = key expires in n years
    Key is valid for? (0)		// 密钥失效时间，默认为 (0) 永不失效
    Key does not expire at all
    Is this correct? (y/N) y		// 确认上述配置正确
    
    You need a user ID to identify your key; the software constructs the user ID
    from the Real Name, Comment and Email Address in this form:
        "Heinrich Heine (Der Dichter) <heinrichh@duesseldorf.de>"
    
    Real name: Test User		// 输入自己的真实名字
    Email address: test@example.com		// 输入自己的邮箱
    Comment:		// 补充内容
    You selected this USER-ID:
        "Test User <test@example.com>"
    
    Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o		// 确认选项，可修改输入的内容，没问题输入 (O) 回车确认
    You need a Passphrase to protect your secret key.		// 输入一个密码来保护私钥不被盗用
    
    We need to generate a lot of random bytes. It is a good idea to perform
    some other action (type on the keyboard, move the mouse, utilize the
    disks) during the prime generation; this gives the random number
    generator a better chance to gain enough entropy.
    
    Not enough random bytes available.  Please do some other work to give
    the OS a chance to collect more entropy! (Need 201 more bytes)
    
    // 等待密钥生成，这时可以做一些其它事情（像是敲打键盘、移动鼠标、读写硬盘之类的），
    // 这会让随机数字发生器有更好的机会获得足够的熵数。
    // 建议另起一个终端，写一个耗性能的 for 循环脚本运行，否则要等很久。
    
    .........+++++
    .....+++++
    We need to generate a lot of random bytes. It is a good idea to perform
    some other action (type on the keyboard, move the mouse, utilize the
    disks) during the prime generation; this gives the random number
    generator a better chance to gain enough entropy.
    
    Not enough random bytes available.  Please do some other work to give
    the OS a chance to collect more entropy! (Need 84 more bytes)
    ..+++++
    
    Not enough random bytes available.  Please do some other work to give
    the OS a chance to collect more entropy! (Need 120 more bytes)
    .....+++++
    gpg: /home/zero/.gnupg/trustdb.gpg: trustdb created
    gpg: key 675394C8 marked as ultimately trusted
    public and secret key created and signed.
    
    gpg: checking the trustdb
    gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
    gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
    pub   2048R/675394C8 2016-07-16
          Key fingerprint = 86AB 2D80 CEED 59DC F046  F75C D027 CF07 6753 94C8
    uid                  Test User <test@example.com>
    sub   2048R/D2951F3C 2016-07-16

密钥对已经生成好了，ID 是 `675394C8`

管理密钥
----------

列出系统中所有的密钥
::

    gpg --list-keys

::

    /home/zero/.gnupg/pubring.gpg
    -----------------------------
    pub   2048R/675394C8 2016-07-16
    uid                  Test User <test@example.com>
    sub   2048R/D2951F3C 2016-07-16
    
    pub   2048R/4D67E763 2016-07-16
    uid                  Test User2 <test@example.com>
    sub   2048R/D2C6D99E 2016-07-16

第一行显示公钥文件名 (~/.gnupg/pubring.gpg)，第二行显示公钥特征（2048位，Hash字符串和生成时间），
第三行显示用户信息，第四行显示私钥特征。拥有多把密钥会在下面继续列出。

列出系统中所有的私钥
::

    gpg --list-secret-keys

::

    /home/zero/.gnupg/secring.gpg
    -----------------------------
    sec   2048R/675394C8 2016-07-16
    uid                  Test User <test@example.com>
    ssb   2048R/D2951F3C 2016-07-16

列出系统中所有的公钥
::

    gpg --list-public-keys

::

    /home/zero/.gnupg/pubring.gpg
    -----------------------------
    pub   2048R/675394C8 2016-07-16
    uid                  Test User <test@example.com>
    sub   2048R/D2951F3C 2016-07-16
    
    pub   2048R/4D67E763 2016-07-16
    uid                  Test User2 <test@example.com>
    sub   2048R/D2C6D99E 2016-07-16

从密钥列表中删除某个公钥
::

    gpg --delete-key 675394C8

从密钥列表中删除某个私钥
::

    gpg --delete-secret-keys 675394C8

从密钥列表中删除某个私钥和公钥的密钥对
::

    gpg --delete-secret-and-public-keys 675394C8

导出密钥
----------

公钥文件以二进制形式储存在 (~/.gnupg/pubring.gpg)，`--armor` 参数可以将其转换为 ASCII 码显示。

导出公钥到文件
::

    gpg --armor --output public-key.asc --export 675394C8

`--export` 指定导出哪把公钥，`--output` 指定输出的文件名 `public-key.asc`

导出私钥到文件
::

    gpg --armor --output private-key.asc --export-secret-keys 675394C8

`--export-secret-keys` 指定导出哪把私钥，`--output` 指定输出的文件名 `private-key.asc`

导出撤销证书
::

    gpg --output revoke-key.asc --gen-revoke 675394C8

撤销证书是以备以后密钥作废时，可以请求外部的公钥服务器撤销你的公钥。

上传公钥
----------

你可以把公钥放在自己网站上供其它人获取，或者上传到网络上专门储存用户公钥的服务器。

上传公钥到 `subkeys.pgp.net` 服务器
::

    gpg --send-keys 675394C8 --keyserver hkp://subkeys.pgp.net

生成公钥指纹
::

    gpg --fingerprint 675394C8

::

    pub   2048R/675394C8 2016-07-16
          Key fingerprint = 86AB 2D80 CEED 59DC F046  F75C D027 CF07 6753 94C8
    uid                  Test User <test@example.com>
    sub   2048R/D2951F3C 2016-07-16

由于公钥服务器没有检查机制，任何人都可以用你的名义上传公钥，所以没有办法保证服务器上的公钥的可靠性。
通常你可以在网站上公布一个公钥指纹，让其他人核对下载到的公钥是否为真。

倒入密钥
----------

倒入密钥文件
::

    gpg --import public-key.asc

从默认公钥服务器 `keys.gnupg.net` 上倒入公钥
::

    gpg --recv-keys 675394C8

从公钥服务器上倒入密钥并验证公钥指纹
::

    gpg --keyserver hkp://subkeys.pgp.net --search-keys 675394C8
    gpg --fingerprint 675394C8

加密和解密
==========

示例文件 `msg.txt`，内容是 `Hello World`

对称加密
----------

对称加密无需使用到密钥，类似与普通的秘密加密。

加密文件
::

    gpg -c msg.txt

键入 2 次密码后会生成一个 `.gpg` 的加密文件。

解密文件
::

    gpg msg.txt.gpg

使用 `-o` 参数指定解密输出的文件名，`-d` 指定被解密的文件。
::

    gpg -o hello.txt -d msg.txt.gpg

公钥加密
----------

加密文件
::

    gpg --recipient 675394C8 --output msg-encrypt.txt.gpg --encrypt msg.txt

简写命令
::

    gpg -ea -r test@example.com msg.txt

`-e` 就是 `--encrypt` 表示加密数据， `-a` 就是 `--armor` 表示创建 ASCII 的输出（不使用这个参数输出的文件是一个二进制文件，以 `.gpg` 结尾），`-r` 就是 `--recipient` 指定加密的用户ID名称，可以是 Hash 值或邮箱。


解密文件
::

    gpg --output msg_1.txt --decrypt msg-encrypt.txt.gpg

简写命令
::

    gpg msg.txt.asc

签名和验证
==========

数字签名
----------

::

    gpg -o msg.txt.sig -s msg.txt

`-o` 就是 `--output` 表示指定输出到哪个文件，`-s` 就是 `--sign` 表示指定一个要签名的文件

文本签名
----------

::

    gpg -o msg.txt.sig --clearsign msg.txt

分离式签名
----------

::

    gpg -o msg.txt.sig -ab msg.txt

`msg.txt.sig` 仅包括签名，分离式签名的意思是原文件和签名是分开的，`-a` 就是 `--armor` 表示创建 ASCII 的输出，`-b` 就是 `--detach-sign` 表示分离式签名。

签名和加密
----------

::

    gpg --local-user 675394C8 --recipient 4D67E763 --armor --sign --encrypt msg.txt

`--local-user` 表示发送者ID，也就是自己的私钥ID用于签名，`--recipient` 表示接收者的公钥ID。

验证签名
----------

::

    gpg --verify msg.txt.sig
