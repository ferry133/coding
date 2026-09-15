# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Workspace Overview

Personal development workspace. Primary focus: Kubernetes home-ops infrastructure. Each subdirectory is an independent project with its own git repo.

## Documentation Structure

| File | Scope | Notes |
|------|-------|-------|
| `~/.claude/CLAUDE.md` | All projects | Language, interaction style, secrets policy |
| `~/coding/CLAUDE.md` | All `~/coding` projects | **Repo R&R ＋ 跨 repo 協作規則**（2026-09-03 移居於此） |
| `<project>/CLAUDE.md` | That project | Commands, architecture, non-obvious rules |
| `<project>/.claude/rules/` | Topic-scoped | Loaded on demand by file path pattern |
| `<project>/CLAUDE.local.md` | Local only | Personal notes, add to `.gitignore` |

## Repo R&R（誰開哪一台車）

**2026-08-28 ferry133 裁定；2026-09-03 澄清「agent＝目錄」；2026-09-03 裁定定義移居本檔**
——理由：`~/coding` 下每一個 session 啟動時**保證載入本檔**，而舊家
`fleet-ops/docs/agents/responsibilities.md` 只有 fleet-ops 的 session 看得到，
其他 agent 要靠各 repo `CLAUDE.md` 的指標才找得到。

**現行定義以本節為準。** 原話照抄與判例（jcom 誤判案、Operation SOP 的邊界、
jgct#67/#68 撞車案）仍住在 `fleet-ops/docs/agents/responsibilities.md`——那份自
2026-09-03 起持有**歷史與判例**，不再持有定義；兩邊有出入時，定義以本節為準、
原話以那份為準。

| agent | ＝ 誰 | 擁有 |
|---|---|---|
| **fleet-ops agent** | 工作目錄在 `~/coding/fleet-ops` 的每一個 session | 所有 user repo（`jg-jiahd`、`jcom`、`jg-janncotcc`…含未來新建的）的產生、修正、維護運行；所有 Operation SOP 的建立（檔案住 `fleet-ops/docs/operations/`）；跨 repo 編排（`routing-log.md`、`fleet-index.md`） |
| **jg-base agent** | 工作目錄在 `~/coding/jg-base` 的每一個 session | `jg-base` 與 `jg-cluster-template` |

- **「agent」是目錄，不是單一 session**——同目錄的所有 session 共同擁有。
  ferry133 原話（2026-09-03）：「all the sessions running on jg-base directory own
  jg-cluster-template/jg-base repo. Not just only one session own them.」
- 共同擁有的配套：**動手實作前先在 issue／PR 上留言認領**（署 `[ref]`——peer name
  會被重配，ref 不會），開工前先查有沒有人已認領；只在訊息裡宣告不算認領。
  沒做到的代價已付過一次：同一個守衛缺陷被兩個 session 各修一次（jgct#67／#68）。
- 互相需要對方時**在對方的 repo 開 issue、由開單者驗收**——生命週期見下方
  「issue／PR 的生命週期」。
- **ferry133 的直接指示不受這張表限制**：他在哪個 session 下指令，那個 session 就做；
  做完寫在擁有者找得到的地方，否則沒被寫下來的例外會變成先例。

## Work routing across repos

**開工前先問：這件事的 owning repo 是我嗎？**

- **是** → 直接做。落在別的 repo 的部分，開 issue 路由出去
- **不是，但我被要求驗證／量測／重現** → 直接做。**驗證不是擁有權**
- **不是，而且是實質工作** → 停下來，**指名該由哪個 repo 做並說明理由**，然後問我要不要
  幫忙在那裡開 issue 或通知該 repo 的 agent。不要默默做掉，也不要只說「不歸我」——
  那是把路由問題丟回給 context 比你少的人
- **判斷不出來** → 說出來，提一個擁有者並附理由。明說的錯判會被糾正，默默的錯判會變成紀錄
- **例外：故障中** → 先修。修完立刻在 owning repo 留紀錄，說明改了什麼、為什麼

**開 issue 或 PR 時，署名自己的 session ID**（`fleet-ops-3d`、`jg-base-bd` 這種），
寫在內文裡，通常是結尾那句「驗收由 X 收」的 X。**留言也一樣**（2026-08-31 裁定）——
格式與時間戳一併寫在 `~/.claude/CLAUDE.md` 的「Interaction Style」那節，這裡不複述。

