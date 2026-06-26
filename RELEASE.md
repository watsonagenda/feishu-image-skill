# Release Notes - v1.0.0

**发布日期**: 2026-03-09
**适用版本**: OpenClaw 2026.3.1+

## 🎯 解决的问题

在 OpenClaw 中通过飞书（Feishu）发送图片时，如果直接使用本地文件路径，飞书只会发送路径文本，而不是实际图片。

**问题表现**：
- 用户看到的是 `/tmp/image.png` 文本
- 不是实际图片

## ✨ 解决方案

发现并验证了 `media:` 协议前缀的使用方法：

```javascript
// ✅ 正确用法
{
  "action": "send",
  "channel": "feishu",
  "media": "media:/完整/绝对/路径/图片.jpg",
  "caption": "图片说明"
}
```

## 📦 包含内容

- ✅ `SKILL.md` - 完整技能文档
- ✅ `README.md` - 使用说明
- ✅ `EXAMPLES.md` - 使用示例
- ✅ `LICENSE` - MIT 许可证
- ✅ `.gitignore` - Git 忽略文件

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

## 🚀 使用方法

### 方法 1：安装技能包

```bash
cd ~/.openclaw/workspace/skills/
git clone https://github.com/watsonagenda/feishu-image-skill.git
```

### 方法 2：直接使用协议

在任何需要发送图片的场景，使用 `media:` 前缀 + 绝对路径。

## 📋 路径格式规范

| 格式 | 是否支持 | 示例 |
|------|---------|------|
| `media:/绝对路径` | ✅ **推荐** | `media:/Users/jo/.openclaw/media/inbound/img.jpg` |
| 绝对路径 | ❌ 不支持 | `/tmp/img.jpg` |
| `~` 开头路径 | ❌ 不支持 | `~/workspace/img.jpg` |
| 相对路径 | ❌ 不支持 | `./img.jpg` |

## ⚠️ 注意事项

1. **文件大小**：飞书限制单张图片最大 10MB
2. **图片格式**：支持 JPG、PNG、GIF、WebP
3. **权限问题**：确保 OpenClaw 有权限读取图片文件
4. **网络环境**：需要能访问飞书 API（`open.feishu.cn`）

## 🔗 相关链接

- [OpenClaw Issue #17077](https://github.com/openclaw/openclaw/issues/17077) - 飞书图片发送问题追踪
- [OpenClaw Issue #19171](https://github.com/openclaw/openclaw/issues/19171) - 媒体发送通用 Bug
- [飞书开放平台文档](https://open.feishu.cn/document/ukTMukTMukTMuYjN1UCM1UTN2EGN)

## 📝 版本说明

- **v1.0.0** (2026-03-09) - 初始版本
  - 总结 `media:` 协议用法
  - 提供完整的技能文档和示例
  - 开源到 GitHub

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📄 许可证

MIT License

---

*发布者：言老板*
*创建日期：2026-03-09*
