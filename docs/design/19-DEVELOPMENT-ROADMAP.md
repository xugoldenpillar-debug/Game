# 19 — 开发路线图与里程碑

## 1. 总原则

先证明“好玩”，再扩内容。严禁一开始就制作48关和100把武器。所有阶段都以可运行版本为产物。

## Phase 0 — 工程基础

目标：建立可持续开发骨架。

交付：
- TypeScript/Vite/Phaser 工程。
- Boot/Preload/Game 场景。
- 输入抽象。
- GameEventBus。
- 基础实体组件。
- 数据加载与校验。
- Debug overlay。
- CI 基础测试。

完成条件：开发环境一条命令启动；无渲染核心测试可运行；数据错误有清晰提示。

## Phase 1 — Combat Sandbox

交付：
- 玩家移动、跳跃、翻滚、蹲伏。
- 徒手轻/重攻击。
- Hitbox/Hurtbox。
- HP/Armor/Poise。
- 受击、击退、倒地、Ragdoll。
- 手枪、SMG、霰弹枪。
- 换弹、弹药、切枪。
- 训练假人和基础暴徒。

Gate：单个训练场反复战斗 15 分钟仍有良好操作反馈，无明显状态机Bug。

## Phase 2 — Vertical Slice

内容：第一章代表性完整关卡。

交付：
- 3 场景区域。
- 5 种敌人。
- 1 Boss。
- 6–8 武器。
- 手雷/医疗包。
- 环境爆炸与可破坏物。
- AI感知/掩体/近战槽。
- HUD。
- 任务目标。
- 检查点/死亡重开。
- 简化存档。
- 基础音乐和音效。

Gate：外部试玩者可以无开发者解释从开始玩到Boss并完成；无阻断问题；战斗反馈达到项目目标。

## Phase 3 — Core Systems

交付：
- 完整武器框架。
- 配件/升级。
- 技能树。
- 装备/库存。
- 商店/经济。
- 状态效果。
- 伙伴基础。
- 支线系统。
- 对话系统。
- 完整存档版本化。
- 关卡脚本事件框架。

Gate：新增普通武器/敌人/任务主要靠数据配置。

## Phase 4 — Content Production A

制作第1–4章：24主线关。

目标内容：
- 50左右武器。
- 20+敌人。
- 4章Boss。
- 8–12支线。
- 6个场景主题。

同时进行性能、资产管线和数值调整。

## Phase 5 — Content Production B

制作第5–8章：剩余24主线。

加入：
- RED能力。
- 伙伴完整剧情。
- 重武器/机甲/载具事件。
- 剩余武器达到约100件。
- 40类敌人/变体。
- 最终Boss与结局系统。

## Phase 6 — Alpha

所有主线从头到尾可通。

工作重点：
- 软锁/存档Bug。
- 关卡节奏。
- 缺失资源。
- AI异常。
- 任务逻辑。
- 性能。

不再大规模新增系统。

## Phase 7 — Beta / Content Lock

全部主要内容锁定。

重点：
- 全武器平衡。
- 各难度。
- 支线与收集品。
- 音效/音乐。
- VFX。
- UI/UX。
- 浏览器兼容。
- 可访问性。

## Phase 8 — Release Candidate

执行多轮完整通关测试；修复 blocker/critical；验证存档升级；构建生产版本；压缩资源；建立部署与回滚流程。

## 2. 推荐开发顺序

最小依赖顺序：
`input -> movement -> action state -> hit/damage -> weapon -> enemy -> AI -> encounter -> mission -> save -> progression -> content`。

不要先做：复杂基地动画、几十种菜单皮肤、大量剧情插画、100武器美术。它们都依赖核心战斗已成立。

## 3. 每个迭代必须包含

- 一个玩家可见改进。
- 自动或手工回归测试。
- 文档/数据同步。
- 不留无法解释的临时硬编码。

## 4. Issue 标签建议

`type:feature, type:bug, type:content, type:tech, type:balance, area:combat, area:ai, area:level, area:ui, area:audio, area:save, priority:blocker/high/medium/low, milestone:vertical-slice/alpha/beta/release`。

## 5. 分支策略

`main` 始终保持可运行。较大功能使用短期 feature branch + PR；小型低风险内容可直接提交，但仍要求测试通过。不要建立长期巨型分支。

## 6. 版本规范

开发阶段：`0.x.y`。例如 Vertical Slice 可标记 `0.1.0`，Alpha `0.5.0`，Beta `0.8.0`，RC `0.9.x`，正式 `1.0.0`。

## 7. 成功标准

项目真正成功的判据：核心战斗有辨识度；48关不是重复走廊；武器之间存在策略差异；AI能形成组合压力；网页端稳定流畅；玩家可从新游戏连续通关且存档可靠。
