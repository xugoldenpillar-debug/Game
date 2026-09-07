# 13 — AI 状态机、战术决策与群体协作

## 1. AI 分层

AI 分为三层：
1. 感知层 Perception：视觉、听觉、伤害来源、队友警报。
2. 决策层 Decision：根据角色类型、距离、危险和冷却选择意图。
3. 执行层 Action：移动、瞄准、攻击、换弹、躲避、互动。

不要在动画回调中硬编码战术逻辑；行为应由可测试的状态/效用系统驱动。

## 2. 基础状态

`Idle / Patrol / Suspicious / Investigate / Search / Combat / Reposition / TakeCover / Advance / Retreat / AttackMelee / AttackRanged / Reload / Dodge / ThrowGrenade / SupportAlly / CallReinforcement / Stunned / KnockedDown / Flee / Dead`。

## 3. 黑板 Blackboard

每个 AI 保存：
`targetId, targetLastSeenPos, targetLastSeenTime, targetVelocity, currentCover, reservedCombatSlot, ammoInMag, hpRatio, armorRatio, nearbyAllies[], nearbyThreats[], heardNoisePos, alertLevel, actionCooldowns{}, suppression, morale, role`。

## 4. 感知

视觉检查频率可按距离降频。近距离高频，屏幕外远距离低频。视觉条件：距离、视角、射线遮挡、烟雾/黑暗修正、玩家姿态。听觉事件：枪声、爆炸、奔跑、玻璃破碎、呼叫、近战撞击。

声音只传递事件位置与强度，不直接泄露玩家精确位置。

## 5. 战术位置

近战敌人围绕玩家建立 `combat slots`，例如左近、右近、左中、右中。只有获得近战攻击槽的单位允许真正贴身发动攻击，其他近战单位保持移动、威吓或寻找侧翼，防止所有敌人堆在同一点。

远程敌人使用 Cover Nodes：`coverId, position, height, leftPeek, rightPeek, quality, occupiedBy`。选择掩体时综合距离、视线、玩家朝向、危险物、是否已被占用。

## 6. 角色效用示例

步枪兵：
- 有好掩体且玩家中远距离 → TakeCover 权重高。
- 玩家正在换弹且距离合适 → Advance。
- 玩家贴脸 → Retreat 或枪托攻击。

霰弹兵：
- 远距离 → Advance/Flank。
- 近距离且有弹 → Fire 高权重。
- 玩家持重武器正面压制 → Dodge/Flank。

医疗兵：
- 附近关键盟友低血 → SupportAlly。
- 自身受压 → Retreat。
- 无需治疗 → 使用手枪支援但保持后排。

## 7. 压制值 Suppression

玩家持续朝敌人附近射击会提高 suppression。高 suppression AI 倾向保持掩体、减少探头、寻找换位；精英单位受影响较小。这样即使命中率不高，机枪仍具有战术价值。

## 8. 士气 Morale

可选系统：队长死亡、多人短时间被击杀、爆炸造成大规模伤亡时降低士气。低士气普通人类敌人可能后撤或短暂迟疑，但 NOVA 精英、感染者和机器人不使用普通士气。

## 9. 手雷逻辑

AI 投雷必须满足：
- 目标在有效范围。
- 预测落点不会严重伤害队友。
- 玩家长时间固守掩体或多人聚集。
- 冷却未结束。

手雷投出后给玩家明确声音与可见警告，不允许离屏无提示秒杀。

## 10. 增援

只有特定关卡节点和特定职业可呼叫援军。呼叫动作可被玩家打断。增援数量受 Encounter Budget 限制，避免无限刷兵。

## 11. Boss AI

Boss 使用阶段状态机 + 招式效用选择。禁止直接读取玩家当前按键。可读取合法信息：距离、玩家最近行为统计、玩家血量范围、Boss自身状态、场地状态。

## 12. 性能

屏幕内完整 AI：60Hz 或 30Hz 决策更新；远离玩家单位：5–10Hz；完全不可见且不参与事件的单位可休眠。寻路和视线检测必须分帧/限频，禁止所有敌人每帧全量寻路。

## 13. 调试工具

开发模式必须可显示：
- 当前 AI state。
- 目标与最后已知位置。
- 视觉锥。
- 掩体节点。
- 战斗槽。
- 当前行为效用分数。
- 路径。
- suppression/morale。

## 14. 验收

- 同类敌人不会所有人同一帧同时攻击。
- AI 能绕过简单障碍，不反复卡角。
- 玩家脱离视线后 AI 搜索最后位置，而不是持续锁墙。
- AI 不会把手雷稳定扔到队友脚下。
- 远程敌人会保持合理距离；近战敌人能有效接近。
