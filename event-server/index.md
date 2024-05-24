
# PaperCard事件服务器

---- 基于`WebSocket`技术实现实时事件推送

## 意义

为了实现：实时地多端消息互通。如在**网页**上封禁了一个玩家，要将该事件发布出去，MC服接收到此事件后作出响应踢出相关玩家。

## 连接地址

`ws://paper-card.cn/event`

## 连接认证

只有授权的客户端（主要为MC服）才能连接到事件服务器，来接收事件进行处理，或者发布事件。

客户端提供客户端ID、密钥HASH等信息进行认证。

## 发布的事件消息格式

基本格式：

```json5
{
    "type": "事件名称",
    "client_id": "发布事件的客户端ID",
    // ... 其他参数
}
```

假设一个玩家进入MC服务器事件：

```json5
{
    "type": "player.join.server",
    "client_id": "发布事件的客户端ID",
    "name": "玩家游戏名",
    "id": "玩家游戏ID",
    "time": 0, // 秒级时间戳
    "online": 1, // 在线玩家数量
}
```

[查看所有事件类型](./events.md)

## 事件发布

连接到事件服务器的客户端通过发送JSON消息来发布事件，事件服务器不检查消息格式，只会将事件广播给其它客户端。

消息格式与发布事件的基本相同，但是不需要提供`client_id`，事件服务器能够识别哪个客户端发布事件。