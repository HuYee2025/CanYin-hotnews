---
name: china-hotnews
description: 抓取国内热点新闻（微博/知乎/抖音/B站等平台热搜），支持定时推送。触发：用户要求"抓热点"、"看热搜"、"今日新闻"、"早上新闻推送"。
version: 1.0.0
author: HuYee
license: MIT
tags: [news, hotnews, china, weibo, zhihu, douyin]
---

# china-hotnews: 国内热点新闻抓取

一键抓取微博、知乎、抖音、B站等国内主流平台热搜榜单，支持定时推送。

**适用于：OpenClaw / Claude Code / Codex / 其他AI助手**

## 支持平台

| 平台 | 端点 | 数据 |
|------|------|------|
| 微博热搜 | weibo | 标题+热度值+链接 |
| 知乎热榜 | zhihu | 标题+详情+热度+回答数 |
| 抖音热点 | douyin | 标题+热度值+封面 |
| B站热门 | bilibili | 标题+播放量 |
| 今日头条 | toutiao | 标题+热度 |
| 百度热搜 | baidu | 标题+热度值 |
| 每天60秒 | 60s | 15条精选新闻 |

## API配置

**公共API（默认，免费无需Key）：**
```
https://60s.viki.moe/v2/{platform}
```

所有人安装后直接可用，无需额外配置。

**自建API（可选，长期稳定）：**

如果你想更稳定、不受公共API限制，可以自己部署：

```bash
# Docker部署
docker run -d --restart always --name 60s -p 4399:4399 vikiboss/60s:latest

# 或Node.js部署
git clone https://github.com/vikiboss/60s.git
cd 60s && pnpm install && PORT=4399 pnpm start
```

自建后修改配置文件：`~/.config/china-hotnews/config.json`
```json
{
  "api_base": "http://localhost:4399/v2",
  "default_platforms": ["weibo", "zhihu", "douyin"],
  "top_n": 5
}
```

**注意：** 自建API只有你自己能用，其他人默认使用公共API。

---

## 使用方式

### 1. 查看热搜（手动触发）

用户说"抓热点"、"看热搜"、"今日新闻"时执行：

```bash
# 默认使用公共API（所有人可用）
curl -s "https://60s.viki.moe/v2/weibo" | jq -r '.data[:5] | .[] | "• \(.title) (\(.hot_value)热度)"'

curl -s "https://60s.viki.moe/v2/zhihu" | jq -r '.data[:5] | .[] | "• \(.title) (\(.hot_value_desc))"'

curl -s "https://60s.viki.moe/v2/douyin" | jq -r '.data[:5] | .[] | "• \(.title) (\(.hot_value)热度)"'
```

**如果配置了自建API，优先使用本地服务：**
```bash
API_BASE=$(cat ~/.config/china-hotnews/config.json 2>/dev/null | jq -r '.api_base' || echo "https://60s.viki.moe/v2")
curl -s "$API_BASE/weibo" | jq -r '.data[:5] | .[] | "• \(.title) (\(.hot_value)热度)"'
```

**输出格式：**
```
🔥 微博热搜（前5）
• 歌手首发阵容 (350万热度)
• 多家酒店做不到床单一客一换 (145万热度)
...

💡 知乎热榜（前5）
• 王毅介绍中美元首会晤 (4621万热度)
...

📺 抖音热点（前5）
• 金价银价大跌 (1145万热度)
...
```

### 2. 单平台查询

用户指定平台时（如"微博热搜"、"知乎热榜"）：

```bash
curl -s "https://60s.viki.moe/v2/weibo" | jq '.data[:10]'
```

### 3. 每天60秒看世界

用户说"60秒新闻"、"每日新闻"时：

```bash
curl -s "https://60s.viki.moe/v2/60s" | jq '.data'
```

**输出：**
```
📰 每天60秒看世界（2026-05-16）

1. 2025年平均工资出炉：城镇非私营单位年平均工资129441元...
2. 三部门联合发文：居民换购住房个人所得税退税政策延续...
...
15. 外媒：俄罗斯对基辅发动大规模空袭...

💡 每日微语：生活从不会主动优待谁，主动奔赴前路拼搏，方能握住人生主动权
```

### 4. 定时推送（Cron）

**OpenClaw用户：**
```bash
openclaw cron add \
  --name "morning-hotnews" \
  --cron "0 9 * * *" \
  --system-event '{"kind":"hotnews_push","platforms":["weibo","zhihu","douyin"],"top_n":5}' \
  --session main \
  --tz "Asia/Shanghai"
```

**其他系统：**
```bash
# 添加到crontab
0 9 * * * curl -s https://60s.viki.moe/v2/weibo | jq '.data[:5]'
```

---

## 数据实时性

**验证结果：**
- 抖音数据更新延迟约20秒
- 微博/知乎数据实时同步
- **适合定时推送场景**

---

## 自建部署（长期方案）

**为什么自建：**
- 公共API可能在2026年7月20日后受影响（Deno Deploy Classic终止）
- 自建后永久可用，不受第三方影响

**部署步骤：**

1. Fork项目：https://github.com/vikiboss/60s

2. Docker部署（推荐）：
```bash
docker run -d --restart always --name 60s -p 4399:4399 vikiboss/60s:latest
```

3. Node.js部署：
```bash
git clone https://github.com/vikiboss/60s.git
cd 60s && pnpm install
PORT=4399 pnpm start
```

4. 更新配置：
```json
{
  "api_base": "http://localhost:4399/v2",
  "default_platforms": ["weibo", "zhihu", "douyin"],
  "top_n": 5
}
```

---

## 兼容性

| 平台 | 支持 | 说明 |
|------|------|------|
| OpenClaw | ✅ | 完全支持，推荐使用 |
| Claude Code | ✅ | 可用，需手动调用curl |
| Codex | ✅ | 可用，需手动调用curl |
| Cursor | ✅ | 可用，需手动调用curl |
| 其他AI助手 | ✅ | 只需能执行shell命令 |

---

## 安装方式

**OpenClaw：**
```bash
openclaw skills install china-hotnews
```

**手动安装：**
```bash
mkdir -p ~/.openclaw/workspace/skills/china-hotnews
# 下载SKILL.md到该目录
```

---

## 注意事项

- 公共API免费但可能不稳定，建议自建
- 数据实时性约20秒延迟，足够定时推送
- 微博/知乎/抖音数据最完整，建议优先使用
- 无需API Key，完全免费

---

## 来源

- API项目：https://github.com/vikiboss/60s
- 技能作者：HuYee
- 技能地址：https://github.com/HuYee2025/china-hotnews