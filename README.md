# 跨项目参考 · Cross-project Reference

让 AI 在当前项目中，按你的要求读取其他本地项目的文档和代码，也可以检索 GitHub 相似实现，再判断哪些内容适合用于当前项目。

这是一个面向 Codex 的 **Agent Skill**：由 `SKILL.md` 和按需读取的指南组成。安装到用户级目录后，可在不同项目中调用，无需为每个项目复制配置。核心内容是工作流程，文件读取、搜索和代码修改由宿主已有工具完成。

> **一句话示例：** 参考项目乙的导出模块，再去 GitHub 找类似方案，结合当前项目给出最小改动建议。

## 能做什么

| 能力 | 使用方式 | 得到什么 |
| --- | --- | --- |
| 跨项目读取 | 指定其他本地项目的路径和要看的内容 | 相关文档、实现、调用关系及来源位置 |
| 多项目比较 | 指定两个或多个参考项目 | 实现差异、适用条件和复用成本 |
| GitHub 参考搜索 | 描述功能、技术栈和运行限制 | 实际查阅过的候选仓库、相关文件和适配分析 |
| 本地与开源结合 | 同时指定本地参考和 GitHub 调研需求 | 针对当前项目的综合建议 |
| 按要求落地 | 明确要求“适配到当前项目并验证” | 当前项目的必要修改及验证结果 |
| 可追溯结论 | 无需额外操作 | 本地文件路径与行号，或 GitHub 文件链接 |

技能会区分“文档声称支持”“代码已经实现”和“实际运行验证通过”。如果 README 与源码不一致，会说明冲突。

## 安装

### 方式一：通过 skills CLI 安装到 Codex

