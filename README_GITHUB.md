# 🚀 上传到 GitHub 的步骤

## 方法 1：使用 GitHub CLI（推荐）

```bash
# 1. 创建新仓库（在 GitHub 上操作）
# 访问 https://github.com/new
# 仓库名：feishu-image-skill
# 描述：OpenClaw 飞书图片发送技能 - 解决飞书只能发送路径文本的问题

# 2. 推送代码
cd ~/Desktop/feishu-image-skill
git remote add origin https://github.com/watsonagenda/feishu-image-skill.git
git branch -M main
git push -u origin main

# 3. 完成！
# 访问 https://github.com/watsonagenda/feishu-image-skill
```

## 方法 2：使用 Git 命令

```bash
# 1. 创建 GitHub 仓库
# 访问 https://github.com/new
# 创建名为 "feishu-image-skill" 的仓库

# 2. 推送代码
cd ~/Desktop/feishu-image-skill
git remote add origin https://github.com/watsonagenda/feishu-image-skill.git
git branch -M main
git push -u origin main
```

## 方法 3：使用 GitHub Desktop

1. 打开 GitHub Desktop
2. 点击 "Add" → "Add Existing Repository"
3. 选择 `~/Desktop/feishu-image-skill` 文件夹
4. 点击 "Publish repository" 发布到 GitHub

## 发布后的操作

### 1. 设置开源许可证
- 已经包含 MIT License（`LICENSE` 文件）

### 2. 添加话题标签（Topics）
在 GitHub 仓库页面添加：
- `openclaw`
- `feishu`
- `image-upload`
- `skill`
- `plugin`

### 3. 创建 GitHub Release
1. 访问 https://github.com/watsonagenda/feishu-image-skill/releases
2. 点击 "Create a new release"
3. Tag version: `v1.0.0`
4. Release title: `v1.0.0 - Initial Release`
5. 复制 `RELEASE.md` 的内容到描述
6. 点击 "Publish"

### 4. 添加到 OpenClaw 技能市场（可选）
```bash
# 提交到 ClawHub
# 访问 https://clawhub.com 或相关文档
```

## 📝 仓库描述建议

```
🖼️ OpenClaw 飞书图片发送技能

解决 OpenClaw 通过飞书发送图片时只能发送路径文本的问题。
使用简单的 media: 协议前缀，即可发送实际图片。

✨ 特性：
- 一行代码解决图片发送问题
- 支持所有图片格式（JPG/PNG/GIF/WebP）
- 完整的文档和示例
- MIT 开源协议

📦 安装：
cd ~/.openclaw/workspace/skills/
git clone https://github.com/watsonagenda/feishu-image-skill.git

🔗 相关：
- OpenClaw: https://github.com/openclaw/openclaw
- 问题追踪：https://github.com/openclaw/openclaw/issues/17077
```

## 🎯 下一步

1. ✅ 推送到 GitHub
2. ✅ 创建 Release
3. ⏯️ 在 OpenClaw 社区分享
4. ⏯️ 更新 ClawHub 技能市场

---

*创建日期：2026-03-09*
