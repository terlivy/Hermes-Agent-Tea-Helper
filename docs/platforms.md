# 小拾山茶事 平台配置指南

## 抖音

### 支持功能
- 视频发布 ✅
- 商品链接 ✅

### CLI 命令
```bash
cd ~/social-auto-upload
source ~/.venv-sau/bin/activate

# 登录（--headed 弹出浏览器窗口扫码）
python sau_cli.py douyin login --account tea_shop --headed

# 验证登录状态
python sau_cli.py douyin check --account tea_shop

# 发布视频（--headed 手动确认发布）
python sau_cli.py douyin upload-video \
  --account tea_shop \
  --file ~/Videos/demo.mp4 \
  --title "示例标题" \
  --desc "示例简介" \
  --tags "小区茶店,下午茶,喝茶日常" \
  --headed
```

### 注意事项
- 视频大小限制：≤4GB
- 封面建议：1080×1920（竖屏）
- 标题限制：30字以内
- `--headed` 模式打开浏览器手动点发布，不加则全自动发布

---

## 小红书

### 支持功能
- 视频发布 ✅

### CLI 命令
```bash
cd ~/social-auto-upload
source ~/.venv-sau/bin/activate

# 登录
python sau_cli.py xiaohongshu login --account tea_shop --headed

# 验证登录状态
python sau_cli.py xiaohongshu check --account tea_shop

# 发布视频
python sau_cli.py xiaohongshu upload-video \
  --account tea_shop \
  --file ~/Videos/demo.mp4 \
  --title "示例标题" \
  --desc "示例简介" \
  --headed
```

### 注意事项
- 正文限制：1000字以内
- 标签限制：10个以内

---

## 视频号

### 支持功能
- 视频发布 ❌（CLI 开发中）
- 定时发布 ❌

### 当前方案
手动发布到视频号助手后台

---

## 平台对比

| 平台 | 自动发布 | CLI | Web UI | 备注 |
|------|---------|-----|--------|------|
| 抖音 | ✅ | ✅ | ✅ | 推荐 --headed 模式 |
| 小红书 | ✅ | ✅ | ✅ | |
| 视频号 | ❌ | ❌ | ❌ | 手动发布 |
