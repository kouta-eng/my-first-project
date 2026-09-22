# Phase 0：系統的文献レビュー — 抽出表（v1）

**作成日**：2026-09-22
**対象研究**：²²⁵Ac/¹⁷⁷Lu 混合放射線場に対する統一的微視的線量計算フレームワーク（`docs/research-plan-ac225-lu177-mixed-microdosimetry.md`）
**検索方法**：WebSearch による反復的ターゲット検索（PubMed/EJNMMI Physics/Med Phys/Phys Med Biol/Sci Rep/bioRxiv を横断）。本計画書 付録C の検索式を出発点とし、得られた論文の引用・関連論文を追跡（snowballing）。

**重要な限界（更新）**：本セッションの実行環境では WebFetch（一次資料の本文取得）が egress ポリシーによりブロックされており、大部分の抽出内容は WebSearch が返す抄録レベルの要約に基づく。**ただし L2（Hu et al. 2025）と L5（Tranel et al. 2022）の2件は、利用者からPDFファイルが直接提供されたため全文で確認済み**（2026-09-22）。この2件は本研究の貢献A・貢献Bそれぞれにとって最も脅威度の高い先行研究であり、全文確認によって当初の暫定評価（§3.1, §3.2）が確定した。残る文献（L1, L3, L4, L6, L8–L12）は抄録レベルの要約のままであり、**本表はなお「確定版」ではなく一次ドラフトと位置づける**。

---

## 1. 抽出表（本研究の貢献 A・B・C・D との関連順）

