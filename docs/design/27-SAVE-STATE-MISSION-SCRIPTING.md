# 27 — 存档、检查点与任务脚本规范

## 1. 存档分层

建议分为：
- `settings`：音量、键位、无障碍，不属于角色进度。
- `profile`：全局解锁、统计、挑战记录。
- `campaignSave`：主线进度、角色、库存、任务、伙伴、世界选择。
- `checkpointSnapshot`：当前关卡重开所需最小状态。

## 2. Campaign Save

核心字段：
`saveVersion, createdAt, updatedAt, playTime, difficulty, playerLevel, xp, skillPoints, skills{}, redAbilities{}, inventory, equipped, credits, parts, intel, redSamples, unlockedWeapons[], discoveredAttachments[], missionStates{}, sideQuestStates{}, companionStates{}, storyFlags{}, collectibleFlags{}, currentMissionId, lastCheckpointId`。

## 3. Checkpoint Snapshot

只保存重建当前关卡必要信息：
- 当前任务及脚本阶段。
- 已完成目标。
- 已触发不可重复事件。
- 玩家生命/护甲（按检查点最低值规则修复）。
- 当前武器、弹匣和合理备用弹药。
- 消耗品数量。
- Boss阶段（通常Boss前检查点不保存Boss中途阶段，特殊设计除外）。
- 已救援/死亡的重要NPC。

不保存：粒子、弹壳、普通尸体坐标、临时AI路径等表现状态。

## 4. 状态重建

加载检查点不是“反序列化整个世界”。流程：加载地图初始状态 → 应用 mission flags → 应用 checkpoint script state → 生成应存在实体 → 设置玩家 → 恢复UI/目标。

## 5. 幂等脚本

任务事件必须尽可能幂等。例如 `unlockDoor("door_a")` 重复执行仍保持解锁；`setObjectiveComplete(id)` 重复调用不重复给奖励。

一次性奖励必须通过稳定flag防止重载重复领取。

## 6. 任务脚本状态

推荐每个任务使用受控变量：
`phase, objectiveStates, eventFlags, counters, timers`。

不要把任意闭包、对象引用或Scene实例写进存档。

## 7. Trigger

标准 Trigger：
`onMissionStart, onEnterZone, onExitZone, onInteract, onEnemyKilled, onGroupCleared, onTimer, onObjectiveComplete, onItemCollected, onHealthThreshold, onBossPhase, onDialogueComplete`。

## 8. Action

标准 Action：
`spawn, despawn, lockDoor, unlockDoor, enableObject, disableObject, playDialogue, setObjective, completeObjective, failObjective, setCheckpoint, giveItem, giveCurrency, setFlag, cameraEvent, playVfx, playSfx, startTimer, stopTimer, startEncounter, completeMission`。

复杂剧情可由组合Action实现；真正特殊逻辑才写自定义script handler。

## 9. 顺序与队列

剧情动作支持 Sequence/Parallel：
- Sequence：锁门 → 对话 → 生成Boss → 解除镜头锁。
- Parallel：警报音 + 红灯 + 敌人运输车进入。

必须有超时/异常保护，避免某个资源或NPC缺失导致整个Sequence永不结束。

## 10. 任务失败

失败条件尽量明确：关键NPC死亡、目标对象被毁、逃生计时结束。普通玩家死亡走检查点，不把它混成任务脚本失败状态。

## 11. 存档写入

关键写入点：任务开始、检查点、任务完成、基地购买/升级、技能变化、重要剧情选择。不要每帧写LocalStorage。

写入建议：构建新对象 → 校验 → 写临时key → 再替换主key。保留最近一次有效备份。

## 12. Save Version Migration

每次破坏性结构变化增加版本：`v1 -> migrateV1ToV2 -> ...`。迁移必须纯函数化、可测试。无法迁移的字段使用安全默认值，并记录警告。

## 13. 防篡改范围

单机网页本地存档无需追求不可破解；重点是健壮。可加入校验和用于识别意外损坏，但不要把客户端秘密当作真正安全边界。

## 14. 验收场景

- 在每个检查点死亡重开。
- 刷新浏览器继续。
- 完成可选目标后重开。
- 领取唯一武器后重开，不能重复获得。
- Boss前退出再进入。
- 旧版本存档迁移。
- 人为删除一个非关键字段后可修复加载。
