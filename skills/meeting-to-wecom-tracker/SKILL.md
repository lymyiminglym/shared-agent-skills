---
name: meeting-to-wecom-tracker
description: >-
  Append meeting notes to a WeCom (企业微信) smartsheet tracker through the
  local wecom.sh wrapper. Use when the user wants meeting outcomes, progress
  updates, notes, or hours written into their WeCom tracker.
---

# 会议纪要写入企业微信跟踪表

把一次会议的结论写成跟踪表里的新行。只通过本技能目录中已在本机填好的 `wecom.sh` 调用企业微信命令行。令牌、配置目录、本机路径和私有 docid 留在本机，不写入仓库，也不贴进回复。

## 何时使用

用户要求把会议纪要、进展、备注或工时写入企业微信智能表格时使用本技能。

## 准备

1. 包装脚本是本技能目录下的 `wecom.sh`。它由 `wecom.sh.template` 复制而来，三处 `/PATH/TO/...` 已填好，并且已经 `chmod +x`。运行模板文件无效。不要自行拼接命令行工具的绝对路径，也不要把填好的 `wecom.sh` 写回仓库。
2. 若 `wecom.sh` 不存在，停下来，请用户按仓库 README 完成本机配置。
3. 在技能目录执行 `./wecom.sh auth show --status`。输出不是 `authorized` 时停止，请用户在 WorkBuddy 客户端里重新授权。不要执行 `auth init`，不要重装命令行工具，不要改配置目录里的文件。

下文用 `$S` 表示这个 `wecom.sh`。

## 定位表

docid 和子表名只来自用户本次说明，或用户指出的、未纳入 git 的本机笔记。用户没给时先询问，不要猜测，也不要沿用任何写在仓库里的示例。

```bash
$S smartsheet sheets list --json '{"docid":"DOCID"}'
$S smartsheet fields list --json '{"docid":"DOCID","sheet_title":"SHEET","limit":50}'
```

先读字段，再起草要写的内容。单选项的 id 只能从 `fields list` 读取。

## 从会议整理行

从用户给出的纪要里抽出要写入的事实。表里如果有下列列名，按这张表对应；列名不同时，根据 `fields list` 询问用户每一列该填什么。

| 会议里的信息 | 常见列 |
| --- | --- |
| 日期 | 时间 |
| 进展或结论 | 进展 |
| 补充说明、附件名 | 备注/附件 |
| 工时 | 工时 |

纪要里没有的事实留空。写入前把拟定行展示给用户，等用户确认。

## 写入与回读

只新增用户确认过的行。覆盖整篇文档，或删除记录，都要等用户另行明确指出目标，并先回读确认。

文本和数字写成标量。单选写成 `[{"id":"OPTION_ID","text":"选项文字"}]`，其中 `OPTION_ID` 来自字段定义。自动编号由系统生成，不要写入。

```bash
$S smartsheet records add --json '{
  "docid":"DOCID",
  "sheet_title":"SHEET",
  "records":[{"values":{"进展":"会议结论"}}]
}'
```

写完后用 `smartsheet records query` 回读，把实际落表的内容告诉用户。接口返回成功但回读不一致时，说明差异，不要连续重试。授权失败时停止，把错误信息交给用户。

## 不要做的事

- 不要把 docid、令牌、配置目录、本机路径或填好的 `wecom.sh` 写进仓库或贴进回复。
- 不要执行 `auth init`，不要安装另一份命令行工具。
- 不要在未读字段、未获用户确认时写入。