| # | 引用 | 年 | 誌 | スケール | 核種 | 手法 | 主要内容 | 本研究との関係 |
|---|---|---|---|---|---|---|---|---|
| L1 | Koniar H, Miller C, Rahmim A, Schaffer P, Uribe C. "A GATE simulation study for dosimetry in cancer cell and micrometastasis from the ²²⁵Ac decay chain." *EJNMMI Phys*. 2023;10:47. doi:10.1186/s40658-023-00564-5 | 2023 | EJNMMI Physics | 細胞〜微小転移巣 | ²²⁵Ac 連鎖（娘核種個別） | GATE MC、単細胞（10 µm、核8 µm）＋細胞クラスター、線源位置：膜/細胞質/核/全細胞 | ²²⁵Ac 連鎖の娘核種ごとの S 値を GATE で計算し MIRDcell と比較 | **貢献Aへの直接の先行研究**。連鎖を核種分解して S 値を出す点は既に実施済み。ただし娘核種の**保持率 `η_i` を連続変数として空間再分布をモデル化**しているかは要確認（抄録からは「線源位置固定」の設計に見える） |
| L2 | Hu Z, Qu S, Liu H, Zhang Y, Yan S, Hu A, Qiu R, Wu Z, Zhang H, Li J. "Evaluation of relative biological effectiveness of ²²⁵Ac and its decay daughters with Monte Carlo track structure simulations." *EJNMMI Phys*. 2025;12:65. doi:10.1186/s40658-025-00765-0（**全文確認済み 2026-09-22**） | 2025 | EJNMMI Physics | 細胞核（track structure、Tsinghua大学） | ²²⁵Ac 連鎖の**7核種**（²²¹Fr, ²¹⁷At, ²¹³Po, ²¹³Bi, ²⁰⁹Tl, ²⁰⁹Pb）＋²²⁵Ac 自身＋¹⁷⁷Lu（参照、計8核種） | NASIC（track structure、電子カットオフ11 eV）、V79/HSG/Renca 3細胞（核半径8.1 µm固定、domain半径0.2–0.5 µm）、mSMKM（Sato/InaniwaのSMKM拡張）でRBE算出。48条件（8核種×6分布）×10反復 | **6種の空間分布は確認の通り離散的な幾何配置カテゴリ**：①核内一様、②細胞質内一様、③全細胞一様、④細胞外、⑤細胞内外両方、⑥膜結合——連続的な保持率パラメータではない。核内均一分布でのAbstractの結果：V79細胞でAc-225のRBE_M=6.91±0.04、²²¹Fr=6.81±0.04、²¹⁷At=6.67±0.02、²¹³Po=6.43±0.05、²¹³Bi=5.91±0.09（β/α混合崩壊のため他のα核種より17%低い）、β核種²⁰⁹Tl/²⁰⁹Pbは1に近い。**分布依存性は吸収線量に対して最大80%だが、RBEに対しては最大10%**（V79細胞での`²²⁵Ac`の場合）。RBE_Mは細胞種間でも変動（V79:6.91、HSG:6.78、Renca:9.76、最大44%差）。Discussion冒頭で「To the best of our knowledge, the RBE for each individual decay daughter in the ²²⁵Ac decay chain has not been calculated」と明言し、先行研究(Rumiantcev et al.)は「全娘核種が同じ場所で崩壊すると仮定し核種別計算をしていない」と対比 | **貢献A・貢献Bの両方に強く関連する最重要先行研究、全文で確定**。核種分解自体（Table 3の核種別Gy/decay表）と離散的な幾何配置に対する感度は既に確立。**しかし、以下3点で本研究の拡張①は依然として新規性を持つ**：(i) 6分布は離散カテゴリであり、`η_i`のような0–1連続変数で束縛/遊離集団を補間するモデルではない、(ii) 単一細胞スケール（10 µm）の解析であり、本研究が想定する腫瘍スケール（1 mm³）でのTIA分布への接続は行っていない、(iii) 論文自身がDiscussionで「Zaiderらの重み付け法を用いて今後mixed radiation fieldのRBE加重線量を決定できる」と将来課題として明言しており、**本研究の混合場フレームワークが埋めるべき空白を論文自身が示唆している**。また²²⁵Acと¹⁷⁷Luは同一腫瘍に同時投与する混合場としては扱われていない（¹⁷⁷LuはRBE計算の基準線源としてのみ使用） |
| L3 | Ghaseminejad S, De Sarno D, Bauman G, Lee TY. "Framework to calculate ²²⁵Ac, ¹⁷⁷Lu, and ¹⁶¹Tb radiation dose and biological effect in metastatic castration-resistant prostate cancer treatment." *Med Phys*. 2025;52(8). doi:10.1002/mp.18035 | 2025 | Medical Physics | 細胞（2 µm分解能） | ²²⁵Ac／¹⁷⁷Lu／¹⁶¹Tb（核種ごとに独立） | 2 µm DPK＋Biological Effect Cell Kernel (BECK)、3D畳み込みでcrossfireを評価 | 臨床投与量ベースで3核種を比較、DSB数を算出（²²⁵Ac: 48 DSB/線源、¹⁷⁷Lu: 0.022、¹⁶¹Tb: 0.083 等） | 当初 P3 として想定していた論文。**µm分解能マルチ核種DPKフレームワークは既に確立**。ただし3核種は「候補としての比較」であり、**2核種を同一腫瘍に混合投与するfとcの2変数関数として扱ってはいない** |
| L4 | Yan K, Jiang Y, Wang R, Xu W, Guo J, Gao H, Chen Y, Wei S, Wang X, Zheng M, Niu B, Hu L, Dong J, Zhang Y, Wu X, Feng B, Sun L. "A fast convolution-based method for microdosimetric comparison of ²²⁵Ac, ²¹¹At, ¹⁷⁷Lu and ¹⁶¹Tb at the cell cluster scale." *Phys Med Biol*. 2025. doi:10.1088/1361-6560/ae7892 | 2025 | Phys Med Biol | 細胞クラスター | ²²⁵Ac／²¹¹At／¹⁷⁷Lu／¹⁶¹Tb（独立比較） | PHITS で核S値計算＋FFT畳み込み＋飽和補正MKモデル、標識率・クラスタサイズ・対数正規不均一性を変化 | 畳み込み再構成とPHITS直接計算の一致（偏差<5%）、²²⁵Ac のTCP90%必要量が¹⁷⁷Lu/¹⁶¹Tbの1/1000 | **本計画のPhase 2（FFT畳み込みエンジン）と方法論的にほぼ同一の先行研究**。「畳み込みでマルチ核種微視的線量比較を高速に行う」という手法自体の新規性はここで大きく減じる。ただし本研究のように**2核種を`f`（活性比）で混合し`c`（空間相関）を連続変化させる**設計ではなく、単一核種ごとの独立比較である点は維持されている |
| L5 | Tranel J, Palm S, Graves SA, Feng FY, Hope TA. "Impact of radiopharmaceutical therapy (¹⁷⁷Lu, ²²⁵Ac) microdistribution in a cancer-associated fibroblasts model." *EJNMMI Phys*. 2022;9:67. doi:10.1186/s40658-022-00497-5（**全文確認済み 2026-09-22**） | 2022 | EJNMMI Physics | mm〜µm（3mm球状腫瘍モデル、20µm voxel） | ¹⁷⁷Lu（DVK畳み込み/重ね合わせ、GATEで事前生成）／²²⁵Ac（GATE MC、full decay chain、崩壊連鎖は一括） | 腫瘍(75%)とCAF(25%)を混在させた5モデルでクラスタサイズ（`Lmean`＝92/116/181/341/1030 µm）を変化。各核種を独立にCAF or 腫瘍を線源として計算し、DVH・efficacy ratio (ER)を評価 | ²²⁵AcのER：CAF→CAF自己線量で1.5→3.7（Lmean 92→1030µm）、腫瘍→CAFでは0.8→0.1に低下。¹⁷⁷LuのERはより緩やか（1.2→2.7、0.9→0.3）。結論：クラスタが大きいほど²²⁵Acの優位性が低下し、¹⁷⁷Luの方が有効になる | **全文で以下を確定**：(1)「**As we focused on differences...radioisotopes were modeled to be with...CAF and tumors were not considered as sources at the same time**」と明記——**²²⁵Acと¹⁷⁷Luを同一腫瘍に同時投与する設計は一切行っていない**。常にどちらか一方の核種を、どちらか一方の細胞集団を線源として計算する4通りの組合せの比較。放射能比`f`という変数も存在しない。(2) Limitationsで「**Re-distribution of the parent or the ²²⁵Ac daughters were not simulated**」と明記——**娘核種再分布は明示的に非考慮**（本研究の拡張①がこの限界を埋める）。(3) `Lmean`は**2つの異なる細胞集団（CAF vs 腫瘍）間の平均距離**であり、本研究の`c`（同一組織内の2核種分布の相関係数）とは異なる幾何定義。**貢献Bとの差別化は当初評価より一層明確になった**——Tranel 2022は「単核種を使うときの標的/線源の組合せ最適化」問題であり、「2核種混合投与の設計」問題ではない。**Paper III Introductionで最重要の対比先行研究として詳述する** |
| L6 | Tranel J, Feng FY, James SS, Hope TA. "Effect of microdistribution of alpha and beta-emitters in targeted radionuclide therapies on delivered absorbed dose in a GATE model of bone marrow." *Phys Med Biol*. 2021;66(3):035016. doi:10.1088/1361-6560/abd3ef | 2021 | Phys Med Biol | mm〜µm（骨髄モデル） | ⁹⁰Y／¹⁷⁷Lu／²¹¹At／²²⁵Ac（独立比較） | GATE、血管プール限定 vs BMMI（骨髄浸潤）の2シナリオ、各核種を個別に評価 | α線源は血管壁から70 µm以内にエネルギーの大部分を沈着、骨髄毒性が理論より低い可能性 | 候補核種比較の先行研究。混合投与ではない。貢献Bとの差別化に使う参照文献 |
| L7 | Delker A, Schleske M, Liubchenko G, Berg I, Zacherl MJ, Brendel M, Gildehaus FJ, Rumiantcev M, Resch S, Hürkamp K, Wenter V, Unterrainer LM, Bartenstein P, Ziegler SI, Beyer L, Böning G. "Biodistribution and dosimetry for combined [¹⁷⁷Lu]Lu-PSMA-I&T/[²²⁵Ac]Ac-PSMA-I&T therapy using multi-isotope quantitative SPECT imaging." *Eur J Nucl Med Mol Imaging*. 2023;50(5):1280–1290. doi:10.1007/s00259-022-06092-1 | 2023 | EJNMMI | 患者（臓器/voxel） | ¹⁷⁷Lu＋²²⁵Ac（同時投与患者） | Dual-isotope 定量SPECT/CT（1時間、24時間後） | 腎臓・病変への線量を核種別に算出（RBE=5でα換算） | **P2 確定**。臨床combination dosimetryは既に実施済み。ただし各核種の空間分布は**実測SPECT画像をそのまま使用**しており、`c`を明示的パラメータとして変化させる解析ではない。臓器/voxelスケール（mm）であり、µmスケールの微視的評価ではない |
| L8 | Unterrainer LM et al. "Image-based dosimetry for [²²⁵Ac]Ac-PSMA-I&T therapy and the effect of daughter-specific pharmacokinetics." *Eur J Nucl Med Mol Imaging*. 2024. doi:10.1007/s00259-024-06681-2 | 2024 | EJNMMI | 患者（臓器/voxel） | ²²⁵Ac＋²²¹Fr＋²¹³Bi（娘核種別PK） | 5患者、娘核種別の時間放射能曲線を個別に画像から推定 | 娘核種の薬物動態が親核種と異なることを定量化し線量計算に反映 | **貢献A/拡張①への強い先行研究**。「娘核種を独立に扱う」という考え方自体は**臓器スケールで既に確立**。本研究の拡張①は、これを**微視的（µm）スケールに、連続的な保持率`η_i`パラメータとして**持ち込む点に新規性を絞り込む必要がある |
| L9 | [著者未確認・TUM/Helmholtz Munich] "[²²⁵Ac]Ac-PSMA I&T: A Preclinical Investigation on the Fate of Decay Nuclides and Their Influence on Dosimetry of Salivary Glands and Kidneys." *J Nucl Med*. 2025 (early view). PubMed 41043997 | 2025 | J Nucl Med | 前臨床（マウス、臓器） | ²²⁵Ac＋²²¹Fr＋²¹³Bi | マウスモデル、時点別（10分・1時間・平衡時）の臓器内核種別放射能測定 | ²¹³Bi の腎臓集積が平衡時比2倍、唾液腺で最大8.5倍。線量への寄与を係数1.3（腎）・2.5（唾液腺）と定量化 | 娘核種再分布の**定量的な臓器レベル影響**を示す一次データ。本研究のH3・拡張①の`η_i`パラメータ範囲設定の参考データとして極めて有用（実測に基づく`η`の目安が得られる可能性） |
| L10 | Rumiantcev M, Li WB, Lindner S, Liubchenko G, Resch S, Bartenstein P, Ziegler SI, Böning G, Delker A. "Estimation of relative biological effectiveness of ²²⁵Ac compared to ¹⁷⁷Lu during [²²⁵Ac]Ac-PSMA and [¹⁷⁷Lu]Lu-PSMA radiopharmaceutical therapy using TOPAS/TOPAS-nBio/MEDRAS." *EJNMMI Phys*. 2023;10:56. doi:10.1186/s40658-023-00567-2 | 2023 | EJNMMI Physics | 細胞・DNA損傷 | ²²⁵Ac／¹⁷⁷Lu（独立比較） | TOPAS/TOPAS-nBio（track structure）＋MEDRAS（DNA修復モデル） | 初期損傷ベースRBE ≈ 2.14（一定）、修復考慮後は線量依存（0–50 Gyで9.38→1.46） | **拡張②（生物学的効果層）のLQ/RBEパラメータの直接的な文献値ソース**。本研究の`α_Ac`, `β_Ac`との対応づけに使用可能 |
| L11 | [著者未確認] "Computational Pathology and Spatial Microdosimetry Guide Radiopharmaceutical Selection for TROP2-Targeted Alpha versus Beta Radionuclide Drug Conjugates (RDCs)." *bioRxiv*. 2026 Aug. doi:10.64898/2026.08.19.745876v1（プレプリント、査読前） | 2026 | bioRxiv（プレプリント） | 病理画像ベース微視的線量 | ¹⁷⁷Lu様β核種 vs ²²⁵Ac様α核種（TROP2標的、PSMA系ではない） | デジタルIHC画像から空間分布を再構成し線量シミュレーション | 「不均一な空間分布下でのα/β核種選択は未解決の臨床課題」と明言 | **2026年8月時点でも「不均一分布下でのα/β選択」が未解決と評価されている一次証拠**。ただし対象はTROP2でありPSMA/Ac-Lu併用ではなく、かつ「選択」問題であって「併用」問題ではない。§2.3のギャップ記述を支持する状況証拠として引用可能 |
| L12 | Peter R, Bidkar AP, Bobba KN, Zerefa L, Dasari C, Meher N, Wadhwa A, Oskowitz A, Liu B, Miller BW, Vetter K, Flavell RR, Seo Y. "3D small-scale dosimetry and tumor control of ²²⁵Ac radiopharmaceuticals for prostate cancer." *Sci Rep*. 2024;14. doi:10.1038/s41598-024-70417-3 | 2024 | Scientific Reports | µm（オートラジオグラフィ） | ²²⁵Ac 単独 | デジタルαオートラジオグラフィ＋組織染色、cold spot と壊死領域の対応を解析 | 不均一分布でも cold spot が壊死領域と一致すれば TCP 維持 | ²²⁵Ac単独の微視的不均一性研究。Ac/Lu併用ではない。**当初「complementary carriers」と誤読していたが、実際はキレータ(Macropa)と標的分子(YS5)の組み合わせの話であり、Ac+Lu併用のことではなかった**（§11.2の記述を訂正） |
| L13 | de Kruijff RM, Raavé R, Kip A, Molkenboer-Kuenen J, Morgenstern A, Bruchertseifer F, et al. "The in vivo fate of ²²⁵Ac daughter nuclides using polymersomes as a model carrier." *Sci Rep*. 2019;9:1–13.（Tranel et al. 2022 の引用 [28] より把握。**本文は未読、Tranel論文中の記述からの間接引用**） | 2019 | Scientific Reports | 前臨床（マウス） | ²¹³Bi（²²⁵Acナノ粒子担体からの遊離） | ポリマーソーム（100 nmナノ粒子）に²²⁵Acを封入し、娘核種の体内動態を追跡 | **²¹³Biの保持率が少なくとも69%**（Tranel et al. 2022 本文の記述："a ²¹³Bi retention of at least 69% and a much-decreased renal uptake of free ²¹³Bi compared to no retention strategies at all"） | **本研究のPhase 3b（η_Bi感度解析）のパラメータ範囲設定に使える実測アンカー値**。当初設定した`η_Bi`の探索範囲（0.3, 0.5, 0.7, 0.9, 1.0）は、この実測値（≥0.69）を自然に挟み込んでおり、範囲設定の妥当性が裏付けられた。全文精読は正式Phase 0で行う |

