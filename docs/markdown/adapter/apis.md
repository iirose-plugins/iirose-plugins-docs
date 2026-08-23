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

1. **连接状态**: 确保机器人处于在线状态再调用API
2. **权限检查**: 某些操作需要管理员权限
3. **频率限制**: 避免过于频繁的API调用
4. **错误处理**: 建议对所有API调用进行错误处理
5. **数据格式**: 返回的数据格式可能随IIROSE更新而变化
6. **网络异常**: 处理网络超时和连接失败的情况
7. **房间权限**: 某些操作需要在特定房间或具有特定权限才能执行

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

:::tip
`permanent: true` 会先使用黑名单实现“永久无法再次加入”，再执行踢出。当前是按黑名单协议推断实现，需要实际环境验证。
:::

### muteGuildMember

禁言群组成员。

```typescript
muteGuildMember(guildId: string, userId: string, duration: number, reason?: string): Promise<void>
```

| 参数       | 类型     | 说明                                     |
| :--------- | :------- | :--------------------------------------- |
| `guildId`  | `string` | 群组ID (房间ID)                          |
| `userId`   | `string` | 要禁言的用户ID                           |
| `duration` | `number` | 禁言时长 (毫秒)，默认 30 分钟；超过99999秒视为永久禁言 |
| `reason`   | `string` | 禁言原因 (可选)                          |

**返回值:** `Promise<void>`。

:::tip
传入 `duration: 0` 表示解除禁言；时长会自动格式化为 `d/h/m/s`，例如 `30m`、`1h`。
:::

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
bot.internal.moveRoom(roomIdOrMoveData: string | { roomId: string; roomPassword?: string }, roomPassword?: string): Promise<void>
```

| 参数                    | 类型     | 说明            |
| :---------------------- | :------- | :-------------- |
| `roomIdOrMoveData`      | `string \| object` | 目标房间ID，或包含 `roomId` / `roomPassword` 的对象 |
| `roomPassword`          | `string` | 使用字符串形式时传入的房间密码 (可选) |

**示例:**

```typescript
// 移动到公开房间
await bot.internal.moveRoom('newroom123456')
await bot.internal.moveRoom({ roomId: 'newroom123456' })

// 移动到加密房间
await bot.internal.moveRoom('privateroom123456', 'room_password')
await bot.internal.moveRoom({ roomId: 'privateroom123456', roomPassword: 'room_password' })
```

### joinRoom

加入/切换到目标房间，行为与 `moveRoom()` 相同，使用对象参数。

```typescript
bot.internal.joinRoom(moveData: { roomId: string; roomPassword?: string }): Promise<void>
```

| 参数                    | 类型     | 说明            |
| :---------------------- | :------- | :-------------- |
| `moveData`              | `object` | 移动数据对象    |
| `moveData.roomId`       | `string` | 目标房间ID      |
| `moveData.roomPassword` | `string` | 房间密码 (可选) |

### getRoomId

获取机器人当前所在的房间ID。

```typescript
bot.internal.getRoomId(): string
```

**返回值:** `string` - 当前房间的ID。

**示例:**

```typescript
const currentRoom = bot.internal.getRoomId();
ctx.logger.info('机器人当前在房间:', currentRoom);
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

### getRoomMaxUsers

查询当前房间最大人数限制，返回 `null` 表示不限制。

```typescript
bot.internal.getRoomMaxUsers(): Promise<number | null>
```

### setRoomMaxUsers

设置当前房间最大人数；不传或传 `null` 表示恢复不限制。

```typescript
bot.internal.setRoomMaxUsers(count?: number | null): Promise<boolean>
```

### getRoomMaxGuests

查询当前房间最大游客人数限制，返回 `null` 表示不限制。

```typescript
bot.internal.getRoomMaxGuests(): Promise<number | null>
```

### setRoomMaxGuests

设置当前房间最大游客人数；不传或传 `null` 表示恢复不限制。

```typescript
bot.internal.setRoomMaxGuests(count?: number | null): Promise<boolean>
```

### getRoomMinImpression

查询当前房间最低印象门槛，返回 `null` 表示默认及格线以上。

```typescript
bot.internal.getRoomMinImpression(): Promise<number | null>
```

### setRoomMinImpression

