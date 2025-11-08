# API 文档

## 获取 Bot 实例

以下所有 bot 均通过这样获取：

```typescript
const bot = (Object.values(ctx.bots) as Bot[]).find(b => b.selfId === botId || b.user?.id === botId);
if (!bot || bot.status !== Universal.Status.ONLINE) {
  ctx.logger.error(`机器人离线或未找到。`);
  return;
}
if (bot == null) return;

// 在这里继续使用 bot.方法
```

## 注意事项

1.  **连接状态**: 确保机器人处于在线状态再调用API
2.  **权限检查**: 某些操作需要管理员权限
3.  **频率限制**: 避免过于频繁的API调用
4.  **错误处理**: 建议对所有API调用进行错误处理
5.  **数据格式**: 返回的数据格式可能随IIROSE更新而变化
6.  **网络异常**: 处理网络超时和连接失败的情况
7.  **房间权限**: 某些操作需要在特定房间或具有特定权限才能执行

## Bot 通用方法

### sendMessage

向指定频道发送消息。

```typescript
sendMessage(channelId: string, content: Fragment, guildId?: string, options?: SendOptions): Promise<string[]>
```

| 参数        | 类型          | 说明                                               |
| :---------- | :------------ | :------------------------------------------------- |
| `channelId` | `string`      | 频道ID，格式为 `public:房间ID` 或 `private:用户ID` |
| `content`   | `Fragment`    | 消息内容，支持文本和图片                           |
| `guildId`   | `string`      | 可选的群组ID                                       |
| `options`   | `SendOptions` | 可选的发送选项                                     |

**返回值:** `Promise<string[]>` - 发送成功的消息ID列表。

**示例:**
```typescript
// 发送公聊消息
await bot.sendMessage('public:room123abc456', 'Hello everyone!')

// 发送私聊消息
await bot.sendMessage('private:user123abc456', 'Hello!')

// 发送图片消息
await bot.sendMessage('public:room123abc456', h.image('https://example.com/image.jpg'))
```

### sendPrivateMessage

向指定用户发送私信。

```typescript
sendPrivateMessage(userId: string, content: Fragment, guildId?: string, options?: SendOptions): Promise<string[]>
```

| 参数      | 类型          | 说明           |
| :-------- | :------------ | :------------- |
| `userId`  | `string`      | 用户ID         |
| `content` | `Fragment`    | 消息内容       |
| `guildId` | `string`      | 可选的群组ID   |
| `options` | `SendOptions` | 可选的发送选项 |

**返回值:** `Promise<string[]>` - 发送成功的消息ID列表。

**示例:**
```typescript
await bot.sendPrivateMessage('user123abc456', 'Hello private!')
```

### getSelf

获取机器人自身信息。

```typescript
getSelf(): Promise<Universal.User>
```

**返回值:** `Promise<Universal.User>` - 机器人用户信息

**示例:**
```typescript
const selfInfo = await bot.getSelf()
ctx.logger.info('机器人名称:', selfInfo.name)
ctx.logger.info('机器人ID:', selfInfo.id)
```

### getUser

获取指定用户信息。

```typescript
getUser(userId: string, guildId?: string): Promise<Universal.User>
```

| 参数      | 类型     | 说明         |
| :-------- | :------- | :----------- |
| `userId`  | `string` | 用户ID       |
| `guildId` | `string` | 可选的群组ID |

**返回值:** `Promise<Universal.User>` - 用户信息对象。

**示例:**
```typescript
const userInfo = await bot.getUser('user123abc456')
ctx.logger.info('用户名:', userInfo.name)
ctx.logger.info('头像:', userInfo.avatar)
```

### getGuildMember

获取群组成员信息。

```typescript
async getGuildMember(guildId: string, userId: string): Promise<Universal.GuildMember>
```

| 参数      | 类型     | 说明   |
| :-------- | :------- | :----- |
| `guildId` | `string` | 群组ID |
| `userId`  | `string` | 用户ID |

**返回值:** `Promise<Universal.GuildMember>` - 群组成员信息。

### getGuildMemberList

获取群组成员列表。

```typescript
async getGuildMemberList(guildId: string, next?: string): Promise<Universal.List<Universal.GuildMember>>
```