---

## 2. 成熟度マップの改訂（§2.2 の再評価）

Phase 0 の暫定結果を踏まえ、当初計画書 §2.2 の成熟度評価を以下のように改める。

| 領域 | 当初評価 | **改訂後の評価** | 根拠 |
|---|---|---|---|
| ²²⁵Ac 崩壊連鎖を核種別に分解した DPK 群（貢献A） | △ 限定的 | **○ 確立に近づいている** | L1（Koniar 2023）・L2（Hu 2025）が既に核種分解＋微視的スケールでの検証を実施 |
| 空間分布が線量に与える影響の定量化（単一核種） | 未評価 | **◎ 確立**（L2で実証済み） | 分布違いで線量最大80%変化（L2） |
| **2核種同時投与における空間共局在度を連続変数とした一般式（貢献B）** | △〜弱 | **依然として弱いが、L5（Tranel 2022）が最も近い先行研究として存在** | L5は空間分離パラメータを扱うが、2核種混合投与ではなく「どちらか一方の候補選択」の文脈 |
| µm分解能マルチ核種DPK/畳み込みフレームワーク | ○ 研究あり | **◎ 確立**（L3, L4） | Ghaseminejad 2025、Yan 2025 が同種の手法を確立 |
| 娘核種再分布の臓器スケール定量化 | 弱 | **◎ 確立**（L8, L9） | Unterrainer 2024、TUM/Helmholtz 2025 preclinical |
| 娘核種再分布の微視的（µm）スケール定量化、連続的保持率η | 弱 | **△ 部分的**（L2が最も近いが連続変数化はしていない可能性） | 要全文確認 |
| 「不均一分布下でのα/β核種選択」問題の臨床的重要性 | （評価なし） | **2026年時点でも未解決と明言する一次資料あり（L11）** | 状況証拠として有用 |

