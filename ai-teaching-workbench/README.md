# AI Teaching Workbench — MVP v0.1

这是一个用于生成教学设计、课堂逐字稿、HTML 课件草稿和配套视觉素材的半自动工作流。此目录是新增模块，不修改现有 OpenCode agent 文件。

## 当前版本能做什么

- 从 Google Drive 的 JSON 任务单读取教学要求。
- 通过 Gemini API 生成教学设计、逐字稿、课件页内容和图片提示词。
- 导出 Markdown 文档、可浏览器打开的 HTML 课件、图片提示词，以及基于 SVG 的简易视觉卡片。
- 将成果和运行日志写回 Google Drive。
- 每次只处理一个任务；运行前由教师人工检查任务 JSON。

**图片能力边界：** MVP 输出可用的 SVG 视觉卡片和供后续图片模型使用的提示词；它不声称已连接真实的文生图服务。真实场景插图 API 留作第二阶段，避免假定账号已开通特定模型。

## 架构与任务传递

```
教师编辑 task.json (Google Drive)
        ↓ 人工确认
Colab Notebook 读取任务并调用 Gemini API
        ↓
生成 outputs/<task_id>/ 的文档、HTML、SVG、提示词和日志
        ↓
教师打开文件验收
Grok Bot：第一阶段只做任务整理/提醒；接口验证后再接 Webhook/API 触发
```

GitHub 保存代码和模板，不保存 API key、学生个人信息或实际生成结果。Google Drive 保存任务和成果。Colab 是临时运行环境，不能作为持久存储。

## 首次配置

1. 打开 Google Colab，新建 Notebook；或从仓库打开 `colab/teaching_workbench.ipynb`。
2. 在 Google Drive 建立 `AI教学工作台/tasks` 和 `AI教学工作台/outputs` 文件夹。
3. 将本目录的 `task_template.json` 上传到 `AI教学工作台/tasks/task.json`。
4. 在 Colab 左侧 **Secrets（钥匙图标）** 添加 `GEMINI_API_KEY`，值从 Google AI Studio 获取。只给当前 Notebook 授权访问；不要把密钥写进代码、JSON、GitHub 或 Drive。
5. 打开 Notebook，运行单元格。默认任务路径为 `/content/drive/MyDrive/AI教学工作台/tasks/task.json`。
6. 首次先用测试任务运行，检查 `outputs/<task_id>/` 下所有文件后再投入正式备课。

## 任务文件

编辑 `task_template.json` 的教材、课文、年级、课时、教学风格和额外要求。每个任务请使用唯一的 `task_id`。不要在任务文件里放密钥或学生可识别信息。

## 模型与费用

默认模型由 Notebook 变量 `GEMINI_MODEL` 控制（默认 `gemini-2.5-flash`，如不可用请改成你账号当前可用的模型）。API 免费额度、可用模型和速率限制会变化；运行前查看 Google AI Studio 的账号额度与条款。Colab 免费 GPU 并非保证分配，本 MVP 的文本生成通常不需要 GPU。

## Grok Bot 连接策略

当前没有假设 Grok Bot 拥有可用的外部 Webhook、API 或文件访问能力。先采用人工交接：Bot 输出符合任务模板的 JSON → 教师复核 → 放入 Drive → 在 Colab 点击运行。只有确认你所用 Grok Bot 的具体产品、权限和可用接口后，才增加自动触发；不得把 API key 交给 Bot 聊天内容或写入任务文件。

## 权限与安全

- Colab 挂载 Drive 后可访问相应账号授权范围内的文件；尽量使用专门的工作 Google 账号和独立工作目录。
- 不把 API key 提交到 GitHub。若曾提交过，立即在提供方撤销并重新生成。
- 只使用可信的 Notebook；检查代码后再运行。不要运行来源不明的外部脚本。
- 输出默认保存在私人 Drive 文件夹；分享文件时逐项检查权限，避免“知道链接的任何人”。
- AI 输出必须人工审核事实、教材版本、教学目标、课堂时长、引用和图片版权。
- 运行日志不应记录 API key；不要输入学生姓名、联系方式、成绩等敏感数据。

## 验收清单

- [ ] Colab 成功挂载 Drive。
- [ ] 读取到任务 JSON，任务字段正确。
- [ ] API key 从 Colab Secrets 读取，且未出现在输出日志。
- [ ] 生成教学设计、逐字稿、课件 HTML、图片提示词、SVG 视觉卡片。
- [ ] HTML 可打开；中文显示正常。
- [ ] Drive 中成果文件齐全，日志状态为 success。
- [ ] 教师检查后才将成果用于课堂。
