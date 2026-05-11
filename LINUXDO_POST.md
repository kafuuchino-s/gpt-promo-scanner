# 看这里就行了！最低价48月GPT Business也许就在这里，感谢佬友思路

> 本文首发于 [LinuxDo](https://linux.do)
> 开源工具: [GitHub](https://github.com/JUk1-GH/chatgpt-team-scanner)
> 最后更新: 2026-05-11

---

## 两个脚本，搞定全流程

用 AI 搞了一套全链路脚本，可以对 ChatGPT Business（原 Team）优惠码进行爆破。

### 脚本一：批量检测

你只需要准备（或者让 AI 生成）可能的优惠码，扔进去，就会批量检测哪些码有效：

```bash
# 全矩阵交叉扫描：8个已知base × 34个国家 = 280个组合
python discover_codes.py --cross
```

检测结果分三类：
- **ELIGIBLE** → 当前账户可用，直接付款
- **EXISTS** → 码存在但地区不匹配，需要对应国家节点
- **not found** → 猜错了

目前已验证 **22 个有效码**，覆盖 11 个国家和地区。

### 脚本二：自动比价

把有效的码扔进去，自动查看每个码在每个国家的价格优惠和美元换算：

```bash
# 全自动扫描所有地区，生成价格表
python auto_scan.py
```

你需要准备好 **Clash 多国节点**，脚本会自动切换地区 IP 进行价格探测，效果大概这样：

```
============================================================
📊 扫描完成 — 共 22 个可用码
============================================================

促销码                       地区     本地折扣               ≈ USD
----------------------------------------------------------------------
  thealloynetwork          US     $30/月 off x 48 months  $30/月
  alongsideus              US     $30/月 off x 48 months  $30/月
  monicaius                US     $30/月 off x 48 months  $30/月
  talentgeniusus           US     $25/月 off x 48 months  $25/月
  firstfocusus             US     $25/月 off x 48 months  $25/月
  wildmangous              US     $25/月 off x 48 months  $25/月
  codestonede              DE     €29/月 off x 48 months  $34/月
  codestonees              ES     €28/月 off x 48 months  $33/月
  codestonefr              FR     €28/月 off x 48 months  $33/月
  firstfocus               AU     A$45/月 off x 48 months $33/月
  monicaica                CA     C$42/月 off x 48 months  $31/月
  ...
```

---

## 使用方式

### 前置条件

- **Clash Verge**（或兼容 Clash API 的客户端）
- **Python 3.9+**
- **ChatGPT 账号**（免费版即可）

### 1. 安装依赖

```bash
pip install curl_cffi
cp config.toml.example config.toml
```

### 2. 获取 Token

```javascript
// 在 chatgpt.com F12 Console 执行
const s = await (await fetch('/api/auth/session')).json();
console.log(s.accessToken);
```

填到 `config.toml` 的 `[openai] token` 字段。

### 3. 跑扫描

```bash
# 发现新码
python discover_codes.py --cross

# 对发现的码自动比价
python auto_scan.py
```

---

## 目前已确认的 22 个有效码

**US — 当前账户可直接用：**

| 促销码 | 省/月 | 48 月省 | 来源 |
|--------|-------|---------|------|
| `thealloynetwork` | $30 | $1,440 | The Alloy Network |
| `alongsideus` | $30 | $1,440 | Alongside |
| `monicaius` | $30 | $1,440 | Monica AI |
| `talentgeniusus` | $25 | $1,200 | TalentGenius |
| `firstfocusus` | $25 | $1,200 | First Focus |
| `wildmangous` | $25 | $1,200 | WildMango |

**其他地区（需对应节点付款）：**

| 促销码 | 地区 | 原价(2席) | 折扣后 | 省/月 | ≈ USD |
|--------|------|----------|--------|-------|-------|
| `codestonede` | 🇩🇪 DE | €58 | €29 | €29 | **$34** |
| `codestonees` | 🇪🇸 ES | €56 | €28 | €28 | **$33** |
| `codestonefr` | 🇫🇷 FR | €56 | €28 | €28 | **$33** |
| `firstfocus` | 🇦🇺 AU | A$90 | A$45 | A$45 | **$33** |
| `monicaica` | 🇨🇦 CA | C$84 | C$42 | C$42 | **$31** |
| `wildmangoke` | 🇰🇪 KE | $60 | $30 | $30 | **$30** |
| `talentgeniusbr` | 🇧🇷 BR | R$260 | R$130 | R$130 | **$26** |
| `wildmangofr` | 🇫🇷 FR | €44 | €22 | €22 | **$26** |
| `talentgeniusau` | 🇦🇺 AU | A$70 | A$35 | A$35 | **$25** |
| `talentgeniusca` | 🇨🇦 CA | C$68 | C$34 | C$34 | **$25** |
| `talentgeniusuk` | 🇬🇧 GB | £36 | £18 | £18 | **$25** |
| `aibuildgroupgb` | 🇬🇧 GB | £36 | £18 | £18 | **$25** |
| `wildmangong` | 🇳🇬 NG | ₦67,200 | ₦33,600 | ₦33,600 | **$25** |
| `firstfocusnz` | 🇳🇿 NZ | NZ$82 | NZ$41 | NZ$41 | **$24** |
| `wildmangoza` | 🇿🇦 ZA | R800 | R400 | R400 | **$24** |

> 全部 48 个月固定折扣，ChatGPT Business（原 Team）计划。

---

## 公司分布规律

```
TalentGenius    → US($25)  AU($25)  BR($26)  CA($25)  GB($25)
WildMango       → US($25)  FR($26)  KE($30)  NG($25)  ZA($24)
First Focus     → US($25)  AU($33)  NZ($24)
Codestone       → DE($34)  ES($33)  FR($33)
Monica AI       → US($30)  CA($31)
The Alloy Network→ US($30)
Alongside       → US($30)
AI Build Group  → GB($25)
```

规律：**有码的都是中小型 MSP/IT 服务和 AI 工具公司，不是大型咨询公司。**

---

## 完整挖掘过程（太长不看版）

<details>
<summary>点击展开 — 从零到22个码的完整故事 + 7个踩坑</summary>

<br>

### 事情的起因

上个月想搞个 ChatGPT Team 给团队用，Google 了一圈发现 OpenAI 给 SMB 渠道合作伙伴发促销码，格式是 `公司名+国家码`。

已知有这几个：
```
talentgeniusus  → $25/月 off
thealloynetwork → $30/月 off
alongsideus     → $30/月 off
monicaius       → $30/月 off
```

问题是——**还有没有更多的？**

### 第一阶段：从公开合作伙伴入手（全灭）

搜 OpenAI SMB 合作伙伴名单：SearchKings（CA）、Samsung SDS（KR）、ITPartners+（US）……

列了 180 个候选码，结果：

```
AU (11 codes) — 全部 not found
CA (7 codes)  — 全部 not found
US (54 codes) — 只有已知的 4 个，50 个新码全部 not found
```

**命中率 0%。** 公开合作伙伴 ≠ 有促销码的合作伙伴。

### 第二阶段：网络搜索找到线索

```
firstfocus          → EXISTS!  (AU — 无国家后缀！)
codestonede         → EXISTS!  (DE)
codestonefr         → EXISTS!  (FR)
wildmangoke         → EXISTS!  (KE)
```

两个关键发现：
1. `firstfocus` 没有 AU 后缀却存在 — 命名规则有无后缀的例外
2. Codestone 是英国公司，却有 DE 和 FR 码 — 同一公司可能有多国码

### 第三阶段：全矩阵交叉扫描

8 个 base name × 34 个国家 = 280 个组合。

新发现：
```
codestonees, talentgeniusau, talentgeniusbr, talentgeniusca,
monicaica, firstfocusnz, wildmangoza, wildmangong
```

加上已知的达到 22 个有效码。

### 第四阶段：价格收集

Metadata API 返回本地化折扣。切到 DE 节点查 `codestonede` 返回 €29，切到 NG 节点查 `wildmangong` 返回 ₦33600。实时汇率换算 USD。

### 踩坑合集

#### 坑 1：CRN 报道的合作伙伴全部无效
花了一天按公开列表枚举 ~100 个码，0 命中。大部分合作走 Enterprise Resale / Service Partner 模式，跟 Team 促销码是两套体系。

#### 坑 2：跑 1286 个请求前没先测一个
直接生成 1286 个候选码开跑，全部被 Cloudflare 拉黑。**先单测 → 再批量 → 再全量。**

#### 坑 3：Clash 全局模式 vs 规则模式
节点切换代码硬编码了 `🤖 AI` 组，但 Clash 跑在 global 模式，流量走 `GLOBAL` 组。必须动态检测：

```python
mode = get("/configs").get("mode", "rule")
group = "GLOBAL" if mode == "global" else "🤖 AI"
```

#### 坑 4：UK 国家后缀不统一
`talentgeniusuk` 有效但 `talentgeniusgb` 不存在。`aibuildgroupgb` 有效但 `aibuildgroupuk` 不存在。UK 必须两种都试。

#### 坑 5：Cloudflare 限速
连续 ~50 个请求后被拦截。需要控制频率、被限时切换节点、每次都新建 Session。

#### 坑 6：Metadata 折扣是 per-seat
`discount.value` 是每席位的折扣。2 席位时实际省 = value × 2。

#### 坑 7：促销码有有效期
`geccogb` 和 `codestonegb` 曾是最便宜的（~£11/月），等测试时已过期。

</details>

---

## 最后

开源脚本放在 GitHub（私有仓库），包含：
- `discover_codes.py` — 批量检测脚本
- `auto_scan.py` — 自动比价脚本（需 Clash 多国节点）
- `known_codes.json` — 已知 22 个有效码数据库
- `config.toml.example` — 配置模板

想自己继续挖新码的方向：
1. **中小型 AI 工具公司**，特别是 Chrome 插件类
2. **区域性 MSP**，各国前 20 名 IT 服务商
3. **TG 频道监控** — 新码通常先在 Telegram 流出

---

*完整日志见 [DISCOVERY_LOG.md](DISCOVERY_LOG.md)*
