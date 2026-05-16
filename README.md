# china-hotnews

今日餐饮热点新闻抓取技能 - 支持微博/知乎/抖音/B站等平台餐饮新闻热搜，以及红餐等行业平台最新文章。
激活技能关键语：餐饮新闻、餐饮热点

## 快速开始

```bash
# 微博热搜（可点击查看详情）
curl -s "https://60s.viki.moe/v2/weibo" | jq -r '.data[:5] | .[] | "• [\(.title)](\(.link)) (\(.hot_value)热度)"'

# 知乎热榜（可点击查看详情）
curl -s "https://60s.viki.moe/v2/zhihu" | jq -r '.data[:5] | .[] | "• [\(.title)](\(.link)) (\(.hot_value_desc))"'

# 抖音热点（可点击查看详情）
curl -s "https://60s.viki.moe/v2/douyin" | jq -r '.data[:5] | .[] | "• [\(.title)](\(.link)) (\(.hot_value)热度)"'
```

**点击标题即可查看详细内容！**

## 安装

**OpenClaw:**
```bash
openclaw skills install china-hotnews
```

**手动安装:**
```bash
mkdir -p ~/.openclaw/workspace/skills/china-hotnews
# 下载SKILL.md到该目录
```

## 兼容性

✅ OpenClaw  
✅ Claude Code  
✅ Codex  
✅ Cursor  
✅ 任何能执行shell的AI助手

## 来源

- API项目: https://github.com/vikiboss/60s
- 作者: HuYee
