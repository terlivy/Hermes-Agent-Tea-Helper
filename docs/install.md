# 小拾山茶事 AI 运营助手 安装指南

## 前置要求

- macOS 或 Linux 系统
- 已安装 Python 3.10+
- 已安装 Git
- 账号：抖音创作者账号、小红书账号

## 第一步：克隆项目

```bash
cd ~
git clone https://github.com/terlivy/Hermes-Agent-Tea-Helper.git
cd Hermes-Agent-Tea-Helper
```

## 第二步：安装 social-auto-upload

```bash
# 创建独立虚拟环境（避免依赖冲突）
python3 -m venv ~/.venv-sau
source ~/.venv-sau/bin/activate

# 安装 social-auto-upload
cd ~/social-auto-upload  # 如果你单独克隆的话
pip install -r requirements.txt

# 安装 Playwright 浏览器
playwright install chromium
```

## 第三步：登录平台账号

```bash
source ~/.venv-sau/bin/activate

# 登录小红书（会弹浏览器窗口扫码）
sau xiaohongshu login --account tea_shop

# 登录抖音（会弹浏览器窗口扫码）
sau douyin login --account tea_shop

# 登录 B站（测试用）
sau bilibili login --account tea_shop
```

> 扫码只需一次，Cookie 保存在本地 `db/` 目录。

## 第四步：导入 Hermes Skills

把 `skills/` 目录下的文件内容复制到 Hermes Agent 创建对应 Skill。

或者直接告诉 Hermes：
```
帮我创建5个 skill：tea-topic（选题）、tea-prompt（提示词）、
tea-copy（文案）、tea-reply（回复）、tea-daily（定时调度）
```

## 第五步：配置定时任务

```bash
# 每天早上9点生成内容
hermes cron add \
  --name "tea-daily-content" \
  --schedule "0 9 * * *" \
  --prompt "执行 tea-daily 流程，生成当日内容"

# 每周三、六下午6点发布
hermes cron add \
  --name "tea-publish" \
  --schedule "0 18 * * 3,6" \
  --prompt "读取 ~/tea-content 最新视频，发布到抖音和小红书"
```

## 验证安装

```bash
# 检查 social-auto-upload 是否正常
source ~/.venv-sau/bin/activate
sau --version

# 检查 cron 任务
hermes cron list
```

## 常见问题

### Q: playwright install 失败
```bash
# 使用国内镜像
playwright install chromium --registry https://npmmirror.com/mirrors/playwright/
```

### Q: sau command not found
```bash
# 确保虚拟环境已激活
source ~/.venv-sau/bin/activate
# 或使用完整路径
~/.venv-sau/bin/sau
```

### Q: 扫码登录过期
```bash
# 重新登录
sau xiaohongshu login --account tea_shop
```
