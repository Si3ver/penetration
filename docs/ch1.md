# 笔记

## Django 快速搭建一个网站

+ manage.py   用于管理本项目的命令行工具 
+ settings.py 配置文件，本项目引用的组件，项目名，数据库配置，时间，语言，静态文件访问地址，存储路径
+ wsgi.py (Web Server gateway interface) 接口信息用于服务器部署
+ urls.py     用户访问的网址到项目文件的映射关系

```sh
# 安装 Django、初始化项目
$ python3.7 -m pip install django
$ django-admin startproject mysite
$ python3.7 manage.py runserver 0.0.0.0:8000 # 启动本地服务

# 创建APP
$ django-admin startapp firstapp
# ...创建自己的页面

# 初始化数据库 & 创建superuser账号
$ python3.7 manage.py makemigrations
$ python3.7 manage.py migrate
$ python3.7 manage.py createsuperuser
Username (leave blank to use 'zhouweilin'): 
Email address: izhouwl@163.com
Password: 
Password (again): 
Superuser created successfully.
```

## 常见的Web安全漏洞

+ 客户端脚本安全
  + 浏览器安全
  + XSS
    + 反射型
    + 存储型
    + DOM型
  + CSRF
  + Click Jacking
  + 网页钓鱼
+ 服务端应用安全
  + 认证与会话管理
  + AC
  + 加密算法安全
  + SQL注入
    + GET注入
    + POST注入
    + HTTP头注入
  + 文件上传漏洞
  + Web框架漏洞
  + 拒绝服务攻击（应用层、传输层）
  + 开发语言安全
    + 文件包含漏洞
    + 变量覆盖漏洞
    + 代码执行漏洞
  + Web Server 配置安全
    + 远程命令执行
+ 业务逻辑安全

## 渗透工具

+ 客户端软件
  + Burp Suite
  + cURL
  + Postman
+ 浏览器插件
  + HackBar(SQL injection, XSS, 加密等)
  + Wappalyzer(建站技术分析)
