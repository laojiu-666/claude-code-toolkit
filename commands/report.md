---
description: 从多个项目的远端提交记录生成统一日报/周报
argument-hint: [daily|weekly=daily] [polite] [since=auto] [until=now]
allowed-tools: Bash(git fetch:*), Bash(git config:*), Bash(git log:*), Bash(cd:*), Read
---

按 Git 提交生成日报/周报（中文），支持多项目汇总。只输出"工作内容"：不含哈希/命令/路径/要点/场面话。

参数：
- mode=$1(默认daily)；若 $2=="polite" 则 tone=polite 否则 plain
- 时间：plain 用 since=$2 until=$3；polite 用 since=$3 until=$4
- since 为空：daily="1 day ago"，weekly="7 days ago"；until 默认 now

项目配置：
- 配置文件：`~/.claude/report-projects.txt`，每行一个 git 项目绝对路径
- 若配置文件不存在或为空，则只扫描当前目录
- 使用 `/report-scan <path>` 命令可自动扫描并生成配置文件

执行（直接做，不要解释步骤）：
1. 读取 `~/.claude/report-projects.txt`，获取所有项目路径
2. 获取 author：优先 `git config --global user.email`，否则 `git config --global user.name`
3. 遍历每个项目：
   - 进入目录，执行 `git fetch --all --prune`
   - 获取项目名：取目录名作为项目名
   - 获取 commits：`git log @{u} --since="<since>" --until="<until>" --no-merges --date=short --pretty=format:'%ad|%s' --author="<author>"`
   - 记录：项目名 + commits 列表
4. 汇总所有项目的 commits

写作规则：
- 按项目分组，将每个项目的 commits 归并成 2~5 条工作项。
- plain：每条 1 句，只写"完成了什么"。
- polite：每条 2~3 句，按"为什么→做了什么→影响/价值"写，但不写寒暄。
- 某项目无提交：该项目不显示在报告中。
- 所有项目均无提交：输出"本期无代码提交"。

输出格式（严格）：
```
【<日报/周报>】（统计区间：<start> ~ <end>）

## <项目名1>
- <工作项1>
- <工作项2>

## <项目名2>
- <工作项1>
- ...
```