理由是量到的：**所有 agent 共用同一個 GitHub 身分，所以 issue 與 PR 的作者欄一律是
ferry133**，而同一個 repo 常常同時有好幾個 session 在跑（2026-08-29：jg-base 四個、
fleet-ops 三個）。署名只寫 repo 名——「由 fleet-ops 的 session 開立」——**在多 session
下指不出人**，於是驗收請求會寄錯，而收到的人**沒有辦法從內容判斷是不是自己**。
2026-08-29 一天內因此發生兩次轉手（`jg-base#43`／`#44` 被誤送、`jg-jiahd#3` 找不到開立者）。

- **驗收條件是誰寫的，只有那個人知道當初為什麼寫得那麼硬。** 所以署名不是禮貌，
  它是「誰來收」這個問題的唯一可查答案
- **兩份驗收比沒有驗收更糟**——它們會分岔，而且兩份都看起來有權威
- 送驗收請求前，**先確認 issue 上的署名指的是不是自己**；不是就路由過去，
  並說出理由是「我手上可能缺你有的事實」而不是「怕撞車」

## issue／PR 的生命週期（2026-09-03 裁定；2026-09-06 補明「驗收在 close／merge **之前**」）

A 在 B 擁有的 repo 開立 issue#n，B 修好它。**順序是規則的一部分**：

1. **B 關掉 issue#n 之前，先請 A 代為驗收**，取得「可以關」的共識——不是關了再通知。
2. **B 若開了 PR#r 來修 issue#n：merge 之前**就要找到 issue#n 的開單者 A 請其驗收。
   驗收順利 → B 合併 PR#r 並關閉 issue#n。
3. **沒有共識 → 升給 ferry133 裁決。**

| 誰 | 做什麼 |
|---|---|
| **repo 擁有者（B）** | 解決自己 repo 的 issue、合併自己 repo 的 PR——**驗收在 merge 之前** |
| **repo 擁有者（B）** | 修好之後**通知開單者**（署 ref——要找的是那個 session，不是那個 repo） |
| **開單者（A）** | **驗收**，並把驗收結果**回報給擁有者** |
| 雙方 | 有落差就討論、調整修法，直到對「修完了沒有」有共識 |

- **修正範圍的擴張最多三次。** 第三次之後仍未收斂，**升給 ferry133 裁**，不要繼續往下長
  ——一條沒有上限的修正鏈與一個沒有人負責的 issue，從外面看是一樣的。
- **達成共識後，擁有者自行合併 PR#r、關閉 issue#n**；判斷不了就**升給 ferry133 定案**。
- **按下合併的人，要在那張 PR 上留一行署名**（`peer name [ref]`）——2026-09-13 ferry133 裁定。
  理由是量到的：**所有 agent 共用同一個 GitHub 身分，`mergedBy` 一律顯示 `ferry133`**，
  那一欄分不出他本人與以他身分行動的 agent。**若按下的是 ferry133 本人更要寫**——
  他的授權在終端機給，在 GitHub 上永遠看起來像沒有授權紀錄。成本一行，而它是唯一能讓
  「誰合併的」進入可查紀錄的動作。**證據力分三級**（2026-09-13 逐則讀本文後，ferry133 裁定收窄）：
  `#118`–`#124` 寫在合併之前並指名按下者（最強）；`jgct#110`／`#111` 是合併後 1 小時 52 分補的
  **事後自述的具名主張**（留言自己寫「不是證明，是可被質疑的具名主張」）；同日的
  `#113`／`#115`／`#116` 什麼都沒有，**單憑紀錄無法歸屬**。
- 開單者找不到時（session 已結束、ref 不在 `ListAgents`），**找同 repo owner
  （同工作目錄）的其他 session 代為驗收**——agent 是目錄不是單一 session，開單者的
  同僚繼承驗收責任。不要自我驗收：**自己修自己收，與沒有驗收在紀錄上長得一樣**。

