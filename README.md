## Links

- [简体中文介绍](README.zh-CN.md)

## Introduction

Enjoy Mock is a persistent service that generates mock data quickly and provids
visualization view.

## Features

- Support API proxying
- Convenient shortcuts
- Support Collaborative editing
- Support team project
- Support RESTful
- Support [Swagger](https://swagger.io) | OpenAPI Specification ([1.2](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/1.2.md) & [2.0](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md) & [3.0](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.0.md))
  - Create project quickly based on Swagger
  - Support displaying parameters and the return value
  - Support displaying class model
- More flexible and extensible in response data
- Support for custom response configuration (example: status/headers/cookies)
- Use [Mock.js](http://mockjs.com/) schema
- Support [restc](https://github.com/ElemeFE/restc) to preview API

## Quick Start

> Before starting, we assume that you're already have installed
> [Node.js](https://nodejs.org) (**v8.x, ~~v10.x is not supported~~**) & [MongoDB](https://www.mongodb.com) (**>= v3.4**) & [Redis](https://redis.io)（**>= v4.0**）.

### Installation

```shell
$ git clone git@github.com:bruceliu68/Enjoy-mock.git
$ cd easy-mock && npm install
```

### Configuration

Find **config/default.json** or create **config/local.json** to overwrite some
configuration.

> Enjoy Mock will load different configuration files according to your
> environment. Reference to [node-config](https://github.com/lorenwest/node-config)
> to get more information because Enjoy Mock uses node-config as its
> configuration module.

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
    "port": 6379,
    "host": "localhost",
    "password": "",
    "db": 0
  },
  "blackList": {
    "projects": [], // projectId, e.g."5a4495e16ef711102113e500"
    "ips": [] // ip, e.g. "127.0.0.1"
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
    "server": "", // Set server to enable LDAP login. e.g. "ldap://localhost:389" or "ldaps://localhost:389"（use SSL）
    "bindDN": "", // Username，e.g. "cn=admin,dc=example,dc=com"
    "password": "",
    "filter": {
      "base": "", // Base where we can search for users，e.g. "dc=example,dc=com"
      "attributeName": "" // e.g. "mail" or "email" etc.
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

**Note**:

- The default value of `publicPath` is `'/dist/'`. You can replace it to your
  own CDN if necessary.
- If you changed some configuration of `fe`, you should run `build` command
  to adapt that changes.

**Background**:

Enjoy Mock supports two background service,
[Unsplash](https://unsplash.com/developers) and [Bing](http://bing.com).

If you leave `unsplashClientId` blank, the background will be provided by Bing.

### Launch

```sh
$ npm run dev
# Visit http://127.0.0.1:7300
```

## More Commands

```sh
# Build front-end assets
$ npm run build

# Run Enjoy Mock as production environment (You should run `build` first)
$ npm run start

# Run unit test
$ npm run test

# Test lint
$ npm run lint
```

## Deployment

> Please configure your configuration files before this step.

### PM2

We're recommending you to use [PM2](https://github.com/Unitech/pm2) as your
daemon process.

#### Install PM2 Globally

```sh
$ [sudo] npm install pm2 -g
```

#### Launch via PM2

> You should run `build` before this step.

```sh
$ NODE_ENV=production pm2 start app.js
```
