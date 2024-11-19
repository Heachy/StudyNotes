# SpringCloud

## 负载均衡

![1675929027384](./springcloud.assets/1675929027384.png)

![1675930769015](./springcloud.assets/1675930769015.png)

![1675930876472](./springcloud.assets/1675930876472.png)

这两个结合使用

![1676002567126](./springcloud.assets/1676002567126.png)

## 熔断保护

![1676002626946](./springcloud.assets/1676002626946.png)

![1676016462578](./springcloud.assets/1676016462578.png)

![1676016480413](./springcloud.assets/1676016480413.png)

## 服务降级

![1676016624917](./springcloud.assets/1676016624917.png)

监控页面依赖

spring-cloud-starter-netflix-hystrix-dashboard

用这个

![1676017103346](./springcloud.assets/1676017103346.png)

在服务提供端的启动类下面增加一个监控路径的servelet

![1676017525112](./springcloud.assets/1676017525112.png)

开启注解

![1676017614891](./springcloud.assets/1676017614891.png)

没有监控页面的



![1676017392680](./springcloud.assets/1676017392680.png)

![1676034610048](./springcloud.assets/1676034610048.png)

zuul设置隐藏服务名称

