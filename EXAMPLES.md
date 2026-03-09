# 使用示例 (Examples)

## 📸 示例 1：发送桌面截图

```bash
# Step 1: 截图
screencapture -x /tmp/screenshot.png

# Step 2: 复制到工作区（推荐）
cp /tmp/screenshot.png ~/.openclaw/workspace/

# Step 3: 发送（在 message 工具中）
{
  "action": "send",
  "channel": "feishu",
  "media": "media:/Users/jo/.openclaw/workspace/screenshot.png",
  "caption": "桌面截图"
}
```

## 🖼️ 示例 2：发送多张图片

```javascript
// 发送图片 1
{
  "action": "send",
  "channel": "feishu",
  "media": "media:/Users/jo/.openclaw/workspace/image1.png",
  "caption": "第一张图片"
}

// 发送图片 2
{
  "action": "send",
  "channel": "feishu",
  "media": "media:/Users/jo/.openclaw/workspace/image2.png",
  "caption": "第二张图片"
}
```

## 📷 示例 3：从接收的图片目录发送

```javascript
// 从 inbound 目录发送（用户发送的图片）
{
  "action": "send",
  "channel": "feishu",
  "media": "media:/Users/jo/.openclaw/media/inbound/received.jpg",
  "caption": "这是你刚才发的图片"
}
```

## 🔄 示例 4：在子 Agent 中发送图片

```javascript
// 在主 Agent 中
const subAgent = await sessions_spawn({
  task: "处理图片并返回结果",
  tools: ["read", "message"]
});

// 子 Agent 完成后，主 Agent 发送结果图片
{
  "action": "send",
  "channel": "feishu",
  "media": "media:/Users/jo/.openclaw/workspace/result.png",
  "caption": "处理完成！"
}
```

## ❌ 错误示例（不要用！）

```javascript
// ❌ 错误 1：没有 media: 前缀
{
  "media": "/tmp/image.png"  // 只会发送路径文本
}

// ❌ 错误 2：使用 ~ 路径
{
  "media": "media:~/workspace/image.png"  // 不支持
}

// ❌ 错误 3：相对路径
{
  "media": "media:./image.png"  // 不支持
}

// ✅ 正确：绝对路径 + media: 前缀
{
  "media": "media:/Users/jo/.openclaw/workspace/image.png"  // 正确！
}
```

## 🎯 最佳实践

1. **统一存储位置**：所有要发送的图片先复制到 `~/.openclaw/workspace/`
2. **规范命名**：使用有意义的文件名（如 `desktop-20260309-1715.png`）
3. **及时清理**：定期清理 `/tmp/` 下的临时图片
4. **错误处理**：发送失败时，先检查路径是否正确，再检查 `media:` 前缀

---

*最后更新：2026-03-09*
