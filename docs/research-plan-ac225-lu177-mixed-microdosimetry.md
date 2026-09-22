# 研究計画書

## ²²⁵Ac/¹⁷⁷Lu 混合放射線場に対する統一的微視的線量計算フレームワークの構築と検証
### — 検証済み単核種カーネルから、放射能比・空間共局在・娘核種再分布・不確かさを統合した一般化モデルへ —

| 項目 | 内容 |
|---|---|
| 仮題（和文） | ²²⁵Ac/¹⁷⁷Lu 併用核医学治療における微視的線量場の統一的定式化と不確かさ評価 |
| 仮題（英文） | A unified microdosimetric dose-field framework for ²²⁵Ac/¹⁷⁷Lu combination radiopharmaceutical therapy: generalization over activity ratio, spatial co-localization, daughter redistribution, and uncertainty |
| 研究区分 | 博士課程研究（3年計画）／計算物理・医学物理 |
| 主分野 | 放射線物理学・医学物理学（内用放射線治療線量評価、microdosimetry） |
| 主要手法 | Monte Carlo 輸送計算（Geant4 / Geant4-DNA / GATE）、畳み込み線量計算、感度解析・不確かさ伝播、放射線生物モデリング |
| 主要成果物 | (1) 検証済みマルチ核種 dose point kernel (DPK) ライブラリ、(2) 混合場線量計算フレームワークおよびオープンソース実装、(3) 不確かさ定量化（UQ）報告、(4) 査読論文 3 編 |

---

## 要旨

α線放出核種 ²²⁵Ac と β線放出核種 ¹⁷⁷Lu の併用核医学治療（combination radiopharmaceutical therapy, RPT）は、飛程・LET・線量率が大きく異なる 2 種類の放射線場を同一腫瘍内に同時に形成する。本研究は、この「混合放射線場」に対する微視的線量評価を、単核種ごとに検証されたカーネルを基盤として一般化することを目的とする。

本研究の出発点として重要なのは、**物理吸収線量に関しては混合場が線形重ね合わせで書ける**という事実である。したがって「混合場の線量計算式が存在しない」という主張は成立しない。本研究が埋めるのは、その線形性が成り立つ前提そのものが臨床的に崩れる領域、すなわち

1. ²²⁵Ac の崩壊連鎖が *in situ* で完結するという暗黙の仮定（娘核種再分布により崩壊）、
2. ²²⁵Ac と ¹⁷⁷Lu の微視的分布が同一であるという暗黙の仮定（非共局在により崩壊）、
3. 放射能比が時間的に一定であるという暗黙の仮定（物理半減期・生物学的半減期の差により崩壊）、
4. 生物学的効果が総物理線量の関数であるという暗黙の仮定（放射線質依存性・LQ交差項により崩壊）、

という 4 つの前提である。これら 4 つを明示的な変数として取り込んだ一般化された微視的線量場モデルを構築し、単核種レベルで公表値との整合を検証したうえで、混合場における線量不均一性とその不確かさを定量化する。

---

## 1. 研究背景

### 1.1 核医学内用療法の現状

¹⁷⁷Lu 標識薬剤（[¹⁷⁷Lu]Lu-DOTATATE、[¹⁷⁷Lu]Lu-PSMA-617 など）は第III相試験を経て標準治療に組み込まれ、β線放出核種による内用療法が確立した。一方、α線放出核種 ²²⁵Ac を用いた治療（[²²⁵Ac]Ac-PSMA-617 等）は、¹⁷⁷Lu 治療抵抗性症例に対しても応答が報告され、臨床開発が急速に進展している。

両核種の物理的性質は大きく異なる（詳細は付録B）。

| 特性 | ²²⁵Ac（崩壊連鎖） | ¹⁷⁷Lu |
|---|---|---|
| 物理半減期 | 9.92 d（親核種） | 6.647 d |
| 主放射線 | α線（1 崩壊あたり 4 α、総α運動エネルギー ≈ 27.8 MeV） | β⁻線（Eβmax ≈ 497 keV、平均 ≈ 134 keV） |
| 飛程（水中） | ≈ 47–85 µm（細胞〜数細胞スケール） | 平均 ≈ 0.2 mm、最大 ≈ 1.8 mm（数百細胞スケール） |
| LET | ≈ 60–230 keV/µm（高LET） | ≈ 0.2 keV/µm（低LET） |
| cross-fire | ほぼ無い（自己線量支配） | 大きい |
| γ線 | ²¹³Bi の 440 keV 等（画像化可能だが低収率） | 113 keV / 208 keV（SPECT 定量可能） |

この「飛程が 1–2 桁異なる 2 つの線源が同一微小体積に共存する」という状況は、従来の臓器平均線量（MIRD 形式）ではほとんど記述できない。²²⁵Ac の α線飛程は voxel SPECT の空間分解能（数 mm）より 2 桁小さく、臓器平均線量と細胞レベルの実効線量が大きく乖離する。

### 1.2 併用療法という設計の合理性

²²⁵Ac と ¹⁷⁷Lu の併用が合理的とされる理由は、両者が**相補的な空間スケール**を持つことにある。

- ²²⁵Ac：高LET・短飛程 → 標的細胞を確実に致死させるが、抗原陰性細胞・低取り込み領域を取り逃す。
- ¹⁷⁷Lu：低LET・長飛程 → cross-fire により抗原陰性細胞もカバーするが、単独では不均一取り込み時の線量不足が生じやすい。

すなわち併用の本質的利点は「合計線量が増えること」ではなく「**線量分布の空間的相補性**」にある。したがって併用療法の定量的評価には、必然的に微視的（cellular〜sub-mm）スケールの線量分布評価が要求される。ここが本研究の核心的動機である。

### 1.3 現行線量評価の限界

現行の臨床 dosimetry は概ね以下の構成をとる。

```
定量画像（SPECT/PET）→ 時間放射能曲線 → 時間積分放射能 TIA → voxel S-value / DPK 畳み込み → voxel 線量 → 臓器平均線量・線量体積ヒストグラム
```

この枠組みを ²²⁵Ac/¹⁷⁷Lu 併用に適用すると、次の 4 つの問題が生じる。