设置当前房间最低印象门槛；不传或传 `null` 表示恢复默认及格线以上。

```typescript
bot.internal.setRoomMinImpression(score?: number | null): Promise<boolean>
```

### whiteList

将用户添加到白名单。

```typescript
bot.internal.whiteList(data: { username: string; time: string | number; intro?: string }): void
```

| 参数            | 类型     | 说明           |
| :-------------- | :------- | :------------- |
| `data`          | `object` | 白名单数据对象 |
| `data.username` | `string` | 用户名         |
| `data.time`     | `string \| number` | 有效时间，默认 30 分钟；支持 `d/h/m/s` 或毫秒 |
| `data.intro`    | `string` | 说明 (可选)    |

### getMediaWhitelist

查询当前房间“限制发言&点播”白名单。

```typescript
bot.internal.getMediaWhitelist(): Promise<MediaWhitelistEntry[] | null>
```

**返回值:** `Promise<MediaWhitelistEntry[] | null>` - 白名单列表，失败或超时返回 `null`。

```typescript
interface MediaWhitelistEntry {
  username: string
  uid: string
  expireAt: number
  intro: string
}
```

### addMediaWhitelist

添加“限制发言&点播”白名单。

```typescript
bot.internal.addMediaWhitelist(username: string, duration: string | number, intro: string): Promise<boolean>
```

| 参数       | 类型     | 说明             |
| :--------- | :------- | :--------------- |
| `username` | `string` | 用户名           |
| `duration` | `string \| number` | 持续时间，默认 30 分钟；支持 `d/h/m/s` 或毫秒 |
| `intro`    | `string` | 备注             |

### removeMediaWhitelist

移除“限制发言&点播”白名单。

```typescript
bot.internal.removeMediaWhitelist(uid: string): Promise<boolean>
```

| 参数  | 类型     | 说明   |
| :---- | :------- | :----- |
| `uid` | `string` | 用户ID |

### clearMediaWhitelist

清空当前房间“限制发言&点播”白名单。

```typescript
bot.internal.clearMediaWhitelist(): Promise<boolean>
```

### setRoomSpeechLevel

设置房间发言限制。

```typescript
bot.internal.setRoomSpeechLevel(level: 0 | 1 | 2 | 3 | 4 | 5): Promise<boolean>
```

| 等级 | 含义 |
| :--- | :--- |
| `0` | 所有人 |
| `1` | 普通成员以上 |
| `2` | 带星成员以上 |
| `3` | 仅房主 |
| `4` | 白名单以上 |
| `5` | 仅白名单 |

### setRoomMusicLevel

设置房间点播限制。

```typescript
bot.internal.setRoomMusicLevel(level: 0 | 1 | 2 | 3 | 4 | 5): Promise<boolean>
```

等级含义与 `setRoomSpeechLevel` 相同。

### setRoomBothLevel

同时设置房间发言和点播限制。

```typescript
bot.internal.setRoomBothLevel(level: 0 | 1 | 2 | 3 | 4 | 5): Promise<boolean>
```

等级含义与 `setRoomSpeechLevel` 相同。

### getMuteList

查询当前房间禁言列表。

```typescript
bot.internal.getMuteList(): Promise<MuteListEntry[] | null>
```

```typescript
interface MuteListEntry {
  username: string
  uid: string
  expireAt: number
  intro: string
  type: number
}
```

`type`：`1` 禁止发言，`2` 禁止点播，`3` 同时禁止。

### muteUser

禁言用户。

```typescript
bot.internal.muteUser(type: 'chat' | 'music' | 'all', username: string, duration: string | number, intro: string): Promise<boolean>
```

| 参数       | 类型     | 说明 |
| :--------- | :------- | :--- |
| `type`     | `string` | `chat` 禁止发言，`music` 禁止点播，`all` 同时禁止 |
| `username` | `string` | 用户名 |
| `duration` | `string \| number` | 持续时间，默认 30 分钟；支持 `d/h/m/s` 或毫秒 |
| `intro`    | `string` | 备注 |

:::tip
服务器返回 `_~m1` 表示“此用户不存在”，禁言、黑名单、白名单相关 API 会返回 `false`。
:::

### unmuteUser

解除禁言。

```typescript
bot.internal.unmuteUser(uid: string): Promise<boolean>
```

