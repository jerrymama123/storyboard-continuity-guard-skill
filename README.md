# Storyboard Continuity Guard Skill

一个给 Codex 使用的本地 Skill，用于审核和修复 AI 短剧生产里的分镜线稿、场景参考图、视频指导帧连续性问题。

它来自一次真实生产问题：ChatGPT Image 生成楼道线稿时，上一张楼道是左拐，下一张却镜像成右拐，导致空间连续性崩掉。这个 Skill 把经验固化为可复用检查流程。

## 适用场景

- ChatGPT Image / Seedance / Kling / Sora 的分镜线稿和视频参考图审核
- 同一场景跨多个镜头时，需要锁定空间方向、左右关系、门窗/灯/扶手/管线位置
- 第一人称 POV 镜头需要避免主角正脸、身体、背影或完整倒影入画
- 需要区分“线稿可带静态 UI”与“视频提示词应使用空白 UI 占位”

## 核心经验

- 不要只写“楼道”“出租屋”“办公室”这类泛场景词。
- 必须上传上一张已批准参考图，并明确它锁定了哪些视觉事实。
- 必须写死左/右、前景/中景/背景、道具位置、门灯扶手管线关系。
- 必须禁止镜像翻转、换场景、换楼道、换角色站位。
- 如果参考图里有箭头、标注、边框，要明确“只用于理解构图，新图里不要画出来”。

## 安装

把整个仓库目录复制到 Codex skills 目录：

```bash
cp -R storyboard-continuity-guard-skill ~/.codex/skills/storyboard-continuity-guard
```

或只复制核心文件：

```bash
mkdir -p ~/.codex/skills/storyboard-continuity-guard/agents
cp SKILL.md ~/.codex/skills/storyboard-continuity-guard/SKILL.md
cp agents/openai.yaml ~/.codex/skills/storyboard-continuity-guard/agents/openai.yaml
```

## 使用

在 Codex 中提到 `$storyboard-continuity-guard`，或在处理分镜线稿、场景图、视频参考图连续性时让 Codex 自动触发。

示例：

```text
Use $storyboard-continuity-guard to audit these storyboard frames for spatial continuity and POV constraints.
```