1. **分解能の不整合**：α線飛程（数十 µm）≪ 画像 voxel（数 mm）。voxel 内が均一線源であるという仮定が、²²⁵Ac では物理的に成立しない。
2. **娘核種の取り扱い**：²²⁵Ac の崩壊連鎖を「親核種位置で全崩壊が起こる」として 1 本のカーネルに畳み込む慣行は、α反跳・化学的解離による ²²¹Fr・²¹³Bi の遊離（再分布）を無視している。
3. **2 核種の空間分布の独立性**：同一標的（例：PSMA）を狙う薬剤であっても、キレート・比放射能・投与時期・受容体飽和・線量率依存的な受容体発現変化により、²²⁵Ac 由来分布と ¹⁷⁷Lu 由来分布は一般に一致しない。
4. **生物学的効果の非加算性**：物理線量は加算できるが、放射線質（LET）が異なる 2 成分の生物学的効果は単純加算できない。

---

## 2. 先行研究の到達点と研究ギャップ

### 2.1 先行研究の到達点（正確な現状認識）

本研究計画の立案にあたり、当該領域の先行研究を整理した。重要なのは、**「²²⁵Ac+¹⁷⁷Lu の混合線量計算は未着手である」という主張は成立しない**という点である。具体的に、以下は既に確立または実施済みである。

- ²²⁵Ac 単核種および ¹⁷⁷Lu 単核種の Monte Carlo ベース DPK / voxel S-value / 細胞 S-value は、複数の独立グループにより生成・相互検証されている。
- ²²⁵Ac / ¹⁷⁷Lu の細胞レベル S 値は GATE 等で計算され、MIRDcell との比較で self-dose・cross-dose ともに数 % 以内の一致が報告されている。
- [¹⁷⁷Lu]Lu-PSMA 系薬剤と [²²⁵Ac]Ac-PSMA 系薬剤の併用療法に対する患者個別 dosimetry は既に実施され、両核種の voxel S-value を用いて各々の吸収線量が算出されている。
- 複数核種（²²⁵Ac / ¹⁷⁷Lu / ¹⁶¹Tb 等）を共通の微視的線量評価フレームワーク（µm オーダー分解能の DPK を用いた `D = TIA ⊗ DPK`）で扱う研究も報告されている。
- ²²⁵Ac / ¹⁷⁷Lu の混合比を変えて腫瘍制御確率を評価する microdosimetry 的検討も存在する。

したがって本研究は「未踏領域の開拓」ではなく、**既存の検証済み単核種モデルの一般化と、そこに残る前提の定量的検証**として位置づける。この位置づけは査読対応上も必須である。

### 2.2 領域ごとの成熟度マップ

| 領域 | 成熟度 | 本研究の関与 |
|---|---|---|
| ¹⁷⁷Lu 単核種 organ/voxel dosimetry | ◎ 確立 | 参照・再現検証 |
| ²²⁵Ac 単核種 organ/voxel dosimetry | ◎ 確立 | 参照・再現検証 |
| ¹⁷⁷Lu DPK / 細胞S値 | ◎ 確立 | 再現検証（ベンチマーク） |
| ²²⁵Ac DPK / 細胞S値（連鎖一括） | ◎ 確立 | 再現検証＋核種分解版へ拡張 |
| ²²⁵Ac 崩壊連鎖を核種別に分解した DPK 群 | △ 限定的 | **主要貢献 (A)** |
| ²²⁵Ac/¹⁷⁷Lu 併用の臨床 dosimetry | ◎ 実施済 | 参照・比較対象 |
| 複数核種を共通 microdosimetric framework で扱う | ○ 研究あり | 参照・整合性確保 |
| 放射能比 f を連続変数とした統一混合カーネル | △ 部分的 | 貢献 (B) |
| **空間共局在度 c を明示変数とした混合線量場の一般式** | △〜弱 | **主要貢献 (B)** |
| **娘核種再分布を含む混合場の一般化** | 弱 | **主要貢献 (A)** |
| **時間依存放射能比 f(t) と線量率効果の統合** | 弱 | **主要貢献 (C)** |
| **混合場における分解能・カットオフ由来不確かさの定量化** | 非常に弱 | **主要貢献 (D)** |
| 混合場に対する検証済み統一モデル（V&V 済） | 非常に弱 | **統合目標** |

### 2.3 研究ギャップの記述（採用する表現）

本研究計画では、以下の表現を採用する。

> ²²⁵Ac および ¹⁷⁷Lu については、それぞれ独立に検証された Monte Carlo ベースの微視的線量カーネル（DPK・細胞 S 値）が確立している。また両核種を併用する治療に対する患者個別 dosimetry、および複数核種を共通の微視的線量評価枠組みで扱う試みも既に報告されている。
>
> 一方、²²⁵Ac/¹⁷⁷Lu 併用療法に対して、(i) ²²⁵Ac 崩壊連鎖の娘核種再分布、(ii) 両核種の微視的空間分布の非共局在性、(iii) 半減期差および薬物動態差に起因する時間依存放射能比、(iv) カーネル生成に伴う分解能・カットオフ由来の不確かさ、を単一の定式化に統合し、微視的線量分布とその不確かさを一貫して評価できる検証済みの一般化フレームワークは確立されていない。
>
> とくに、これら 4 因子のうちどれが混合場の微視的線量分布を支配するのか、その相対的寄与は定量化されていない。

### 2.4 明示的に主張しないこと（査読リスク管理）

以下は本研究の新規性として**主張しない**。

- ❌ 「²²⁵Ac+¹⁷⁷Lu の混合場線量計算は未定式化である」
- ❌ 「混合 DPK `K_mix = f·K_Ac + (1−f)·K_Lu` を初めて提案する」（物理線量の線形性から自明）
- ❌ 「²²⁵Ac/¹⁷⁷Lu 併用の dosimetry を初めて行う」
- ❌ 「複数核種を共通フレームワークで扱うのは初めてである」

---

## 3. 研究目的・研究設問・仮説

### 3.1 全体目的

²²⁵Ac/¹⁷⁷Lu 併用核医学治療における微視的線量場を、**放射能比・空間共局在度・娘核種再分布率・時間構造**の関数として一般化して記述し、その不確かさを定量化した検証済み計算フレームワークを構築する。

### 3.2 研究設問（RQ）と仮説（H）

**RQ1（カーネル基盤）**
²²⁵Ac 崩壊連鎖を核種別に分解した DPK 群を、1 µm 分解能・50 µm カットオフの条件下でエネルギー保存則を満たす形で生成できるか。またその際のカットオフ誤差・分解能誤差はどの程度か。