| 参数  | 类型     | 说明   |
| :---- | :------- | :----- |
| `uid` | `string` | 用户ID |

### clearMuteList

清空当前房间禁言列表。

```typescript
bot.internal.clearMuteList(): Promise<boolean>
```

### getBlacklist

查询当前房间黑名单。

```typescript
bot.internal.getBlacklist(): Promise<MediaWhitelistEntry[] | null>
```

### addBlacklist

添加黑名单。

```typescript
bot.internal.addBlacklist(username: string, duration: string | number, intro: string): Promise<boolean>
```

| 参数       | 类型                 | 说明                                   |
| :--------- | :------------------- | :------------------------------------- |
| `username` | `string`             | 用户名                                 |
| `duration` | `string \| number`   | 持续时间，默认 30 分钟；支持 `d/h/m/s` 或毫秒 |
| `intro`    | `string`             | 备注                                   |

### removeBlacklist

移除黑名单。

```typescript
bot.internal.removeBlacklist(uid: string): Promise<boolean>
```

### clearBlacklist

清空当前房间黑名单。

```typescript
bot.internal.clearBlacklist(): Promise<boolean>
```

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

发送后服务端会返回 `Ds` 回执；适配器会自动维护今日剩余广播次数的本地缓存。

### getBroadcastRemaining

获取今日剩余广播次数。

```typescript
bot.internal.getBroadcastRemaining(): Promise<number>
```

**返回值:** `Promise<number>` - 今日剩余广播次数，默认 `10`，跨天后自动恢复。

**示例:**

```typescript
const remaining = await bot.internal.getBroadcastRemaining()
ctx.logger.info('今日剩余广播次数:', remaining)
```

### recordBroadcastAck

记录一次广播回执并扣减今日剩余广播次数。

```typescript
bot.internal.recordBroadcastAck(): Promise<number>
```

**返回值:** `Promise<number>` - 扣减后的剩余广播次数。

:::tip
一般无需手动调用。适配器收到 `Ds` 广播回执后会自动调用，并写入：

`ctx.baseDir/data/adapter/adapter-iirose/<botId>/wsdata/broadcastCount.json`

文件内容包含 `botId`、`remaining`、`date`，其中 `date` 用于跨日自动重置。
:::

### sendRoomNotice

发送当前房间（聊天室）公告。

```typescript
bot.internal.sendRoomNotice(notice: string): void
```

| 参数     | 类型     | 说明       |
| :------- | :------- | :--------- |
| `notice` | `string` | 公告内容   |

**示例:**

```typescript
bot.internal.sendRoomNotice('这是本房间公告')
```

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

### clearMedia

清空整个播放列表（`cutAll` 的别名）。

```typescript
bot.internal.clearMedia(): void
```

### seekMedia

快进或快退当前媒体。

```typescript
bot.internal.seekMedia(operation: '<' | '>', time: string): Promise<number | null>
```

| 参数        | 类型       | 说明                        |
| :---------- | :--------- | :-------------------------- |
| `operation` | `'<' \| '>'` | `'<'` 快退，`'>'` 快进 |
| `time`      | `string`   | 时间，如 `1s`、`1m`、`1h`  |

**返回值:** `Promise<number | null>` - 移动后的播放位置秒数，失败或超时返回 `null`。

### jumpMedia

跳转到指定播放位置。

```typescript
bot.internal.jumpMedia(time: string): Promise<number | null>
```

| 参数   | 类型     | 说明                 |
| :----- | :------- | :------------------- |
| `time` | `string` | 时间，如 `1:30` 或秒数 |

**返回值:** `Promise<number | null>` - 移动后的播放位置秒数，失败或超时返回 `null`。

### exchangeMedia

交换歌单中两首媒体的位置。

```typescript
bot.internal.exchangeMedia(id1: string, id2: string): void
```

| 参数  | 类型     | 说明                         |
| :---- | :------- | :--------------------------- |
| `id1` | `string` | 媒体ID，格式为 `index_length` |
| `id2` | `string` | 媒体ID，格式为 `index_length` |

### nextMedia

切到下一首媒体。

```typescript
bot.internal.nextMedia(): Promise<boolean>
```

