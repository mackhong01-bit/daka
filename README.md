# 和心連携 · 员工考勤日报系统

更新：2026-10-16

## 现在有什么

| 内容 | 位置 |
| --- | --- |
| 可用原型（考勤日报台） | https://claude.ai/artifact/TFM5bhCz9S7gtGQNqQMbyX |
| 原型源代码 | `prototype/timecard.html` |
| 独立网站方案 | https://claude.ai/code/artifact/af576440-04eb-4ab5-b6e9-f99c388bff7c |

原型源代码是单个 HTML 文件。它的数据存在 claude.ai 上，所以直接在浏览器里打开这个文件只会显示示例数据。正式的员工数据要在上面的 claude.ai 链接里使用。

## 原型功能

- 登录页：显示公司名称和标志，员工第一次进来要登记员工编号、姓名和部门。
- 员工端：上下班打卡和休息打卡、提交日报（业务内容、明日计划、问题、相談事項）、查看和更新老板布置的任务、提交改卡申请。
- 老板端：每日考勤、日报动态（可回复和标记已阅）、月度统计（普通加班、深夜、休日劳动、36协定预警、导出 CSV）、任务布置、改卡审批、设置（规定时间、締め日、法定休日、公司标识）。
- 中文和日文界面切换，内置 2026–2027 年日本祝日。

## 原型的数据结构（迁移时用）

- `config/settings`：start, end, closing, legalDow, fyStart, otMonth, otYear, companyName, companyNameJa, logoId
- `staff/<uid>`：name, dept, empNo, joinedAt, requests{rid: {date, inTime, outTime, breakMin, reason, status, bossNote}}, tasks{tid: {title, detail, due, priority, status, progress, doneAt, withdrawn}}
- `staff/<uid>/days/<YYYY-MM-DD>`：inAt, outAt, breaks[{s,e}], breakMin, report{done, plan, issues, consult}, reportAt, bossComment, reviewedAt, corrected, correctionLog[]
- 时间都是毫秒时间戳，日期按日本时间（Asia/Tokyo）。

## 下一步：独立网站

方案：Next.js + Supabase（东京区域）+ Vercel，用官网的子域名登录。分三个阶段：

1. 登录和考勤上线，包括日报、任务、改卡和月度统计。
2. 「谁在做什么」实时看板和业务知识库。
3. 带薪休假申请，以及知识库 AI 问答（可选）。

开工前要确定的事：

- [ ] 官网域名、想用的子域名、域名在哪里管理
- [ ] Vercel 和 Supabase 账号（用公司邮箱注册）
- [ ] 放代码的 GitHub 私有仓库
- [ ] 员工人数，是否有部门主管
- [ ] 登录方式：员工编号加密码，还是公司 Google / Microsoft 邮箱
- [ ] 预算（正式运营每月大约 7,000 日元，是估计值）

规则：部署、改 DNS、推送代码之前都要先征得老板同意。
