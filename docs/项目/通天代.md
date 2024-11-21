# 通天代 TTD-MS

> 软工大作业

**常规功能点：**

- 用户注册登录绑定学生用户；

- 用户发布需要代拿订单

- 帮手承接订单

- 平台维护订单状态
- 帮手与客户的聊天室(随着订单产生和失效)
- 评价帮手等

**比较好玩的点：**

- 帮手承接订单的时候，可通过webSocket推送相同起始点和重点的订单(新发布的时候)

​		5分钟内吧(估摸着帮手还在起始点附近)

- 推荐路线(还待完善)



# 项目模块

## common

> 存放整个项目里头的通用类，像是什么exception，网络接口返回数据的类型规范，工具类等

### api

`CommonResult` 作为网络接口返回的统一类，可在里面携带前端所需要的数据

### exception

`ApiException`，`SaTokenException`

### utils

工具类包，不过姑且只有阿里云OSS的文件上下传的

## generated

> 存放着由mybatis-plus生成的实体类的包
>
> 感觉写错了，具体业务应该再开个service，不该写道这个模块下，这个应该就只是存放生成后的数据

由domain，mapper，service，vo包组成

## rabbitMQ

> 通过消息队列来实现一个定时的效果

通过与redis相结合，可以实现一个有相似订单起始地点和目标地点进行告知帮手的功能，在帮手承接一个订单后，通过websocket进行推荐订单的功能。

## statistics

> 统计模块，包括路线推荐以及用户对帮手的评价统计

还有超时单数，总接单数，好评数等

## web

> web模块就是编写主要的controller，结合前面的几个模块开放接口

- 聊天室controller，获取对应的聊天记录
- 文件controller，上传文件获取url，比如快递图片，聊天图片等
- 快递任务controller，增删改查
- token的controller，刷新token时间
- 用户的controller，用户注册，绑定学生，用户信息更改等
- 微信controller，微信登录等

## websocket

> 这个模块就是聊天室功能的实现，以及对推荐订单功能的扩展

在用户连接上时，会进行token的校验，避免恶意链接
