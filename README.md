# xhs-post

本地生成文案+图片，扫码发布到小红书。Agent-first。

## 它是什么

xhs-post 是一个面向 Claude Code / Agent 的小红书笔记发布工具：

- **本地生成**文案和图片（免费，用自己的算力）
- **扫码发布**到小红书（服务器生成二维码，手机扫码跳转小红书 App）

## 工作流程

```
用户在 cc 中输入 → /xhs-post "5个AI写作工具"

1. cc 用 LLM 生成小红书风格文案（本地，免费）
2. cc 用 HTML 渲染小红书卡片配图（本地，免费）
3. 服务器创建笔记 + 托管图片
4. 用户预览确认
5. 服务器生成发布二维码
6. 用户手机扫码 → 跳转小红书 → 完成发布
```

## 快速开始

### 1. 安装 Plugin

将本仓库 clone 到 cc 的 plugins 目录：

```bash
# 一键安装
mkdir -p ~/.claude/plugins && \
git clone https://github.com/aganhui/xhs-post.git ~/.claude/plugins/xhs-post
```

### 2. 注册获取 API Key

```bash
curl -X POST https://xhs-post-server.vercel.app/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name": "your_name"}'
```

### 3. 配置环境变量

```bash
export XHS_POST_SERVER="https://xhs-post-server.vercel.app"
export XHS_POST_API_KEY="your-api-key"
```

建议写入 `~/.zshrc` 持久化。

### 4. 开始使用

在 Claude Code 中输入：

```
/xhs-post "5个AI写作工具"
```

或者单独使用子 skill：

```
/xhs-copy "5个AI写作工具"        # 只生成文案
/xhs-html-card "5个AI写作工具"   # 只生成卡片图
/xhs-publish                     # 云服务使用说明
```

## 包含的 Skills

| Skill | 功能 | 费用 |
|-------|------|------|
| `/xhs-post` | 主流程编排，串联所有步骤 | — |
| `/xhs-copy` | 小红书风格文案生成（LLM 直接生成） | 免费 |
| `/xhs-html-card` | HTML 卡片配图生成（内置渲染） | 免费 |
| `/xhs-publish` | 云服务完整使用说明（注册、笔记CRUD、上传、发布） | 按用量付费 |

## 费用

| 操作 | 本地 | 服务器 |
|------|------|--------|
| 文案生成 | 免费 | ¥0.50 |
| 图片生成 | 免费 | 暂不支持 |
| 图片上传 | — | ¥0.10/次 |
| 发布（二维码） | — | ¥2.00 |

典型一篇笔记：本地出图出文 + 服务器发布 = **¥2.10**

新用户注册赠送 **¥10**。

## License

MIT
