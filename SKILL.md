---
name: china-hotnews
description: 抓取国内热点新闻（微博/知乎/抖音/B站等平台热搜），支持定时推送。触发：用户要求"抓热点"、"看热搜"、"今日新闻"、"早上新闻推送"。
---

# china-hotnews: 国内热点新闻抓取

一键抓取微博、知乎、抖音、B站等国内主流平台热搜榜单，支持定时推送。

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

**公共API（免费）：**
```
https://60s.viki.moe/v2/{platform}
```

**自建API（推荐长期使用）：**
```bash
docker run -d --restart always --name 60s -p 4399:4399 vikiboss/60s:latest
```
自建后API地址：`http://localhost:4399/v2/{platform}`

**配置文件：** `~/.config/china-hotnews/config.json`
```json
{
  "api_base": "https://60s.viki.moe/v2",
  "default_platforms": ["weibo", "zhihu", "douyin"],
  "top_n": 5
}
```

## 使用方式

### 1. 查看热搜（手动触发）

用户说"抓热点"、"看热搜"、"今日新闻"时：

**执行：**
```bash
# 获取配置
API_BASE=$(cat ~/.config/china-hotnews/config.json 2>/dev/null | jq -r '.api_base' || echo "https://60s.viki.moe/v2")
PLATFORMS=$(cat ~/.config/china-hotnews/config.json 2>/dev/null | jq -r '.default_platforms[]' || echo "weibo zhihu douyin")
TOP_N=$(cat ~/.config/china-hotnews/config.json 2>/dev/null | jq -r '.top_n' || echo "5")

# 抓取数据
for platform in $PLATFORMS; do
  curl -s "$API_BASE/$platform" | jq ".data[:$TOP_N]"
done
```

**输出格式：**
```
🔥 微博热搜（前5）
1. [标题] - [热度值]
2. [标题] - [热度值]
...

💡 知乎热榜（前5）
1. [标题] - [热度描述]
   [详情摘要]
...

📺 抖音热点（前5）
1. [标题] - [热度值]
...
```

### 2. 定时推送（Cron）

**用户要求定时推送时（如"每天早上9点推送热点"）：**

使用OpenClaw cron功能：
```bash
openclaw cron add \
  --name "daily-hotnews" \
  --cron "0 9 * * *" \
  --system-event \
  --payload '{"kind":"hotnews","platforms":["weibo","zhihu","douyin"],"top_n":5}'
```

**Cron触发后，执行推送：**
```bash
# 抓取数据
API_BASE="https://60s.viki.moe/v2"
HOTNEWS_DATA=""

# 微博
WEIBO=$(curl -s "$API_BASE/weibo" | jq -r '.data[:5] | .[] | "• \(.title) (\(.hot_value)热度)"')
HOTNEWS_DATA+="🔥 微博热搜\n$WEIBO\n\n"

# 知乎
ZHIHU=$(curl -s "$API_BASE/zhihu" | jq -r '.data[:5] | .[] | "• \(.title) (\(.hot_value_desc))"')
HOTNEWS_DATA+="💡 知乎热榜\n$ZHIHU\n\n"

# 抖音
DOUYIN=$(curl -s "$API_BASE/douyin" | jq -r '.data[:5] | .[] | "• \(.title) (\(.hot_value)热度)"')
HOTNEWS_DATA+="📺 抖音热点\n$DOUYIN"

# 推送到飞书
message action=send channel=feishu message="$HOTNEWS_DATA"
```

### 3. 单平台查询

用户指定平台时（如"微博热搜"、"知乎热榜"）：

```bash
curl -s "https://60s.viki.moe/v2/weibo" | jq '.data[:10]'
```

### 4. 每天60秒看世界

用户说"60秒新闻"、"每日新闻"时：

```bash
curl -s "https://60s.viki.moe/v2/60s" | jq '.data.news'
```

**输出：**
```
📰 每天60秒看世界（2026-05-16）

1. [新闻内容]
2. [新闻内容]
...
15. [新闻内容]

💡 每日微语：[微语内容]
```

## 数据实时性

**验证结果：**
- 抖音数据更新延迟约20秒
- 微博/知乎数据实时同步
- **适合定时推送场景**

## 自建部署（长期方案）

**为什么自建：**
- 公共API可能在2026年7月20日后受影响（Deno Deploy Classic终止）
- 自建后永久可用，不受第三方影响

**部署步骤：**

1. Fork项目到你的Github：
   https://github.com/vikiboss/60s

2. Docker部署（推荐）：
```bash
docker run -d \
  --restart always \
  --name 60s \
  -p 4399:4399 \
  vikiboss/60s:latest
```

3. 更新配置：
```bash
mkdir -p ~/.config/china-hotnews
cat > ~/.config/china-hotnews/config.json << 'EOF'
{
  "api_base": "http://localhost:4399/v2",
  "default_platforms": ["weibo", "zhihu", "douyin"],
  "top_n": 5
}
EOF
```

## 技能安装

```bash
openclaw skills install china-hotnews
```

## Cron配置（每天早上9点）

```bash
openclaw cron add \
  --name "morning-hotnews" \
  --cron "0 9 * * *" \
  --system-event \
  --sessionTarget main \
  --payload '{"kind":"hotnews_push","platforms":["weibo","zhihu","douyin"],"top_n":5}'
```

触发后，主会话会收到systemEvent，执行热点推送。

## 注意事项

- 公共API免费但可能不稳定，建议自建
- 数据实时性约20秒延迟，足够定时推送
- 微博/知乎/抖音数据最完整，建议优先使用