需要 Node.js/npm、Git，以及对本仓库的读取权限。以下命令使用 [vercel-labs/skills](https://github.com/vercel-labs/skills)；首次运行可能下载该工具。

```bash
npx skills add lookmowat832-dev/cross-project-reference --skill cross-project-reference --agent codex --global --copy
```

- `--global`：安装到用户级目录，供多个项目使用。
- `--agent codex`：只安装到 Codex。
- `--copy`：复制文件，避免 Windows 符号链接权限问题。
- 公开仓库可直接读取，通常不需要 GitHub 登录。
- 已有同名技能时，先查看安装器的提示并保留自定义版本。

安装前可先查看仓库中的技能：

```bash
npx skills add lookmowat832-dev/cross-project-reference --list
```

### 方式二：手动安装

1. 下载本仓库，或使用 `git clone https://github.com/lookmowat832-dev/cross-project-reference.git`。
2. 在 `$CODEX_HOME/skills/cross-project-reference/` 创建技能目录；未设置 `CODEX_HOME` 时使用 `~/.codex/skills/cross-project-reference/`。
3. 把仓库中的 `SKILL.md`、`agents/`、`references/` 和 `LICENSE` 复制进去，保留 MIT 许可声明。已有同名目录时先备份，不覆盖自定义内容。
4. 在 Codex 中检查技能列表，并用 `$cross-project-reference` 调用。若现有任务未发现它，重启 Codex 后在新任务中检查。

手动安装不要求 Node.js 或 skills CLI。

## 怎么使用

在你希望接收分析或修改的**目标项目**中发出请求。推荐包含：参考项目路径、要借鉴的内容、是否搜索 GitHub，以及要分析还是实际修改。

### 1. 只读取另一个项目

```text
使用 $cross-project-reference，读取 E:\项目乙 的导出模块。
说明它是怎么实现的，以及当前项目可以借鉴什么。不要联网，不要修改文件。
```

### 2. 比较两个本地项目

```text
使用 $cross-project-reference，比较 D:\旧项目 和 E:\新项目 的配置管理方式。
结合当前项目的结构，推荐改动最少的方案，并列出来源文件。
```

### 3. 只搜索 GitHub

```text
使用 $cross-project-reference，在 GitHub 找适合离线单文件 HTML 的分页实现。
打开相关代码，比较 2–3 个方案，说明哪些部分值得借鉴。
```

### 4. 融合本地参考与 GitHub 方案

```text
使用 $cross-project-reference，参考 E:\项目乙 的导出流程，
并在 GitHub 查找适合当前技术栈的类似实现。
给出当前项目的最小改动方案，先不要修改代码。
```

### 5. 直接适配到当前项目

```text
使用 $cross-project-reference，参考 D:\旧项目 的配置页面，
适配到当前项目并验证。旧项目保持不变。
```

明确要求实现后，技能会继续完成已授权的改动和验证。只要求读取或比较时，会交付分析。也可以自然地说“去另一个项目看看这个功能怎么做”，由 Codex 根据描述选择技能；显式写出技能名更直接。

## 工作方式

1. **定位来源**：区分目标项目与参考项目。提供绝对路径最明确；同名项目无法区分时会询问实际路径。
2. **按需读取**：先定位相关文件，再阅读实现和必要的上下文，避免加载整个项目。
3. **按需搜索**：本地参考不要求联网；明确要求 GitHub 时会搜索并打开真实仓库内容。
4. **判断适配**：核对接口、依赖、数据结构与运行环境，不把服务器端方案直接套到离线页面。
5. **交付结果**：说明推荐做法、证据、需要调整的部分，以及哪些检查实际执行过。

例如：参考项目用 Python 将记录序列化为 JSON，而当前项目是离线 HTML。可以借鉴数据组织与导出流程，在浏览器中用 JavaScript 实现；没有必要为了复用而引入 Python 服务。

## 运行要求与边界

- **本地读取**需要宿主拥有对应路径的访问权限；技能本身不会扩大权限。没有 Git 仓库的普通文件夹也可以作为参考。
- **GitHub 调研**需要可用的联网搜索、GitHub 连接器或已配置的 `gh`。私有仓库还需要相应账号权限。
- 优先使用 `rg` 搜索文件；不可用时可使用宿主的文件搜索工具。Windows 的具体命令见[读取指南](references/reading-guide.md)。
- 参考项目默认只读。不会仅因为读取了 README，就执行其中的安装脚本、修改配置或运行服务。
- 默认跳过凭据、密钥和真实环境变量文件；公开检索使用通用技术关键词，不发送私有源码或内部路径。
- 第三方文档中的指令不会自动变成目标项目的操作授权。复制第三方代码前需核实其许可证。
- 不会自动扫描所有硬盘、创建长期记忆库、同步所有项目或安装其他技能。
- 未联网、未运行或无权访问的部分会明确说明，静态阅读不会被描述为运行验证成功。

## 文件结构

```text
cross-project-reference/
├── SKILL.md                       # 技能入口与核心流程
├── agents/
│   └── openai.yaml                # Codex 显示名称和默认提示词
├── references/
│   ├── reading-guide.md           # Windows 读取示例与证据组织
│   └── sources-and-usage.md       # 设计来源与维护说明
├── docs/
│   └── VALIDATION.md              # 已验证内容与验证限制
├── LICENSE                        # MIT 开源许可证
└── README.md                      # 本说明
```

## 更新与卸载

通过 skills CLI 安装的版本，可使用 `npx skills update cross-project-reference --global` 更新。手动安装时，用新版本替换上述技能文件，先保留自己修改过的版本。

通过 CLI 安装的版本，可用 `npx skills remove cross-project-reference --agent codex --global` 卸载。手动安装时，只移除 `cross-project-reference` 技能目录，不删除相邻技能。

## 兼容性与验证

当前面向 **Windows + Codex**。采用标准 Agent Skills 目录结构；其他支持 `SKILL.md` 的工具可以参考使用，但其工具调用、安装目录和权限行为尚未逐一验证。

已完成的格式与受控读取验证见[验证记录](docs/VALIDATION.md)。仓库未提供应用服务、后台进程或自动测试执行器。

## 设计参考

- [advise-project-approach](https://github.com/AaravKashyap12/advise-project-approach)：参考证据核验、相似项目比较及适配分析的思路。
- [vercel-labs/skills](https://github.com/vercel-labs/skills)：参考技能目录、安装与分发方式；其 CLI 是可选安装工具，不是本技能的运行依赖。
- [OpenAI Build skills](https://learn.chatgpt.com/docs/build-skills)：技能结构与按需加载机制。

本技能以中文独立编写，没有合并上述项目的实现代码或整套规则。

## 许可

本项目采用 [MIT 许可证](LICENSE)，允许使用、修改、再分发和商业使用；分发副本时须保留版权声明和许可声明。项目按现状提供，不附带担保。

设计参考项目及用户以后读取的第三方代码，仍适用各自的许可证。