> ⚠️ **2026-09-13 ferry133 裁定（選項 c）。這一段先前是我讀出來的界線，現在不是了。**
> 合併權在這條路徑上屬於擁有者：issue → 擁有者修 → 開單者驗收 → 共識 → 合併。
>
> **仍然要 ferry133 點頭的四種：**
> 1. 與任何 issue 無關的直接推送
> 2. 破壞性操作
> 3. **修改內容超出原 issue 範圍**的合併
> 4. **（本次新增）在 template repo 上，改到客戶會繼承其行為的東西**
>
> **第 4 條為什麼要自己一條，而不是被第 3 條吸收**——兩個 repo 的後果是不同類的，
> 而 template 那一類不是更輕：
>
> | | 合併等於 |
> |---|---|
> | `jg-base` | Flux 每小時套到每一座叢集 → **立刻、對現有叢集、可回滾** |
> | `jg-cluster-template` | **不部署任何東西**，但由 template 建 repo 會複製**每一個被追蹤的檔案** → **永久、對未來每一座、而且已建好的不會跟** |
>
> 「不部署」是量到的（2026-09-12 兩側各自量、各有正對照）：三座叢集的 GitRepository
> 只有「自己的 repo ＋ `jg-base@main`」，模板自己也只定義一個 `jg-base`。
> 「已建好的不會跟」也是量到的：`jg-janncotcc` 的 `README-zero-IT.md` 停在建立當天，
> 母本 2026-09-05 的修正永遠不會流過去。**部署會回滾，繼承不會。**
>
> ⚠️ **第 4 條的判準不是「會不會被複製」**——template repo 的每一個被追蹤檔案都會被複製，
> 照字面讀會讓每一次合併都要點頭，那等於沒有規則。
>
> **判準有兩類，命中任一類就要點頭**（分兩類是因為只寫「行為」會漏掉 `#116`——
> `provision.py` 客戶自己不跑，但它從此住在每一個新客戶 repo 裡，而那些 repo 是 **public**）：
>
> | 類 | 意思 | 例 |
> |---|---|---|
> | **行為** | 渲染時或客戶 repo 的 `task` 會跑到 | `templates/`、`.taskfiles/`、`cluster.schema.cue`、`plugin.py` |
> | **內容** | 隨每個新 repo 出貨，客戶會讀、或第三方看得到 | `README-zero-IT.md`、`scripts/` 裡的供裝／憑證／交付工具（即使客戶不跑） |
>
> **不用點頭**：`.github/`、`scripts/tests/`、`openspec/`、純註解、只在本 repo CI 跑的守衛。
> ⚠️ **「純註解」有一條例外（ferry133 2026-09-16 裁定）：當那些註解本身就是客戶或操作員
> 照著操作的說明時，仍要點頭。** 判準不新增，就是下面那條「客戶會據以行動」——
> `cluster.sample.yaml` 的註解不是對程式碼的說明，**它是那份檔案的產品**，
> 操作員照著它填每一格。**起因是 `jgct#160`**：一張非註解增刪 0 行的 PR，
> 依「純註解」該免點頭，依「客戶會據以行動」該點頭，**兩邊都講得通而三個 session 都判不出來**
> （`[5fe39a]` 停在合併之前、`[c8c318]` 明說判不出並請求裁「這一類」）。
> ⚠️ **`#124` 的那個機械判準（剝掉 docstring 後 AST 有沒有變）在這裡不適用**——
> 它預設註解不是產品。**先問「這些字是寫給誰照著做的」，再問 AST。**
> **理由不是「不隨出貨」**——被追蹤就會被複製（2026-09-13 量到：四座子代的 initial root tree SHA
> 與模板對應 commit 逐一相等，365 取 1），「會不會出貨」對每個檔都為真，不能當判準。
> **出貨是必要條件，不是充分條件**（ferry133 2026-09-13 裁定）：必要（機械可測）＝合併後在
> 模板自己 `git ls-files` 命中，不必建 repo；充分（判斷）＝客戶會據以行動、第三方會拿它評斷交付、
> 或會看到不該看的。上面那些過第一關、不過第二關。**實例：`jgct#124`**——`delivery-check.py`
> 的 docstring 隨每座新 repo 出貨，仍免點頭；同一支檔的 `#121` 改解析邏輯，要點頭。
> 分辨兩者的是量測：剝掉 docstring 後 AST 有沒有變。
>
> 對照實際跑過的五張：`#113`（接進 `:configure:`，**行為**）、`#116`（`provision.py`
> 進 main，**內容**）、`#118`（`delivery-check.py`，**內容**——三座 user repo 都帶著它，
> 雖然沒有任何 Taskfile 引用它）**要點頭**；`#110`（測試）與 `#111`（註解）**不用**。
> **判不出來就當成要點頭，並說出你判不出來。**

