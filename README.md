# OpenClaw 飞书图片发送技能 (Feishu Image Send Skill)

🖼️ 解决 OpenClaw 通过飞书（Feishu）发送图片时只能发送路径文本，无法发送实际图片的问题。

## 📦 问题描述

在 OpenClaw 中通过飞书发送图片时，如果直接使用本地文件路径，飞书只会发送路径文本，而不是实际图片。

**错误示例**：
```json
{
  "action": "send",
  "channel": "feishu",
  "media": "/tmp/screenshot.png"  // ❌ 只发送路径文本
}
```

**用户看到**：`/tmp/screenshot.png`（纯文本）

## ✅ 解决方案

使用 `media:` 协议前缀 + 绝对路径：

```json
{
  "action": "send",
  "channel": "feishu",
  "media": "media:/Users/jo/.openclaw/workspace/screenshot.png",  // ✅ 发送实际图片
  "caption": "桌面截图"
}
```

**用户看到**：🖼️ 实际图片（带说明文字）

## 🚀 快速开始

### 方法 1：安装技能包

```bash
# 1. 克隆此仓库到 OpenClaw skills 目录
cd ~/.openclaw/workspace/skills/
git clone https://github.com/YOUR_USERNAME/feishu-image-skill.git

# 2. 在对话中使用
# 现在你的 Agent 已经知道如何正确发送图片了！
```

### 方法 2：直接使用协议

在任何需要发送图片的场景，使用 `media:` 前缀：

```javascript
// 在 message 工具中
{
  "action": "send",
  "channel": "feishu",
  "media": "media:/完整/绝对/路径/图片.jpg",
  "caption": "图片说明"
}
```

## 📋 路径格式规范

| 格式 | 是否支持 | 示例 |
|------|---------|------|
| `media:/绝对路径` | ✅ **推荐** | `media:/Users/jo/.openclaw/media/inbound/img.jpg` |
| 绝对路径 | ❌ 不支持 | `/tmp/img.jpg` |
| `~` 开头路径 | ❌ 不支持 | `~/workspace/img.jpg` |
| 相对路径 | ❌ 不支持 | `./img.jpg` |

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

## 📁 推荐存储位置

- **✅ 工作区图片**：`~/.openclaw/workspace/`（**推荐**，OpenClaw 信任目录）
- **接收的图片**：`~/.openclaw/media/inbound/`
- **临时图片**：`/tmp/`（⚠️ **不能直接发送**，需先复制到工作区）

## ⚠️ 关键注意事项（2026-03-09 更新）

### ❌ 常见错误：从临时目录发送
```json
{
  "media": "media:/tmp/screenshot.png"  // ❌ 失败：/tmp/ 目录权限不足
}
```

### ✅ 正确做法：先复制到工作区
```bash
# 1. 复制到工作区
cp /tmp/screenshot.png ~/.openclaw/workspace/

# 2. 从工作区发送
{
  "media": "media:/Users/jo/.openclaw/workspace/screenshot.png"  // ✅ 成功
}
```

**原因**：飞书插件只能访问 OpenClaw 信任的目录（如 `~/.openclaw/workspace/`），临时目录（`/tmp/`）可能因权限问题导致发送失败。

## 🚨 注意事项

1. **文件大小**：飞书限制单张图片最大 10MB
2. **图片格式**：支持 JPG、PNG、GIF、WebP
3. **权限问题**：确保 OpenClaw 有权限读取图片文件
4. **网络环境**：需要能访问飞书 API（`open.feishu.cn`）

## 📚 完整文档

查看 [SKILL.md](./SKILL.md) 获取完整的技能文档。

## 🔗 相关链接

- [OpenClaw Issue #17077](https://github.com/openclaw/openclaw/issues/17077) - 飞书图片发送问题追踪
- [OpenClaw Issue #19171](https://github.com/openclaw/openclaw/issues/19171) - 媒体发送通用 Bug
- [飞书开放平台文档](https://open.feishu.cn/document/ukTMukTMukTMuYjN1UCM1UTN2EGN)

## 📝 版本历史

- **v1.0.0** (2026-03-09) - 初始版本，总结 `media:` 协议用法

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📄 许可证

MIT License

---

*最后更新：2026-03-09（添加目录权限说明）*
*适用版本：OpenClaw 2026.3.1+*
