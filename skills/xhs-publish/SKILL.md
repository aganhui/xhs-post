---
name: xhs-publish
description: xhs-post 云服务使用说明。注册、创建笔记、上传图片、预览、扫码发布的完整 API 参考。
---

# xhs-publish: 云服务使用说明

xhs-post-server 的完整 API 使用说明。所有付费操作通过此服务完成。

## 前置配置

```bash
export XHS_POST_SERVER="https://xhs-post-server.vercel.app"
export XHS_POST_API_KEY="your-api-key"
```

### 注册获取 API Key

```bash
curl -X POST $XHS_POST_SERVER/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name": "your_name"}'
```

新用户注册赠送 ¥10。

### 查看账户信息

```bash
curl -s $XHS_POST_SERVER/api/me \
  -H "x-api-key: $XHS_POST_API_KEY" | python3 -m json.tool
```

---

## API 1: 创建笔记（免费，自提供文案）

```bash
curl -X POST $XHS_POST_SERVER/api/notes \
  -H "x-api-key: $XHS_POST_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "标题",
    "content": "正文内容\n\n#标签1 #标签2"
  }'
```

**注意**：标签以 `#标签` 格式写在 content 末尾，不是单独字段。

返回：
```json
{
  "id": "note-uuid",
  "title": "...",
  "content": "...",
  "images": [],
  "status": "draft",
  "preview_url": "https://xhs-post-server.vercel.app/p/note-uuid"
}
```

**必须将 `preview_url` 展示给用户**，让其在浏览器中预览和编辑。

### 服务器生成文案（¥0.50）

如果用户明确要求用服务器生成文案：

```bash
curl -X POST $XHS_POST_SERVER/api/notes \
  -H "x-api-key: $XHS_POST_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"topic": "5个超好用的AI写作工具"}'
```

服务器使用火山引擎 doubao 模型生成文案，风格规则与本地 /xhs-copy 一致。

## API 2: 笔记列表（免费）

```bash
curl -s $XHS_POST_SERVER/api/notes \
  -H "x-api-key: $XHS_POST_API_KEY" | python3 -m json.tool
```

## API 3: 查看单条笔记（免费）

```bash
curl -s $XHS_POST_SERVER/api/notes/note-uuid \
  -H "x-api-key: $XHS_POST_API_KEY" | python3 -m json.tool
```

## API 4: 修改笔记（免费）

```bash
curl -X PATCH $XHS_POST_SERVER/api/notes/note-uuid \
  -H "x-api-key: $XHS_POST_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"title": "新标题", "content": "新内容\n\n#标签"}'
```

## API 5: 上传图片（¥0.10/次）

```bash
curl -X POST $XHS_POST_SERVER/api/notes/upload-image \
  -H "x-api-key: $XHS_POST_API_KEY" \
  -F "images=@/tmp/xhs-cards/card-1.png" \
  -F "images=@/tmp/xhs-cards/card-2.png" \
  -F "note_id=note-uuid"
```

最多上传 9 张图片。

## API 6: 扫码发布（¥2.00/次）

**必须等用户确认后才能调用！**

```bash
curl -X POST $XHS_POST_SERVER/api/notes/note-uuid/publish \
  -H "x-api-key: $XHS_POST_API_KEY"
```

返回：
```json
{
  "id": "note-uuid",
  "status": "published",
  "publish_url": "https://...",
  "qrcode": "https://...",
  "balance": 800
}
```

**发布流程**：
1. 服务器调用 myaibot 生成发布二维码
2. 将 `qrcode` URL 展示给用户
3. 用户用小红书 App 扫码
4. 跳转小红书发布页面，确认发布

## API 7: 删除笔记（免费）

```bash
curl -X DELETE $XHS_POST_SERVER/api/notes/note-uuid \
  -H "x-api-key: $XHS_POST_API_KEY"
```

## API 8: 只生成文案不创建笔记（¥0.50）

```bash
curl -X POST $XHS_POST_SERVER/api/notes/generate-copy \
  -H "x-api-key: $XHS_POST_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"topic": "5个超好用的AI写作工具"}'
```

生成文案但不创建笔记，适用于用户想先看看服务器生成的文案效果。

---

## 错误处理

| 状态码 | 含义 | 处理 |
|--------|------|------|
| 401 | API Key 无效 | 检查 XHS_POST_API_KEY 是否正确 |
| 402 | 余额不足 | 告知用户余额不足，需要充值 |
| 404 | 笔记不存在 | 检查 note_id 是否正确 |
| 429 | 请求过于频繁 | 等待后重试 |

## 费用总结

| 操作 | 费用 |
|------|------|
| 注册 | 免费，赠送 ¥10 |
| 创建笔记（自提供文案） | 免费 |
| 修改/删除/查看笔记 | 免费 |
| 上传图片 | ¥0.10/次 |
| 扫码发布 | ¥2.00/次 |
| 服务器生成文案 | ¥0.50/次（可选） |

典型一篇笔记：本地出图出文 + 上传1次 + 发布 = **¥2.10**