| 参数      | 类型     | 说明            |
| :-------- | :------- | :-------------- |
| `guildId` | `string` | 群组ID          |
| `next`    | `string` | 分页参数 (可选) |

**返回值:** `Promise<Universal.List<Universal.GuildMember>>` - 群组成员列表。

### getGuild

获取群组信息。

```typescript
async getGuild(guildId: string): Promise<Universal.Guild>
```

| 参数      | 类型     | 说明   |
| :-------- | :------- | :----- |
| `guildId` | `string` | 群组ID |

**返回值:** `Promise<Universal.Guild>` - 群组信息。

### getGuildList

获取群组列表。

```typescript
async getGuildList(next?: string): Promise<Universal.List<Universal.Guild>>
```

| 参数   | 类型     | 说明            |
| :----- | :------- | :-------------- |
| `next` | `string` | 分页参数 (可选) |

**返回值:** `Promise<Universal.List<Universal.Guild>>` - 群组列表。

### getChannel

获取频道信息。

```typescript
async getChannel(channelId: string): Promise<Universal.Channel>
```

| 参数        | 类型     | 说明   |
| :---------- | :------- | :----- |
| `channelId` | `string` | 频道ID |

**返回值:** `Promise<Universal.Channel>` - 频道信息。

### getChannelList

获取频道列表。

```typescript
async getChannelList(guildId: string): Promise<Universal.List<Universal.Channel>>
```

| 参数      | 类型     | 说明   |
| :-------- | :------- | :----- |
| `guildId` | `string` | 群组ID |

**返回值:** `Promise<Universal.List<Universal.Channel>>` - 频道列表。

### getMessage

获取指定频道中的特定消息详情。

```typescript
getMessage(channelId: string, messageId: string): Promise<Universal.Message>
```

| 参数        | 类型     | 说明   |
| :---------- | :------- | :----- |
| `channelId` | `string` | 频道ID |
| `messageId` | `string` | 消息ID |

**返回值:** `Promise<Universal.Message>` - 消息详情对象或 `undefined`。

**示例:**
```typescript
const message = await bot.getMessage('public:room123abc456', 'msg_key_123')
```
### getMessageKeys

获取当前缓存的所有消息ID。

```typescript
getMessageKeys(): string[]
```

**返回值:** `string[]` - 缓存的消息ID列表。

**示例:**
```typescript
const messageIds = bot.getMessageKeys();
ctx.logger.info('所有缓存的消息ID:', messageIds);
```

:::tip
此方法返回的是适配器内部 `sessionCache` 中缓存的消息 ID 列表。缓存的大小由配置项 `sessionCacheSize` 决定。
:::


### deleteMessage

撤回指定频道中的特定消息。

```typescript
deleteMessage(channelId: string, messageId: string | string[]): Promise<void>
```

| 参数        | 类型     | 说明      |
| :---------- | :------- | :-------- |
| `channelId` | `string` | 频道ID    |
| `messageId` | `string  | string[]` | 消息ID或消息ID数组 |

**返回值:** `Promise<void>`。

**支持的撤回操作:**
- 公共频道消息撤回：支持撤回自己发送的消息
- 私信消息撤回：支持撤回自己发送的私信

**注意事项:**
- 只能撤回自己发送的消息
- 撤回后会触发 `message-deleted` 事件
- 撤回有时间限制，过久的消息可能无法撤回

**示例:**
```typescript
// 撤回公共频道消息
await bot.deleteMessage('public:room123abc456', 'msg_key_123')

// 撤回私信消息
await bot.deleteMessage('private:user123abc456', 'msg_key_456')

// 批量撤回
await bot.deleteMessage('public:room123abc456', ['msg_key_123', 'msg_key_456'])
```

### kickGuildMember

踢出群组成员。

```typescript
kickGuildMember(guildId: string, userId: string, permanent?: boolean): Promise<void>
```

| 参数        | 类型      | 说明                |
| :---------- | :-------- | :------------------ |
| `guildId`   | `string`  | 群组ID (房间ID)     |
| `userId`    | `string`  | 要踢出的用户ID      |
| `permanent` | `boolean` | 是否永久踢出 (可选) |

**返回值:** `Promise<void>`。

**示例:**
```typescript
await bot.kickGuildMember('room123abc456', 'user123abc456')
```

### muteGuildMember

禁言群组成员。

