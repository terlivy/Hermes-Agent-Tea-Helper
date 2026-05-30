# 小拾山茶事 平台配置指南

## 抖音

### 账号类型
创作者账号（个人或企业）

### 支持功能
- 视频发布 ✅
- 图文发布 ✅
- 定时发布 ✅
- 商品链接 ✅

### CLI 命令
```bash
# 登录
sau douyin login --account tea_shop

# 检查登录状态
sau douyin check --account tea_shop

# 发布视频
sau douyin upload-video \
  --account tea_shop \
  --file videos/demo.mp4 \
  --title "示例标题" \
  --desc "示例简介" \
  --tags "小区茶店,下午茶,喝茶日常"

# 发布图文
sau douyin upload-note \
  --account tea_shop \
  --images videos/1.png videos/2.png \
  --title "图文标题" \
  --note "图文正文"
```

### 注意事项
- 视频大小限制：≤4GB
- 封面建议：1080×1920（竖屏）
- 标题限制：30字以内

---

## 小红书

### 账号类型
专业号（个人或企业）

### 支持功能
- 视频发布 ✅
- 图文发布 ✅
- 定时发布 ✅

### CLI 命令
```bash
# 登录
sau xiaohongshu login --account tea_shop

# 检查登录状态
sau xiaohongshu check --account tea_shop

# 发布视频
sau xiaohongshu upload-video \
  --account tea_shop \
  --file videos/demo.mp4 \
  --title "示例标题" \
  --desc "示例简介"

# 发布图文
sau xiaohongshu upload-note \
  --account tea_shop \
  --images videos/1.png videos/2.png videos/3.png \
  --title "图文标题" \
  --note "图文正文"
```

### 注意事项
- 图片数量：1-9张
- 正文限制：1000字以内
- 标签限制：10个以内

---

## 视频号

### 账号类型
视频号（个人或机构）

### 支持功能
- 视频发布 ⬜（支持弱，CLI 在开发中）
- 图文发布 ❌
- 定时发布 ❌

### 当前方案
手动发布：
1. 打开微信视频号助手
2. 上传视频
3. 复制文案粘贴

### 待跟进
关注 [social-auto-upload](https://github.com/dreammis/social-auto-upload) 更新

---

## 微信公众号

### 支持功能
- 图文发布 ❌（暂无支持）

### 当前方案
手动发布到公众号后台

---

## B站（测试用）

### CLI 命令
```bash
# 登录
sau bilibili login --account tea_shop

# 发布视频
sau bilibili upload-video \
  --account tea_shop \
  --file videos/demo.mp4 \
  --title "示例标题" \
  --desc "示例简介" \
  --tid 249  # 生活类
```

---

## 平台对比

| 平台 | 自动发布 | CLI | 定时发布 | 图文支持 |
|------|---------|-----|---------|---------|
| 抖音 | ✅ | ✅ | ✅ | ✅ |
| 小红书 | ✅ | ✅ | ✅ | ✅ |
| 视频号 | ⬜ | ⬜ | ❌ | ❌ |
| 微信公众号 | ❌ | ❌ | ❌ | ❌ |
| B站 | ✅ | ✅ | ✅ | ❌ |
