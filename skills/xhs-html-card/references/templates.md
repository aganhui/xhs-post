# HTML 模板变体

## 标准模板（默认）

上方 SKILL.md 中的基础模板，适用于大多数列表型内容。

## 封面卡模板

封面卡视觉冲击力更强，没有编号列表：

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
    justify-content: center;
    padding: 80px 72px;
    color: {{TEXT_COLOR}};
  }
  .tag {
    font-size: 28px;
    font-weight: 600;
    letter-spacing: 4px;
    opacity: 0.7;
    margin-bottom: 48px;
  }
  .title {
    font-size: 88px;
    font-weight: 900;
    line-height: 1.15;
    margin-bottom: 32px;
    max-width: 936px;
  }
  .subtitle {
    font-size: 36px;
    line-height: 1.6;
    opacity: 0.7;
    max-width: 800px;
  }
  .decoration {
    position: absolute;
    bottom: 80px;
    right: 72px;
    font-size: 180px;
    font-weight: 900;
    opacity: 0.06;
    line-height: 1;
  }
</style>
</head>
<body>
  <div class="tag">{{CATEGORY_TAG}}</div>
  <div class="title">{{TITLE}}</div>
  <div class="subtitle">{{SUBTITLE}}</div>
  <div class="decoration">{{EMOJI}}</div>
</body>
</html>
```

## 结尾卡模板

结尾卡用于总结和引导互动：

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
    justify-content: center;
    align-items: center;
    padding: 80px 72px;
    color: {{TEXT_COLOR}};
    text-align: center;
  }
  .summary {
    font-size: 48px;
    font-weight: 700;
    line-height: 1.4;
    margin-bottom: 48px;
    max-width: 800px;
  }
  .divider {
    width: 120px;
    height: 4px;
    background: currentColor;
    opacity: 0.2;
    border-radius: 2px;
    margin-bottom: 48px;
  }
  .cta {
    font-size: 36px;
    line-height: 1.6;
    opacity: 0.7;
    max-width: 700px;
  }
  .footer {
    font-size: 24px;
    opacity: 0.4;
    position: absolute;
    bottom: 80px;
  }
</style>
</head>
<body>
  <div class="summary">{{SUMMARY}}</div>
  <div class="divider"></div>
  <div class="cta">{{CTA_TEXT}}</div>
  <div class="footer">❤️ 点赞收藏，下次不迷路</div>
</body>
</html>
```

## 对比型模板

适合 A/B 对比、前后对比等内容：

```html
<!-- body 样式同标准模板，items 区域改为两列 -->
<div class="compare">
  <div class="compare-col bad">
    <div class="compare-header">❌ 之前</div>
    <div class="compare-item">{{BAD_ITEM}}</div>
  </div>
  <div class="compare-col good">
    <div class="compare-header">✅ 之后</div>
    <div class="compare-item">{{GOOD_ITEM}}</div>
  </div>
</div>
```

```css
.compare { display: flex; gap: 32px; flex: 1; }
.compare-col { flex: 1; border-radius: 16px; padding: 32px; }
.compare-col.bad { background: rgba(239,68,68,0.1); }
.compare-col.good { background: rgba(34,197,94,0.1); }
.compare-header { font-size: 32px; font-weight: 700; margin-bottom: 24px; }
.compare-item { font-size: 28px; line-height: 1.6; opacity: 0.8; }
```