> **H1**：1 µm ボクセル・50 µm カットオフでは、²²⁵Ac 連鎖の α 成分についてはエネルギー保存が数 % 以内で成立するが、²¹³Bi の β 成分および特性X線・γ線成分については有意なエネルギー漏出が生じ、カットオフ誤差が支配的となる。¹⁷⁷Lu ではカットオフ誤差が ²²⁵Ac より 1 桁以上大きい。

**RQ2（空間共局在）**
²²⁵Ac と ¹⁷⁷Lu の微視的分布の共局在度 `c` を変化させたとき、混合場の線量不均一性指標（`CV_D`、`D_90`、cold volume fraction）はどのように変化するか。

> **H2**：`c` の低下（非共局在化）は、²²⁵Ac 成分の局所線量ピークを鋭くする一方で、²²⁵Ac 由来の cold region を ¹⁷⁷Lu の cross-fire が補償するため、`D_90` は非単調に振る舞う。すなわち**最適な共局在度は c = 1（完全共局在）ではない**。

**RQ3（娘核種再分布）**
²²¹Fr / ²¹³Bi の遊離率を変化させたとき、腫瘍・正常組織の微視的線量分布は放射能比 `f` や共局在度 `c` の変化と比べてどの程度敏感か。

> **H3**：娘核種遊離率は、腫瘍微視的線量に対しては ²¹³Bi（T½ = 45.6 min、α 8.38 MeV を伴う）の遊離が支配的因子となり、その感度は放射能比 `f` の不確かさに匹敵するかそれを上回る。

**RQ4（時間構造と生物効果）**
半減期差（9.92 d vs 6.647 d）および薬物動態差により時間依存する放射能比 `f(t)` は、LQ/MKM 枠組みでの生物学的実効線量にどの程度影響するか。

> **H4**：物理線量では加算性が成立するが、LQ の二次項に起因する交差項 `2β·D_Ac·D_Lu` および放射線質依存の `α` により、生物学的実効線量は「総物理線量」の単一関数として表現できず、投与順序・投与間隔に依存する。

**RQ5（統合・支配因子の同定）**
上記 4 因子（`f`、`c`、娘核種遊離率、分解能／カットオフ）のうち、混合場微視的線量の不確かさに対する寄与が最大のものはどれか。

> **H5**：臨床的に想定される範囲では、不確かさ寄与は「空間共局在度・微視的分布の不確かさ > 娘核種遊離率 > 放射能比 > 数値的（分解能・カットオフ）誤差」の順になる。すなわち、計算精度の向上よりも微視的分布の実測が優先課題である。

---

## 4. 研究の新規性と独創性

| # | 貢献 | 内容 | なぜ新規か |
|---|---|---|---|
| A | **マルチ核種分解カーネル** | ²²⁵Ac 崩壊連鎖を親核種一括ではなく 7 核種（²²⁵Ac, ²²¹Fr, ²¹⁷At, ²¹³Bi, ²¹³Po, ²⁰⁹Tl, ²⁰⁹Pb）に分解し、核種ごとに独立した DPK と独立した空間分布を許す定式化 | 既存の ²²⁵Ac DPK は連鎖が親核種位置で完結すると仮定した「実効カーネル」。遊離娘核種を独立線源として扱う微視的線量場の一般化は確立していない |
| B | **共局在度を明示変数化** | 混合場を `f`（放射能比）と `c`（空間相関）の 2 変数関数として定式化し、`D(r; f, c)` の位相図を得る | 既存研究は `f` を変数とするが、`c` は暗黙に 1（完全共局在）または固定。`c` を連続変数として扱う一般式と感度解析は未確立 |
| C | **時間構造の統合** | `f(t)` および線量率の時間発展を LQ/MKM に接続し、投与順序・間隔を設計変数として扱う | 既存の混合比検討は時間積分後の比を扱う。半減期差に起因する `f(t)` の効果は定量化されていない |
| D | **混合場 UQ** | 4 因子の不確かさを共通の枠組みで伝播させ、分散寄与を分解（感度指標の算出） | 単核種 dosimetry の UQ ガイダンスは存在するが、混合場・微視的スケールへの拡張は未整備 |
| E | **再現可能な公開実装** | 検証済みカーネルライブラリ（HDF5）と畳み込み・UQ コードを公開 | 領域内でカーネルの生成条件（分解能・カットオフ・規格化）が非統一であり、比較可能性が低い |

**独創性の核心**：本研究は「混合場の線量式を新しく作る」研究ではない。**線形重ね合わせという既知の結論が成立するための前提を 4 つ特定し、それらを明示的なパラメータとして定式化に組み込み、どの前提の破れが臨床的に重要かを定量的に順序付ける**研究である。

---

## 5. 理論的枠組み（定式化）

### 5.1 基本式

微視的位置 `r` における吸収線量は、核種 `i` ごとの時間積分放射能密度 `ã_i(r)` と核種固有の dose point kernel `K_i(r)` の畳み込み和として

$$
D(\mathbf{r}) \;=\; \sum_{i \in \mathcal{N}} \int \tilde{a}_i(\mathbf{r}') \, K_i(\mathbf{r}-\mathbf{r}') \, d^3\mathbf{r}'
\;=\; \sum_{i \in \mathcal{N}} \left( \tilde{a}_i \otimes K_i \right)(\mathbf{r})
$$

ここで核種集合は

$$
\mathcal{N} = \underbrace{\{{}^{225}\mathrm{Ac},\, {}^{221}\mathrm{Fr},\, {}^{217}\mathrm{At},\, {}^{213}\mathrm{Bi},\, {}^{213}\mathrm{Po},\, {}^{209}\mathrm{Tl},\, {}^{209}\mathrm{Pb}\}}_{^{225}\mathrm{Ac\ chain}} \;\cup\; \{{}^{177}\mathrm{Lu}\}
$$

とする。**²²⁵Ac 連鎖を 1 核種として扱わないことが本研究の第一の一般化である。**

### 5.2 放射能比と空間分布の分離

各核種の時間積分放射能密度を、総量・比率・規格化空間分布に分解する。

$$
\tilde{a}_i(\mathbf{r}) = \tilde{A}_{\mathrm{tot}} \cdot f_i \cdot p_i(\mathbf{r}), \qquad \int p_i(\mathbf{r})\, d^3\mathbf{r} = 1, \qquad \sum_i f_i = 1
$$

2 核種（連鎖を一括した近似）に縮約した場合、`f ≡ f_Ac` として

$$
D(\mathbf{r};f) = \tilde{A}_{\mathrm{tot}} \Big[ f\,\big(p_{\mathrm{Ac}} \otimes K_{\mathrm{Ac}}\big)(\mathbf{r}) + (1-f)\,\big(p_{\mathrm{Lu}} \otimes K_{\mathrm{Lu}}\big)(\mathbf{r}) \Big]
$$

