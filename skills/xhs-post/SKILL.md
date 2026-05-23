---
name: xhs-post
description: 小红书笔记发布主流程。本地生成文案+图片，扫码发布到小红书。
---

# xhs-post: 小红书笔记发布

在 cc 中一句话完成：AI 生成小红书风格文案 + 配图，扫码发布到小红书。

## 产品定位

| 能力 | 本地（免费） | 服务器（付费） |
|------|-------------|---------------|
| 文案生成 | cc 用 LLM 生成 | ¥0.50（火山引擎 doubao） |
| 图片生成 | /xhs-html-card（插件内置） | 暂不支持 |
| 发布 | — | ¥2.00，生成二维码，手机扫码发布 |

**核心原则**：出图出文默认本地完成（免费），只有发布走服务器。

## 服务器信息

- 线上：`https://xhs-post-server.vercel.app`
- 配置：
  ```bash
  export XHS_POST_SERVER="https://xhs-post-server.vercel.app"
  export XHS_POST_API_KEY="your-api-key"
  ```
- 注册获取 API Key：
  ```bash
  curl -X POST $XHS_POST_SERVER/api/auth/register \
    -H "Content-Type: application/json" \
    -d '{"name": "your_name"}'
  ```

## 完整流程

```
用户在 cc 中说：帮我发一篇小红书，主题是"XXX"

Step 1: 本地生成文案（免费）
  调用 /xhs-copy 生成小红书风格文案

Step 2: 本地生成图片（免费）
  调用 /xhs-html-card 生成卡片配图

Step 3: 服务器创建笔记 + 上传图片
  调用 /xhs-publish 的 API 说明，执行 curl 命令

Step 4: 预览确认
  展示 preview_url，用户在浏览器中预览和编辑

Step 5: 扫码发布
  调用 /xhs-publish 的发布 API，展示二维码，用户扫码发布
```

## 子 Skill

| Skill | 功能 | 费用 |
|-------|------|------|
| `/xhs-copy` | 小红书风格文案生成规则 | 免费 |
| `/xhs-html-card` | HTML 卡片配图生成 | 免费 |
| `/xhs-publish` | 云服务 API 调用说明 | 按用量付费 |

## Agent 交互规范

1. **本地优先**：文案和图片默认本地生成（免费），用户要求时才用服务器 API
2. **预览优先**：创建笔记后，必须将 `preview_url` 展示给用户
3. **确认发布**：发布前必须询问用户，收到明确肯定后才调用 publish
4. **图片必须**：没有图片不允许发布
5. **余额检查**：402 错误 → 告知余额不足
6. **标签在正文里**：标签以 `#标签` 格式写在正文末尾，不需要单独字段
7. **二维码展示**：publish 返回 qrcode 后，必须展示给用户让其扫码

## 费用

典型一篇笔记：本地出图出文 + 服务器发布 = **¥2.10**（上传1次 ¥0.10 + 发布 ¥2.00）
