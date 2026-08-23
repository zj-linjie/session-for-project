<p align="center">
  <img src="./assets/readme/hero.svg" width="100%" alt="Session for Project：为 Codex 提供可跨会话复用的项目记忆" />
</p>

<p align="center">
  <strong>一个把项目上下文沉淀进仓库的可复用 Codex Skill。</strong><br />
  开启新会话？先读项目，再继续工作。
</p>

<p align="center">
  <a href="./README.md">English</a> ·
  <a href="./README.zh-CN.md">简体中文</a>
</p>

<p align="center">
  <a href="#解决什么问题">解决什么问题</a> ·
  <a href="#工作方式">工作方式</a> ·
  <a href="#安装">安装</a> ·
  <a href="#使用">使用</a>
</p>

## 解决什么问题

聊天记录不适合保存项目的长期事实。重要决策会在 handoff 中反复出现，临时路径会失效，新会话还要从脏工作区重新推断意图。

`session-for-project` 把这些上下文整理成一套小而可审计的文档系统：

- `AGENTS.md` 保持精简，把不同类型的任务路由到对应细节。
- `docs/DESIGN.md` 保存已经确认的产品与视觉决策。
- `docs/ARCHITECTURE.md` 保存已核对的数据流、状态、权限边界与部署边界。
- `docs/STATUS.md` 只保存代码和 Git 无法直接推断的未完成目标。
- 更新记忆文档前，先将旧版本连同校验和复制到带时间戳的归档目录。

它适用于 Git 仓库、非 Git 目录和空项目。它面向同一项目目录中的普通新会话，让 handoff 回到例外位置，而不是成为上下文存储层。

## 工作方式

<p align="center">
  <img src="./assets/readme/workflow.svg" width="100%" alt="五阶段恢复流程：ground、snapshot、route、validate、resume" />
</p>

整个流程保持克制且可追溯：

1. 先根据当前文件、指令和工作区状态建立事实基线。
2. 修改前为已有记忆文档创建带校验和的快照。
3. 将长期事实路由到规范文档中，避免重复记录。
4. 校验链接、路径、状态文档长度、归档内容和三条恢复路径。
5. 让下一次会话依据 `AGENTS.md` 选择需要读取的文档。

## 安装

将这个私有仓库克隆到 Codex Skill 目录：

```bash
git clone git@github.com:zj-linjie/session-for-project.git "${CODEX_HOME:-$HOME/.codex}/skills/session-for-project"
```

如果目录已经存在，则直接更新：

```bash
git -C "${CODEX_HOME:-$HOME/.codex}/skills/session-for-project" pull --ff-only
```

仓库包含 Skill 入口、文档契约和本 README/视觉层；它不会安装依赖，也不会修改项目代码。

## 使用

在项目目录中直接调用 Skill：

```text
$session-for-project 为当前项目建立跨会话记忆
```

也可以指定其他目录：

```text
$session-for-project 为 /path/to/project 建立跨会话记忆
```

Skill 会先读取项目现有的指令文件，保留正在生效的 `AGENTS.md` 与 `CLAUDE.md` 角色，归档旧的记忆文档，并报告无法安全推断的内容。

## 安全边界

### 长期事实与临时状态

长期产品决策写入 `DESIGN.md`；长期系统与工作流决策写入 `ARCHITECTURE.md`；未完成目标保留在 `STATUS.md` 中，完成后将状态恢复为 `clear`。

### 先归档再重写

归档是位于下方目录中的精确、经过校验和验证的快照：

```text
docs/archive/session-memory/<UTC-timestamp>/
```

工具加载的指令会保留在原位。像已被吸收的 handoff 这类明确临时文件，只有在核对引用后才会移入归档。

### 同目录边界

记忆文档属于项目本地。被忽略的草稿、未提交改动和机器本地素材不会自动跟随到其他 Git worktree 或另一台电脑；Skill 会记录这一限制，而不是隐藏它。

## 仓库结构

```text
SKILL.md                              # 面向模型的入口
agents/openai.yaml                   # Codex UI 元数据
references/document-contract.md      # 审计、快照与校验规则
assets/readme/                        # README 视觉资源与可编辑 SVG 源文件
```

## 明确不会做什么

- 不会在未保留原有规则的情况下替换项目正在生效的指令文件。
- 不会编造架构、设计决策、测试、采用情况或兼容性声明。
- 不会在建立记忆时发布内容、部署网站、提交代码或推送改动。
- 不会把每一份旧 README、API 文档或 ADR 都转成会话记忆。

## 许可证

这个私有仓库目前尚未声明许可证。

