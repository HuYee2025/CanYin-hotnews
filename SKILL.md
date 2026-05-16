---
name: china-hotnews
description: 餐饮热点洞察 - 从社会热点中挖掘餐饮营销切入点。触发：用户说"餐饮热点"。
version: 1.0.0
author: HuYee
license: MIT
tags: [news, hotnews, china, catering, restaurant]
---

# china-hotnews: 餐饮热点洞察

从社会热点中挖掘餐饮营销切入点，为餐饮咨询顾问提供决策支持。

**适用于：OpenClaw / Claude Code / Codex / 其他AI助手**

## 触发关键词

**用户说"餐饮热点"时触发**

就这么简单！

## 支持平台

| 平台 | 端点 | 数据 |
|------|------|------|
| 微博热搜 | `/weibo` | 标题+热度值+链接 |
| 知乎热榜 | `/zhihu` | 标题+详情+热度+回答数 |
| 抖音热点 | `/douyin` | 标题+热度值+封面 |
| B站热门 | `/bili` | 标题+播放量 |
| 小红书热榜 | `/rednote` | 标题+热度+链接 |
| 今日头条 | `/toutiao` | 标题+热度+封面 |
| 百度热搜 | `/baidu/hot` | 标题+热度+描述 |
| 贴吧热议 | `/baidu/tieba` | 标题+讨论数 |
| 懂车帝 | `/dongchedi` | 汽车资讯热榜 |
| 每天60秒 | `/60s` | 15条精选新闻 |

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

### 4. 餐饮热点洞察（餐饮咨询顾问专用）

用户说"餐饮热点洞察"、"餐饮视角"、"餐饮切入点"时：

**功能：从社会热点中挖掘餐饮相关的切入点**

**人设：**
你是一个专业的餐饮新闻情报助手，擅长从社会热点中挖掘餐饮营销切入点，为餐饮咨询顾问提供决策支持。

**任务：**
1. 调用搜索工具，抓取今日社会热点新闻
2. AI分析每个热点是否有餐饮关联
3. 筛选出5条最有营销价值的热点
4. 提取餐饮切入点并给出营销建议

**筛选标准：**
- 当天热度高、全网关注度高
- 有餐饮营销价值
- 排除纯娱乐八卦

**输出格式：**
```
🍽️ 今日（X月X日）餐饮热点洞察

✍️ 总结：（100字内，简短精炼，概括今日餐饮营销机会）

---------------------------

🧩 餐饮切入点列表（按营销价值排序）

1. [新闻标题](链接)
新闻摘要：（100字左右，概括新闻核心内容）
餐饮切入点：具体切入点说明
营销建议：具体可执行的营销建议

2. [新闻标题](链接)
新闻摘要：...
餐饮切入点：...
营销建议：...

（共5条）
```

**示例输出：**
```
🍽️ 今日（5月16日）餐饮热点洞察

✍️ 总结：今日热点中，特朗普访华国宴、黄仁勋北京餐饮选择成为餐饮营销切入点。酒店卫生问题持续发酵，餐饮食品安全话题升温。饭店创意营销获千万关注，建议餐饮品牌借势国宴话题、强化食品安全宣传。

---------------------------

🧩 餐饮切入点列表（按营销价值排序）

1. [特朗普访华晚宴菜单曝光](链接)
新闻摘要：特朗普访华期间，国宴菜单引发关注，菜品选择体现中美餐饮文化交流，高端餐饮品牌借势营销机会显现。
餐饮切入点：国宴菜品设计、高端餐饮品牌曝光、中美餐饮文化交流
营销建议：推出"国宴同款"菜品，借势营销

2. [黄仁勋北京吃炸酱面喝蜜雪冰城](链接)
新闻摘要：英伟达CEO黄仁勋在北京胡同品尝炸酱面、喝蜜雪冰城，名人餐饮选择引发关注，北京特色美食传播效应显著。
餐饮切入点：名人餐饮选择、北京特色美食传播、平价餐饮品牌借势
营销建议：北京餐饮可推出"黄仁勋同款"套餐

3. [饭店喷水鱼获1000万网友围观](链接)
新闻摘要：某饭店创意菜品"喷水鱼"获千万网友围观，餐饮营销创意引发热议，网红餐厅打造成为行业关注焦点。
餐饮切入点：餐饮营销创意、网红餐厅打造、内容传播策略
营销建议：学习创意营销手法，打造网红菜品

4. [红霉素软膏绝不能随便用](链接)
新闻摘要：红霉素软膏使用不当引发健康话题，食品安全话题升温，餐厅用药规范成为餐饮行业关注点。
餐饮切入点：食品安全话题、餐厅用药规范、健康饮食观念
营销建议：餐饮品牌强调食品安全标准

5. ["千滚水""隔夜水"到底能不能喝](链接)
新闻摘要：饮水安全话题引发热议，餐厅饮水标准成为消费者关注点，餐饮健康知识科普需求增加。
餐饮切入点：餐厅饮水安全、餐饮健康知识科普、服务细节
营销建议：餐厅可宣传饮水标准，茶饮品牌可借势科普健康饮水知识
```

