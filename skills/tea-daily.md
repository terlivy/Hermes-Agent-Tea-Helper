# 小拾山茶事 每日定时调度 Skill

**用途**：每天定时执行选题→提示词→文案全套生成流程

## 激活条件

定时触发，或用户说"跑一下今天的流程"、"执行日更"。

## Prompt

```
执行小拾山茶事每日内容生成流程。

【第一步：选题】
调用 tea-topic skill，生成当日选题。

【第二步：生成视频提示词】
调用 tea-prompt skill，根据选题生成 HappyHorse 视频提示词。

【第三步：生成多平台文案】
调用 tea-copy skill，生成抖音、小红书、视频号三版本文案。

【第四步：保存到本地】
在 ~/tea-content/YYYY-MM-DD/ 目录下创建以下文件：
- topic.txt          （选题信息）
- prompt.txt        （视频提示词）
- copy-douyin.txt   （抖音文案）
- copy-xiaohongshu.txt （小红书文案）
- copy-video.txt    （视频号文案）

【门店信息】
- 店名：小拾山茶事
- 定位：小区社区茶店，温馨烟火风
- 视频规格：竖屏 9:16，时长 15-30秒

【完成后输出】
今日选题：[选题名称]
提示词已生成：~/tea-content/日期/prompt.txt
文案已生成：~/tea-content/日期/copy-*.txt
请复制 prompt.txt 内容到 Happy Horse 生成视频
```

## 使用方式

1. 每天早上 9:00 自动触发（通过 Hermes cron 配置）
2. 或用户手动说"跑日更流程"触发
3. 生成完毕后提醒用户去 Happy Horse 操作
