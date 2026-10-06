# web-search-panel 交接文档

> **交接日期**: 2026-10-07
> **插件版本**: 1.4.0
> **适配 DSH 版本**: 0.1.7-rc.1（当前）→ 0.2.0-rc.2（升级目标，已金标准验证兼容）
> **源码位置**: `C:\Users\lcl\Desktop\DSH插件开发\dsh-web-search`
> **npm**: https://www.npmjs.com/package/web-search-panel
> **GitHub**: https://github.com/new-256/dsh-web-search

---

## 一、这个插件是什么

DSH 的网页搜索增强插件。为模型提供网页搜索与抓取能力：

| 工具/能力 | 作用 |
|---|---|
| `web_search_multi` | 39 引擎/渠道 · 8 大类，`engine="auto"` 智能路由（分析查询特性→选引擎或多引擎并行 RRF 融合） |
| `web_fetch_url` | 抓取指定公开 URL（HTML 自动转 markdown） |
| 插件页配置卡片 | 在设置面板可视化配置引擎/渠道 |

支持显式指定引擎（学术 arxiv/openalex/crossref、新闻、github/npm、bilibili/youtube、中文问答等）。

---

## 二、运行/加载机制

1. profile `package.json` → `dsh.profile.bundles` 含 `"web-search-panel"`
2. 入口（`main`）：`lib/index.mjs`
3. 经 `cordis.patch.yml`（`dsh.bundle.patch`）注册，随 DSH 启动加载
4. 纯 Host 侧工具（自持 fetch），无浏览器 UI 依赖

---

## 三、0.2.0 兼容性（已验证）

| 检查项 | 结论 |
|---|---|
| peerDependencies | **无声明** → 0.2.0 强制校验直接通过，**无需 version-exemption** |
| 金标准验证 | 隔离环境 0.2.0-rc.2 安装 + 官方兼容性评估实跑：**PASS**（见主交接文档 §6） |
| API 使用 | 自持 fetch + 原生工具注册，与 DSH 内部 API 解耦，低风险 |

---

## 四、构建 / 测试 / 发布

- 无编译步骤（`lib/index.mjs` 直接运行）
- **发布流程**：
  ```bash
  # 改代码 → bump version（CHANGELOG 如有）→ git commit + tag
  git tag vX.Y.Z && git push --tags
  npm publish --registry=https://registry.npmjs.org
  ```
- npm 账号开 2FA：用勾选 "Bypass 2FA" 的 Granular Token 发布，或加 `--otp=xxxxxx`

---

## 五、接手注意事项

1. 坚持"零 @deepseek-ai 运行时依赖"纪律，勿引入对 DSH 内部包的硬依赖。
2. 部分引擎渠道需要 API key（serper/tavily/brave-api/exa），在插件页配置；无 key 时走匿名/key-free 渠道。
3. 引擎路由逻辑集中在 lib 内，新增引擎改路由表即可。
4. 相关文档：`README.md`。
