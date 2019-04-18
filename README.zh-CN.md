## 介绍

Enjoy Mock 是一个可视化，并且能快速生成**模拟数据**的持久化服务。

## 特性

- 支持接口代理
- 支持快捷键操作
- 支持协同编辑
- 支持团队项目
- 支持 RESTful
- 支持 [Swagger](https://swagger.io) | OpenAPI Specification ([1.2](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/1.2.md) & [2.0](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md) & [3.0](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.0.md))
  - 基于 Swagger 快速创建项目
  - 支持显示接口入参与返回值
  - 支持显示实体类
- 支持灵活性与扩展性更高的响应式数据开发
- 支持自定义响应配置（例：status/headers/cookies）
- 支持 [Mock.js](http://mockjs.com/) 语法
- 支持 [restc](https://github.com/ElemeFE/restc) 方式的接口预览

Enjoy-mock快速开始

> 在开始之前，假设你已经成功安装了 [Node.js](https://nodejs.org)（**v8.x, ~~不支持 v10.x~~**）& [MongoDB](https://www.mongodb.com)（**>= v3.4**）& [Redis](https://redis.io)（**>= v4.0**）。

### 安装

```sh
$ git clone git@github.com:bruceliu68/Enjoy-mock.git
$ cd Enjoy-mock && npm install
```

### 配置文件

找到 **config/default.json**，或者创建一个 **config/local.json** 文件，将如下需要替换的字段换成自己的配置即可。

> 不同环境会加载不同的配置文件，在此之前你应该对 [node-config](https://github.com/lorenwest/node-config) 有所了解。

```js
{
  "port": 7300,
  "host": "0.0.0.0",
  "pageSize": 30,
  "proxy": false,
  "db": "mongodb://localhost/enjoy-mock",
  "unsplashClientId": "",
  "redis": {
    "keyPrefix": "[Enjoy Mock]",
    "port": 6379,njoy "host": "localhost",
    "password": "",
    "db": 0
  },
  "blackList": {
    "projects": [], // projectId，例："5a4495e16ef711102113e500"
    "ips": [] // ip，例："127.0.0.1"
  },
  "rateLimit": { // https://github.com/koajs/ratelimit
    "max": 1000,
    "duration": 1000
  },
  "jwt": {
    "expire": "14 days",
    "secret": "shared-secret"
  },
  "upload": {
    "types": [".jpg", ".jpeg", ".png", ".gif", ".json", ".yml", ".yaml"],
    "size": 5242880,
    "dir": "../public/upload",
    "expire": {
      "types": [".json", ".yml", ".yaml"],
      "day": -1
    }
  },
  "ldap": {
    "server": "", // 设置 server 代表启用 LDAP 登录。例："ldap://localhost:389" 或 "ldaps://localhost:389"（使用 SSL）
    "bindDN": "", // 用户名，例："cn=admin,dc=example,dc=com"
    "password": "",
    "filter": {
      "base": "", // 查询用户的路径，例："dc=example,dc=com"
      "attributeName": "" // 查询字段，例："mail"
    }
  },
  "fe": {
    "copyright": "",
    "storageNamespace": "enjoy-mock_",
    "timeout": 25000,
    "publicPath": "/dist/"
  }
}
```

**注意：**

- `publicPath` 默认是 `'/dist/'`。如有需要，可以将其替换成自己的 CDN；
- 关于 `fe` 的配置，一旦发生改变应该重新执行 build 命令。

### 启动

```sh
$ npm run dev
# 访问 http://127.0.0.1:7300
```

## 更多命令

```sh
# 前端静态资源构建打包
$ npm run build

# 以生产环境方式启动，需要提前执行 build
$ npm run start

# 单元测试
$ npm run test

# 语法检测
$ npm run lint
```

## 服务器部署

> 在此之前请先配置好配置文件。

### PM2

当在内网服务器部署时，推荐使用 [PM2](https://github.com/Unitech/pm2) 来守护你的应用进程。

#### 全局安装 PM2

```sh
$ [sudo] npm install pm2 -g
```

#### 用 PM2 启动

> 在此之前，你应该已经完成了 build。

```sh
$ NODE_ENV=production pm2 start app.js
```