---

## 3. 研究計画への影響と対応（改訂版）

### 3.1 貢献A（マルチ核種分解カーネル）：新規性が当初より狭まった【L2全文確認により確定】

L1・L2（**L2は2026-09-22に全文確認済み**）により、「²²⁵Ac連鎖を核種分解してDPK/S値を計算する」こと自体はもはや新規ではない。**貢献Aは以下のように、さらに狭く、しかし依然として妥当な主張に絞り込む：**

> 既存研究（L1, L2）は核種分解カーネルを「特定の幾何学的配置（線源位置固定、離散的な分布パターン——L2では核内/細胞質/全細胞/細胞外/内外両方/膜結合の6カテゴリ）」の比較として扱っている。本研究の貢献は、娘核種の**保持率 `η_i` を0〜1の連続変数**とし、束縛集団の分布 `p_parent(r)` と遊離集団の分布 `p_i^free(r)` の線形補間としてモデル化し、`η_i`に対する線量の感度（勾配）を連続的に評価する点、および臓器スケールで既に測定されている娘核種の再分布の程度（L8, L9, L13：²¹³Bi保持率≥69%）を`η_i`の推定範囲として微視的スケールに接続する点にある。

**L2の全文確認により、この絞り込みは確定した**（当初の「要確認」ステータスを解消）。さらにL2のDiscussionは「Zaiderらの重み付け法で mixed radiation field の RBE加重線量を決定できる」と将来課題として明言しており、これは本研究の混合場フレームワークがまさに埋めるべき空白を論文自身が示唆していると解釈できる。**L2は貢献Aへの脅威ではなく、むしろ本研究のPhase 1拡張①が接続すべき「核種別S値のカタログ」を提供する有用な先行研究として引用すべきである。**

