# 自动化任务 Prompt 模板 — 每日花卉推荐

> 用途：将本 Skill 接入 WorkBuddy / Codex / Claude 等平台的定时任务（Automation）。
> 使用前请将下方所有 `{PROJECT_DIR}` 占位符替换为你的实际项目目录绝对路径。

---

执行每日花卉推荐任务：

1. 读取 `{PROJECT_DIR}/pushed_flowers.md` 查看已推送列表
2. 从未推送花卉中选择下一个品种（紫藤、栀子花、茉莉花、荷花、向日葵、玫瑰等）
3. 读取模板文件 `{PROJECT_DIR}/templates/flower-template.html` 作为结构基准

4. 生成内容规范：
   - **结构**：严格按照模板的模块生成，不要遗漏
   - **色彩主题**：根据花卉气质定制配色，修改 CSS 变量中的 4 个值（--primary、--primary-light、--primary-dark、--secondary）
   - **SVG插图**：写实风格，野外可辨识，包含比例尺和科学标注，viewBox="0 0 680 500"
   - **诗词赏析**：2首诗词 + 详细植物学对照表（4行），优先选择真实古诗词（白居易、杨万里、王维、苏轼等名家咏花诗）
   - **近亲植物**：3个卡片 + 对比表（4列：名称、学名、花色、与主花卉差异）
   - **趣味知识**：保留完整，5-6条，不精简
   - **本地物候**：包含本地化观赏信息

5. 保存HTML文件到 `{PROJECT_DIR}/generated-images/flower_[花卉拼音]_[日期YYYYMMDD].html`
6. 通过附件发送HTML文件给用户
7. 更新 `{PROJECT_DIR}/pushed_flowers.md`，标记该花卉为推送日期

## 异常处理
- pushed_flowers.md 不存在 → 按 templates/pushed-flowers-template.md 创建
- 历史表格损坏 → 备份后重建
- 模板缺失 → 按 SKILL.md 描述的模块结构从零生成
- 所有花卉已推送 → 同科不同品种补充推送
- HTML 渲染失败 → 重试一次，降级 Markdown
- 每次执行后必须确认 pushed_flowers.md 写入成功（历史记录完整性是去重机制的生命线）
