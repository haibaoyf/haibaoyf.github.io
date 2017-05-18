---
layout: docwithnav
assignees:
- ashvayka
title: RabbitMQ插件

---

RabbitMQ插件负责将消息发送到由特定规则触发的RabbitMQ实例

您可以指定以下配置参数：

- rabbitmq实例主机
- rabbitmq实例端口
- rabbitmq虚拟主机
- 授权用户名
- 授权密码
- 在发生故障时启用自动化连接重新发送
- 连接超时
- 握手超时
- 可用于连接到特定的rabbitmq实例的其他客户端属性

在本例中，我们将演示如何配置RabbitMQ扩展，以便每当设备的新遥测消息到达时，能够将消息发送到特定队列。
继续前提RabbitMQ扩展配置：

- 事件板已启动并运行
- RabbitMQ实例已启动并运行

##RabbitMQ插件配置

我们首先配置RabbitMQ插件。转到插件菜单，点击'+'按钮并创建新的插件：
![image](http://help.gzhaibaogd.com/images/rabbitmq/rabbitmq-plugin-config-1.png)

![image](/images/rabbitmq/rabbitmq-plugin-config-2.png)
请设置正确的主机，端口和凭据，以便扩展能够连接到RabbitMQ代理。
保存插件并点击“激活”插件按钮：

![image](http://help.gzhaibaogd.com/images/rabbitmq/rabbitmq-activate-plugin.png)

##RabbitMQ调用规则

现在是时候创建适当的规则了。

![image](http://help.gzhaibaogd.com/images/rabbitmq/rabbitmq-rule-config.png)

添加POST_TELEMETRY消息类型的过滤器

![image](http://help.gzhaibaogd.com/images/rabbitmq/post-telemetry-filter.png)

点击“添加”按钮添加过滤器。
然后在插件字段的下拉框中选择“RabbitMQ插件”：

![image](http://help.gzhaibaogd.com/images/rabbitmq/rabbitmq-plugin-selection.png)

添加将特定RabbitMQ队列发送设备温度遥测的操作：

![image](http://help.gzhaibaogd.com/images/rabbitmq/rabbitmq-rule-action-config.png)

点击“添加”按钮，然后激活规则。
##RabbitMQ示例

现在，您的任何设备发送包含“temp”遥测的遥测信息：

```json
{"temp":73.4}
```
一旦您发布此消息，您应该在适当的RabbitMQ队列中收到消息'73 .4'。
以下是一个将单个遥测消息发布到本地安装的事件板的命令示例：
```bash
mosquitto_pub -d -h "localhost" -p 1883 -t "v1/devices/me/telemetry" -u "$ACCESS_TOKEN" -m '{"temp":73.4}'
```
