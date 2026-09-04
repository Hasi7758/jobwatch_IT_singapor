# 新加坡 IT 职位监控

每天自动抓取新加坡新发布的 **Cloud / DevOps / Platform / AI / Software Engineer** 职位,结果发布到网页。
面向 1 年左右工作经验:已过滤 Senior / Lead / Manager 等资深岗,以及 Data 类岗位。

判断"新"靠自己建库做差分:职位 ID 首次出现的那天才算新,不看平台标注的日期。

**网址**: https://hasi7758.github.io/jobwatch_IT_singapor/

## 数据来源
- **MyCareersFuture** — 新加坡政府官方求职门户,公开接口,提供真实发布日期。
  按 15 个关键词分别搜索。按公平考量框架,多数要申请 EP 的岗位必须先在这里公开 14 天,
  所以 Amazon / Google / Shopee / DBS 这类没有公开接口的大公司也基本不会漏。
- **公司直连** — 30 多家公司的招聘系统接口(GovTech、Grab、Stripe、ByteDance、GitLab、Coinbase、
  NVIDIA、Salesforce…)。公司发到自家系统永远早于聚合平台。

## 页面怎么看
- 顶部彩色标签是统计:**方向**(云/DevOps/平台 · AI/ML · 软件开发)、**初级友好**、**行业**(含德企/德语区、招聘中介)。
- 每条职位带同样的标签;绿色 **初级友好** = 职位名里有 Junior / Associate / Graduate / Engineer I 等字样,
  这些排在最前面。
- **德企/德语区**(粉色)= 德国、瑞士、奥地利公司,有单独一节不限日期列出,德语能派上用场。
- "初级 / 毕业生友好岗位" 一节不限日期,把库里所有初级岗都列出来。
- 列表排序:初级友好 → 直接雇主 → 中介/外包,同组内新的在前。
- **走 EP 的注意**:「政府与公共部门」岗(GovTech / DSTA / ST Engineering 等)多数只收公民或 PR,
  已通过 `config.yaml` 的 `display.hide_tags` 整体隐藏(仍在库里,删掉那行就显示)。
  优先看跨国公司、银行、德企;中介/外包商很多是能办 EP 的,别一律跳过。
- 页面显示最近 5 天内的职位,更早的折叠在下方。
- **招聘中介** 标签的职位多是外包/合同岗,真实雇主未必是标出来的那家。

定时:每天 UTC 22:40(新加坡时间次日 06:40)。

## 改关键词
在 GitHub 网页上点开 `config.yaml`,编辑 `keywords.include` / `exclude`,提交即可。
- 只看职位名,整词匹配(`lead` 命中 "Tech Lead",不会误伤 "Leadership Programme")。
- `exclude` 优先级高于 `include`。
- 觉得 `developer` 兜底太宽就删掉它;想看 Data 岗就把 `data` 那组删掉。

## 改标签规则
标签规则在 `jobwatch.py` 里的 `ROLE_RULES` / `JUNIOR_TERMS` / `CATEGORY_RULES`(公司名整词匹配)。
改完把 `TAGS_VERSION` 字符串换一下,下次运行会给库里全部职位重新打标签。

## 加公司
打开公司招聘页,点一个职位看地址栏跳到哪个域名,按 `companies.yaml` 末尾的对照表填进去。
`companies.yaml` 里 B 段的公司是凭经验加的,首次运行后看 Actions 日志,标 `[失败]` 的删掉。

## 首次部署
1. GitHub 新建**公开**仓库 `jobwatch_IT_singapor`,把这些文件推上去。
2. Settings → Pages → Source 选 *Deploy from a branch*,分支 `main`,目录 `/docs`。
3. Actions 页面选 `daily-jobwatch-it-sg` → Run workflow,跑一次建基线(第一次不算新职位)。
4. 可选:Settings → Secrets → 加 `TELEGRAM_BOT_TOKEN` 和 `TELEGRAM_CHAT_ID`,有新职位就推 Telegram。

## 注意
GitHub 规定公开仓库连续 60 天无真人活动会停用定时任务,
届时会收到邮件,去 Actions 页面点一下 Enable workflow 即可。
