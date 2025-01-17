# chatgpt-api

封装 OpenAI 网页版最新 ChatGPT 接口, 不需要使用 API Key, 完全免费

## 部署

需要 `部署在海外服务器` 或 `使用代理`, 否则可能无法正常调用 OpenAI

### 方式一

```bash
docker run -d -p 3000:3000 zhuweiyou/chatgpt-api:latest
```

### 方式二

安装 `nodejs 18.x` 和 `python 3.x` 环境

```bash
pip3 install -r requirements.txt
npm install
npm start
```

## 文档

### Base URL

<http://127.0.0.1:3000>

### 请求失败响应

```json
{
    "message": "错误消息"
}
```

### 所有 API 都使用 POST 请求

#### `/get_access_token` 获取 token

-   `email` OpenAI 帐号 (不消耗免费额度, 也不需要花钱, 只用于登录获取 token)
-   `password` OpenAI 密码

#### 响应

```json
{
    "access_token": "xxx"
}
```

> 有一段时间有效期, 建议开发者缓存至少 1 个小时以上, 而不是每次都调用获取

如果获取失败, 临时方案可以手动登录网页版复制 `access_token`

![截图](https://user-images.githubusercontent.com/8413791/225305658-188ec53c-c3ee-4ec6-9306-9ff9ce2c94af.png)

#### `/send_message` 向 ChatGPT 提问

-   `access_token` 在 /get_access_token 中获取的 access_token
-   `prompt` 提问内容
-   `timeout` 可选. 超时时间(毫秒), 默认无限等待
-   `conversation_id` 可选. 前一次 /send_message 的结果中返回, 用于上下文连续会话
-   `parent_message_id` 可选. 前一次 /send_message 的结果中返回, 用于上下文连续会话
-   `prompt_prefix` 可选. 默认为 'return the result in Chinese' 会让它尽量用中文回答
-   `prompt_suffix` 可选. 默认为 空
-   `reverse_proxy` 可选. 反向代理服务器, 用于绕过 cloudflare 人机验证, 默认内置

#### 响应

```json
{
    "text": "ChatGPT的回答",
    "conversation_id": "xxx",
    "parent_message_id": "yyy"
}
```

## 感谢

-   [transitive-bullshit/chatgpt-api](https://github.com/transitive-bullshit/chatgpt-api) 提供 ChatGPT 问答
-   [acheong08/OpenAIAuth](https://github.com/acheong08/OpenAIAuth) 提供 OpenAI 登录
