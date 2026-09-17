# ArcReel 官方市场源

[English](README.en.md)

这是 [ArcReel](https://github.com/ArcReel/ArcReel) 的官方**市场源**：一个存放可安装条目的 GitHub 仓库。首期唯一的条目类型是**调用端点**，也就是自定义调用端点的定义文件。ArcReel 预置了本市场源，在设置页的「市场」里就能浏览和安装。

本仓库同时是 GitHub 模板仓：fork 或 **Use this template** 之后，你就有了一个合规的第三方市场源。

本仓库不包含 ArcReel 随版内置的调用端点定义。内置端点的键和市场条目的 slug 是两套独立的命名。

## 浏览与安装

1. 打开 ArcReel 设置页的「市场」，官方市场源已预置并启用。
2. 选中条目后先看安装确认页：`auth` 节原文、凭证发往的 `submit.url` / `poll.url`，以及 `hints`。确认无误再安装。
3. 安装后的端点会出现在「调用端点」列表里，可以原地编辑。上游发布新版本后，端点显示「可更新」。

市场里的定义只描述如何调用某个接口，不带任何凭证。API Key 在你创建供应商时填写，只保存在你自己的 ArcReel 实例里。

## 仓库结构

```text
arcreel-market.json              # 索引，由 CI 从 endpoints/ 生成，不要手改
endpoints/
  <slug>/
    definition.json              # 定义文件，条目元数据只写在它的 meta 里
    icon.png                     # 可选：PNG / WebP / SVG，正方形，不超过 64 KB
```

- 目录名就是 slug，规则 `^[a-z0-9][a-z0-9-]{0,63}$`，在本源内唯一。
- 索引条目是定义 `meta` 的投影：`name`、`author`、`version`、`description`、`homepage`、`min_app_version` 都从 `meta` 读取。
- `meta.min_app_version` 可选，填运行这份定义所需的最低 ArcReel 版本。

`endpoints/<slug>/definition.json` 示例片段：

```json
{
  "kind": "declarative",
  "schema_version": "1.1.0",
  "meta": {
    "name": "Example Video Gateway",
    "author": "your-github-name",
    "version": "1.0.0",
    "description": "Example definition for an OpenAI-style video generation gateway.",
    "homepage": "https://github.com/your-github-name/example-video-gateway",
    "hints": {
      "base_url": "https://api.example.com",
      "suggested_models": [{ "id": "example-video-1" }]
    }
  },
  "auth": { "headers": { "Authorization": "Bearer {{ api_key }}" } },
  "submit": {
    "method": "POST",
    "url": "{{ base_url }}/v1/videos",
    "body": { "model": "{{ model }}", "prompt": "{{ prompt }}", "duration": "{{ duration }}" },
    "extract": { "task_id": ["$.id"], "error": ["$.error.message"] }
  },
  "poll": {
    "method": "GET",
    "url": "{{ base_url }}/v1/videos/{{ task_id }}",
    "extract": { "status": ["$.status"], "video_url": ["$.video_url"], "error": ["$.error.message"] }
  }
}
```

实际投稿时不必手写：在 ArcReel 的「调用端点」里调好定义后点「导出」，把文件放进 `endpoints/<slug>/definition.json` 即可。

## 投稿

向本仓库提 PR。流程、内容准则和下架方式见 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 开自己的市场源

1. Fork 本仓库，或点 **Use this template** 新建仓库。
2. 在新仓库的 **Settings → Actions** 里启用 workflow（fork 默认不运行 workflow）。
3. 增删 `endpoints/<slug>/` 后 push 到默认分支，bot 会自动重生成 `arcreel-market.json` 并提交。默认分支不叫 `main` 时，把 `.github/workflows/publish-index.yml` 里的分支名改掉。默认分支没有开启保护时无需任何配置；开启了保护时见下方「进阶」。
4. 在 ArcReel 设置页「市场」→「管理市场源」添加市场源，地址填 `owner/repo`。

两个 workflow 只负责调用 ArcReel 主仓的可复用工作流，校验与生成规则由主仓维护，随主仓更新，你无需维护。

### 不用 CI

可以关掉 Actions，手写仓根的 `arcreel-market.json`。空索引长这样：

```json
{
  "schema_version": "1.0.0",
  "name": "my-market",
  "entries": []
}
```

每个条目写 `type`（固定 `endpoint`）、`slug`、`path`（如 `endpoints/<slug>/definition.json`）、`media_type`（固定 `video`），以及和定义 `meta` 一致的 `name`、`author`、`version` 等字段；有图标时加 `icon`。字段与 `meta` 不一致时，ArcReel 会拒绝安装。

也可以不用 GitHub：任意以 `arcreel-market.json` 结尾的 `https://` 直链都能作为市场源地址，条目路径相对索引所在目录解析。

### 进阶

在 ArcReel 源码目录执行 `uv run python -m lib.market check <市场源目录>`，可以在本地跑与 CI 相同的全部校验；`generate <市场源目录>` 则在本地重生成索引。

默认分支受保护时（官方仓即如此），bot 需要用 GitHub App 令牌推送索引：准备一个 GitHub App，装到你的仓库并在 ruleset 里把它列为 bypass actor，再在仓库 secrets 里添加 `MARKET_APP_CLIENT_ID`（App 的 client id）与 `MARKET_APP_PRIVATE_KEY`（App 的 private key）。`publish-index.yml` 已映射这两个 secrets：两者齐备时以 `<app-slug>[bot]` 身份推送；未配置时改用 `GITHUB_TOKEN`、以 `github-actions[bot]` 身份推送，这要求默认分支允许它直接推送。

## 许可证

[MIT](LICENSE)
