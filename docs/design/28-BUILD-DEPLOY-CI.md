# 28 — 构建、部署与 CI 规范

## 1. 目标

任何开发者或代码代理应能通过少量标准命令完成：安装依赖、启动开发服务器、类型检查、测试、生产构建和本地预览。

建议脚本：
- `npm run dev`
- `npm run typecheck`
- `npm run test`
- `npm run test:watch`
- `npm run build`
- `npm run preview`
- `npm run lint`

若项目最终选择其他包管理器，命令语义保持一致。

## 2. 环境

客户端单机版本原则上不需要敏感环境变量。若未来加入API/云存档，`.env.example` 只包含变量名称与说明，不提交真实密钥。

## 3. CI 最低 Gate

每个PR/主分支提交执行：
1. install with lockfile
2. typecheck
3. lint
4. unit tests
5. production build

后期增加：E2E smoke、bundle size检查、资产引用校验、数据schema校验。

## 4. 锁文件

必须提交 lockfile，保证依赖可复现。禁止在同一项目同时维护多个包管理器lockfile。

## 5. Production Build

生产构建：关闭开发debug入口、保留必要错误日志、压缩代码与资产、生成内容hash、输出静态站点文件。

不要在构建时把大型未使用开发素材复制进dist。

## 6. 部署模型

1.0 优先静态托管。前端路由若使用SPA history模式，服务器需正确回退；更简单方案可保持单入口游戏页面和hash/内部场景导航。

可部署至支持静态站点的平台。部署与具体厂商解耦。

## 7. 缓存

版本化静态资源使用长期缓存；入口HTML和manifest使用较短缓存或明确更新策略。发布新版本不能让旧JS引用不存在的新资源。

## 8. Asset Manifest

构建前校验：所有definition引用的纹理、动画、音频key存在；未引用的大型资源报告但不一定立即失败；关键资源缺失直接失败。

## 9. Bundle/加载策略

首屏只加载Boot、菜单、基础UI和必要字体。进入章节时再加载章节地图与敌人资产。公共武器/角色资源按共享包管理。

## 10. 错误处理

加载失败应出现可理解错误/重试界面，而不是黑屏。生产错误记录至少包含版本、场景、资源key或数据ID等非敏感调试信息。

## 11. 浏览器兼容

正式发布前至少覆盖当前主流 Chromium、Firefox、Safari 桌面版本。输入、WebAudio、全屏、LocalStorage/IndexedDB、Canvas/WebGL能力均需验证。

## 12. 发布流程

`main green -> version bump -> changelog -> production build -> smoke test -> deploy -> post-deploy smoke -> tag release`。

## 13. 回滚

保留上一个可部署版本。若新版本出现存档损坏或主线阻断，应能立即回滚静态构建。存档迁移必须尽量向前兼容或在升级前备份。

## 14. 版本信息

游戏设置/主菜单底部显示版本号和构建标识，便于Bug报告。

## 15. CI 验收

- 任一数据引用错误能在CI中被发现。
- TypeScript错误无法进入发布构建。
- 测试失败阻止合并/发布。
- 生产构建从全新环境可重复生成。
