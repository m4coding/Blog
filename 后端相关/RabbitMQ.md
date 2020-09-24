# RabbitMQ

RabbitMQ是一个被广泛使用的开源消息队列。它是轻量级且易于部署的，它能支持多种消息协议。RabbitMQ可以部署在分布式和联合配置中，以满足高规模、高可用性的需求。

## 常用业务

1、在处理未支付订单超时取消时，可以利用延迟队列来处理订单的取消，避免相隔一段时间去轮询缓存订单或者数据库去取消订单（避免轮询在订单量很大的情况下是很消耗资源的）。

## 注意点

在springboot的yml文件配置可参考

  rabbitmq:
    host: ip地址
    port: 5672
    virtual-host: /happy
    username: happy
    password: happy
    publisher-confirms: true #如果对异步消息需要回调必须设置为true
    
其中port为5672，而不是15672 (15672是通过web页登录查看时所用到的端口)

如果port不是为5672，则运行会抛出异常：

org.springframework.amqp.AmqpTimeoutException: java.util.concurrent.TimeoutException

## 参考

[mall整合RabbitMQ实现延迟消息](https://juejin.im/post/6844903863636492301)

[记录使用rabbit mq处理订单超时业务](https://juejin.im/post/6844904126405410830)

[Ubuntu 18.04 安装 RabbitMQ](https://wangxin1248.github.io/linux/2020/03/ubuntu-install-rabbitmq.html)