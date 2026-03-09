# Feishu Image Send — 飞书图片发送技能

## 🎯 问题描述

在 OpenClaw 中通过飞书（Feishu）发送图片时，如果直接使用本地文件路径，飞书只会发送路径文本，而不是实际图片。

**错误示例**：
```json
{
  "action": "send",
  "channel": "feishu",
  "media": "/tmp/screenshot.png"  // ❌ 错误：只发送路径文本
}
```

**用户看到**：`/tmp/screenshot.png`（纯文本）

---

## ✅ 正确做法

使用 `media:` 协议前缀 + 绝对路径：

```json
{
  "action": "send",
  "channel": "feishu",
  "media": "media:/Users/jo/.openclaw/media/inbound/screenshot.jpg",
  "caption": "这是图片说明"
}
```

**用户看到**：🖼️ 实际图片（带说明文字）

---

## 📋 使用规范

### 1. 路径格式
| 格式 | 是否支持 | 示例 |
|------|---------|------|
| `media:/绝对路径` | ✅ **推荐** | `media:/Users/jo/.openclaw/media/inbound/img.jpg` |
| 绝对路径 | ❌ 不支持 | `/tmp/img.jpg` |
| `~` 开头路径 | ❌ 不支持 | `~/workspace/img.jpg` |
| 相对路径 | ❌ 不支持 | `./img.jpg` |

### 2. 推荐存储位置
- **接收的图片**：`~/.openclaw/media/inbound/`
- **工作区图片**：`~/.openclaw/workspace/`
- **临时图片**：`/tmp/`（需先复制）

### 3. 完整流程示例

```bash
# Step 1: 截图
screencapture -x /tmp/desktop.png

# Step 2: 复制到工作区（可选，但推荐）
cp /tmp/desktop.png ~/.openclaw/workspace/desktop.png

# Step 3: 使用 media: 协议发送
# 在 message 工具中：
{
  "action": "send",
  "channel": "feishu",
  "media": "media:/Users/jo/.openclaw/workspace/desktop.png",
  "caption": "桌面截图"
}
```

---

## 🔧 技术细节

### 为什么需要 `media:` 前缀？
飞书插件在处理消息时，会检查 `media` 参数的格式：
- 如果是 `media:` 开头 → 识别为媒体文件，调用飞书 API 上传并发送
- 如果是普通路径 → 当作文本处理，直接发送路径字符串

### 飞书 API 流程
1. OpenClaw 读取本地图片文件
2. 调用飞书 `upload_image` API 上传图片
3. 获取 `image_key`
4. 发送消息时引用 `image_key`
5. 飞书客户端渲染为实际图片

---

## 🚨 注意事项

1. **文件大小**：飞书限制单张图片最大 10MB
2. **图片格式**：支持 JPG、PNG、GIF、WebP
3. **权限问题**：确保 OpenClaw 有权限读取图片文件
4. **网络环境**：需要能访问飞书 API（`open.feishu.cn`）

---

## 📚 相关文件

- 飞书开放平台文档：https://open.feishu.cn/document/ukTMukTMukTMuYjN1UCM1UTN2EGN
- OpenClaw Issue #17077：飞书图片发送问题追踪
- OpenClaw Issue #19171：媒体发送通用 Bug

---

## 🔄 版本历史

- **2026-03-09**：创建技能，总结 `media:` 协议用法
- **未来**：如果飞书插件更新支持直接路径，此技能可能需要更新

---

## 💡 最佳实践

1. **统一存储**：所有要发送的图片先复制到 `~/.openclaw/workspace/`
2. **规范命名**：使用有意义的文件名（如 `desktop-20260309-1715.png`）
3. **及时清理**：定期清理 `/tmp/` 下的临时图片
4. **错误处理**：发送失败时，先检查路径是否正确，再检查 `media:` 前缀

---

*最后更新：2026-03-09*
*适用版本：OpenClaw 2026.3.1+*
