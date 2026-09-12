# 参考与使用说明

## 设计参考

于 2026-09-11 查看以下公开资料；本技能采用独立撰写的中文流程，没有复制其实现代码或整体规则。链接内容可能继续更新。

- [AaravKashyap12/advise-project-approach](https://github.com/AaravKashyap12/advise-project-approach)：参考其实际证据、相似项目比较和根据当前约束判断可迁移部分的思路。本技能重点补充按用户指定范围读取其他本地项目，并保留用户已授权的实现流程。
- [vercel-labs/skills](https://github.com/vercel-labs/skills)：参考标准技能目录、全局/项目安装以及跨工具分发方式。它是技能管理工具；本技能没有把其 CLI 安装为运行依赖，也不把“跨项目可用”等同于“自动获得文件访问权”。
- [OpenAI Build skills](https://learn.chatgpt.com/docs/build-skills)：参考 `SKILL.md`、按需加载支持文档和 UI 元数据的结构。

## 当前安装

技能名称为 `cross-project-reference`，以用户级技能安装在 `$CODEX_HOME/skills`；未设置该变量时使用 `~/.codex/skills`。它不需要在每个项目里复制一份，不依赖固定项目列表或后台服务。

Codex 可以根据任务描述选择它，也可以显式调用 `$cross-project-reference`。若已打开的任务没有发现新技能，重启 Codex 后在新任务中检查技能列表；不要仅凭文件存在就宣称宿主已自动加载。

## 示例

- “使用 $cross-project-reference，去 E:\项目乙 读取导出模块，只给当前项目建议，不要联网。”
- “参考 D:\旧项目 的配置页面，适配到当前项目并验证。”
- “比较这两个本地项目的实现，再去 GitHub 查有没有更适合当前需求的开源方案。”

其他支持 Agent Skills 的工具也可以读取核心 `SKILL.md`，但各工具的安装目录、项目发现工具和访问权限需分别配置与验证；当前安装只面向 Codex。

## 移除

移除本次创建的 `cross-project-reference` 技能目录即可撤销安装；不要移除相邻技能。没有更改项目代码、其他技能、宿主配置或既有记忆。