**使用场景：**
- 餐饮咨询顾问日常热点追踪
- 餐饮品牌营销策划
- 餐饮行业趋势分析

---

### 5. 行业热点筛选

用户说"餐饮行业热点"、"科技行业热点"、"金融热点"时：

**预设行业：**
- 餐饮、科技、金融、教育、医疗、娱乐、房产、汽车、时尚、农业

**执行：**
```bash
# 加载行业关键词
INDUSTRY="餐饮"
KEYWORDS=$(cat ~/.config/china-hotnews/industries.json | jq -r ".industries.\"$INDUSTRY\" | join(\"|\")")

# 从各平台筛选行业相关热点
curl -s "https://60s.viki.moe/v2/weibo" | jq -r ".data | .[] | select(.title | test(\"$KEYWORDS\"; \"i\")) | \"• [\(.title)](\(.link)) (\(.hot_value)热度)\"" | head -5
```

**自定义行业关键词：**
```bash
# 用户可以自定义关键词筛选
curl -s "https://60s.viki.moe/v2/weibo" | jq -r ".data | .[] | select(.title | test(\"咖啡|奶茶|火锅|烧烤\"; \"i\")) | \"• \(.title)\""
```

---

### 6. 餐饮专业资讯抓取（NewsCrawler集成）

用户说"餐饮专业资讯"、"餐饮新闻抓取"、"抓取新闻"时：

**功能：抓取腾讯新闻、新浪新闻、网易新闻的餐饮专业资讯**

**数据源（NewsCrawler支持）：**
- 腾讯新闻（news.qq.com）
- 网易新闻（163.com）
- 搜狐新闻（sohu.com）
- 今日头条（toutiao.com）
- 微信公众号（mp.weixin.qq.com）

**公共API（所有人可用）：**
```
http://43.163.201.57:8001/api/extract
```

**使用方式：**
```bash
# 抓取单篇新闻
curl -X POST "http://43.163.201.57:8001/api/extract" \
  -H "Content-Type: application/json" \
  -d '{"url": "https://www.163.com/dy/article/xxx.html"}'
```

**输出格式：**
```json
{
  "status": "success",
  "data": {
    "title": "新闻标题",
    "texts": ["段落1", "段落2"],
    "images": ["图片URL"],
    "platform": "netease"
  }
}
```

**示例：抓取网易餐饮新闻**
```bash
curl -X POST "http://43.163.201.57:8001/api/extract" \
  -H "Content-Type: application/json" \
  -d '{"url": "https://www.163.com/dy/article/JMUCT9OD05228QIT.html"}' | jq '.data.title'
```

**查看支持的平台：**
```bash
curl http://43.163.201.57:8001/api/platforms
```

---

## 技术架构

### 数据源

| 功能 | 数据源 | API地址 |
|------|--------|---------|
| 社会热点 | 60s API | http://localhost:4398/v2 |
| 餐饮专业资讯 | NewsCrawler | http://localhost:8001/api |

### 服务部署

**60s API服务：**
- 端口：4398
- systemd服务：/etc/systemd/system/60s-api.service
- 项目路径：/root/60s

**NewsCrawler服务：**
- 端口：8001
- 项目路径：/root/NewsCrawler
- 启动方式：`uv run news-extractor-backend`

---

## 配置文件

- 主配置：~/.config/china-hotnews/config.json
- 推送脚本：/root/.openclaw/workspace/scripts/hotnews-push.sh
- 餐饮洞察脚本：/root/.openclaw/workspace/scripts/catering-insights.sh
- 餐饮资讯抓取：/root/.openclaw/workspace/scripts/catering-news-crawler.sh