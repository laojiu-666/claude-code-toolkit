---
description: 智能 git commit，自动生成规范提交信息
argument-hint: [staged|all]
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git add:*), Bash(git commit:*)
---

提交范围：
- 无参数：当前对话涉及的文件
- `staged`：暂存区
- `all`：所有改动（git add -A）

流程：
1. `git status --porcelain` 检查改动
2. 按参数 add 文件
3. `git diff --cached` 分析改动

提交格式（Conventional Commits）：
```
<type>(<scope>): <subject>

<body>
```

type：feat|fix|refactor|docs|style|chore|test
scope：模块名（**优先用对话中用户提到的中文名**，公共代码省略 scope）
subject：一句话总结
body：具体功能点列表

**禁止**：不要生成 `Co-Authored-By` 行

示例：
```
feat(用户): 新增登录功能

- 添加登录接口
- 实现 token 生成
```

```
fix: 修复日期工具时区问题

- 统一使用 UTC 时间
```

生成后展示确认，用户 ok 后执行 commit。
