# 小拾山茶事 AI 运营助手

基于 Hermes Agent + Happy Horse + social-auto-upload 的茶叶店短视频全自动运营方案。

## 方案概述

```
┌──────────────────────────────────────────────────────────────┐
│  每天早上 9:00（定时触发）                                    │
│    Hermes 自动生成当日内容                                     │
│    ├── 选题策划                                              │
│    ├── HappyHorse 视频提示词                                   │
│    ├── 标题 + 正文 + 标签                                      │
│    └── 多平台适配（抖音/小红书/视频号）                          │
│              ↓                                                 │
│    保存到 ~/tea-content/YYYY-MM-DD/                          │
│              ↓                                                 │
│  你打开 Happy Horse，粘贴提示词，生成视频                        │
│              ↓                                                 │
│  视频保存到 ~/Videos/                                        │
│              ↓                                                 │
│  Hermes 调用 social-auto-upload                              │
│    ├── 抖音 → 自动发布 ✅                                      │
│    ├── 小红书 → 自动发布 ✅                                    │
│    └── 视频号 → 手动发布（项目支持弱）                          │
└──────────────────────────────────────────────────────────────┘
```

## 工具栈

| 工具 | 用途 | 状态 |
|------|------|------|
| [Hermes Agent](https://github.com/NousResearch/hermes-agent) | AI 智能体，内容生成核心 | ✅ 已配置 |
| [MiniMax CLI](https://github.com/MiniMax-AI/cli) | 额度高，速度快 | ✅ 已登录 |
| [Happy Horse](https://www.happyhorse.video) | AI 视频生成 | ✅ 已测试 |
| [social-auto-upload](https://github.com/dreammis/social-auto-upload) | 多平台自动发布 | ⬜ 待安装 |

## 快速开始

### 1. 安装依赖

```bash
# 克隆 social-auto-upload
cd ~
git clone https://github.com/dreammis/social-auto-upload.git
cd social-auto-upload

# 创建虚拟环境
python3 -m venv ~/.venv-sau
source ~/.venv-sau/bin/activate

# 安装依赖
pip install -r requirements.txt
playwright install chromium
```

### 2. 登录平台账号

```bash
source ~/.venv-sau/bin/activate

# 登录小红书
sau xiaohongshu login --account tea_shop

# 登录抖音
sau douyin login --account tea_shop
```

### 3. 配置 Hermes Skills

参考 `skills/` 目录下的配置文件，逐一导入到 Hermes Agent。

### 4. 配置定时任务

```bash
# 每天早上9点生成内容
hermes cron add \
  --name "tea-daily-content" \
  --schedule "0 9 * * *" \
  --prompt "调用 tea-topic + tea-prompt + tea-copy，生成当日内容，保存到 ~/tea-content/"

# 每周三、六 18:00 发布
hermes cron add \
  --name "tea-publish" \
  --schedule "0 18 * * 3,6" \
  --prompt "读取 ~/tea-content 最新视频，调用 social-auto-upload 发布到抖音和小红书"
```

## 工作流程

### 每日耗时：10-15 分钟

| 时间 | 操作 | 耗时 |
|------|------|------|
| 9:00 | Hermes 自动生成提示词+文案 | 0分钟 |
| 白天 | Happy Horse 生成视频，下载到 ~/Videos | 10分钟 |
| 18:00 | Hermes 自动发布到抖音+小红书 | 0分钟 |
| 视频号 | 手动发布 | 3分钟 |

## 门店信息

- **店名**：小拾山茶事
- **定位**：小区社区茶店，温馨烟火风
- **主营**：红茶、绿茶、白茶，到店下午茶
- **文创**：手绘作品、冰箱贴、手工扎染
- **视频规格**：竖屏 9:16，时长 15-30秒

## 平台支持

| 平台 | 自动发布 | 备注 |
|------|---------|------|
| 抖音 | ✅ 完整 | social-auto-upload CLI |
| 小红书 | ✅ 完整 | social-auto-upload CLI |
| 视频号 | ⬜ 弱 | 暂无 CLI，需手动 |
| 微信公众号 | ❌ 无 | 暂无支持 |

## 目录结构

```
Hermes-Agent-Tea-Helper/
├── README.md              # 本文件
├── skills/                # Hermes Skills 配置
│   ├── tea-topic.md       # 选题策划
│   ├── tea-prompt.md      # HappyHorse 提示词
│   ├── tea-copy.md        # 多平台文案
│   ├── tea-reply.md       # 评论回复
│   └── tea-daily.md       # 定时调度
├── docs/                  # 详细文档
│   ├── install.md         # 安装指南
│   ├── workflow.md        # 工作流说明
│   └── platforms.md      # 平台配置
└── examples/             # 示例内容
    └── sample-videos/    # 视频提示词示例
```

## 安全说明

- social-auto-upload 使用平台官方创作者后台扫码登录
- Cookie 保存在本地 `db/` 目录，不上传任何服务器
- 全程本地操作，账号安全可控

## 参考资料

- [Hermes Agent 官方文档](https://hermes-agent.nousresearch.com)
- [social-auto-upload GitHub](https://github.com/dreammis/social-auto-upload)
- [MiniMax CLI](https://github.com/MiniMax-AI/cli)
- [Happy Horse](https://www.happyhorse.video)
