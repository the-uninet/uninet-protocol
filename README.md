# uninet-protocol

用[uninet-address](https://github.com/the-uninet/uninet-address)实现的方法加密。
`公钥`即`地址`，`私钥`即`密钥`。

## 基于WebSocket+JSON的协议

格式`[type: PacketType, data ...]`。

```typescript
const enum PacketType {
  Servers,
  GetServers,
  RoutingTable,
  GetRoutingTable,
  ProxyMe,
  Packet,
}
```

### Servers

提供一些节点。可以在任何时候发出任意数量的。

为`[t: PacketType.Servers, data]`。

`data`为`低层地址`的数组。

### GetServers

获取一些节点。

为`[t: PacketType.GetServers]`。

### RoutingTable

提供一个`路由表`。可以在任何时候发出任意数量的。

为`[t: PacketType.RoutingTable, data]`。

`data`为一个`object`，`键`为`base64`编码的`地址`，`值`为`低层地址`的数组。

#### 低层地址

为`[protocol: string, low_address: string]`

`protocol`可以是`ws`(`WebSocket`)或`wss`(`WebSocket`)。

### GetRoutingTable

获取`路由表`。

为`[t: PacketType.GetRoutingTable]`。

### ProxyMe

要求`WebSocket`服务器为自己提供代理。

由`WebSocket`客户端发出。

为`[t: PacketType.ProxyMe, address: string]`。

`address`为`base64`编码的`地址`。

### Packet

包。

为`[t: PacketType.Packet, address: string, rawsender: string, rawdata: string]`。

`address`为`base64`编码`接收者`的`地址`。

`rawdata`为`base64`编码的加密的数据。

`rawsender`为`base64`编码的加密的`发送者`的`地址`。

#### `rawdata`的加密

0. 用`发送者`的`密钥`加密。
1. 用`接收者`的`地址`加密。

#### `rawdata`的解密

0. 用`接收者`的`密钥`解密。
0. 用`发送者`的`地址`解密。

#### `rawsender`

* 用`接收者`的`地址`加密。
* 用`接收者`的`密钥`解密。