### 3.2 貢献B（空間共局在度）：L5との差別化が最重要課題【L5全文確認により、差別化はむしろ明確化した】

L5（Tranel 2022、**全文確認済み**）は本研究の貢献Bに最も近い。Paper III（§8.3）の Introduction では、L5 を単なる参考文献ではなく、**明示的に比較対象として論じる**必要がある。差別化の軸（全文確認により以下が確定）：

1. **L5は"CAF and tumors were not considered as sources at the same time"と明記しており、¹⁷⁷Luと²²⁵Acを同一腫瘍に同時投与する設計は一切行っていない。** 常にどちらか一方の核種・どちらか一方の細胞集団を線源とする4通りの組合せを比較する研究であり、「¹⁷⁷Luか²²⁵Acか、どちらか一方を選ぶ」問題。本研究は「両方を同時投与するときの活性比`f`と空間相関`c`」問題であり、**設計思想のレベルで異なる**（当初のWebSearch要約からの推定より、差異はむしろ明確）。
2. L5の空間パラメータは2**細胞集団**（CAF vs 腫瘍）間の平均**距離**（Lmean）。本研究の`c`は同一組織内の2**核種分布**の相関係数。両者は関連するが同一ではない（`c`は`Lmean`のような特定の幾何学的距離モデルに依存しない、より一般的な統計的定義）。
3. L5は`f`（活性比）を変数にしていない。
4. **L5は「Re-distribution of the parent or the ²²⁵Ac daughters were not simulated」と明記**しており、娘核種再分布も明示的に非考慮（本研究の拡張①がこの限界を埋める対象の一つでもある）。
5. L5が引用するde Kruijff et al. 2019（本表L13）の²¹³Bi保持率≥69%というデータは、本研究のPhase 3b（η_Bi感度解析）のパラメータ範囲（0.3–1.0）の妥当性を実測値で裏付ける。

