# china-hotnews 腰部餐厅品牌热点洞察（张总版）

基于张总品牌定位视角，从社交平台热点和餐饮行业资讯中，筛选对**腰部餐厅品牌**（已开数家门店、有品牌化需求的餐饮老板）有参考价值的新闻。

支持微博/知乎/抖音/B站/今日头条/百度热搜，及红餐网等行业平台。

激活关键语：餐饮热点、餐饮新闻

## v2.0.0 更新内容

本次更新将技能从"泛餐饮新闻推送"升级为"张总品牌定位视角版"：

- 🎯 **精准定位**：目标用户聚焦腰部餐厅品牌老板，不再是泛餐饮从业者
- 🚫 **排除规则**：明确排除饮料快消品（康师傅、可口可乐等）、咖啡连锁（瑞幸、星巴克等）、茶饮连锁（喜茶、蜜雪冰城等）、零食预包装
- 🔍 **新增维度词**：连锁、加盟、品牌升级、门店模型、招牌菜、标准化、食安、客单、复购、翻台率等
- 🏷️ **六维分析模型**：每条新闻标注`[选择理由]` `[门店模型]` `[招牌菜]` `[认知溢价]` `[体验闭环]` `[食安/标准]`
- 💡 **输出重构**：从"餐饮切入点+营销建议" → "张总视角：核心判断+给老板的动作"

## 快速开始

```bash
# 微博热搜
curl -s "https://60s.viki.moe/v2/weibo" | jq -r '.data[:5] | .[] | "• \(.title) (\(.hot_value)热度)"'

# 知乎热榜
curl -s "https://60s.viki.moe/v2/zhihu" | jq -r '.data[:5] | .[] | "• \(.title) (\(.hot_value_desc))"'

# 抖音热点
curl -s "https://60s.viki.moe/v2/douyin" | jq -r '.data[:5] | .[] | "• \(.title) (\(.hot_value)热度)"'
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
- 品牌定位方法论: 张总餐饮品牌顾问视角
- 作者: HuYee