```typescript
muteGuildMember(guildId: string, userId: string, duration: number, reason?: string): Promise<void>
```

| 参数       | 类型     | 说明                                     |
| :--------- | :------- | :--------------------------------------- |
| `guildId`  | `string` | 群组ID (房间ID)                          |
| `userId`   | `string` | 要禁言的用户ID                           |
| `duration` | `number` | 禁言时长 (毫秒)，超过99999秒视为永久禁言 |
| `reason`   | `string` | 禁言原因 (可选)                          |

**返回值:** `Promise<void>`。

**示例:**
```typescript
// 禁言10分钟
await bot.muteGuildMember('room123abc456', 'user123abc456', 10 * 60 * 1000, '刷屏')

// 永久禁言
await bot.muteGuildMember('room123abc456', 'user789def', 999999 * 1000, '违规')
```

## Bot Internal

Bot 的 `internal` 属性提供了更多高级管理功能：

```typescript
bot.internal: InternalType
```

## 房间管理

### moveRoom

切换机器人所在的房间。

```typescript
bot.internal.moveRoom(moveData: { roomId: string; roomPassword?: string }): Promise<void>
```

| 参数                    | 类型     | 说明            |
| :---------------------- | :------- | :-------------- |
| `moveData`              | `object` | 移动数据对象    |
| `moveData.roomId`       | `string` | 目标房间ID      |
| `moveData.roomPassword` | `string` | 房间密码 (可选) |

**示例:**
```typescript
// 移动到公开房间
await bot.internal.moveRoom({ roomId: 'newroom123456' })

// 移动到加密房间
await bot.internal.moveRoom({
  roomId: 'privateroom123456',
  roomPassword: 'room_password'
})
```

### kick

通过用户名踢出用户。

```typescript
bot.internal.kick(kickData: { username: string }): void
```

| 参数                | 类型     | 说明           |
| :------------------ | :------- | :------------- |
| `kickData`          | `object` | 踢人数据对象   |
| `kickData.username` | `string` | 要踢出的用户名 |

### setMaxUser

设置当前房间的最大人数。

```typescript
bot.internal.setMaxUser(data: { maxMember: number }): void
```

| 参数             | 类型     | 说明         |
| :--------------- | :------- | :----------- |
| `data`           | `object` | 设置数据对象 |
| `data.maxMember` | `number` | 最大人数     |

### whiteList

将用户添加到白名单。

```typescript
bot.internal.whiteList(data: { username: string; time: string; intro?: string }): void
```

| 参数            | 类型     | 说明           |
| :-------------- | :------- | :------------- |
| `data`          | `object` | 白名单数据对象 |
| `data.username` | `string` | 用户名         |
| `data.time`     | `string` | 有效时间       |
| `data.intro`    | `string` | 说明 (可选)    |

### subscribeRoom

订阅指定房间的事件。

```typescript
bot.internal.subscribeRoom(roomId: string): void
```

| 参数     | 类型     | 说明           |
| :------- | :------- | :------------- |
| `roomId` | `string` | 要订阅的房间ID |

### unsubscribeRoom

取消订阅指定房间的事件。

```typescript
bot.internal.unsubscribeRoom(roomId: string): void
```

| 参数     | 类型     | 说明               |
| :------- | :------- | :----------------- |
| `roomId` | `string` | 要取消订阅的房间ID |

## 音乐管理

### cutOne

切掉播放列表中的一首歌曲。

```typescript
bot.internal.cutOne(data: { id?: string }): void
```

| 参数      | 类型     | 说明                              |
| :-------- | :------- | :-------------------------------- |
| `data`    | `object` | 切歌数据对象                      |
| `data.id` | `string` | 歌曲ID (可选，不提供则切当前歌曲) |

### cutAll

清空整个播放列表。

```typescript
bot.internal.cutAll(): void
```

## 用户相关

### getUserByName

通过用户名获取用户信息。

```typescript
bot.internal.getUserByName(name: string): Promise<Universal.User | undefined>
```

| 参数   | 类型     | 说明   |
| :----- | :------- | :----- |
| `name` | `string` | 用户名 |

**返回值:** `Promise<Universal.User | undefined>` - 用户信息对象或 `undefined`。

### getUserListFile

获取 `userlist.json` 的内容。