### 3.3 貢献C・D：概ね当初評価を維持

時間依存放射能比（貢献C）と4因子統合UQ（貢献D）については、今回の検索で直接の先行研究は見つからなかった。当初評価（弱い）を維持する。ただし検索網羅性はまだ十分ではなく、Phase 0正式実施時に追加確認が必要。

### 3.4 Phase 0 正式実施へのアクションアイテム（更新：L2・L5は完了）

1. ~~L2（Hu et al. 2025）の全文入手・精読~~ → **完了（2026-09-22）**。6種の空間分布は離散的幾何カテゴリと確定。貢献Aの新規性の幅は§3.1の通り確定した。
2. ~~L5（Tranel et al. 2022）の全文入手・精読~~ → **完了（2026-09-22）**。単核種・単一細胞集団の組合せ比較であり、2核種同時投与ではないことを確定。貢献Bの差別化は§3.2の通り確定した。
3. **残タスク**：L1, L3, L4, L8, L9, L10, L11, L12 の全文確認（優先度順）。特にL1（Koniar 2023）はL2と並ぶ貢献Aの直接先行研究であり、次に精読すべき。
4. 本表でカバーできていない検索ブロック（付録C の B5：核データ評価）を追加実施する。
5. WebFetch が使えない環境的制約があったため、L2・L5は利用者からアップロードされたPDFファイルを直接読み込んで全文確認した。**この方式（一次資料PDFの提供→全文精読）は他の未確認文献にも適用可能**。それ以外の文献は、大学契約経由のデータベースアクセス、または同様のPDF提供によって本表を正式版に格上げする。

