# 用户权限

## 1 本地模式

1.1 设置项是 offline 直接进入本地模式，弹出提示”欢迎使用本地模式“, “offline”

1.2 设置项是 online 但是网络错误，或者用户以游客模式登录，变成本地模式, ‘offline’

## 2 在线模式

### 2.1 用户登录正常，免费版 free

### 2.2 用户登录正常，付费版 vip

所以前端界面使用一个变量控制权限 'offline', 'free', 'vip'，在 app 的 state 中存储

| 字段    | 网络状态 | 登录状态 | 本地上传 | 小说数量 | 在线查询 | 高级设置 | 用户管理 |
| ------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| offline | 断网     | 未登录   | √        | 3        | x        | x        | x        |
| free    | 联网     | 已登录   | √        | 10       | x        | x        | x        |
| vip     | 联网     | 已登录   | √        | 100      | √        | √        | x        |
| admin   | 联网     | 已登录   | √        | 不限     | √        | √        | √        |

### 2.3 admin 账户

只能在后台唯一登录（服务端登录），可以批量操作小说和用户，这部分 API 单独写一套 admin。