```typescript
bot.internal.getUserListFile(): Promise<any>
```

**返回值:** `Promise<any>` - `userlist.json` 的解析后数据。

### getRoomListFile

获取 `roomlist.json` 的内容。

```typescript
bot.internal.getRoomListFile(): Promise<any>
```

**返回值:** `Promise<any>` - `roomlist.json` 的解析后数据。

### getUserMomentsByUid

获取用户动态。

```typescript
bot.internal.getUserMomentsByUid(uid: string): Promise<UserMoments | null>
```

| 参数  | 类型     | 说明   |
| :---- | :------- | :----- |
| `uid` | `string` | 用户ID |

**返回值:** `Promise<UserMoments | null>` - 返回一个包含用户动态的对象，或在失败时返回 `null`。

### getFollowList

获取用户的关注和粉丝列表。

```typescript
bot.internal.getFollowList(uid: string): Promise<FollowList | null>
```

| 参数  | 类型     | 说明   |
| :---- | :------- | :----- |
| `uid` | `string` | 用户ID |

**返回值:** `Promise<FollowList | null>` - 返回一个包含关注和粉丝列表的对象，或在失败时返回 `null`。

### sendLike

点赞指定用户。

```typescript
bot.internal.sendLike(uid: string, message?: string): void
```

| 参数      | 类型     | 说明              |
| :-------- | :------- | :---------------- |
| `uid`     | `string` | 用户ID            |
| `message` | `string` | 附带的消息 (可选) |

### sendDislike

点踩指定用户。

```typescript
bot.internal.sendDislike(uid: string, message?: string): void
```

| 参数      | 类型     | 说明              |
| :-------- | :------- | :---------------- |
| `uid`     | `string` | 用户ID            |
| `message` | `string` | 附带的消息 (可选) |

### followUser

关注指定用户。

```typescript
bot.internal.followUser(uid: string): void
```

| 参数  | 类型     | 说明   |
| :---- | :------- | :----- |
| `uid` | `string` | 用户ID |

### unfollowUser

取消关注指定用户。

```typescript
bot.internal.unfollowUser(uid: string): void
```

| 参数  | 类型     | 说明   |
| :---- | :------- | :----- |
| `uid` | `string` | 用户ID |

### gradeUser

为用户打分。

```typescript
bot.internal.gradeUser(uid:string, score: number): Promise<GradeUserCallback | null>
```

| 参数    | 类型     | 说明   |
| :------ | :------- | :----- |
| `uid`   | `string` | 用户ID |
| `score` | `number` | 分数   |

**返回值:** `Promise<GradeUserCallback | null>` - 返回一个包含打分结果的对象，或在失败时返回 `null`。

### cancelGradeUser

取消为用户打分。

```typescript
bot.internal.cancelGradeUser(uid:string): Promise<boolean>
```

| 参数  | 类型     | 说明   |
| :---- | :------- | :----- |
| `uid` | `string` | 用户ID |

**返回值:** `Promise<boolean>` - 返回一个布尔值，表示操作是否成功。

## 经济系统

### payment

向指定用户支付花钞。

```typescript
bot.internal.payment(uid: string, money: number, message?: string): Promise<PaymentCallback | null>
```

| 参数      | 类型     | 说明            |
| :-------- | :------- | :-------------- |
| `uid`     | `string` | 收款用户ID      |
| `money`   | `number` | 支付金额        |
| `message` | `string` | 支付留言 (可选) |

**示例:**
```typescript
// 转账给用户，附带留言
bot.internal.payment('user123abc456', 100, '感谢支持！')

// 转账给用户，不附带留言
bot.internal.payment('user123abc456', 50)
```

### stockBuy

购买股票。

```typescript
bot.internal.stockBuy(amount: number): void
```

| 参数     | 类型     | 说明     |
| :------- | :------- | :------- |
| `amount` | `number` | 购买数量 |

### stockSell

出售股票。

```typescript
bot.internal.stockSell(amount: number): void
```

| 参数     | 类型     | 说明     |
| :------- | :------- | :------- |
| `amount` | `number` | 出售数量 |

### stockGet

获取当前股票信息。

```typescript
bot.internal.stockGet(): Promise<Stock | null>
```

**返回值:** `Promise<Stock | null>` - 返回一个包含股票信息的对象，或在失败时返回 `null`。

