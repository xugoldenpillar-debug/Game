# 26 — 枪械配件、伤害类型与状态效果

## 1. 配件目标

配件用于定制武器手感，不应该把所有枪最终改造成同一种“无后坐高伤害”武器。每个配件必须有定位，强力配件允许伴随代价。

## 2. 配件槽

标准槽：`optic / muzzle / magazine / underbarrel / stock / ammo / special`。并非所有武器拥有全部槽位。

## 3. Optic

- Reflex：瞄准速度快，轻微提升瞄准精度。
- Holo：中距离稳定。
- 2x：中距离精确，ADS略慢。
- 4x：步枪/DMR远距，近距离ADS惩罚。
- Sniper Scope：狙击专用，大倍率。

## 4. Muzzle

- Compensator：垂直后坐降低，噪声略增。
- Brake：大幅降低首发上跳，横向后坐略增。
- Suppressor：降低噪声/枪口焰，射程或伤害衰减略提前。
- Flash Hider：降低枪口视觉遮挡，无显著伤害增益。

## 5. Magazine

- Extended：容量+20–40%，换弹稍慢。
- Fast Mag：换弹加快，容量不变。
- Drum：容量显著提高，举枪/移动/换弹变慢。

## 6. Underbarrel/Stock

- Vertical Grip：持续射击垂直后坐低。
- Angled Grip：ADS与首发控制改善。
- Light Stock：移动/切枪快但后坐高。
- Heavy Stock：稳定但移动慢。

## 7. 弹药配件

- AP：提高穿甲，软目标伤害略降。
- Hollow Point：无甲伤害高，穿甲低。
- Incendiary：积累燃烧，基础伤害略降。
- Shock：积累电击，对机械更有效。
- Slug：霰弹枪单弹头，提高射程、降低近距离群体能力。

特殊弹药必须受资源/库存限制，不作为永远最优选项。

## 8. 伤害类型

`ballistic / slash / blunt / explosion / fire / electric / toxic / energy`。

Armor 对 ballistic/slash 有效；Poise 更受 blunt/explosion 影响；机器人对 electric 更敏感；感染者对部分 toxic 免疫；具体数值由definition决定。

## 9. 状态效果

### Burn
持续伤害，可刷新持续时间但限制叠层。燃烧单位表现明显；部分人类AI可能短时恐慌。

### Bleed
由刀具/特定弹药触发，持续中低伤害；机器人免疫。

### Shock
积累条满后短暂硬直；机械单位持续时间更长。Boss有抗性并触发递减，禁止电到无限动不了。

### Stun
强控制，来源稀少。Boss/精英拥有高抗性。

### Slow
降低移动/攻击部分速度；多次叠加有上限。

### Freeze
仅特殊武器/敌人使用。普通敌人可被冻结，Boss只产生较弱减速/暴露效果。

### Armor Break
临时降低护甲或暴露弱点。

### Suppression
AI行为状态，不直接造成伤害。

### Vulnerability
特定Boss机制使用，受到伤害提高，持续很短。

## 10. 状态积累

强控制状态建议使用 buildup：每次命中累积值，随时间衰减，到阈值触发。这样避免每颗电击子弹固定麻痹。

字段：`buildupPerHit, buildupDecayPerSecond, threshold, duration, immunityWindowAfterTrigger`。

## 11. 抗性递减

Boss同一控制状态连续触发后阈值暂时提高。例如第一次Shock阈值100，短期第二次150，第三次225，脱战/长时间后恢复。

## 12. 配件品质

配件不使用5档纯数值升级链。多数配件只有一种标准版本；稀有配件提供特殊取舍，例如“高压制枪口：后坐降低但移动精度下降”。

## 13. 验收

- 配件装卸立即反映在数据和UI。
- 存档保存配件ID而非复制整个定义。
- 武器不可安装不支持的槽位/配件。
- 状态视觉和音效清晰。
- 所有控制状态对Boss有防无限控制措施。