**返回值:** `Promise<boolean>` - 收到成功回执返回 `true`，当前无媒体或失败返回 `false`。

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

### requestMusic

点歌。调用方需要先自行解析出直链和歌词，再调用适配器发送点歌报文。

```typescript
bot.internal.requestMusic(musicInfo: MusicOrigin): void
```

**示例:**

```typescript
bot.internal.requestMusic({
  type: 'music',
  name: '我的悲伤是水做的',
  signer: 'ChiliChill乐团 & 洛天依Official',
  cover: 'https://p1.music.126.net/cover.jpg',
  link: 'https://music.163.com/#/song?id=1439814454',
  url: 'https://m10.music.126.net/audio.mp3',
  duration: 225.8,
  bitRate: 128,
  color: 'b0c4c7',
  lyrics: '[00:00.000] 歌词内容',
  origin: 'netease'
})
```

### getMusicList

查询当前频道的歌单。

```typescript
bot.internal.getMusicList(): Promise<MediaListItem[] | null>
```

**返回值:** `Promise<MediaListItem[] | null>` - 返回一个包含歌单项目的数组，或在失败时返回 `null`。

:::tip
`MediaListItem.id` 的格式为 `index_length`，例如 `0_98.893787`，可直接用于 `exchangeMedia()`。
:::

```typescript
interface MediaListItem {
  id: string
  length: number
  title: string
  artist: string
  requester: string
  cover: string
}
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

### requestUserList

请求服务器重新下发全服用户在线列表。

```typescript
bot.internal.requestUserList(): void
```

:::tip
适配器在登录时和定时完整报文中已经会自动更新 `userlist.json`，一般无需手动调用。
:::

### getRoomListFile

获取 `roomlist.json` 的内容。

```typescript
bot.internal.getRoomListFile(): Promise<any>
```

**返回值:** `Promise<any>` - `roomlist.json` 的解析后数据。

### getRoomList

获取当前缓存的房间列表。

```typescript
bot.internal.getRoomList(): Promise<any>
```

**返回值:** `Promise<any>` - `roomlist.json` 的解析后数据。

:::tip
房间列表来自登录/定时完整报文，当前没有单独的房间列表请求协议，所以该 API 读取的是本地缓存。
:::

### getRoomStateFile

获取新版登录大包解析出的 `roomState.json` 内容。

```typescript
bot.internal.getRoomStateFile(): Promise<RoomState | null>
```

**返回值:** `Promise<RoomState | null>` - 房间状态对象，或在文件不存在/解析失败时返回 `null`。

```typescript
interface RoomMusicState {
  audioUrl: string
  pageUrl: string
  duration: number
  name: string
  artist: string
  requester: string
  requesterGender: string
  cover: string
  requesterAvatar: string
  position: number
  lyrics: string
}

interface RoomState {
  raw: string
  music?: RoomMusicState
}
```

**示例:**
```typescript
const roomState = await bot.internal.getRoomStateFile()
if (roomState?.music) {
  ctx.logger.info('当前播放:', roomState.music.name)
}
```

:::tip
该文件只在存在房间状态的新版 `%1` 登录包中生成；没有音乐等状态的老版 `%*"` 包不会创建 `roomState.json`。
:::

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

### getUserProfileByName

通过用户名获取用户资料（兼容接口）。

```typescript
bot.internal.getUserProfileByName(username: string): Promise<FullUserProfileByName | null>
```

| 参数       | 类型     | 说明   |
| :--------- | :------- | :----- |
| `username` | `string` | 用户名 |

**返回值:** `Promise<FullUserProfileByName | null>` - 返回完整用户资料对象，或在失败时返回 `null`。

:::tip
该接口保留用于兼容，类型已统一为 `FullUserProfileByName`，不再单独公开 `UserProfileByName`。
:::

### getFullUserProfileByName

通过用户名获取完整的用户资料（`+1` 资料报文）。

```typescript
bot.internal.getFullUserProfileByName(username: string): Promise<FullUserProfileByName | null>
```

| 参数       | 类型     | 说明   |
| :--------- | :------- | :----- |
| `username` | `string` | 用户名 |

**返回值:** `Promise<FullUserProfileByName | null>` - 返回一个包含完整用户资料的对象，或在失败时返回 `null`。

