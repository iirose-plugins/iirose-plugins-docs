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

### guild-member-added

当有新用户加入房间时触发。

```typescript
ctx.on('guild-member-added', (session) => {
  ctx.logger.info(`用户 ${session.user.name} (${session.userId}) 加入了房间 ${session.guildId}`)
})
```

**事件数据:** Koishi Session 对象

**使用场景:**

- 发送欢迎消息
- 记录新成员信息

### guild-member-removed

当有用户离开房间时触发。

```typescript
ctx.on('guild-member-removed', (session) => {
  ctx.logger.info(`用户 ${session.user.name} (${session.userId}) 离开了房间 ${session.guildId}`)
})
```

**事件数据:** Koishi Session 对象

**使用场景:**

- 发送送别消息
- 清理用户数据

## 房间与用户

### iirose/guild-member-refresh

房间成员列表刷新时触发。

```typescript
ctx.on('iirose/guild-member-refresh', (session) => {
  ctx.logger.info('房间成员列表已刷新')
})
```

- **`session`**: Koishi Session 对象

### iirose/guild-member-switchRoom

用户切换房间时触发。

```typescript
ctx.on('iirose/guild-member-switchRoom', (session, data) => {
  ctx.logger.info(`用户 ${data.username} 从 ${data.fromRoom} 切换到 ${data.toRoom}`)
})
```

- **`data`**: `MessageType['switchRoom']` - 包含用户名、来源房间和目标房间。

### iirose/room-state

收到新版登录大包时触发，携带完整包最前面的当前房间状态信息，例如正在播放的音乐。

```typescript
ctx.on('iirose/room-state', (state) => {
  ctx.logger.info('当前播放:', state.music?.name)
  ctx.logger.info('点歌人:', state.music?.requester)
})
```

- **`state`**: `RoomState` - 包含 `raw` 原始状态，以及可选的 `music` 当前音乐信息。

:::tip
该状态同时会写入 `ctx.baseDir/data/adapter-iirose/<botId>/wsdata/roomState.json`，可通过 `bot.internal.getRoomStateFile()` 读取。
:::

没有房间状态的老版 `%*"` 登录包不会触发该事件。

### iirose/media-whitelist-list

收到“限制发言&点播”白名单列表时触发。

```typescript
ctx.on('iirose/media-whitelist-list', (list) => {
  ctx.logger.info('白名单人数:', list.length)
})
```

- **`list`**: `MediaWhitelistEntry[]` - 白名单成员列表。

### iirose/media-whitelist-event

收到“限制发言&点播”白名单增删事件时触发。

```typescript
ctx.on('iirose/media-whitelist-event', (data) => {
  ctx.logger.info('白名单变更:', data.type)
})
```

- **`data`**: `MediaWhitelistEvent` - 包含 `type: 'added' | 'removed'`，以及可选的 `expireAt`、`intro`、`roomId`。

### iirose/room-restriction

收到房间发言或点播限制变化时触发。

```typescript
ctx.on('iirose/room-restriction', (data) => {
  ctx.logger.info('限制类型:', data.type)
  ctx.logger.info('限制等级:', data.level)
})
```

- **`data`**: `RoomRestrictionEvent` - `type` 为 `speech | music | both`，`level` 为 `0-5`。

### iirose/mute-list

收到禁言列表时触发。

```typescript
ctx.on('iirose/mute-list', (list) => {
  ctx.logger.info('禁言人数:', list.length)
})
```

- **`list`**: `MuteListEntry[]` - 禁言列表。

### iirose/mute-event

收到禁言或解除禁言事件时触发。

```typescript
ctx.on('iirose/mute-event', (data) => {
  ctx.logger.info('禁言事件:', data.type)
})
```

- **`data`**: `MuteEvent` - `type` 为 `added | removed`，并包含 `muteType`。

### iirose/blacklist-list

收到黑名单列表时触发。

```typescript
ctx.on('iirose/blacklist-list', (list) => {
  ctx.logger.info('黑名单人数:', list.length)
})
```

- **`list`**: `MediaWhitelistEntry[]` - 黑名单列表。

### iirose/blacklist-event

收到黑名单增删或登录被拦截事件时触发。

```typescript
ctx.on('iirose/blacklist-event', (data) => {
  ctx.logger.info('黑名单事件:', data.type)
})
```

- **`data`**: `BlacklistEvent` - `type` 为 `added | blocked`。

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

### iirose/mailbox

收到任意信箱内容时触发；之后还会按具体类型触发下方细分事件。

```typescript
ctx.on('iirose/mailbox', (session, data) => {
  ctx.logger.info('信箱类型:', data.type)
})
```

- **`session`**: Koishi Session 对象
- **`data`**: `MailboxMessageData`，`data.type` 为 `roomNotice | follower | like | dislike | payment`

:::tip
`MailboxMessageData` 已从插件入口导出，可在插件中直接引入使用。
:::

### iirose/roomNotice

收到房间公告时触发。

```typescript
ctx.on('iirose/roomNotice', (session, data) => {
  ctx.logger.info(`收到房间公告: ${data.notice}`)
})
```

- **`session`**: Koishi Session 对象
- **`data`**: 包含公告内容的对象，例如 `notice`, `background`, `timestamp`。

### iirose/follower

收到新的关注者（粉丝）时触发。

```typescript
ctx.on('iirose/follower', (session, data) => {
  ctx.logger.info(`新粉丝: ${data.username}`)
})
```

- **`session`**: Koishi Session 对象
- **`data`**: 包含粉丝信息，例如 `username`, `avatar`, `gender`。

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
  ctx.logger.info(`收到来自 ${data.username} 的 ${data.money} 花钞`)
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

### iirose/broadcast-ack

收到广播发送回执时触发。IIROSE 服务端会返回 `Ds`，但不会在回执中携带剩余次数；适配器会根据本地缓存维护今日剩余广播次数。

```typescript
ctx.on('iirose/broadcast-ack', (remaining) => {
  ctx.logger.info('今日剩余广播次数:', remaining)
})
```

- **`remaining`**: `number` - 本地缓存中的今日剩余广播次数。

:::tip
该次数是本地估算值。缓存文件位于 `ctx.baseDir/data/adapter-iirose/<botId>/wsdata/broadcastCount.json`，跨日会自动重置。
:::
