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
