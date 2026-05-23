# API 响应格式详细参考

## 创建笔记响应

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "title": "这3个AI工具让我准时下班",
  "content": "正文内容...\n\n#AI工具 #效率 #准时下班",
  "images": [],
  "status": "draft",
  "preview_url": "https://xhs-post-server.vercel.app/p/550e8400-e29b-41d4-a716-446655440000",
  "created_at": "2026-05-24T00:00:00Z",
  "updated_at": "2026-05-24T00:00:00Z"
}
```

## 上传图片响应

```json
{
  "note_id": "550e8400-e29b-41d4-a716-446655440000",
  "images": [
    {
      "url": "https://xxx.vercel-storage.com/xhs/card-1-xxxx.png",
      "index": 0
    },
    {
      "url": "https://xxx.vercel-storage.com/xhs/card-2-xxxx.png",
      "index": 1
    }
  ]
}
```

## 发布响应

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "published",
  "publish_url": "https://www.xiaohongshu.com/discovery/item/xxxxx",
  "qrcode": "https://myaibot.vip/qrcode/xxxxx",
  "balance": 800
}
```

## 错误响应格式

```json
{
  "error": "Insufficient balance",
  "code": "INSUFFICIENT_BALANCE",
  "balance": 50
}
```

## 笔记列表响应

```json
{
  "notes": [
    {
      "id": "uuid",
      "title": "标题",
      "status": "draft",
      "images_count": 3,
      "created_at": "2026-05-24T00:00:00Z"
    }
  ],
  "total": 10
}
```

## 生成文案响应

```json
{
  "title": "5个超好用的AI写作工具",
  "content": "正文内容...\n\n#AI写作 #效率工具 #文案",
  "word_count": 320
}
```
