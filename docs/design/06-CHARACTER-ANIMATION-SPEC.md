# 06 — 角色、动作与动画制作规范

> 本文是角色动画、战斗状态机和动作资源的制作基准。所有玩家、普通人形敌人和可招募伙伴优先共享同一套标准骨架与动作标签。

## 1. 统一骨架

基础骨架节点：`root / pelvis / spine_01 / spine_02 / neck / head / shoulder_L/R / upperArm_L/R / foreArm_L/R / hand_L/R / thigh_L/R / calf_L/R / foot_L/R`。

附加挂点：`weapon_main`、`weapon_off`、`muzzle`、`back_slot`、`hip_slot`、`throw_origin`、`fx_chest`、`fx_head`。

角色碰撞与动画骨架分离：逻辑碰撞体由 `bodyCapsule + footSensor + hurtboxes + attackHitboxes` 组成，骨骼只用于视觉与武器挂点。

## 2. 动作状态优先级

从高到低：
1. 死亡/剧情锁定
2. 处决/被处决
3. 抓取/投掷
4. 击飞/倒地/起身
5. 强制受击
6. 闪避/翻滚
7. 攻击/射击/换弹
8. 互动
9. 跳跃/落地
10. 奔跑/行走/待机

禁止低优先级动作打断高优先级动作。每个动作必须声明 `interruptibleFrom` 和 `cancelWindow`。

## 3. 玩家动作清单

### 3.1 移动 24 项
`idle_relaxed`、`idle_combat`、`idle_injured`、`walk_fwd`、`walk_back`、`run`、`sprint`、`turn_180`、`start_run`、`stop_run`、`crouch_enter`、`crouch_idle`、`crouch_walk`、`crouch_exit`、`jump_start`、`jump_rise`、`jump_apex`、`fall`、`land_light`、`land_heavy`、`roll_fwd`、`roll_back`、`dash_fwd`、`slide`。

### 3.2 环境 16 项
`vault_low`、`vault_high`、`climb_ladder_up/down`、`ledge_grab`、`ledge_climb`、`drop_down`、`door_open`、`door_kick`、`pickup_ground`、`pickup_table`、`push_object`、`pull_object`、`activate_panel`、`enter_vehicle`、`exit_vehicle`。

### 3.3 徒手 30 项
轻拳链 4 段、重拳 3 种、轻踢链 3 段、重踢 3 种、冲刺拳、冲刺踢、下蹲拳、扫腿、跳拳、飞踢、上勾拳、下砸、背击、抓取起手、前投、后投、过肩摔、倒地追击、墙边重击、空中追击、终结技 A/B/C。

### 3.4 防御与受击 24 项
`block_high/low`、`perfect_block`、`parry`、`dodge_short`、`hurt_light_front/back`、`hurt_heavy_front/back`、`hurt_head`、`hurt_leg`、`stagger`、`guard_break`、`knockback`、`launch_up`、`air_hit`、`wall_hit`、`ground_bounce`、`downed_faceup/facedown`、`getup_fast/slow`、`death_front/back/explosion`。

### 3.5 枪械动作
每一类枪共享一套动作组：`equip`、`unequip`、`aim_idle`、`aim_walk`、`fire`、`fire_crouch`、`reload_tactical`、`reload_empty`、`melee_bash`、`jam_clear`。

特殊：
- 泵动霰弹枪：`pump_cycle`、逐发装填、可中断装填。
- 左轮：开轮、逐发/快速装弹、甩轮归位。
- 狙击枪：开镜、退镜、拉栓。
- 轻机枪：较慢举枪、重型换弹。
- 火箭筒：肩扛、尾焰后坐、重新装填。

## 4. 动作参数

每个攻击动作必须具备：
- `startupMs` 前摇
- `activeMs` 生效帧
- `recoveryMs` 后摇
- `moveDistance`
- `turnLock`
- `staminaCost`
- `hitStopMs`
- `cameraShake`
- `damageMultiplier`
- `poiseDamage`
- `knockback`
- `launchY`
- `cancelWindow`
- `comboNext[]`

## 5. 受击与布娃娃

普通受击保持关键帧动画；当满足以下条件之一切换 Ragdoll：死亡、爆炸冲量、车辆撞击、从高处坠落、Boss 巨型击飞、脚下地形坍塌。

Ragdoll 结束后若角色仍存活，应执行：物理速度衰减 → 姿态判断 → 对齐地面 → 播放对应起身动画。不可瞬间弹回站立。

## 6. 动画混合

上半身枪械瞄准允许和下半身行走/跑步混合。腰部以上使用 Aim Layer；持枪角度由鼠标方向驱动，限制肩关节角度，超过阈值时角色整体翻向。

## 7. 动作可读性

敌人危险攻击至少需要 180–450ms 的明确前摇。Boss 主要招式必须通过姿势、音效或特效给出读招信号，禁止无提示瞬发高伤害。

## 8. 验收标准

- 玩家在 60 FPS 下动作切换不得明显抽搐。
- 连击输入缓存建议 120–180ms。
- 轻攻击命中反馈即时；重击必须显著区别于轻击。
- 跑动射击、蹲射、空中受击、倒地起身均不能造成逻辑状态卡死。
- 同骨架敌人能够复用至少 70% 的基础动画资源。
