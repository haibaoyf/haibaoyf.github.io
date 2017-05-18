---
layout: docwithnav
assignees:
- ashvayka
title: Kafka插件

---

Kafka插件负责向特定规则触发的Kafka经纪人发送消息

卡夫卡的配置参数如下：

- 引导服务器 – 卡夫卡经纪人列表
- 如果连接失败，尝试重新连接到卡夫卡的次数
- 在客户端上单元批量的消息数
- 发送到卡夫卡经纪人之前在本地进行缓冲的时间（以ms为单位）
- 客户端缓冲区最大大小
- 必须承认写入的最小副本数
- 主题键序列化程序默认情况下 - org.apache.kafka.common.serialization.StringSerializer
- 主题值序列化程序默认情况下 - org.apache.kafka.common.serialization.StringSerializer
- 可以为kafka代理连接提供任何其他附加属性


无相关配置

在这个例子中，我们将演示如何配置此扩展，以便每当设备的新遥测消息到达时，能够向Kafka主题发送消息。
继续前提条件Kafka扩展配置：

- 卡夫卡经纪人正在运行
- 创建适当的Kafka主题
- 事件板已启动并运行

## Kafka插件配置

我们首先配置Kafka插件。转到插件菜单并创建新的插件：

![image](http://help.gzhaibaogd.com/images/kafka/kafka-plugin-config-1.png)

![image](http://help.gzhaibaogd.com/images/kafka/kafka-plugin-config-2.png)

请设置正确的Kafka Bootstrap服务器URL和插件配置部分中适用于您的案例的任何其他参数，以便Kafka扩展能够连接到Kafka代理。

点击*“Active”*插件按钮：

![image](http://help.gzhaibaogd.com/images/kafka/kafka-activate-plugin.png)

## kafkad调用规则

现在是时候创建适当的规则了。

![image](http://help.gzhaibaogd.com/images/kafka/kafka-rule-config.png)

添加POST_TELEMETRY消息类型的过滤器

![image](http://help.gzhaibaogd.com/images/kafka/post-telemetry-filter.png)

点击*“ADD”*按钮添加过滤器。
然后在*“PLUGIN”*字段的下拉框中选择*“Kafka Plugin”*：

![image](http://help.gzhaibaogd.com/images/kafka/kafka-plugin-selection.png)

添加将设备温度遥测的特定卡夫卡主题的操作：

![image](http://help.gzhaibaogd.com/images/kafka/send-temp-telemetry.png)

点击*“ADD”*按钮，然后激活规则。
## Kafka示例

现在，您的任何设备发送包含*“temp”*遥测的遥测信息：
{"temp":73.4}

一旦您发布此消息，您应该在适当的Kafka主题中看到**'73.4'**消息。

以下是一个将单个遥测消息发布到本地安装的事件板的命令示例：

```bash
mosquitto_pub -d -h "localhost" -p 1883 -t "v1/devices/me/telemetry" -u "$ACCESS_TOKEN" -m '{"temp":73.4}'
```