```typescript
interface FullUserProfileByName {
  username: string // 查询使用的用户名
  surname: string // 姓
  givenName: string // 名
  nickname: string // 昵称/显示名
  id: string // 唯一标识 ID（+1 资料包中通常为空）
  gender: 'male' | 'female' | 'unknown' // 性别
  birthday: string // 生日
  age: number // 年龄
  residence: string // 居住地/住址
  hobbies: string[] // 爱好列表
  friends: string[] // 好友列表
  hobby: string // 爱好（兼容字段）
  sandbox: string // 个人空间标识（兼容字段）
  isCertified: boolean // 是否已认证（兼容字段）
  registrationTime: string // 注册时间
  bio: string // 个人简介
  album: string // 相册置顶图片
  background: string // 个人资料背景图
  backgroundMusic: {
    name: string // 歌名
    artist: string // 作者
    audio: string // 音频地址
    cover: string // 封面地址
    link: string // 歌曲链接
  }
  lastLoginTime: string // 最后登录时间
  visits: number // 访问量
  title: string // 头衔
  accountStatus: string // 账户状态原始标记
  status: string // 在线状态原始标记
  timezone: string // 时区
  likes: number // 获赞数
  likers: string[] // 获赞者列表
  topLiker: string // 获赞最多者
  money: number // 金钱（钞）
  following: number // 关注数
  followers: number // 粉丝数
  contribution: number // 贡献
  communities: string[] // 社区列表
  tag: string // 标签
  onlineDuration: number // 在线时长（小时）
  todayActivity: number // 今日活跃（分钟）
  totalActivity: number // 总活跃（分钟）
  activity: number // 活跃时长（分钟，兼容字段）
  credit: number // 信用分
  bankDeposit: number // 银行存款
  debt: number // 欠款
  donations: number // 捐款
  dislikes: number // 踩
  location: string // 坐标/当前位置（兼容字段）
  locationId: string // 坐标 ID（兼容字段）
  comments: string[] // 留言/评论（兼容字段）
  impression: {
    count: number // 印象人数
    percentage: number // 印象占比
    multiplier: number // 印象倍数
  }
  albumImages: {
    url: string // 图片地址
    timestamp?: number // 发布时间
    description: string // 图片描述
  }[]
}
```

:::tip
**说明:** 用户资料类型已统一为 `FullUserProfileByName`；旧的 `getUserProfileByName` 保留用于兼容，需要完整字段时优先使用此方法。
:::

### getMoments

查询朋友圈。

```typescript
bot.internal.getMoments(): Promise<Moments | null>
```

**返回值:** `Promise<Moments | null>` - 返回一个包含朋友圈信息的对象，或在失败时返回 `null`。

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

### getBalance

查询自身余额。

```typescript
bot.internal.getBalance(): Promise<number | null>
```

**返回值:** `Promise<number | null>` - 返回一个数字，表示自身余额，或在失败时返回 `null`。

## 商店与订单

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

## 系统功能

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

### getLeaderboard

查询排行榜。

```typescript
bot.internal.getLeaderboard(): Promise<Leaderboard | null>
```

**返回值:** `Promise<Leaderboard | null>` - 返回一个包含排行榜信息的对象，或在失败时返回 `null`。

### getChangelog

获取 IIROSE 版本更新日志。适配器会请求 `https://iirose.com/lib/php/function/changes.php` 并将纯文本解析为结构化 JSON。

```typescript
bot.internal.getChangelog(): Promise<ChangelogData | null>
```

**返回值:** `Promise<ChangelogData | null>` - 返回解析后的版本日志对象，或在失败时返回 `null`。

```typescript
interface ChangelogEntry {
  version: string
  changes: string[]
}

interface ChangelogData {
  latest: string
  versions: ChangelogEntry[]
}
```

**示例:**

```typescript
const changelog = await bot.internal.getChangelog()
if (changelog) {
  ctx.logger.info('最新版本:', changelog.latest)
  ctx.logger.info('版本数:', changelog.versions.length)
}
```

## 杂项

### summonDice

召唤骰子。

```typescript
bot.internal.summonDice(diceId: number): void
```

| 参数     | 类型     | 说明         |
| :------- | :------- | :----------- |
| `diceId` | `number` | 骰子ID (0-7) |
