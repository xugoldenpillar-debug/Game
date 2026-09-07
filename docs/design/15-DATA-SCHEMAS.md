# 15 — 数据驱动内容规范

## 1. 原则

所有可批量扩展内容都使用定义数据，不把数值散落在代码中。开发期可用 TypeScript 常量 + JSON；正式内容应有运行时校验。

数据 ID 一旦进入存档或发布版本，不随显示名称改变。

命名建议：`wpn_rifle_carbine_a`、`enemy_nova_rifleman`、`boss_red07`、`mission_c03_m04`、`skill_gun_fast_reload`。

## 2. WeaponDefinition

```ts
interface WeaponDefinition {
  id: string;
  displayName: string;
  category: 'melee'|'pistol'|'smg'|'rifle'|'shotgun'|'sniper'|'lmg'|'heavy'|'special';
  rarity: 'common'|'uncommon'|'rare'|'epic'|'legendary';
  fireMode?: 'semi'|'burst'|'auto'|'pump'|'bolt'|'continuous';
  damage: number;
  pellets?: number;
  fireRate?: number;
  magSize?: number;
  reserveMax?: number;
  reloadType?: 'mag'|'perRound'|'single'|'energy';
  reloadTime?: number;
  accuracyHip?: number;
  accuracyAim?: number;
  recoilX?: number;
  recoilY?: number;
  range: number;
  falloffStart?: number;
  falloffEnd?: number;
  headshotMultiplier?: number;
  armorPenetration: number;
  knockback: number;
  poiseDamage: number;
  ammoType?: string;
  equipTime: number;
  moveSpeedMultiplier: number;
  projectile?: ProjectileDefinition;
  attachmentSlots: string[];
  tags: string[];
  effects: EffectRef[];
  animationSet: string;
  soundSet: string;
}
```

## 3. EnemyDefinition

```ts
interface EnemyDefinition {
  id: string;
  displayName: string;
  faction: string;
  role: 'frontline'|'pressure'|'disruptor'|'support'|'priority';
  maxHp: number;
  armor: number;
  poise: number;
  moveSpeed: number;
  runSpeed: number;
  sightRange: number;
  hearingScale: number;
  preferredRange: [number, number];
  weaponLoadout: WeightedRef[];
  abilities: string[];
  resistances: Record<string, number>;
  aiProfile: string;
  lootTable: string;
  animationSet: string;
  tags: string[];
}
```

## 4. BossDefinition

在 EnemyDefinition 基础上增加：`phases, phaseThresholds, weakPoints, arenaEvents, moveSetByPhase, dialogueTriggers, uniqueRewards`。

## 5. SkillDefinition

字段：`id, tree, tier, maxRank, prerequisites[], levelRequired, currency, cost, description, modifiers[], unlockActions[]`。

Modifier 不直接写任意 JS 代码，使用受控类型：`stat_add, stat_mul, action_unlock, tag_add, conditional_bonus, resource_cap, cooldown_mul`。

## 6. MissionDefinition

字段：
`id, chapter, index, title, mapId, durationTarget, briefing, objectives[], optionalObjectives[], checkpoints[], encounterRefs[], scriptRefs[], dialogueRefs[], lootOverrides[], failConditions[], completionRewards[], unlocks[]`。

Objective 类型：`reach, kill, survive, defend, escort, interact, collect, destroy, boss, timer, stealth, escape`。

## 7. EncounterDefinition

```ts
interface EncounterDefinition {
  id: string;
  budget: number;
  spawnZones: string[];
  waves: EncounterWave[];
  completion: 'killAll'|'timer'|'objective'|'script';
  lockArena: boolean;
  reward?: string;
}

interface EncounterWave {
  trigger: TriggerDefinition;
  groups: SpawnGroup[];
  delayMs?: number;
  maxAlive?: number;
}
```

SpawnGroup：`enemyId, count, eliteChance, zoneTags, formation, delayBetweenSpawns`。

## 8. LootTable

支持固定项、加权随机、保底和条件：
`entries[{ref, weight, min, max, condition}], guaranteed[], pityRules[]`。

## 9. Dialogue

字段：`conversationId, nodes[]`。每个节点：`speaker, textKey, voiceKey?, portrait?, choices?, next?, conditions?, actions?`。

文本必须使用本地化 key，而不是把所有最终文案直接散落逻辑文件。

## 10. StatusEffect

标准状态：`burn, bleed, shock, stun, slow, freeze, suppression, armorBreak, vulnerability, overdrive`。

字段：`id, duration, stackMode, maxStacks, tickRate, damagePerTick, statModifiers, visuals, immunitiesTags`。

## 11. 数据校验

启动时和 CI 中校验：
- ID 唯一。
- 引用存在。
- 数值范围合法。
- 必填资源 key 存在。
- 任务目标不会引用不存在的 encounter/zone。
- 技能依赖不形成循环。
- 掉落表不会出现非法权重。

## 12. 版本管理

数据文件包含可选 `schemaVersion`。发布后的存档只保存稳定ID和必要runtime状态，避免保存完整definition快照。

## 13. 内容工具目标

后期可制作内部简单编辑器：武器表、敌人表、关卡遭遇表。第一阶段不必做大型可视化工具，但必须从一开始保持数据结构清晰，避免后期迁移成本。
