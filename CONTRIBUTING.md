# 投稿指南 / Contributing

[中文](#中文) · [English](#english)

## 中文

### 投稿流程

1. 在 ArcReel 的「调用端点」里写好定义，并用真实供应商跑通一次生成。
2. 点「导出」得到定义文件。确认 `meta` 里的 `name`、`author`、`version` 正确，按需补 `description`、`homepage`、`min_app_version`。
3. Fork 本仓库，把文件放到 `endpoints/<slug>/definition.json`。slug 是目录名，规则 `^[a-z0-9][a-z0-9-]{0,63}$`，不能与已有条目重复。可选放一个 `icon.png` / `icon.webp` / `icon.svg`（正方形，不超过 64 KB）。
4. 提 PR，填 PR 模板里的两行，并保持「投稿来源」为「手工 / manual」。**不要改 `arcreel-market.json`**：合入后由 bot 重生成。

CI 会校验目录、slug、图标、定义本身，以及由你的改动生成的索引。CI 通过后，维护者只按下面的内容准则审核。

### 从 ArcReel 客户端一键提交

不想 fork 和手写 PR 时，可以在 ArcReel 的「调用端点」列表中，点击对应端点的「分享到官方市场」。客户端先在本地校验，通过后由官方服务代你向本仓开 PR，不需要 GitHub 账号，可选填 GitHub 用户名以便在 PR 里 @ 你。之后在客户端里能看到提交状态：审核中、已采纳或已拒绝。

- 内容准则、`meta.version` 规则与手工投稿完全相同，一次提交对应一个条目。
- 从同一 ArcReel 实例再次提交同一 slug 时，如果上一份 PR 还在审核，新内容会更新到那个 PR 上，不会另开。PR 合并或关闭后再提交，则是一个新 PR。
- 合并即采纳，关闭即拒绝；拒绝理由见 PR 里的评论。

### 机器人 PR 的审核

客户端提交的 PR 由机器人开出，可以这样识别：

- 分支名形如 `submission/<slug>-<随机串>`。
- 标题形如 `[endpoint] <slug>`。
- 正文以 `<!-- arcreel-hub-submission -->` 开头，并列出 slug、名称、版本、`auth` 原文、`submit.url` 与 `poll.url`。

审核口径：

- CI 与手工投稿是同一套，必须通过；机器人 PR 与手工 PR 一样由维护者合并。
- 内容只按上面的[内容准则](#内容准则)审核，**不接受的内容同手工投稿**。重点看正文列出的 `auth` 与 URL 指向哪里。
- 提交者建议的 slug 在合并前可以调整。
- 带 `conflict` 标签的 PR 表示这个 slug 可能与别人撞车：slug 已在本源中，但最近一次已合并的客户端提交来自其他 ArcReel 实例，或没有已合并的客户端提交记录；或 slug 尚未收录，但其他实例正在提交同名条目。标签不代表拒绝，也不能直接证明作者不同。维护者应先核对作者与更新归属：同作者的更新按内容准则审核；不同作者使用同一 slug 时，请提交者改名后重新提交。
- 下架仍按下面的流程，用 PR 删除目录。

### PR 粒度

- 新增条目：一个 PR 只加一条。
- 更新已有条目：一个 PR 可以改多条。

### 内容准则

1. **收**供应商官方 API：`meta.hints.base_url` 是供应商自有域名，或其公有云官方入口。
2. **收**通用或开源网关协议：`meta.hints.base_url` 留空或写示例地址。
3. **不收**指向具体第三方中转站、代理或转售站点的端点。
4. **不收**与已有条目同作者、同名的重复定义；改进已有定义请直接更新那一条。
5. **不收**与 ArcReel 随版内置定义重复的定义。
6. **不收**以推广某项商业服务为主要目的的投稿。这类合作请联系 support@arc-reel.com。
7. 条目内容有任何变更，都**必须升** `meta.version`。ArcReel 按版本号判断已安装端点是否可更新。

### 下架

提 PR 删除 `endpoints/<slug>/` 整个目录，合入后 bot 重生成索引，条目即从市场消失。已安装的用户会看到该端点「市场中不可用」，本地端点不受影响。下架不留痕，slug 之后可以被复用。

---

## English

### Process

1. Write the definition under **Endpoints** in ArcReel and complete at least one real generation with the provider.
2. Click **Export** to get the definition file. Check `name`, `author` and `version` in `meta`, and add `description`, `homepage` or `min_app_version` as needed.
3. Fork this repository and put the file at `endpoints/<slug>/definition.json`. The slug is the directory name; it must match `^[a-z0-9][a-z0-9-]{0,63}$` and must not collide with an existing entry. Optionally add `icon.png` / `icon.webp` / `icon.svg` (square, at most 64 KB).
4. Open a pull request, fill in the two lines of the template, and keep the submission source as `manual`. **Do not edit `arcreel-market.json`**: the bot regenerates it after merge.

CI validates the directory, slug, icon, the definition itself, and the index generated from your change. Once CI passes, maintainers review only against the content guidelines below.

### One-click submission from the ArcReel client

If you would rather not fork and hand-write a pull request, choose **Share to official market** on an endpoint under **Endpoints** in ArcReel. The client validates locally, then the official service opens a pull request against this repository on your behalf. No GitHub account is needed; you may enter a GitHub username so the pull request can @-mention you. The client then shows the submission status: under review, accepted or rejected.

- The content guidelines and the `meta.version` rule are exactly the same as for manual submissions. One submission is one entry.
- Submitting the same slug again from the same ArcReel instance while the previous pull request is still under review updates that pull request instead of opening a new one. After it is merged or closed, a new submission opens a new pull request.
- Merged means accepted; closed means rejected. The reason is in a comment on the pull request.

### Reviewing bot pull requests

Pull requests from the client are opened by a bot. You can recognise them by:

- a branch named like `submission/<slug>-<random suffix>`;
- a title like `[endpoint] <slug>`;
- a body that starts with `<!-- arcreel-hub-submission -->` and lists the slug, name, version, the raw `auth` section, `submit.url` and `poll.url`.

Review policy:

- CI is the same as for manual submissions and must pass. Maintainers merge bot pull requests just like manual ones.
- Review only against the [content guidelines](#content-guidelines) above; **what is not accepted is the same as for manual submissions**. Pay particular attention to where the listed `auth` and URLs point.
- The slug proposed by the submitter may be adjusted before merge.
- A pull request labelled `conflict` means the slug may collide with someone else's: it is already in this source but the most recently merged client submission came from another ArcReel instance, or there is no merged client submission on record; or it is not yet listed but another instance is submitting an entry with the same slug. The label is neither a rejection nor proof of different authors. Maintainers should first verify authorship and who the update belongs to: review updates from the same author against the content guidelines; ask different authors using the same slug to rename and resubmit.
- Removal follows the process below: a pull request that deletes the directory.

### Pull request scope

- New entries: one entry per pull request.
- Updates to existing entries: any number per pull request.

### Content guidelines

1. **Accepted**: official provider APIs, where `meta.hints.base_url` is the provider's own domain or its official public cloud endpoint.
2. **Accepted**: generic or open-source gateway protocols, where `meta.hints.base_url` is empty or an example address.
3. **Not accepted**: endpoints pointing at a specific third-party relay, proxy or reseller site.
4. **Not accepted**: duplicates of an existing entry with the same author and name. To improve an existing definition, update that entry.
5. **Not accepted**: definitions that duplicate ones built into ArcReel.
6. **Not accepted**: submissions whose main purpose is to promote a commercial service. For that kind of collaboration, contact support@arc-reel.com.
7. Any change to an entry **must bump** `meta.version`. ArcReel compares versions to decide whether an installed endpoint can be updated.

### Removal

Open a pull request that deletes the whole `endpoints/<slug>/` directory. After merge the bot regenerates the index and the entry leaves the market. Users who installed it see the endpoint as unavailable in the market; their local endpoint is untouched. Removals leave no record, and the slug may be reused later.
