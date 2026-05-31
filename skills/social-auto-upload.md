# social-auto-upload 全自动多平台发布

**用途**：自动发布视频到抖音/小红书，支持 CLI 和 Web UI 两种方式

## 前提条件

- Python 3.14 + 虚拟环境 `~/.venv-sau`
- 依赖已安装：`pip install -r requirements.txt`
- Chromium 已安装：`playwright install chromium`
- 代理（国内必需）：ClashX Pro（端口 7890）

## 项目结构

```
~/social-auto-upload/
├── cookies/              # CLI cookie（扫码后自动写入）
├── cookiesFile/         # Web UI cookie（需手动同步）
├── db/database.db       # SQLite：账号表、文件表
├── videoFile/           # 上传视频目录
├── sau_backend.py       # Flask 后端（端口 5409）
├── sau_frontend/        # Vue3 前端（端口 5173）
├── myUtils/postVideo.py # 发布核心逻辑
└── uploader/
    ├── douyin_uploader/main.py
    ├── xiaohongshu_uploader/main.py
    ├── tencent_uploader/main.py
    └── ks_uploader/main.py
```

---

## 启动

```bash
cd ~/social-auto-upload
source ~/.venv-sau/bin/activate

# 终端1：后端
python sau_backend.py &

# 终端2：前端（开发用）
cd sau_frontend && npm run dev &
```

访问 `http://localhost:5173`

---

## 账号添加（CLI → Web UI 同步）

CLI 和 Web UI 使用**两套独立的 cookie 系统**，需要手动同步：

```python
import sqlite3, shutil, os
from pathlib import Path

BASE = Path.home() / "social-auto-upload"
SRC  = BASE / "cookies"
DEST = BASE / "cookiesFile"
DB   = BASE / "db" / "database.db"

PLATFORM_MAP = {"douyin": 3, "xiaohongshu": 1, "ks": 4, "tencent": 2}
platform_key = "douyin"   # ← 改这里
account_name = "tea_shop"

cookie_files = [f for f in os.listdir(SRC)
                if f.startswith(platform_key) and f.endswith(".json")]
for cf in cookie_files:
    shutil.copy2(SRC / cf, DEST / cf)

conn = sqlite3.connect(DB)
cur = conn.cursor()
cur.execute("""
    INSERT OR IGNORE INTO user_info (type, filePath, userName, status)
    VALUES (?, ?, ?, 1)
""", (PLATFORM_MAP[platform_key], cookie_files[0], account_name))
conn.commit()
conn.close()
print("✅ 同步完成")
```

**类型码**：`1=小红书`，`2=视频号`，`3=抖音`，`4=快手`

---

## CLI 命令

```bash
cd ~/social-auto-upload
source ~/.venv-sau/bin/activate

# 登录（--headed 弹出浏览器窗口）
python sau_cli.py douyin login --account tea_shop --headed
python sau_cli.py xiaohongshu login --account tea_shop --headed

# 验证 cookie 有效
python sau_cli.py douyin check --account tea_shop
python sau_cli.py xiaohongshu check --account tea_shop

# 发布视频（--headed 手动确认发布）
python sau_cli.py douyin upload-video \
  --account tea_shop \
  --file ~/Videos/demo.mp4 \
  --title "标题" \
  --desc "简介" \
  --tags "茶叶,茶文化" \
  --headed

python sau_cli.py xiaohongshu upload-video \
  --account tea_shop \
  --file ~/Videos/demo.mp4 \
  --title "标题" \
  --desc "简介" \
  --headed
```

---

## Web UI 发布流程

1. 左侧 → **发布中心**
2. 上传视频（本地或素材库）
3. 选择**平台**（抖音/小红书）
4. 点**选择账号** → 弹窗选对应平台账号
5. 填**标题**（必填）
6. 可选：话题、原创声明、商品链接（抖音）
7. 点**发布**

---

## 数据库结构

**user_info（账号表）**
| 列 | 说明 |
|----|------|
| id | 主键 |
| type | 平台：1=小红书, 2=视频号, 3=抖音, 4=快手 |
| filePath | cookie 文件名 |
| userName | 显示名 |
| status | 0=异常, 1=正常, -1=验证中 |

**file_records（文件表）**
| 列 | 说明 |
|----|------|
| id | 主键 |
| filename | 原始文件名 |
| filesize | MB |
| file_path | 保存路径（含 UUID）|

---

## 已修复 Bug

**抖音发布死循环**：原代码 `publish_clicked = True` 在 `try` 块内，`wait_for_url` 超时抛异常时该行被跳过，导致每次循环重重点发布按钮。

**修复**：`publish_clicked = True` 移到 `wait_for_url` 调用**之前**。

---

## 常见问题

**「选择账号」弹窗为空**
- 后端是否运行：`curl -s http://localhost:5409/getAccounts`
- 数据库是否有数据：`sqlite3 ~/social-auto-upload/db/database.db "SELECT * FROM user_info"`
- 账号 type 和平台是否匹配

**上传失败**：确认 `videoFile/` 目录存在
**后端 404**：确认 Flask 在正确目录启动
