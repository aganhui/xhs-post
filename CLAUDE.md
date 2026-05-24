# CLAUDE.md

xhs-post: 本地生成文案+图片，扫码发布到小红书。Agent-first。

## Architecture

```
cc/Agent → /xhs-post (主编排) → /xhs-copy (文案) + /xhs-html-card (图片) + /xhs-publish (发布)
```

Skills are exposed through the single `xhs-post` plugin in `.claude-plugin/marketplace.json`.

| Skill | Function | Cost |
|-------|----------|------|
| `xhs-post` | 主编排，串联文案→图片→上传→预览→发布 | — |
| `xhs-copy` | 小红书风格文案生成（LLM 直接生成） | 免费 |
| `xhs-html-card` | HTML 卡片配图生成 | 免费 |
| `xhs-publish` | 云服务完整使用说明 | 按用量付费 |

Each skill contains `SKILL.md` (YAML front matter + docs), optional `references/`, `prompts/`, `scripts/`.

## Server

- Production: `https://xhs-post-server.vercel.app`
- Env vars: `XHS_POST_SERVER`, `XHS_POST_API_KEY`
- Register: `POST /api/auth/register`

## Key Constraints

- 标签在正文末尾 `#标签` 格式，不是单独字段
- 标题权重≤20（中文=1，英文/数字=0.5）
- myaibot 是付费接口，测试时不要调用发布
- 图片存 Vercel Blob

## Skill Self-Containment

Each skill is distributed and consumed independently. Therefore:

- **Never link from `SKILL.md` or its `references/` to files outside the skill's own directory.**
- **Inline any shared convention** directly in the skill rather than referencing an out-of-skill doc.

## 开发流程（修改+同步+推送）

**本仓库是唯一修改入口**：`~/cc_work/projects/my_projects/xhs-post/`

cc 运行时读的是插件目录 `~/.claude/plugins/xhs-post/`，不是本仓库。因此：

### 修改后必须同步

```bash
# 1. 编辑本仓库中的文件
# 2. 同步到插件目录（cc 才能读到变更）
rsync -av --exclude='.git' ~/cc_work/projects/my_projects/xhs-post/ ~/.claude/plugins/xhs-post/
# 3. 新 cc 会话自动加载，无需重启终端
```

### 推送到 GitHub

```bash
cd ~/cc_work/projects/my_projects/xhs-post/
git add -A && git commit -m "描述" && git push
```

### 要改哪个文件

| 想改什么 | 文件 |
|---------|------|
| 文案生成规则 | `skills/xhs-copy/SKILL.md` |
| 卡片配图模板/配色 | `skills/xhs-html-card/SKILL.md` 或 `references/` |
| API 调用方式 | `skills/xhs-publish/SKILL.md` 或 `references/` |
| 主流程编排 | `skills/xhs-post/SKILL.md` |
| 插件元信息 | `.claude-plugin/plugin.json` + `marketplace.json` |

## Release Process

1. Update version in `marketplace.json` and `plugin.json`
2. Update `README.md` if needed
3. rsync 同步到插件目录
4. Commit all files together before tag
