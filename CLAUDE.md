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

## Release Process

1. Update version in `marketplace.json` and `plugin.json`
2. Update `README.md` if needed
3. Commit all files together before tag
