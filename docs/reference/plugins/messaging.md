---
layout: docwithnav
assignees:
- ashvayka
title: 设备间RPC插件

---


该RPC插件可通过服务器实现各种物联网设备之间的通信。因此设备只有在属于同一个客户时才能交换消息。插件实现可以定制，以涵盖更复杂的安全功能。


配置界面，您可以指定以下配置参数：

- 每个客户的最大设备数量
- 默认请求超时
- 最大请求超时


设备RPC API有两个：*getDevices*和*sendMsg*。下面以MQTT为例进行说明，CoAP和HTTP可参照执行。

## 获取设备列表API
为了向其他设备发送消息，您需要使用设备标识符。设备可以使用*getDevices* RPC调用来请求属于同一客户的其他设备的列表。
执行命令如下：

	{% capture tabspec %}mqtt-get-device-list
	A,mqtt-get-device-list.sh,shell,resources/mqtt-get-device-list.sh,/docs/reference/plugins/resources/mqtt-get-device-list.sh
	B,mqtt-get-device-list.js,javascript,resources/mqtt-get-device-list.js,/docs/reference/plugins/resources/mqtt-get-device-list.js
	C,response.json,javascript,resources/mqtt-get-device-list.json,/docs/reference/plugins/resources/mqtt-get-device-list.json{% endcapture %}

## 发送消息 API

设备可以使用sendMsg RPC调用向同一客户的其他设备发送消息。
下面的示例将尝试从设备“Test Device A1”发送消息到设备“Test Device A2”。
执行命令如下：

	{% capture tabspec %}mqtt-send-msg-fail
	A,mqtt-send-msg.sh,shell,resources/mqtt-send-msg.sh,/docs/reference/plugins/resources/mqtt-send-msg.sh
	B,mqtt-send-msg.js,javascript,resources/mqtt-send-msg.js,/docs/reference/plugins/resources/mqtt-send-msg.js{% endcapture %}

可能接收到如下消息：

```json
{"error":"No active connection to remote device!"}
```

让我们启动目标设备A2的仿真器，并再次发送消息

	{% capture tabspec %}mqtt-receive-msg
	A,mqtt-receive-msg.sh,shell,resources/mqtt-receive-msg.sh,/docs/reference/plugins/resources/mqtt-receive-msg.sh
	B,mqtt-receive-msg.js,javascript,resources/mqtt-receive-msg.js,/docs/reference/plugins/resources/mqtt-receive-msg.js{% endcapture %}

因此，您应该从设备收到以下响应：

```json
{"status":"ok"}
```
请注意，目标设备ID，访问令牌，请求和响应实体将被硬编码为脚本并对应于演示设备。
可在“插件->设备RPC消息插件”中查看插件详细信息。

![img](http://help.gzhaibaogd.com/images/plugin-rpcmessage.png)