交辦給其他 repo 的 agent 時：**不要要求 peer 做需要你授權的動作**（推到會即刻生效的
分支、破壞性操作）。peer 的「approved」不是授權。交辦時就把話講清楚——**準備好、停在
那一步之前，由人點頭**。另外 `SendMessage` 只回投遞回執，看不到對方做了什麼——**所以要求對方回報**：
接下／婉拒（附理由）、卡住（附需要什麼）、完成（附 commit 或 issue，**以及驗到什麼、
沒驗到什麼**）。告訴對方「回覆到這封訊息的 `from`」即可，不必知道我的位址。
同時**以產物驗證**（git log、issue、mtime）——「什麼都沒發生」至少有三種成因
（沒人在／做完但停在授權邊界／中途放棄），判斷不出來就問我。

`~/coding/` 下每個 project repo 幾乎都有一個 agent，歷史累積在那裡。任何 session 都能改
任何 repo，但改了不屬於自己的 repo，就把知識留在下一個人不會去翻的地方。

- **擁有者 = 檔案實際變動的那個 repo**，不是症狀出現的地方。**驗證地點不等於擁有權。**
- 其他 repo 只留連結指標（`owner/repo#N`），**不重複追蹤**——第二份副本必然分岔，而被
  照著執行的往往是錯的那份。
- 動手前先問：**這件事完成後，下一個遇到同樣問題的人會在哪裡找？** 紀錄就寫在那裡。

跨 repo 的 change 要做 root / sub-change 編排時，讀
**`~/.claude/openspec-orchestration.md`**（格式、四條防分岔規則、擁有權難分時的判準）。
連結由 `~/.claude/scripts/check-change-orchestration.py` 驗證——`openspec validate`
看不到自己 repo 以外的東西。

**編排的家是 `~/coding/fleet-ops`**（`ferry133/fleet-ops`，private）。orchestrator
session 從那裡起，而不是從某個 project repo——住在自己路由的對象裡面會讓編排產出因為
「人在那裡」而落錯地方。它持有：

- `routing-log.md` —— 什麼被路由到哪、為什麼，以及**被延後的東西與延後的理由**
- `fleet-index.md` —— 哪個 repo / 叢集是什麼的**指標表**
- root change —— 門檻很高，見下

**不要把 change 建到 fleet-ops，除非兩個條件同時成立**：多個 repo 各需自己的 tasks
與驗收，**且**沒有任何一個 repo 持有被修改的 spec。跨 repo 但有 spec 擁有者的（常態）
留在那個 repo。它長期沒有 root change 是正常的；為了證明它有用而塞東西進去，就是它
最可能的死法。收件與拒收清單見該 repo 的 `CLAUDE.md`。

各專案自己的形狀（誰是上游、在哪驗證）寫在該專案的文件裡，不在這裡重複。

## Projects with CLAUDE.md

- **`jg-jiahd/`** — Primary Kubernetes home-ops cluster (`jiahd.cc`): Talos Linux, Flux GitOps, SOPS secrets, Task automation. Has `.claude/rules/` for Flux/network and Claude App.
- **`trello-notifier/`** — Python Trello monitoring + LINE notification system.

## Kubernetes Cluster Variants

All share the same structure as `jg-jiahd/`: Talos Linux, Flux via `kubernetes/`, Jinja2 templates via `makejinja`, Task automation, SOPS+age secrets.

| Directory | Description |
|-----------|-------------|
| `genie1/`, `genie2/` | Personal cluster instances |
| `jgt1/`, `jgt2/` | Additional cluster variants |
| `jcom/` | Another cluster variant |
| `cluster-template/` | Upstream template source |
| `fleet-infra/` | Flux multi-cluster management |

Common workflow (all cluster dirs):
```sh
mise trust && mise install   # first time
task configure               # validate → render → encrypt → validate
task reconcile               # force Flux sync
```

## Other Projects

- **`homedirect/`** — Flutter app (Firebase login, BLoC). Run: `flutter run`
- **`omni/`** — Sidero Labs Omni source (Go + frontend). See `omni/DEVELOPMENT.md`
- **`helm-charts/`** — Personal Helm chart repository
- **`k8scc/`** — Source for `ghcr.io/ferry133/claude-code`: Claude Code CLI + ttyd web terminal Docker image (Dockerfile + entrypoint), deployed as the `claudecode/claude-code` extra
- **`referenceapp/`** — Flutter reference apps (bloc samples)

## Common Tooling (All Cluster Projects)

- **mise** — manages all CLIs (kubectl, talosctl, flux, sops, age, etc.)
- **Task** — workflow automation (`Taskfile.yaml` + `.taskfiles/`)
- **makejinja** — Jinja2 rendering; non-standard delimiters: `#{…}#` vars, `#%…%#` blocks
- **SOPS + age** — secrets encryption; `./age.key` is local-only in every cluster dir