:::tip
**说明:**

此方法会由适配器自动调用，并会下发 `iirose/stock-update` 事件，建议在该事件中获取股票数据。

当外部插件手动调用此接口时，可以直接通过函数返回值获得股票数据，此时适配器**不会**下发 `iirose/stock-update` 事件。
:::

### bankGet

获取银行信息。

```typescript
bot.internal.bankGet(): Promise<BankCallback | null>
```

**返回值:** `Promise<BankCallback | null>` - 返回一个包含银行信息的对象，或在失败时返回 `null`。

:::tip
**说明:**

此方法会由适配器自动调用，并会下发 `iirose/bank-update` 事件，建议在该事件中获取银行数据。

当外部插件手动调用此接口时，可以直接通过函数返回值获得银行数据，此时适配器**不会**下发 `iirose/bank-update` 事件。
:::

### bankDeposit

存款到银行。

```typescript
bot.internal.bankDeposit(amount: number): void
```

| 参数     | 类型     | 说明     |
| :------- | :------- | :------- |
| `amount` | `number` | 存款金额 |

### bankWithdraw

从银行取款。

```typescript
bot.internal.bankWithdraw(amount: number): void
```

| 参数     | 类型     | 说明     |
| :------- | :------- | :------- |
| `amount` | `number` | 取款金额 |

## 音乐相关

### makeMusic

点播音乐或视频。

```typescript
bot.internal.makeMusic(musicInfo: MusicOrigin): void
```

**参数:**
- `musicInfo`: 音乐信息对象

**示例:**
```typescript
// 播放音乐
bot.internal.makeMusic({
  type: 'music',
  name: '歌曲名称',
  signer: '歌手名称',
  cover: 'https://example.com/cover.jpg',
  link: 'https://example.com/music.mp3',
  url: 'https://example.com/music.mp3',
  duration: 240, // 秒
  bitRate: 320,
  color: '#66ccff',
  lyrics: '歌词内容',
  origin: 'netease'
})
```

## 广播相关

### broadcast

发送全服广播。

```typescript
bot.internal.broadcast(broadcast: { message: string, color: string }): void
```

| 参数                | 类型     | 说明                        |
| :------------------ | :------- | :-------------------------- |
| `broadcast`         | `object` | 广播数据对象                |
| `broadcast.message` | `string` | 广播内容                    |
| `broadcast.color`   | `string` | 广播颜色 (十六进制颜色代码) |

**示例:**
```typescript
// 发送红色广播
bot.internal.broadcast({
  message: 'Hello World!',
  color: '#ff0000'
})
```
### getSelfInfo

获取自身账号信息。

```typescript
bot.internal.getSelfInfo(): Promise<SelfInfo | null>
```

**返回值:** `Promise<SelfInfo | null>` - 返回一个包含自身账号信息的对象，或在失败时返回 `null`。

### updateSelfInfo

修改自身账号信息。

```typescript
bot.internal.updateSelfInfo(profileData: ProfileData): Promise<boolean>
```

| 参数          | 类型          | 说明         |
| :------------ | :------------ | :----------- |
| `profileData` | `ProfileData` | 个人资料对象 |

**返回值:** `Promise<boolean>` - 返回一个布尔值，表示操作是否成功。

### getMusicList

查询当前频道的歌单。

```typescript
bot.internal.getMusicList(): Promise<MediaListItem[] | null>
```

**返回值:** `Promise<MediaListItem[] | null>` - 返回一个包含歌单项目的数组，或在失败时返回 `null`。

### getForum

查询论坛。

```typescript
bot.internal.getForum(): Promise<Forum | null>
```

**返回值:** `Promise<Forum | null>` - 返回一个包含论坛信息的对象，或在失败时返回 `null`。

### getTasks

查询任务。

```typescript
bot.internal.getTasks(): Promise<Tasks | null>
```

**返回值:** `Promise<Tasks | null>` - 返回一个包含任务信息的对象，或在失败时返回 `null`。

### getMoments

查询朋友圈。

```typescript
bot.internal.getMoments(): Promise<Moments | null>
```

**返回值:** `Promise<Moments | null>` - 返回一个包含朋友圈信息的对象，或在失败时返回 `null`。

### getLeaderboard

