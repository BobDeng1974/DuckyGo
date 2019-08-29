# Singo

Singo: Simple Single Golang Web Service

使用Singo开发Web服务: 用最简单的架构，实现够用的框架，服务海量用户

ForK来自: https://github.com/bydmm/singo

## 目的

本项目采用了一系列Golang中比较流行的组件，可以以本项目为基础快速搭建Restful Web API

## 特色

本项目已经整合了许多开发API所必要的组件：

1. [Gin](https://github.com/gin-gonic/gin): 轻量级Web框架，自称路由速度是golang最快的 
2. [GORM](http://gorm.io/docs/index.html): ORM工具。本项目需要配合Mysql使用 
3. [Gin-Session](https://github.com/gin-contrib/sessions): Gin框架提供的Session操作工具
4. [Go-Redis](https://github.com/go-redis/redis): Golang Redis客户端
5. [godotenv](https://github.com/joho/godotenv): 开发环境下的环境变量工具，方便使用环境变量
6. [Gin-Cors](https://github.com/gin-contrib/cors): Gin框架提供的跨域中间件
7. 自行实现了国际化i18n的一些基本功能
8. 本项目是使用基于cookie实现的session来保存登录状态的，如果需要可以自行修改为token验证

本项目已经预先实现了一些常用的代码方便参考和复用:

1. 创建了用户模型
2. 实现了```/api/v1/user/register```用户注册接口
3. 实现了```/api/v1/user/login```用户登录接口
4. 实现了```/api/v1/user/me```用户资料接口(需要登录后获取session)
5. 实现了```/api/v1/user/logout```用户登出接口(需要登录后获取session)
6. 实现了```/api/v1/user/changepassword```用户修改密码接口

本项目已经预先创建了一系列文件夹划分出下列模块:

1. api文件夹就是MVC框架的controller，负责协调各部件完成任务
2. model文件夹负责存储数据库模型和数据库操作相关的代码
3. service负责处理比较复杂的业务，把业务代码模型化可以有效提高业务代码的质量（比如用户注册，充值，下单等）
4. serializer储存通用的json模型，把model得到的数据库模型转换成api需要的json对象
5. cache负责redis缓存相关的代码
6. auth权限控制文件夹
7. util一些通用的小工具
8. conf放一些静态存放的配置文件，其中locales内放置翻译相关的配置文件

## LOG_LEVEL说明
```text

当设置LOG_LEVEL设置为ERROR
就只会显示 error panic

当设置LOG_LEVEL设置为WARNING
就只会显示 warning error panic

当设置LOG_LEVEL设置为INFO
就只会显示 info warning error panic

当设置LOG_LEVEL设置为DEBUG
则全部显示

```

## Godotenv

项目在启动的时候依赖以下环境变量，但是在也可以在项目根目录创建.env文件设置环境变量便于使用(建议开发环境使用)

```shell
MYSQL_DSN="db_user:db_passwd@tcp(127.0.0.1:3306)/db_name?charset=utf8&parseTime=True&loc=Local" # Mysql连接配置
REDIS_ADDR="127.0.0.1:6379" # Redis端口和地址
REDIS_PW=""                 # Redis连接密码
REDIS_DB=""                 # Redis库从0到10，不填即为0
SESSION_SECRE=""            # Seesion密钥，必须设置而且不要泄露
GIN_MODE="debug"            # 设置gin的运行模式，有 debug 和 release
LOG_LEVEL="ERROR"           # 设置为ERROR基本不会记录log 设置为DEBUG则会详细记录每次请求
```

Windows安装MySQL和Redis麻烦?:no_mouth: 你可以使用[Docker](https://hub.docker.com/)啊！:sunglasses:

- 快速起Redis: `docker run -di --name redis -p 6379:6379 redis` 
- 快速起MySQL: `docker run -di --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql` 

因为启动容器指定了--name, 后续可以使用`docker start|stop redis|mysql` 来进行开启或者关闭.

如需要使用navicat等工具管理MySQL，可能会出现报错等情况：:dizzy_face:
```shell
docker exec -it mysql /bin/bash    # 打开mysql bash交互
mysql -u root -p                   # 进入mysql交互
ALTER USER 'root'@'%' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;      # 更改加密方式
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';   # 更改密码
FLUSH PRIVILEGES;                                                          # 刷新
```
    
## Go Mod

本项目使用[Go Mod](https://github.com/golang/go/wiki/Modules)管理依赖。

```shell
go mod init go-crud
export GOPROXY=http://mirrors.aliyun.com/goproxy/
go run main.go // 自动安装
```

## 运行

```shell
go run main.go
```

项目运行后启动在3000端口（可以修改，参考gin文档)   
本项目修改端口请查看`main.go`


## 编译
```shell
go build main.go
```
如需交叉编译请看[这里](https://studygolang.com/articles/13760)