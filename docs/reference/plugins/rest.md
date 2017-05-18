---
layout: docwithnav
assignees:
- ashvayka
title: REST API Call Plugin

---


Rest API插件负责将HTTP请求发送到由特定规则触发的特定端点
您可以指定以下配置参数：

- http端点服务器主机
- http端点服务器端口
- http请求的基本路径
- http认证方式。目前没有授权或基本支持
- 用户名，如果是基本的验证方法
- 基本auth方法的密码

在本例中，我们将演示如何配置Rest API Call扩展，以便每当设备的新的遥测消息到达时，能够将POST或PUT请求发送到特定的REST端点。

继续之前的先决条件REST API呼叫扩展配置：

- 事件板已启动并运行
- 能够接收POST或PUT请求的第三方HTTP服务器和特定端点启动并运行
##REST插件配置

我们首先配置REST API Call plugin。转到插件菜单，点击'+'按钮并创建新的插件：

![image](http://help.gzhaibaogd.com/images/rest-api-call/rest-api-call-plugin-config-1.png)

![image](http://help.gzhaibaogd.com/images/rest-api-call/rest-api-call-plugin-config-2.png)

请为要转移请求的REST端点设置正确的URL，端口，路径和身份验证方法。保存插件并点击“激活”插件按钮：

![image](http://help.gzhaibaogd.com/images/rest-api-call/rest-api-call-activate-plugin.png)

##REST调用规则

现在是时候创建适当的规则了。

![image](http://help.gzhaibaogd.com/images/rest-api-call/rest-api-call-rule-config.png)

添加POST_TELEMETRY消息类型的过滤器

![image](http://help.gzhaibaogd.com/images/rest-api-call/post-telemetry-filter.png)

点击“添加”按钮添加过滤器。
然后在插件字段的下拉框中选择“REST API调用插件”：

![image](http://help.gzhaibaogd.com/images/rest-api-call/rest-api-call-plugin-selection.png)

Add action that will send temperature telemetry of device to particular REST action-path. In the action you can provide request type and result code that you expected from the REST endpoint in case of successful call.

![image](http://help.gzhaibaogd.com/images/rest-api-call/rest-api-call-rule-action-config-1.png)

![image](http://help.gzhaibaogd.com/images/rest-api-call/rest-api-call-rule-action-config-2.png)

Click ‘Add’ button and then activate Rule.
##REST示例

Now for any of your devices send Telemetry message that contains ‘temp’ telemetry:
```json
{"temp":73.4}
```

You should see ‘73.4’ as a body request in appropriate rest endpoint once you’ll post this message.
Here is an example of a command that publish single telemetry message to locally installed Thingsboard:
mosquitto_pub -d -h "localhost" -p 1883 -t "v1/devices/me/telemetry" -u "$ACCESS_TOKEN" -m '{"temp":73.4}'

```bash
mosquitto_pub -d -h "localhost" -p 1883 -t "v1/devices/me/telemetry" -u "$ACCESS_TOKEN" -m '{"temp":73.4}'
```