**完全共局在（`p_Ac = p_Lu = p`）の特殊ケースに限り**、混合カーネルが定義できる。

$$
K_{\mathrm{mix}}(\mathbf{r};f) = f\,K_{\mathrm{Ac}}(\mathbf{r}) + (1-f)\,K_{\mathrm{Lu}}(\mathbf{r}), \qquad D = \tilde{A}_{\mathrm{tot}}\,(p \otimes K_{\mathrm{mix}})
$$

この式自体は線形性から自明であり、本研究の新規性ではない。**本研究の主張は、この「混合カーネルへの縮約」が成立する条件を明示し、それが破れる場合の一般式を扱うことである。**

### 5.3 空間共局在度のモデル化

`p_Ac ≠ p_Lu` を許す。共局在度を次の分解でパラメータ化する。

$$
p_{\mathrm{Lu}}(\mathbf{r}) = c\, p_{\mathrm{Ac}}(\mathbf{r}) + (1-c)\, q(\mathbf{r}), \qquad c \in [0,1]
$$

ここで `q(r)` は `p_Ac` と統計的に独立（実装上は voxel 単位でランダム置換、または標的抗原発現とは独立な分布）となる規格化分布。`c = 1` が完全共局在、`c = 0` が完全非共局在に対応する。

検証用の共局在度指標として、voxel 値間の Pearson 相関係数

$$
\rho = \frac{\mathrm{Cov}\!\left(p_{\mathrm{Ac}}, p_{\mathrm{Lu}}\right)}{\sigma_{p_{\mathrm{Ac}}}\, \sigma_{p_{\mathrm{Lu}}}}
$$

および Manders 共局在係数を併用し、モデルパラメータ `c` と実測可能量 `ρ` の対応関係を確立する（これにより将来の顕微オートラジオグラフィ実測と接続可能になる）。

### 5.4 娘核種再分布のモデル化

核種 `i` の娘核種が親核種位置に保持される割合を保持率 `η_i ∈ [0,1]` とし、遊離分は独立分布 `p_i^free(r)`（例：血流・間質への拡散を表す広がった分布）に従うとする。

$$
p_i(\mathbf{r}) = \eta_i \, p_{\mathrm{parent}}(\mathbf{r}) + (1-\eta_i)\, p_i^{\mathrm{free}}(\mathbf{r})
$$

この定式化により、従来の「連鎖一括カーネル」は `η_i = 1 ∀i` の特殊ケースとして回収される。

$$
K_{\mathrm{Ac}}^{\mathrm{chain}}(\mathbf{r}) \;=\; \sum_{i \in {}^{225}\mathrm{Ac\ chain}} n_i\, K_i(\mathbf{r}) \qquad (\eta_i = 1,\ \forall i)
$$

ここで `n_i` は親核種 1 崩壊あたりの核種 `i` の生成数（分岐比を考慮）。この回収関係の数値的確認を検証項目 V3 とする。

### 5.5 時間依存放射能比

物理崩壊と生物学的クリアランスを併せた実効崩壊定数 `λ_i^eff = λ_i^phys + λ_i^bio` を用い、瞬時放射能比は

$$
f(t) = \frac{A_{\mathrm{Ac}}(0)\, e^{-\lambda_{\mathrm{Ac}}^{\mathrm{eff}} (t - t_{\mathrm{Ac}})}}{A_{\mathrm{Ac}}(0)\, e^{-\lambda_{\mathrm{Ac}}^{\mathrm{eff}} (t-t_{\mathrm{Ac}})} + A_{\mathrm{Lu}}(0)\, e^{-\lambda_{\mathrm{Lu}}^{\mathrm{eff}} (t-t_{\mathrm{Lu}})}}
$$

となり、投与時刻差 `Δt = t_Lu − t_Ac` を設計変数として導入できる。時間積分放射能比 `f̃` が同一でも `f(t)` の時間発展は `Δt` に依存する点が重要である。

### 5.6 生物学的効果層（非加算性）

物理線量は加算可能である。

$$
D_{\mathrm{total}}(\mathbf{r}) = D_{\mathrm{Ac}}(\mathbf{r}) + D_{\mathrm{Lu}}(\mathbf{r})
$$

しかし生物学的効果は一般に加算できない。線形二次（LQ）モデルを放射線質別に適用すると、細胞生存率は

$$
S = \exp\Big[ -\big(\alpha_{\mathrm{Ac}} D_{\mathrm{Ac}} + \alpha_{\mathrm{Lu}} D_{\mathrm{Lu}}\big) - \big(\sqrt{\beta_{\mathrm{Ac}}} D_{\mathrm{Ac}} + \sqrt{\beta_{\mathrm{Lu}}} D_{\mathrm{Lu}}\big)^2 \, G(\Lambda) \Big]
$$

となり、二次項の展開により**交差項** `2√(β_Ac β_Lu) D_Ac D_Lu · G` が現れる。ここで `G(Λ)` は線量率・時間分割に依存する Lea–Catcheside 因子であり、`f(t)` と `Δt` を通じて時間構造に依存する。