---

## 4. 全体としての結論（L2・L5全文確認後、更新）

当初計画書の §2.1 で示した「先行研究の到達点」は、今回の検索でおおむね裏付けられた。ただし、**当初の想定より先行研究は密度が高く**、特に

- 核種分解カーネル（貢献A）
- µm分解能マルチ核種比較フレームワーク（Phase 2 相当）

は、想定より確立が進んでいた。一方で、**最も脅威度が高いと判断した2件（L2, L5）を全文で確認した結果、以下が確定した**：

- **貢献B（2核種同時投与・活性比fと空間共局在度cを独立変数とする混合場の一般式）は、依然として文献上未確立である。** 最も近いL5（Tranel 2022）でさえ「²²⁵Acと¹⁷⁷Luを同一腫瘍に同時投与する設計は一切行っていない」ことが本文の明記（"CAF and tumors were not considered as sources at the same time"）から確定した。**したがって、本研究のコア（§12 コア・トラック）である貢献Bを主軸に据えた設計判断は、全文確認を経てなお支持される。**
- **貢献A（娘核種保持率η_iの連続変数化）も、全文確認により新規性の範囲が確定した。** L2（Hu 2025）の「6種の空間分布」は離散的な幾何カテゴリであり、L5（Tranel 2022）は娘核種再分布を明示的に「シミュレートしていない」。したがって「η_iを0–1の連続変数として束縛/遊離集団を線形補間する」という本研究の定式化は、L1・L2という直接の先行研究が存在してもなお、狭いながら妥当な貢献として残る。
- **副産物として、L5経由でL13（de Kruijff et al. 2019, ²¹³Bi保持率≥69%）という実測アンカー値を得た。** これはPhase 3bのパラメータ範囲設定（§6）の妥当性を補強する。

**全体として、暫定検索でL2・L5を全文確認したことは、本研究計画の根幹（コア＝貢献B、拡張①＝貢献Aの絞り込み）を弱めるどころか、より精密な根拠を与える結果となった。** 残る文献（特にL1, L3, L4, L8–L10）の全文確認が、正式なPhase 0における次の優先課題である。
