# 14 — 网页游戏技术架构

## 1. 技术目标

目标平台：现代桌面浏览器优先，后续适配移动浏览器。要求 60 FPS 战斗、快速加载、可扩展内容、数据驱动、可测试、可存档。

推荐基础栈：
- TypeScript
- Vite
- Phaser 3（场景、输入、2D渲染、音频、基础物理/摄像机）
- 自定义战斗状态机与数据层
- JSON/TypeScript schema 驱动内容
- Vitest（逻辑测试）
- Playwright（关键流程E2E，可在后期加入）

原则：避免把核心战斗规则直接绑死在 Phaser Scene 中；游戏逻辑、数据、表现层尽量解耦。

## 2. 建议目录

```text
src/
  app/
    boot/
    config/
  game/
    scenes/
    entities/
      player/
      enemies/
      bosses/
      companions/
    combat/
      actions/
      hit/
      damage/
      status/
      weapons/
      projectiles/
    ai/
      perception/
      states/
      utility/
      navigation/
    world/
      level/
      encounters/
      interaction/
      destruction/
    progression/
    inventory/
    economy/
    narrative/
    save/
    audio/
    vfx/
    ui/
  data/
    weapons/
    enemies/
    bosses/
    skills/
    missions/
    loot/
  shared/
    math/
    events/
    types/
    debug/
assets/
  sprites/
  animations/
  weapons/
  tilesets/
  levels/
  audio/
  vfx/
```

## 3. 游戏循环

固定逻辑更新建议 60Hz；渲染跟随浏览器帧。关键战斗判定使用 delta-independent 逻辑，禁止基于“每帧移动固定像素”导致不同刷新率不同手感。

更新顺序：Input → Intent → State Machine → Movement/Physics → Hit Detection → Damage/Status → AI → Encounter → Camera/VFX → UI。

## 4. 实体架构

不要求完整 ECS，但采用组件化：
`Transform, Movement, Health, Armor, Poise, Hurtbox, ActionState, WeaponHolder, Inventory, Faction, AIController, StatusEffects, AnimationController`。

玩家、敌人和伙伴共享大量组件，差异通过控制器和数据配置形成。

## 5. 战斗系统

### Hitbox/Hurtbox
攻击动作在特定时间窗口激活 hitbox。每个 attackInstance 对同一 target 默认只命中一次，除非标记 `multiHit`。

### Damage Pipeline
`rawDamage -> difficulty modifier -> armor -> resistance -> critical/headshot -> status modifier -> finalDamage`。

同时独立计算：`poiseDamage, knockbackImpulse, launchImpulse, hitStop, hitReaction`。

### Friendly Fire
默认玩家子弹不伤伙伴；爆炸对伙伴只造成极低或零伤害（按模式配置）。敌人爆炸可伤敌人，以支持环境战术。

## 6. 武器系统

武器实例 = WeaponDefinition + RuntimeState。

Definition 为不可变数据；Runtime 保存：当前弹匣、热量、耐久/临时状态、已装配件、冷却。

射击模式：hitscan / projectile / pellet / beam / continuous / explosive。

## 7. 动画与逻辑

逻辑状态不依赖动画播放完成事件作为唯一真相。ActionDefinition 明确持续时间和阶段；动画负责表现。若帧率掉落，逻辑仍按时间推进。

## 8. 物理

人物移动建议自定义可控 Character Motor；环境刚体和死亡布娃娃使用引擎物理。避免正常人物完全依赖自由刚体，否则平台移动和近战手感不可控。

## 9. 摄像机

功能：跟随、前视偏移、瞄准偏移、战斗区域限制、Boss Arena 锁定、轻/中/重三档震屏、剧情镜头。

必须提供“减少屏幕震动”设置。

## 10. 关卡系统

关卡由地图 + MissionDefinition + EncounterDefinition 组合。关卡脚本只引用事件 ID，不把大量剧情逻辑硬写在地图对象中。

事件例：`enter_zone, enemy_wave_cleared, interact, timer, boss_hp_threshold, item_collected, dialogue_finished`。

动作例：`spawn_group, lock_door, unlock_door, play_dialogue, move_camera, explode_object, start_timer, set_checkpoint, complete_objective`。

## 11. 事件总线

建立强类型 GameEventBus，例如：
`ENTITY_DAMAGED, ENTITY_KILLED, WEAPON_FIRED, RELOAD_STARTED, ENCOUNTER_STARTED, ENCOUNTER_CLEARED, OBJECTIVE_UPDATED, CHECKPOINT_REACHED, BOSS_PHASE_CHANGED`。

系统通过事件通信，避免模块互相直接深层调用。

## 12. 存档

LocalStorage/IndexedDB 保存：设置、进度、技能、库存、章节、支线、收集品、检查点、统计。所有存档带 `saveVersion`，提供迁移函数。

重要：不要信任反序列化数据。读取后进行 schema 校验和默认值修复。

## 13. 加载与资源

按章节/地图分包加载；主菜单不需要加载所有 100 把武器的全部音频和所有地图。建立 AssetManifest 并引用资源 key。

图片优先 texture atlas；大背景按区域拆分；音效使用压缩格式并区分短SFX和长音乐。

## 14. 性能预算

桌面目标：60 FPS。
- 同屏普通敌人建议 12–20，特殊波次最多约 25，依据性能测试调整。
- 活跃弹道采用对象池。
- 弹壳、血花、火花全部对象池或批量系统。
- 屏幕外 AI 降频。
- 禁止每帧分配大量临时数组/对象。
- 粒子上限和尸体上限可配置。

## 15. 输入

Input abstraction 输出 `Move, Aim, Jump, Dodge, AttackPrimary, AttackSecondary, Melee, Reload, Interact, Throwable, Weapon1/2/3, Pause`。

键鼠、手柄、移动触摸映射到同一动作接口。支持重绑定。

## 16. 可访问性

至少：字幕、字幕背景、屏幕震动强度、闪光强度、色觉友好标记、按住/切换瞄准、输入重绑定、音量分类、难度选择。

## 17. 调试模式

开发版提供：无敌、无限弹药、传送关卡、生成敌人、生成武器、显示碰撞箱、AI状态、伤害数字、Encounter预算、FPS/对象数、快速Boss阶段切换。

## 18. 架构验收

新增武器不应修改 Player 核心类；新增普通敌人不应修改主游戏循环；新增任务主要通过数据和少量脚本事件实现；所有数据加载失败给出明确错误路径和字段；核心伤害/技能/掉落逻辑可以在无渲染环境进行单元测试。
