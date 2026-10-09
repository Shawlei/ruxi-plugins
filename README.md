# 入戏 · 插件商店注册表

这是「入戏 · AI 角色扮演」（https://ruxi.covisuki.cn）内置插件商店的目录真相源。

- `plugin-store.json` —— 商店目录，客户端经 jsDelivr 拉取：
  `https://cdn.jsdelivr.net/gh/Shawlei/ruxi-plugins@main/plugin-store.json`
- 这里**只存插件的元数据与仓库地址**，不托管插件代码。插件代码仍在作者各自的公开仓库。

## 目录字段规范

`plugin-store.json` 顶层结构：

```json
{
  "schemaVersion": 1,
  "plugins": [
    {
      "id": "quick-reply",
      "name": "快捷回复",
      "description": "一键插入常用台词",
      "author": "你的名字",
      "repo": "https://github.com/你的用户名/quick-reply",
      "version": "1.0.0",
      "compat": "ok",
      "tags": ["效率", "台词"]
    }
  ]
}
```

| 字段 | 必填 | 说明 |
|------|------|------|
| `id` | 是 | 全局唯一标识，建议 `作者/插件名` 或短横线命名 |
| `name` | 是 | 商店里展示的名称 |
| `description` | 是 | 一句话简介，说明插件做什么 |
| `author` | 是 | 作者署名 |
| `repo` | 是 | 插件**公开** GitHub 仓库地址，`https://github.com/owner/repo` 或带 `tree/分支/子目录` 均可 |
| `version` | 是 | 插件最新版本号（如 `1.1.0`）。商店卡片展示；客户端每次打开/刷新都会拉取并与本地已装版本比对，不一致时显示「更新」按钮 |
| `compat` | 是 | 兼容性预估：`ok`（可用）/ `partial`（部分缺失）/ `unknown`（未验证）。安装后客户端仍会做真实体检，以实检为准 |
| `tags` | 否 | 分类标签，用于商店展示 |

约束：
- `repo` 必须指向**公开仓库**（入戏只能安装公开仓库）。
- 不收录含恶意行为（窃取本地数据、外发隐私、挖矿等）的插件；`compat` 请如实填写。

## 插件更新发在插件仓库，不发应用公告

- 插件升级时：先在自己插件仓库里更新代码并写 `CHANGELOG.md`，再回来**更新本仓库 `plugin-store.json` 里的 `version` 字段**。
- 客户端每次打开/刷新都会拉取目录、比对版本，检测到新版本即在已安装插件旁显示「更新」按钮，无需发应用公告、无需让主应用发版。

## 如何投稿（fork + PR，零注册）

1. **把插件推到自己的公开 GitHub 仓库**，确保有入口 `index.js`（或 `dist/index.js`）。
2. **Fork 本仓库**（`Shawlei/ruxi-plugins`）到你自己的账号。
3. 在你的 fork 里编辑 `plugin-store.json`，在 `plugins` 数组末尾**加一条**（保持合法 JSON，逗号正确）。
4. **提交并 push 到你的 fork**，然后**发起 Pull Request** 到 `Shawlei/ruxi-plugins` 的 `main` 分支。
5. 维护者审核合并后，插件即上架；客户端下次打开商店会自动出现，无需发版。

## 维护者审核清单

合并 PR 前，逐条确认：

- [ ] `plugin-store.json` 是合法 JSON（无逗号错误、无重复 `id`）
- [ ] `repo` 是公开仓库、地址可访问
- [ ] `name` / `description` / `author` 真实且无违规内容
- [ ] `compat` 预估合理（无法判断时填 `unknown`）
- [ ] 可选：点进作者仓库，瞄一眼 `index.js` / `manifest.json`，确认没有可疑外发（如 `fetch` 到陌生域名、读写 `document.cookie`）

合并后 jsDelivr 会自动跟随 `main` 更新，**无需任何部署操作**。
