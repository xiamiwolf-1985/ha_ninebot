# 补丁说明 v1.2.1

## 修复：登录报“接口返回错误 / 无法登录”

### 原因
九号 passport 服务端已对登录接口启用 `Sign` 签名校验。
原实现调用 `v3/openClaw/user/login` 且只提交账号密码，服务端返回
`90051 sign is null` / `90031 sign invalid`，集成侧表现为“接口返回错误”。

### 修复
- 登录改走 App 原生端点 `/v6/user/login`
- 按九号 passport 规范计算签名：`sha256(按参数名排序的 k=v&k=v&...)`
  （参数含 app_version / areaCode / clientKey / device / os / os_language /
  os_version / password / timestamp / url / username）
- 补齐 `App_version` / `Clientid` / `Os*` / `Timestamp` / `Sign` 请求头

### 兼容性
基于上游 Wuty-zju/ha_ninebot v1.2.0，仅修改 `api.py` 与 `const.py`。
