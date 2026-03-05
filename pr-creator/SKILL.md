---
name: pr-creator
description:
  当被要求创建拉取请求 (PR) 时，请使用此技能。它确保所有 PR 都遵循仓库既定的模板和标准。
---

# pr-creator

此技能指导创建符合仓库标准的高质量拉取请求。

## 工作流程

请按照以下步骤创建拉取请求：

1.  **分支管理**: **关键:** 确保你**没有**在 `main` 分支上工作。
    - 运行 `git branch --show-current`。
    - 如果当前分支是 `main`，你必须创建并切换到一个新的描述性分支：
      ```bash
      git checkout -b <new-branch-name>
      ```

2.  **提交更改**: 验证所有预期的更改都已提交。
    - 运行 `git status` 以检查是否有未暂存或未提交的更改。
    - 如果存在未提交的更改，请暂存并提交它们，并附上描述性消息，然后再继续。**切勿**直接提交到 `main`。
      ```bash
      git add .
      git commit -m "类型(范围): 描述"
      ```

3.  **定位模板**: 在仓库中搜索拉取请求模板。
    - 检查 `.github/pull_request_template.md`
    - 检查 `.github/PULL_REQUEST_TEMPLATE.md`
    - 如果存在多个模板（例如在 `.github/PULL_REQUEST_TEMPLATE/` 中），请询问用户使用哪一个，或根据上下文选择最合适的一个（例如 `bug_fix.md` 与 `feature.md`）。

4.  **阅读模板**: 阅读已识别模板文件的内容。

5.  **草拟描述**: 创建严格遵循模板结构的 PR 描述。
    - **标题**: 保留模板中的所有标题。
    - **清单**: 审查每一项。如果已完成，标记为 `[x]`。如果某项不适用，根据模板说明保持未选中状态 `` 或将其移除（但如果模板允许灵活性，建议保持未选中状态以保持透明度）。
    - **内容**: 填写各部分，清晰、简洁地总结你的更改。
    - **相关问题**: 链接此 PR 修复或相关的任何问题（例如，“Fixes #123”）。

6.  **预检**: 在创建 PR 之前，运行工作区的预检脚本，以确保所有构建、检查和测试通过（注意：可选择跳过）。
    ```bash
    npm run preflight
    ```
    如果任何检查失败，请在继续创建 PR 之前解决这些问题。

7.  **推送分支**: 将当前分支推送到远程仓库。
    **关键安全护栏**: 推送前请再次核对你的分支名称。
    如果当前分支是 `main`，**切勿**推送。
    ```bash
    # 验证当前分支不是 main
    git branch --show-current
    # 非交互式推送
    git push -u origin HEAD
    ```

8.  **创建 PR**: 使用 `gh` CLI 创建 PR。为了避免多行 Markdown 的 Shell 转义问题，请先将描述写入临时文件。
    ```bash
    # 1. 将草拟的描述写入临时文件
    # 2. 使用 --body-file 标志创建 PR
    gh pr create --title "类型(范围): 简明描述" --body-file <temp_file_path>
    # 3. 删除临时文件
    rm <temp_file_path>
    ```
    - **标题**: 如果仓库使用 [Conventional Commits](https://www.conventionalcommits.org/) 格式，请确保标题遵循该格式（例如 `feat(ui): 添加新按钮`、`fix(core): 解决崩溃问题`）。

## 原则

- **安全第一**: **切勿**推送到 `main`。这是你的最高优先级。
- **合规性**: 切勿忽略 PR 模板。它存在的理由。
- **完整性**: 填写所有相关部分。
- **准确性**: 不要为未完成的任务勾选方框。