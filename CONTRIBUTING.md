# 投稿指南 / Contributing

[中文](#中文) · [English](#english)

## 中文

### 投稿流程

1. 在 ArcReel 的「调用端点」里写好定义，并用真实供应商跑通一次生成。
2. 点「导出」得到定义文件。确认 `meta` 里的 `name`、`author`、`version` 正确，按需补 `description`、`homepage`、`min_app_version`。
3. Fork 本仓库，把文件放到 `endpoints/<slug>/definition.json`。slug 是目录名，规则 `^[a-z0-9][a-z0-9-]{0,63}$`，不能与已有条目重复。可选放一个 `icon.png` / `icon.webp` / `icon.svg`（正方形，不超过 64 KB）。
4. 提 PR，填 PR 模板里的两行。**不要改 `arcreel-market.json`**：合入后由 bot 重生成。

CI 会校验目录、slug、图标、定义本身，以及由你的改动生成的索引。CI 通过后，维护者只按下面的内容准则审核。

### PR 粒度

- 新增条目：一个 PR 只加一条。
- 更新已有条目：一个 PR 可以改多条。

### 内容准则

1. **收**供应商官方 API：`meta.hints.base_url` 是供应商自有域名，或其公有云官方入口。
2. **收**通用或开源网关协议：`meta.hints.base_url` 留空或写示例地址。
3. **不收**指向具体第三方中转站、代理或转售站点的端点。
4. **不收**与已有条目同作者、同名的重复定义；改进已有定义请直接更新那一条。
5. **不收**与 ArcReel 随版内置定义重复的定义。
6. 条目内容有任何变更，都**必须升** `meta.version`。ArcReel 按版本号判断已安装端点是否可更新。

### 下架

提 PR 删除 `endpoints/<slug>/` 整个目录，合入后 bot 重生成索引，条目即从市场消失。已安装的用户会看到该端点「市场中不可用」，本地端点不受影响。下架不留痕，slug 之后可以被复用。

---

## English

### Process

1. Write the definition under **Endpoints** in ArcReel and complete at least one real generation with the provider.
2. Click **Export** to get the definition file. Check `name`, `author` and `version` in `meta`, and add `description`, `homepage` or `min_app_version` as needed.
3. Fork this repository and put the file at `endpoints/<slug>/definition.json`. The slug is the directory name; it must match `^[a-z0-9][a-z0-9-]{0,63}$` and must not collide with an existing entry. Optionally add `icon.png` / `icon.webp` / `icon.svg` (square, at most 64 KB).
4. Open a pull request and fill in the two lines of the template. **Do not edit `arcreel-market.json`**: the bot regenerates it after merge.

CI validates the directory, slug, icon, the definition itself, and the index generated from your change. Once CI passes, maintainers review only against the content guidelines below.

### Pull request scope

- New entries: one entry per pull request.
- Updates to existing entries: any number per pull request.

### Content guidelines

1. **Accepted**: official provider APIs, where `meta.hints.base_url` is the provider's own domain or its official public cloud endpoint.
2. **Accepted**: generic or open-source gateway protocols, where `meta.hints.base_url` is empty or an example address.
3. **Not accepted**: endpoints pointing at a specific third-party relay, proxy or reseller site.
4. **Not accepted**: duplicates of an existing entry with the same author and name. To improve an existing definition, update that entry.
5. **Not accepted**: definitions that duplicate ones built into ArcReel.
6. Any change to an entry **must bump** `meta.version`. ArcReel compares versions to decide whether an installed endpoint can be updated.

### Removal

Open a pull request that deletes the whole `endpoints/<slug>/` directory. After merge the bot regenerates the index and the entry leaves the market. Users who installed it see the endpoint as unavailable in the market; their local endpoint is untouched. Removals leave no record, and the slug may be reused later.
