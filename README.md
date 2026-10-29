# shared-agent-skills

这是一套可复用的 agent skills 公开集合，任何人都可以克隆。

## 安装

```bash
git clone https://github.com/lymyiminglym/shared-agent-skills.git
```

把 `skills/meeting-to-wecom-tracker` 复制到所用 agent 的 skills 目录：

| Agent | Skills 目录 |
| --- | --- |
| Codex | `~/.codex/skills/` |
| Cursor | `~/.cursor/skills/`，或项目内 `<project>/.cursor/skills/` |
| WorkBuddy | 本机的 skills 目录 |

也可以用符号链接代替复制。之后在本仓库执行 `git pull`，已链接的技能会跟着更新。

## 本机企业微信配置

把 `skills/meeting-to-wecom-tracker/wecom.sh.template` 复制为同目录下的 `wecom.sh`，填好其中三处 `/PATH/TO/...` 占位，然后执行 `chmod +x wecom.sh`。填好的 `wecom.sh` 已被 git 忽略。令牌、配置目录和私有 docid 只留在各自机器上，不进入本仓库。

## Skills

| Skill | 路径 |
| --- | --- |
| meeting-to-wecom-tracker | `skills/meeting-to-wecom-tracker` |

---

# shared-agent-skills

A public collection of reusable agent skills. Anyone can clone it.

## Install

```bash
git clone https://github.com/lymyiminglym/shared-agent-skills.git
```

Copy `skills/meeting-to-wecom-tracker` into the agent's skills folder:

| Agent | Skills folder |
| --- | --- |
| Codex | `~/.codex/skills/` |
| Cursor | `~/.cursor/skills/`, or `<project>/.cursor/skills/` |
| WorkBuddy | its local skills folder |

Symlink that folder instead of copying it to follow updates with `git pull`.

## Local WeCom setup

Copy `skills/meeting-to-wecom-tracker/wecom.sh.template` to `wecom.sh` in the same directory, fill the three `/PATH/TO/...` placeholders, and run `chmod +x wecom.sh`. The filled `wecom.sh` is gitignored. Tokens, config directories, and private docids stay on each machine and never go in this repo.

## Skills

| Skill | Path |
| --- | --- |
| meeting-to-wecom-tracker | `skills/meeting-to-wecom-tracker` |
