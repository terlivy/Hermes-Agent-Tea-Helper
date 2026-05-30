# 小拾山茶事 每日工作流程

## 自动化流程（无需人工干预）

### 早上 9:00
```
Hermes cron 自动触发
    ↓
调用 tea-daily skill
    ↓
生成选题 + 提示词 + 三平台文案
    ↓
保存到 ~/tea-content/YYYY-MM-DD/
    ↓
发送通知（飞书/微信）
```

## 人工操作流程（每天 10-15 分钟）

### Step 1：查看今日选题
```
登录 Hermes → 查看 ~/tea-content/今日日期/
或收到飞书/微信推送
```

### Step 2：生成视频
```
打开 Happy Horse
    ↓
复制 prompt.txt 中的提示词
    ↓
粘贴到 Happy Horse
    ↓
选择竖屏 9:16，时长 15-30秒
    ↓
生成视频（等待 3-5 分钟）
    ↓
下载视频到 ~/Videos/
```

### Step 3：发布（自动或手动）

**抖音 + 小红书**（自动）：
```
Hermes cron 下午6点触发
    ↓
读取 ~/Videos/ 最新视频
    ↓
调用 sau publish 发布
    ↓
自动发布成功
```

**视频号**（手动）：
```
打开 videoj.com / 视频号助手
    ↓
上传视频
    ↓
复制 ~/tea-content/日期/copy-video.txt 文案
    ↓
粘贴发布
```

## 目录结构

```
~/tea-content/
└── 2026-05-30/
    ├── topic.txt           # 选题信息
    ├── prompt.txt          # HappyHorse 提示词
    ├── copy-douyin.txt     # 抖音文案
    ├── copy-xiaohongshu.txt # 小红书文案
    └── copy-video.txt      # 视频号文案

~/Videos/
├── 小拾山茶事_I2V.mp4
├── 小拾山茶事_实景接续.mp4
└── ...
```

## 内容质量检查

发布前确认：
- [ ] 视频画面清晰、稳定
- [ ] 封面有吸引力
- [ ] 标题符合平台风格
- [ ] 标签数量合适（抖音8-10个，小红书5-8个）
- [ ] 没有敏感词

## 复盘（每周一次）

每周查看：
- 各平台播放量、点赞、评论
- 到店顾客增长
- 哪种内容类型表现最好

根据数据调整下一周选题方向。
