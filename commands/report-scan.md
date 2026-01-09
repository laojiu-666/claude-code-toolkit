---
description: 扫描指定路径下所有 git 项目并写入配置文件
argument-hint: <path>
allowed-tools: Bash(find:*), Bash(cd:*), Bash(test:*), Read, Write
---

扫描指定路径下所有 git 项目，将项目绝对路径写入 `~/.claude/report-projects.txt`。

参数：
- path=$1（必填）：要扫描的根目录路径

执行（直接做，不要解释步骤）：
1. 递归扫描 path 目录，查找所有包含 .git 的目录
2. 跳过 node_modules、dist、build、.git 内部目录
3. 将所有发现的 git 项目绝对路径写入 `~/.claude/report-projects.txt`（覆盖写入）
4. 输出扫描结果摘要

输出格式：
```
已扫描 <path>，发现 <n> 个 git 项目，已写入 ~/.claude/report-projects.txt
```
