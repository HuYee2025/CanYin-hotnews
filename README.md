# china-hotnews

国内热点新闻抓取技能 - 支持微博/知乎/抖音/B站等平台热搜

## 快速开始

```bash
# 微博热搜
curl -s "https://60s.viki.moe/v2/weibo" | jq '.data[:5]'

# 知乎热榜
curl -s "https://60s.viki.moe/v2/zhihu" | jq '.data[:5]'

# 抖音热点
curl -s "https://60s.viki.moe/v2/douyin" | jq '.data[:5]'
```

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
