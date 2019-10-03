# uninet-protocol

用[uninet-address](https://github.com/the-uninet/uninet-address)实现的方法加密。
`公钥`即`地址`，`私钥`即`密钥`。

## 基于WebSocket+JSON的协议

格式`[type: PacketType, data]`。

```
const enum PacketType {
  RoutingTable,
  GetRoutingTable,
  ProxyMe,
  Packet,
}
```

### RoutingTable

提供一个`路由表`。`WebSocket`服务器可以在任何时候发出。
`data`为一个`object`，`键`为`base64`编码的`地址`，`值`为`低层地址`的数组。

#### 低层地址

为`[protocol: string, address: string]`

`protocol`可以是`ws`(`WebSocket`)或`wss`(`WebSocket`)。

### GetRoutingTable

获取`路由表`。
`data`为任意的值。

### ProxyMe

要求`WebSocket`服务器为自己提供代理。
由`WebSocket`客户端发出。
`data`为`base64`编码的`地址`。

### Packet

包。
`data`为`[address: string, rawdata: string]`
`address`为`base64`编码的`地址`。
`rawdata`为`base64`编码的加密的数据。

#### 加密