$$
G = \frac{2}{\dot{D}_{\mathrm{tot}}^2 T^2}\int_0^T \! dt \int_0^t \! dt'\; \dot{D}(t)\,\dot{D}(t')\, e^{-\mu (t-t')}
$$

したがって

$$
E_{\mathrm{mix}} \neq E\big(D_{\mathrm{Ac}} + D_{\mathrm{Lu}}\big)
$$

であり、**物理線量の線形性と生物学的効果の非線形性を分離して扱う**ことが本研究の設計方針である。α線成分については微視的線量分布（specific energy `z` の単一事象分布 `f_1(z)`）に基づく MKM 的取り扱いを併用し、低線量域での効果の飽和・確率性を考慮する。

### 5.7 評価指標

微視的線量場 `D(r)` から以下を算出する。

- 線量体積ヒストグラム（微視的 DVH）および `D_10`、`D_50`、`D_90`
- 不均一性 `CV_D = σ_D / ⟨D⟩`
- cold volume fraction `V_{<D_th}`（治療閾値未満の体積分率）
- 等価一様線量 `EUD`
- RBE 重み付け線量および生存率 `S`、腫瘍制御確率 `TCP`
- 細胞核ヒット確率分布・`f_1(z)`（α成分の確率的性質）

---

## 6. 研究方法

### Phase 0：系統的文献レビュー（3 ヶ月）

**目的**：研究ギャップの厳密な証拠固め（§2 の主張を査読可能な水準に）。

- 対象期間：2015–2026（重点 2020–2026）
- データベース：PubMed / Scopus / Web of Science / Google Scholar / arXiv
- 検索式（詳細は付録C）
- PRISMA-ScR 準拠のスコーピングレビューとして実施し、フロー図・抽出表を作成
- 抽出項目：核種、スケール（organ / voxel / cellular / sub-cellular）、MC コード、分解能、カットオフ、規格化法、娘核種の扱い（`η` 相当）、共局在の扱い、時間構造の扱い、UQ の有無、検証対象
- **成果物**：レビュー表（§2.2 の成熟度マップの根拠）、単独論文化（Paper I 候補）

**重要**：Phase 0 の結果により §2.3 のギャップ記述を改訂する。既に本研究の主要貢献 A–D を満たす先行研究が見つかった場合の対応は §11 に記載。

### Phase 1：核種別 DPK の生成と検証（6 ヶ月）

**1-1. 計算条件**

| 項目 | 設定 |
|---|---|
| コード | Geant4（`G4EmStandardPhysics_option4`）＋ Geant4-DNA（低エネルギー電子検証用）、GATE（クロスチェック） |
| 媒質 | 液体水（ρ = 1.0 g/cm³）、無限均質、点等方線源 |
| スコアリング格子 | 1 µm 等方ボクセル（球殻スコアリングと併用） |
| カットオフ半径 | 50 µm（基準）＋ 20 / 100 / 200 / 500 µm（感度解析） |
| 生成/輸送カット | 1 µm 相当（＋ Geant4-DNA では track-structure モード） |
| 統計 | 各核種 10⁷–10⁸ 崩壊、統計誤差 < 1 %（主要領域） |
| 核データ | ICRP Publication 107 / MIRD 崩壊スキーム |
| 出力 | 球殻 DPK `K(r)`（Gy·kg/decay 換算）および 3D voxel kernel（HDF5） |

**1-2. 生成するカーネル**

- ²²⁵Ac 連鎖の各核種（7 核種）について個別 DPK：α成分、β成分、γ/X線成分、Auger 電子成分を分離出力
- ²²⁵Ac 連鎖一括の実効 DPK（`η = 1`）
- ¹⁷⁷Lu の DPK（β成分、γ成分を分離出力）
- 参考核種として ¹⁶¹Tb を追加（Auger 成分の寄与比較、拡張性の実証）

**1-3. 検証（V&V）**

| ID | 検証項目 | 判定基準 |
|---|---|---|
| V1 | エネルギー保存：`∫ D(r) ρ d³r` vs 崩壊あたり総放出エネルギー | カットオフ内成分で偏差 < 2 %（α）、漏出量を明示 |
| V2 | 公表 DPK / S 値との比較（²²⁵Ac、¹⁷⁷Lu の self / cross S 値、MIRDcell 対応条件） | self S 値 < 5 %、cross S 値 < 10 % |
| V3 | 連鎖一括カーネルの回収：`Σ n_i K_i` vs 一括 MC（`η=1`） | 偏差 < 2 % |
| V4 | コード間比較：Geant4 vs GATE vs Geant4-DNA | α飛程 < 2 %、β深部線量 < 5 % |
| V5 | 解析近似との比較（α：Bragg曲線・CSDA飛程、β：既知 β DPK 近似式） | 傾向一致 |
| V6 | 分解能感度：0.5 / 1 / 2 / 5 µm での DVH 指標変化 | 分解能誤差 `ε_res` を定量化 |
| V7 | カットオフ感度：20–500 µm | カットオフ誤差 `ε_cut` を定量化 |

**1-4. 数値的不確かさの定義**

$$
\varepsilon_{\mathrm{cut}}(R) = 1 - \frac{\int_{r<R} D(\mathbf{r})\,\rho\, d^3\mathbf{r}}{E_{\mathrm{tot}}^{\mathrm{decay}}}, \qquad
\varepsilon_{\mathrm{res}}(h) = \frac{\left\| D_h - D_{h_0} \right\|_2}{\left\| D_{h_0} \right\|_2}
$$

（`h_0` は最小ボクセルサイズ）

**成果物**：公開カーネルライブラリ v1.0、検証報告（Paper II の主要部）

### Phase 2：混合場フレームワークの実装（4 ヶ月）

- Python 実装（NumPy / CuPy、FFT 畳み込み）。1 mm³ 観測体積（= 10⁹ voxel @ 1 µm）に対するマルチスケール畳み込み戦略を採用：
  - 近傍場（< 50 µm）：1 µm 直接畳み込み
  - 遠方場（> 50 µm）：粗格子（10 µm）＋補間、またはカットオフ外成分を解析的に補償
- 核種別カーネル・核種別分布を入力に取る一般畳み込みエンジン
- カーネル規格化・単位系の統一（全カーネルを同一分解能・同一規格化に整合させる整合化手順を明文化）
- 検証：完全共局在・`η=1` の条件で既存の 2 核種混合計算（`f·K_Ac + (1−f)·K_Lu`）を再現すること（V8）

### Phase 3：微視的分布モデルと共局在・娘核種感度解析（6 ヶ月）

**3-1. 微視的分布モデル**

段階的に複雑化した幾何モデルを用いる。

1. **理想モデル**：単細胞（核・細胞質・膜）、細胞クラスター（多細胞球）。細胞半径 `R_C`、核半径 `R_N` をパラメータ化。
2. **確率的腫瘍モデル**：血管を中心とした取り込み勾配（Krogh cylinder 型）、抗原発現の不均一性（対数正規分布）、抗原陰性細胞分率
3. **画像由来モデル**（可能な場合）：既報の顕微オートラジオグラフィ／免疫染色データに基づく分布パターンの再現

**3-2. 感度解析設計**

| パラメータ | 範囲 |
|---|---|
| 放射能比 `f`（時間積分ベース） | 0, 0.1, 0.25, 0.5, 0.75, 0.9, 1.0 |
| 共局在度 `c` | 0, 0.25, 0.5, 0.75, 1.0 |
| ²¹³Bi 保持率 `η_Bi` | 0.3, 0.5, 0.7, 0.9, 1.0 |
| ²²¹Fr 保持率 `η_Fr` | 0.5, 0.7, 0.9, 1.0 |
| 抗原陰性細胞分率 | 0, 0.1, 0.3 |
| 取り込み不均一性（対数正規 σ） | 0.2, 0.5, 1.0 |
| 投与間隔 `Δt` | 0, 1, 3, 7, 14 d |

全組合せではなく、Latin Hypercube Sampling による効率的サンプリング（N ≈ 2000–5000）と、Sobol 分散分解による主効果・交互作用の定量化を行う。

**3-3. 主要出力**

- `D_90(f, c)`、`CV_D(f, c)`、`V_{<D_th}(f, c)` の 2 次元位相図
- H2 の検証：`D_90` を最大化する `c*` が 1 未満となるか
- H3 の検証：`∂D_90/∂η_Bi` と `∂D_90/∂f` の比較

### Phase 4：不確かさ定量化（UQ）（4 ヶ月）

4 因子の不確かさを共通枠組みで伝播させる。

$$
\sigma^2_{D_{90}} \;\approx\; \underbrace{\sigma^2_{\mathrm{spatial}}}_{c,\ p_i} + \underbrace{\sigma^2_{\mathrm{daughter}}}_{\eta_i} + \underbrace{\sigma^2_{\mathrm{activity}}}_{f,\ \tilde{A}} + \underbrace{\sigma^2_{\mathrm{numerical}}}_{\varepsilon_{\mathrm{cut}},\ \varepsilon_{\mathrm{res}},\ \mathrm{MC}} + \text{（交互作用項）}
$$

- 手法：Sobol 感度指標（一次 `S_i`、全次 `S_{Ti}`）、Monte Carlo 伝播、代理モデル（Polynomial Chaos または Gaussian Process）による計算コスト削減
- EANM の不確かさ解析ガイダンスの枠組みと整合させ、臨床線量評価へ接続可能な形で報告
- **H5 の検証**：分散寄与の順序付け

### Phase 5：生物学的効果層と臨床シナリオへの適用（5 ヶ月）

- 放射線質別 LQ パラメータ（`α_Ac`、`β_Ac`、`α_Lu`、`β_Lu`）を文献値レンジで設定し、交差項・`G(Λ)` を含む生存率計算
- α成分については specific energy 分布 `f_1(z)` を算出し、MKM 的な飽和補正線量との比較
- RBE 重み付け線量 `D_RBE` を定義し、物理線量ベース評価との乖離を定量化
- 臨床シナリオ解析：
  - シナリオ A：同時投与、完全共局在（理想）
  - シナリオ B：同時投与、部分共局在（現実的）
  - シナリオ C：逐次投与（`Δt` = 7, 14 d）
  - シナリオ D：¹⁷⁷Lu 治療抵抗性（抗原低発現）モデル
- 各シナリオで TCP と正常組織（腎・骨髄・唾液腺）線量を評価し、治療設計上の含意を導出

### Phase 6：統合・論文化（4 ヶ月）

- フレームワークの総合検証、ドキュメント整備、コード・カーネル公開
- 博士論文執筆

---

## 7. 研究計画（3 年・四半期別）

| 年 | Q | 主要活動 | マイルストーン／成果物 |
|---|---|---|---|
| 1 | Q1 | Phase 0 系統的レビュー、計算環境構築、Geant4/GATE 習熟 | レビュー抽出表 v1、環境検証完了 |
| 1 | Q2 | Phase 0 完了、Phase 1 開始（¹⁷⁷Lu・²²⁵Ac 一括カーネル） | **M1**：レビュー論文投稿（Paper I） |
| 1 | Q3 | Phase 1 核種別カーネル生成、V1–V5 検証 | カーネルライブラリ v0.9 |
| 1 | Q4 | Phase 1 完了（V6–V7 分解能・カットオフ感度） | **M2**：カーネル v1.0 公開、国内学会発表 |
| 2 | Q1 | Phase 2 畳み込みエンジン実装、V8 検証 | フレームワーク v0.5 |
| 2 | Q2 | Phase 3 分布モデル構築、感度解析実行 | **M3**：カーネル検証論文投稿（Paper II） |
| 2 | Q3 | Phase 3 完了（`f`–`c` 位相図、娘核種感度） | 位相図データセット |
| 2 | Q4 | Phase 4 UQ 実行、代理モデル構築 | **M4**：国際学会発表（EANM / SNMMI / MIC） |
| 3 | Q1 | Phase 4 完了、Phase 5 生物モデル層実装 | UQ 報告書 |
| 3 | Q2 | Phase 5 臨床シナリオ解析 | **M5**：主論文投稿（Paper III） |
| 3 | Q3 | Phase 6 統合検証、コード公開整備 | フレームワーク v1.0 公開 |
| 3 | Q4 | 博士論文執筆・審査 | **M6**：学位論文提出 |

---

## 8. 期待される成果

### 8.1 学術的成果

1. **²²⁵Ac 崩壊連鎖を核種分解した検証済み µm 分解能カーネルライブラリ**（公開）。娘核種再分布を扱う研究の共通基盤となる。
2. **`f`–`c` 位相図**：放射能比と共局在度の 2 次元空間における線量不均一性マップ。併用療法の設計指針として直接利用可能。
3. **支配因子の定量的順序付け**：混合場微視的線量の不確かさに最も寄与する因子の同定（H5）。計算高精度化よりも実測すべき量を示す。
4. **非加算性の定量化**：物理線量加算性と生物効果非加算性の乖離幅を、臨床的に想定される条件範囲で提示。
5. **カーネル生成条件の標準化提案**：分解能・カットオフ・規格化の報告要件（reporting checklist）。

### 8.2 臨床的含意（仮説段階）

- 併用療法における最適放射能比・最適投与間隔の理論的推定
- 「共局在が高いほど良い」という直観の検証（H2 が支持されれば、むしろ意図的な非共局在設計に意味がある）
- ²²⁵Ac の娘核種再分布に関する実測データ取得の優先度提示

### 8.3 想定される論文構成

| # | 仮題 | 種別 | 投稿先候補 |
|---|---|---|---|
| I | Microdosimetric dose calculation for ²²⁵Ac/¹⁷⁷Lu combination radiopharmaceutical therapy: a scoping review | Review | EJNMMI Physics / Phys Med Biol |
| II | Nuclide-resolved dose point kernels for the ²²⁵Ac decay chain at 1 µm resolution: generation, validation, and resolution/cutoff uncertainty | Original | Med Phys / Phys Med Biol |
| III | A unified microdosimetric framework for ²²⁵Ac/¹⁷⁷Lu mixed fields: activity ratio, spatial co-localization, daughter redistribution, and uncertainty | Original | Med Phys / EJNMMI Physics |
| （IV） | Biological effect non-additivity in ²²⁵Ac/¹⁷⁷Lu mixed fields | Original | Int J Radiat Biol / Radiat Res |

---

## 9. リスクと代替案

| # | リスク | 影響 | 対策・代替案 |
|---|---|---|---|
| R1 | Phase 0 で主要貢献 A–D を既に満たす先行研究が発見される | 新規性喪失 | 貢献の重心を移す：(a) 独立再現検証＋コード間比較（reproducibility 研究として成立）、(b) UQ（最も未整備な貢献 D）へ重心移動、(c) 未報告の核種組合せ（²²⁵Ac/¹⁶¹Tb 等）へ拡張。**Phase 0 を最初に置く設計理由がこれである。** |
| R2 | 娘核種保持率 `η_i` の実測データが乏しい | パラメータ設定の根拠不足 | `η_i` を推定せず**感度解析パラメータとして扱う**。「どの `η` 範囲で結論が変わるか」を出力とする（これ自体が有用な成果） |
| R3 | 1 µm × 1 mm³ の計算コストが過大 | 実行不能 | マルチスケール畳み込み（§Phase 2）、GPU（CuPy）活用、対称性利用、観測体積を (200 µm)³ に縮小した上でカットオフ補償項を解析的に加算 |
| R4 | 公表カーネル値との不一致（V2 不合格） | 検証失敗 | 条件差（媒質、核データ版、カット値、規格化）を系統的に切り分ける比較マトリクスを事前設計。不一致自体を報告価値ある知見として扱う |
| R5 | LQ/MKM パラメータの文献値ばらつきが大きい | 生物モデル層の信頼性 | 単一値を用いず**パラメータレンジでの区間推定**とし、物理線量結果（Phase 1–4）と生物結果（Phase 5）を独立に報告。物理層の結論は生物パラメータに依存しない設計 |
| R6 | 臨床データへのアクセス不可 | シナリオ解析の現実性 | 既報の薬物動態パラメータ（文献値）に基づく in silico 仮想患者コホートで代替。臨床データは得られれば追加検証として扱う |
| R7 | Geant4 の低エネルギー電子輸送の妥当性 | Auger / 低エネルギーβ成分の精度 | Geant4-DNA との併用比較（V4）、該当成分の寄与率を明示して限界を記述 |

---

## 10. 計算資源・データ管理・再現性

### 10.1 計算資源

| 用途 | 見積 |
|---|---|
| Phase 1 カーネル生成（8 核種 × 感度条件） | 約 30,000–60,000 CPU 時間（要 HPC / クラスタ） |
| Phase 3 感度解析（LHS N≈5000） | GPU 1–2 基 × 数週間（FFT 畳み込み） |
| Phase 4 UQ（代理モデル使用） | Phase 3 の 20–30 % 追加 |
| ストレージ | カーネル・中間結果で 2–5 TB |

### 10.2 再現性確保

- 全計算スクリプト・マクロを Git 管理、リリースに DOI 付与（Zenodo）
- カーネルは HDF5＋メタデータ（コード版、物理リスト、カット値、核データ版、規格化、統計誤差）を必須フィールドとして格納
- 乱数シード・実行環境をコンテナ（Docker/Apptainer）で固定
- FAIR 原則に基づくデータ公開

### 10.3 倫理

- 本研究は計算研究であり、新規のヒト被験者・動物実験を含まない。
- 既存臨床データを二次利用する場合は、所属機関の倫理審査委員会の承認および匿名化済みデータの使用に限定する。

---

## 11. 参考文献

### 11.1 基盤文献（確定）

1. Bolch WE, Eckerman KF, Sgouros G, Thomas SR. MIRD Pamphlet No. 21: A generalized schema for radiopharmaceutical dosimetry—standardization of nomenclature. *J Nucl Med*. 2009;50(3):477–484.
2. Sgouros G, Roeske JC, McDevitt MR, et al. MIRD Pamphlet No. 22: Radiobiology and dosimetry of α-particle emitters for targeted radionuclide therapy. *J Nucl Med*. 2010;51(2):311–328.
3. Bolch WE, Bouchet LG, Robertson JS, et al. MIRD Pamphlet No. 17: The dosimetry of nonuniform activity distributions—radionuclide S values at the voxel level. *J Nucl Med*. 1999;40(1):11S–36S.
4. Vaziri B, Wu H, Dhawan AP, Du P, Howell RW. MIRD Pamphlet No. 25: MIRDcell V2.0 software tool for dosimetric analysis of biologic response of multicellular populations. *J Nucl Med*. 2014;55(9):1557–1564.
5. Agostinelli S, et al. Geant4—a simulation toolkit. *Nucl Instrum Methods Phys Res A*. 2003;506(3):250–303.
6. Incerti S, et al. The Geant4-DNA project. *Int J Model Simul Sci Comput*. 2010;1(2):157–178.
7. Jan S, et al. GATE: a simulation toolkit for PET and SPECT. *Phys Med Biol*. 2004;49(19):4543–4561.
8. Sarrut D, et al. A review of the use and potential of the GATE Monte Carlo simulation code for radiation therapy and dosimetry applications. *Med Phys*. 2014;41(6):064301.
9. ICRP. Publication 107: Nuclear decay data for dosimetric calculations. *Ann ICRP*. 2008;38(3).
10. Eckerman KF, Endo A. *MIRD: Radionuclide Data and Decay Schemes*. 2nd ed. Reston, VA: SNMMI; 2008.
11. ICRU. Report 36: Microdosimetry. Bethesda, MD: ICRU; 1983.
12. Hawkins RB. A microdosimetric-kinetic model of cell death from exposure to ionizing radiation of any LET, with experimental and clinical applications. *Int J Radiat Biol*. 1996;69(6):739–755.
13. Gear JI, Cox MG, Gustafsson J, et al. EANM practical guidance on uncertainty analysis for molecular radiotherapy absorbed dose calculations. *Eur J Nucl Med Mol Imaging*. 2018;45(13):2456–2474.
14. Strosberg J, El-Haddad G, Wolin E, et al. Phase 3 trial of ¹⁷⁷Lu-Dotatate for midgut neuroendocrine tumors. *N Engl J Med*. 2017;376(2):125–135.
15. Sartor O, de Bono J, Chi KN, et al. Lutetium-177–PSMA-617 for metastatic castration-resistant prostate cancer. *N Engl J Med*. 2021;385(12):1091–1103.
16. Kratochwil C, Bruchertseifer F, Giesel FL, et al. ²²⁵Ac-PSMA-617 for PSMA-targeted α-radiation therapy of metastatic castration-resistant prostate cancer. *J Nucl Med*. 2016;57(12):1941–1944.

### 11.2 Phase 0 で確定させる文献群（プレースホルダ）

以下は §2.1 で言及した先行研究に対応する。**書誌情報は Phase 0 の系統的レビューで一次資料から確定させる（現時点で未確定のものを引用しない方針）。**

- [P1] ²²⁵Ac/¹⁷⁷Lu 細胞レベル S 値の GATE 計算と MIRDcell 比較（2024 年頃）
- [P2] [¹⁷⁷Lu]Lu-PSMA-I&T ＋ [²²⁵Ac]Ac-PSMA-I&T 併用療法の患者個別 dosimetry（2023 年頃）
- [P3] ²²⁵Ac / ¹⁷⁷Lu / ¹⁶¹Tb を共通フレームワークで扱う µm 分解能 DPK 研究（2025 年頃、*Med Phys*）
- [P4] ²²⁵Ac/¹⁷⁷Lu 併用の混合比を変えた microdosimetry / TCP 検討
- [P5] ²²⁵Ac 娘核種（²²¹Fr, ²¹³Bi）の *in vivo* 再分布に関する実験研究
- [P6] ²²⁵Ac 崩壊連鎖の核データ評価に関する研究

---

## 付録A：記号一覧

| 記号 | 意味 | 単位 |
|---|---|---|
| `D(r)` | 位置 `r` における吸収線量 | Gy |
| `ã_i(r)` | 核種 `i` の時間積分放射能密度 | Bq·s·m⁻³ |
| `K_i(r)` | 核種 `i` の dose point kernel | Gy·m³/(Bq·s) |
| `Ã_tot` | 総時間積分放射能 | Bq·s |
| `f_i`, `f` | 核種 `i` の放射能分率／²²⁵Ac 分率 | — |
| `p_i(r)` | 核種 `i` の規格化空間分布 | m⁻³ |
| `c` | 空間共局在度（1 = 完全共局在） | — |
| `ρ` | voxel 値の Pearson 相関（`c` の実測対応量） | — |
| `η_i` | 娘核種 `i` の親核種位置保持率 | — |
| `n_i` | 親核種 1 崩壊あたりの核種 `i` 生成数 | — |
| `Δt` | ¹⁷⁷Lu と ²²⁵Ac の投与時刻差 | d |
| `ε_cut`, `ε_res` | カットオフ誤差・分解能誤差 | — |
| `CV_D` | 線量変動係数 | — |
| `D_10/50/90` | 体積の 10/50/90 % が受ける最小線量 | Gy |
| `z`, `f_1(z)` | 比エネルギー、単一事象比エネルギー分布 | Gy, Gy⁻¹ |
| `G(Λ)` | Lea–Catcheside 時間因子 | — |
| `S_i`, `S_{Ti}` | Sobol 一次／全次感度指標 | — |

## 付録B：核データ（要確定：ICRP Pub. 107 準拠で最終確認）

### ²²⁵Ac 崩壊連鎖

| 核種 | 半減期 | 主崩壊 | 主要放射線エネルギー | 備考 |
|---|---|---|---|---|
| ²²⁵Ac | 9.92 d | α | ≈ 5.8 MeV | 親核種 |
| ²²¹Fr | 4.8 min | α | ≈ 6.3 MeV | 再分布の可能性 |
| ²¹⁷At | 32.3 ms | α | ≈ 7.1 MeV | 実質的に *in situ* |
| ²¹³Bi | 45.6 min | β⁻ (97.8 %) / α (2.2 %) | β Emax ≈ 1.4 MeV、γ 440 keV | **再分布の主要因** |
| ²¹³Po | 4.2 µs | α | ≈ 8.38 MeV | 実質的に *in situ* |
| ²⁰⁹Tl | 2.16 min | β⁻ | — | 分岐 2.2 % 側 |
| ²⁰⁹Pb | 3.23 h | β⁻ | Emax ≈ 644 keV | → ²⁰⁹Bi（準安定） |

**1 崩壊あたり**：α 4 個、総α運動エネルギー ≈ 27.8 MeV、水中α飛程 ≈ 47–85 µm、LET ≈ 60–230 keV/µm。

### ¹⁷⁷Lu

| 項目 | 値 |
|---|---|
| 半減期 | 6.647 d |
| β⁻ | Emax 497 keV (79 %)、384 keV (9 %)、176 keV (12 %)、平均 ≈ 134 keV |
| γ | 113 keV (6.2 %)、208 keV (10.4 %) |
| 水中β飛程 | 平均 ≈ 0.2 mm、最大 ≈ 1.8 mm |
| LET | ≈ 0.2 keV/µm |

## 付録C：Phase 0 検索式（案）

**PubMed（概念ブロックの AND 結合）**

```
# Block 1: 核種
("225Ac" OR "Ac-225" OR "actinium-225" OR "225-actinium"
 OR "177Lu" OR "Lu-177" OR "lutetium-177")

# Block 2: 線量評価
("dosimetry" OR "microdosimetry" OR "absorbed dose"
 OR "dose point kernel" OR "S value" OR "S-value"
 OR "voxel S value" OR "specific energy" OR "dose kernel")

# Block 3: 手法
("Monte Carlo" OR "Geant4" OR "GATE" OR "MCNP" OR "PHITS"
 OR "EGSnrc" OR "TOPAS" OR "MIRDcell" OR "convolution")

# Block 4（混合場に絞る場合）
("combination" OR "combined" OR "mixed field" OR "cocktail"
 OR "tandem" OR "dual" OR "co-administration" OR "activity ratio")

# Block 5（娘核種に絞る場合）
("daughter" OR "progeny" OR "recoil" OR "redistribution"
 OR "decay chain" OR "213Bi" OR "221Fr")
```

**検索戦略**
- 検索 S1 = B1 AND B2 AND B3（母集団）
- 検索 S2 = S1 AND B4（混合場）
- 検索 S3 = S1 AND B5（娘核種）
- 期間：2015–2026（重点 2020–2026）、言語：英語・日本語
- 引用追跡（forward / backward snowballing）を全採択論文に実施
- 除外基準：診断専用、臓器平均線量のみ（微視的スケール評価なし）、会議抄録のみで方法記述が不十分なもの
