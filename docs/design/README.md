# Design Documentation — Source of Truth

本目录是火柴人横版动作网页游戏的开发设计源（source of truth）。开发代理和人工开发者在实现系统前，应先阅读与任务相关的文档；系统行为与本文档冲突时，应明确更新文档或说明偏差，禁止默默产生第二套规则。

## 推荐阅读顺序

### A. 总体与已有核心文档
1. `01-GAME-DESIGN-DOCUMENT.md` — 产品愿景、核心循环、范围
2. `02-COMBAT-AND-ACTIONS.md` — 移动、近战、防御、受击
3. `03-WEAPONS-AND-ITEMS.md` — 武器/道具基础设计
4. `04-STORY-AND-LEVELS.md` — 世界与章节基础结构
5. `05-ENEMIES-BOSSES-AI.md` — 敌人/Boss/AI基础原则

### B. 完整生产规格
6. `06-CHARACTER-ANIMATION-SPEC.md` — 统一骨架、100+动作和动画状态规范
7. `07-WEAPON-CATALOG.md` — 正式版100件武器目录与差异化规则
8. `08-ITEMS-LOOT-ECONOMY.md` — 消耗品、装备、掉落、经济
9. `09-PROGRESSION-SKILLS.md` — 四基础技能树 + RED能力树
10. `10-CAMPAIGN-MISSION-BIBLE.md` — 8章48个主线任务
11. `11-ENEMY-ROSTER.md` — 40类敌人与遭遇角色
12. `12-BOSS-BIBLE.md` — 8章Boss + 隐藏/小Boss
13. `13-AI-STATE-MACHINES.md` — 感知、状态机、掩体、战斗槽、群体协作
14. `14-TECHNICAL-ARCHITECTURE.md` — TypeScript/Phaser网页端工程架构
15. `15-DATA-SCHEMAS.md` — 武器、敌人、任务、遭遇、技能等数据契约
16. `16-LEVEL-ENCOUNTER-RULES.md` — 关卡、空间、波次、预算、检查点
17. `17-ART-VFX-AUDIO-UI.md` — 美术、VFX、音频、HUD/UI资产规范
18. `18-QA-BALANCE-ACCEPTANCE.md` — 测试、数值、性能与发布Gate
19. `19-DEVELOPMENT-ROADMAP.md` — 从工程基础到1.0的阶段路线
20. `20-AI-DEVELOPMENT-INSTRUCTIONS.md` — AI代码代理最高层执行规则
21. `21-STORY-WORLD-CHARACTERS.md` — 世界观、角色、阵营、结局与叙事
22. `22-COMPANIONS-SIDE-QUESTS.md` — 6伙伴、基地与20–30支线框架
23. `23-GAME-MODES-REPLAYABILITY.md` — 生存、防守、Boss Rush、NG+、随机行动
24. `24-SETTINGS-ACCESSIBILITY-LOCALIZATION.md` — 设置、无障碍、本地化

## 可直接使用的数据样例

`examples/weapon.example.json` — 枪械定义样例  
`examples/enemy.example.json` — 敌人定义样例  
`examples/encounter.example.json` — 遭遇波次样例  
`examples/mission.example.json` — 任务定义样例

## 关键设计原则

1. 先证明战斗好玩，再批量生产内容。
2. 武器差异来自行为、节奏和用途，而非仅伤害数字。
3. 敌人差异来自行为、战术角色和反制方式，而非血量。
4. 环境必须参与战斗：破坏、爆炸、掩体、机关、垂直空间。
5. 火柴人视觉保持可读，深度由动作、物理、枪械、VFX和声音承担。
6. 系统必须数据驱动，新增武器/敌人/任务不应重写核心逻辑。
7. 所有最终角色、剧情、美术、音频和关卡必须原创，不直接复制参考游戏受版权保护内容。
8. `main` 应始终保持可运行；“已完成”必须意味着实现、测试、构建和验收都成立。

## 正式版目标

- 8章 / 48主线关
- 20–30支线
- 约100件武器
- 100+玩家/人形动作
- 40类敌人及精英变体
- 8章节Boss + 6–10小/隐藏Boss
- 6名伙伴
- 12+场景主题
- 完整技能、经济、存档、基地和结局系统
- 生存 / 防守 / Boss Rush / 挑战 / NG+，随机行动作为后期扩展
- 主线8–12小时；完整探索15–25小时

## 当前正确开发起点

不要直接生产48关。严格按 `19-DEVELOPMENT-ROADMAP.md`：
`工程基础 -> Combat Sandbox -> Vertical Slice -> Core Systems -> Content Production -> Alpha -> Beta -> RC`。

Vertical Slice未通过战斗手感与稳定性Gate前，不批准大规模内容生产。
