# 事件系统

本页面详细介绍了 IIROSE 适配器提供的所有事件类型及其使用方法。

## Koishi 通用事件

### before-send

消息发送前触发的事件。

```typescript
ctx.on('before-send', (session) => {
  ctx.logger.info('即将发送消息:', session.content)
  // 可以在这里修改消息内容或阻止发送
})
```

**事件数据:** Koishi Session 对象

**使用场景:**
- 消息内容过滤
- 消息格式化
- 发送权限检查

### send

消息发送后触发的事件。

```typescript
ctx.on('send', (session) => {
  ctx.logger.info('消息已发送:', session.messageId)
})
```

**事件数据:** Koishi Session 对象，包含已发送的消息ID

**使用场景:**
- 消息发送统计
- 发送日志记录
- 后续处理逻辑

### message

接收到消息时触发的事件。

```typescript
ctx.on('message', (session) => {
  ctx.logger.info('收到消息:', session.content)
  ctx.logger.info('发送者:', session.author.name)
  ctx.logger.info('频道:', session.channelId)
})
```

**事件数据:** Koishi Session 对象

**使用场景:**
- 消息处理
- 自动回复
- 消息统计

### message-deleted

消息被撤回时触发的事件。

```typescript
ctx.on('message-deleted', (session) => {
  ctx.logger.info('消息被撤回:', session.messageId)
  ctx.logger.info('撤回者ID:', session.user.id)
  ctx.logger.info('频道ID:', session.channelId)
  ctx.logger.info('撤回时间:', session.timestamp)
})
```

**事件数据:** Koishi Session 对象，包含被撤回的消息信息

## 房间与用户

### iirose/guild-member-refresh

房间成员列表刷新时触发。

```typescript
ctx.on('iirose/guild-member-refresh', (session) => {
  ctx.logger.info('房间成员列表已刷新')
})
```
- **`session`**: Koishi Session 对象

### iirose/switchRoom

用户切换房间时触发。

```typescript
ctx.on('iirose/switchRoom', (session, data) => {
  ctx.logger.info(`用户 ${data.username} 从 ${data.fromRoom} 切换到 ${data.toRoom}`)
})
```
- **`data`**: `MessageType['switchRoom']` - 包含用户名、来源房间和目标房间。

### iirose/selfMove

机器人自身移动房间后触发。

```typescript
ctx.on('iirose/selfMove', (session, data) => {
  ctx.logger.info('机器人移动到房间:', data.roomId)
})
```
- **`data`**: `MessageType['selfMove']` - 包含目标房间ID。

## 媒体

### iirose/music-play

播放列表有新音乐开始播放时触发。

```typescript
ctx.on('iirose/music-play', (session, data) => {
  ctx.logger.info(`正在播放: ${data.name} - ${data.signer}`)
})
```
- **`data`**: `MessageType['music']` - 包含音乐的详细信息。

## 邮箱与互动

:::tip
以下事件均来自于用户的“邮箱”系统，提供了丰富的用户互动信息。
:::

### iirose/roomNotice

收到房间公告时触发。

```typescript
ctx.on('iirose/roomNotice', (session, data) => {
  ctx.logger.info(`收到房间公告: ${data.title}`)
})
```
- **`session`**: Koishi Session 对象
- **`data`**: 包含公告内容的对象，例如 `title`, `message`。

### iirose/follower

收到新的关注者（粉丝）时触发。

```typescript
ctx.on('iirose/follower', (session, data) => {
  ctx.logger.info(`新粉丝: ${data.username}`)
})
```
- **`session`**: Koishi Session 对象
- **`data`**: 包含粉丝信息，例如 `username`, `userId`。

### iirose/like

当机器人收到点赞时触发。

```typescript
ctx.on('iirose/like', (session, data) => {
  ctx.logger.info(`收到来自 ${data.username} 的点赞`)
})
```
- **`session`**: Koishi Session 对象
- **`data`**: 包含点赞者信息和附带消息。

### iirose/dislike

当机器人收到点踩时触发。

```typescript
ctx.on('iirose/dislike', (session, data) => {
  ctx.logger.info(`收到来自 ${data.username} 的点踩`)
})
```
- **`session`**: Koishi Session 对象
- **`data`**: 包含点踩者信息和附带消息。

### iirose/payment

当机器人收到转账时触发。

```typescript
ctx.on('iirose/payment', (session, data) => {
  ctx.logger.info(`收到来自 ${data.username} 的 ${data.amount} 花钞`)
})
```
- **`session`**: Koishi Session 对象
- **`data`**: 包含支付方信息、金额和留言。

## 经济系统

### iirose/stock-update

股票信息更新时触发。此事件由适配器定时获取并分发。

```typescript
ctx.on('iirose/stock-update', (stockData) => {
  ctx.logger.info('股票价格:', stockData.price)
  ctx.logger.info('涨跌:', stockData.change)
})
```
- **`stockData`**: `Stock` - 包含股票价格、涨跌、成交量等信息。

### iirose/bank-update

银行存款信息更新时触发。此事件由适配器定时获取并分发。

```typescript
ctx.on('iirose/bank-update', (bankData) => {
  ctx.logger.info('当前存款:', bankData.savings)
})
```
- **`bankData`**: `BankCallback` - 包含银行存款等信息。

## 其他

### iirose/broadcast

收到全服广播时触发。

```typescript
ctx.on('iirose/broadcast', (session, data) => {
  ctx.logger.info('收到广播:', data.message)
})
```
- **`data`**: `BroadcastMessage` - 包含广播内容和颜色。
