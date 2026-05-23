---
name: xhs-html-card
description: 用 HTML+CSS 生成小红书风格卡片配图，本地免费。
---

# xhs-html-card: HTML 卡片配图生成

用 HTML+CSS 渲染小红书风格卡片，截图后生成 PNG 配图。完全本地，免费。

## 卡片规格

- 尺寸：1080×1440px（3:4，小红书推荐比例）
- 字体：系统默认（macOS 用苹方，Windows 用微软雅黑）
- 背景：渐变色或纯色，根据主题选择
- 支持生成 1-9 张图片

## 使用方式

在 cc 中说：
- "帮我生成3张小红书卡片图" → 直接生成
- 作为 /xhs-post 流程的 Step 2 自动调用

## 生成步骤

### Step 1: 根据主题选择配色方案

根据笔记主题从以下方案中选择：

| 主题类型 | 配色 | 背景 CSS |
|---------|------|---------|
| 科技/AI | 深蓝+青 | `linear-gradient(135deg, #0f172a, #1e3a5f)` |
| 生活/好物 | 暖粉+橙 | `linear-gradient(135deg, #fdf2f8, #fce7f3)` |
| 美食 | 橙+黄 | `linear-gradient(135deg, #fff7ed, #fed7aa)` |
| 职场/效率 | 灰蓝+白 | `linear-gradient(135deg, #f8fafc, #e2e8f0)` |
| 读书/知识 | 米黄+棕 | `linear-gradient(135deg, #fefce8, #fef3c7)` |
| 旅行 | 天蓝+绿 | `linear-gradient(135deg, #ecfeff, #a7f3d0)` |

### Step 2: 生成 HTML 文件

为每张卡片生成一个独立 HTML 文件，保存到 `/tmp/xhs-cards/` 目录。

HTML 模板：

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body {
    width: 1080px;
    height: 1440px;
    background: {{BACKGROUND_CSS}};
    font-family: -apple-system, 'PingFang SC', 'Microsoft YaHei', sans-serif;
    display: flex;
    flex-direction: column;
    padding: 80px 72px;
    color: {{TEXT_COLOR}};
  }
  .tag {
    font-size: 28px;
    font-weight: 600;
    letter-spacing: 4px;
    text-transform: uppercase;
    opacity: 0.7;
    margin-bottom: 40px;
  }
  .title {
    font-size: 72px;
    font-weight: 800;
    line-height: 1.2;
    margin-bottom: 48px;
    max-width: 936px;
  }
  .items {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 32px;
  }
  .item {
    display: flex;
    gap: 24px;
    align-items: flex-start;
  }
  .item-num {
    font-size: 48px;
    font-weight: 800;
    opacity: 0.3;
    min-width: 64px;
  }
  .item-text {
    font-size: 36px;
    line-height: 1.5;
    opacity: 0.9;
  }
  .footer {
    font-size: 24px;
    opacity: 0.4;
    text-align: center;
    margin-top: 40px;
  }
</style>
</head>
<body>
  <div class="tag">{{CATEGORY_TAG}}</div>
  <div class="title">{{TITLE}}</div>
  <div class="items">
    <div class="item">
      <div class="item-num">01</div>
      <div class="item-text">{{ITEM_1}}</div>
    </div>
    <div class="item">
      <div class="item-num">02</div>
      <div class="item-text">{{ITEM_2}}</div>
    </div>
    <!-- 更多条目 -->
  </div>
  <div class="footer">左右滑动查看更多 →</div>
</body>
</html>
```

### Step 3: 截图生成 PNG

使用 Playwright 将 HTML 渲染为 PNG：

```bash
# cc 会自动使用 Playwright MCP 工具完成截图
# 或者手动用浏览器打开 HTML 截图
```

截图参数：
- viewport: 1080×1440
- deviceScaleFactor: 1
- fullPage: false

### Step 4: 保存图片

将截图保存到 `/tmp/xhs-cards/` 目录：
- `card-1.png`, `card-2.png`, ...

## 卡片内容规则

- **封面卡**（第1张）：大标题 + 副标题，视觉冲击
- **内容卡**（第2-8张）：每卡3-5个要点，编号清晰
- **结尾卡**（最后1张）：总结 + 引导互动

## 典型输出

3张卡片的笔记：
1. 封面卡：标题 "这3个AI工具让我准时下班"
2. 内容卡：3个工具的详细介绍
3. 结尾卡：总结 + "你有什么好用的AI工具？评论区聊聊"