查询排行榜。

```typescript
bot.internal.getLeaderboard(): Promise<Leaderboard | null>
```

**返回值:** `Promise<Leaderboard | null>` - 返回一个包含排行榜信息的对象，或在失败时返回 `null`。

### getStore

查询商店。

```typescript
bot.internal.getStore(): Promise<Store | null>
```

**返回值:** `Promise<Store | null>` - 返回一个包含商店信息的对象，或在失败时返回 `null`。

### getSellerCenter

查询卖家中心。

```typescript
bot.internal.getSellerCenter(): Promise<SellerCenter | null>
```

**返回值:** `Promise<SellerCenter | null>` - 返回一个包含卖家中心信息的对象，或在失败时返回 `null`。

### addToCart

加入购物车。

```typescript
bot.internal.addToCart(itemId: string): Promise<boolean>
```

| 参数     | 类型     | 说明   |
| :------- | :------- | :----- |
| `itemId` | `string` | 商品ID |

**返回值:** `Promise<boolean>` - 返回一个布尔值，表示操作是否成功。

### removeFromCart

移除购物车。

```typescript
bot.internal.removeFromCart(itemId: string): Promise<boolean>
```

| 参数     | 类型     | 说明   |
| :------- | :------- | :----- |
| `itemId` | `string` | 商品ID |

**返回值:** `Promise<boolean>` - 返回一个布尔值，表示操作是否成功。

### getPendingPaymentOrders

查询等待付款的订单。

```typescript
bot.internal.getPendingPaymentOrders(): Promise<string | null>
```

**返回值:** `Promise<string | null>` - 返回服务器响应的原始字符串，或在失败时返回 `null`。

### getPendingReceiptOrders

查询待收货的订单。

```typescript
bot.internal.getPendingReceiptOrders(): Promise<string | null>
```

**返回值:** `Promise<string | null>` - 返回服务器响应的原始字符串，或在失败时返回 `null`。

### getPendingConfirmationOrders

查询等待确认的订单。

```typescript
bot.internal.getPendingConfirmationOrders(): Promise<string | null>
```

**返回值:** `Promise<string | null>` - 返回服务器响应的原始字符串，或在失败时返回 `null`。

### getPendingReviewOrders

查询等待评价的订单。

```typescript
bot.internal.getPendingReviewOrders(): Promise<string | null>
```

**返回值:** `Promise<string | null>` - 返回服务器响应的原始字符串，或在失败时返回 `null`。

### getCompletedOrders

查询已完成的订单。

```typescript
bot.internal.getCompletedOrders(): Promise<string | null>
```

**返回值:** `Promise<string | null>` - 返回服务器响应的原始字符串，或在失败时返回 `null`。

### getAfterSaleOrders

查询售后中的订单。

```typescript
bot.internal.getAfterSaleOrders(): Promise<string | null>
```

**返回值:** `Promise<string | null>` - 返回服务器响应的原始字符串，或在失败时返回 `null`。

### getFavorites

查询收藏夹。

```typescript
bot.internal.getFavorites(): Promise<string | null>
```

**返回值:** `Promise<string | null>` - 返回服务器响应的原始字符串，或在失败时返回 `null`。

### getFollowedStores

查询关注店铺。

```typescript
bot.internal.getFollowedStores(): Promise<string | null>
```

**返回值:** `Promise<string | null>` - 返回服务器响应的原始字符串，或在失败时返回 `null`。

### getBalance

查询自身余额。

```typescript
bot.internal.getBalance(): Promise<number | null>
```

**返回值:** `Promise<number | null>` - 返回一个数字，表示自身余额，或在失败时返回 `null`。

### summonDice

召唤骰子。

```typescript
bot.internal.summonDice(diceId: number): void
```

| 参数     | 类型     | 说明         |
| :------- | :------- | :----------- |
| `diceId` | `number` | 骰子ID (0-7) |

### getUserProfileByName

通过用户名获取用户资料。

```typescript
bot.internal.getUserProfileByName(username: string): Promise<UserProfileByName | null>
```

| 参数       | 类型     | 说明   |
| :--------- | :------- | :----- |
| `username` | `string` | 用户名 |

**返回值:** `Promise<UserProfileByName | null>` - 返回一个包含用户资料的对象，或在失败时返回 `null`。