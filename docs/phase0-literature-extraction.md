# Phase 0：系統的文献レビュー — 抽出表（v1）

**作成日**：2026-09-22
**対象研究**：²²⁵Ac/¹⁷⁷Lu 混合放射線場に対する統一的微視的線量計算フレームワーク（`docs/research-plan-ac225-lu177-mixed-microdosimetry.md`）
**検索方法**：WebSearch による反復的ターゲット検索（PubMed/EJNMMI Physics/Med Phys/Phys Med Biol/Sci Rep/bioRxiv を横断）。本計画書 付録C の検索式を出発点とし、得られた論文の引用・関連論文を追跡（snowballing）。

**重要な限界（2026-09-22 第3回更新）**：本セッションの実行環境では WebFetch（一次資料の本文取得）が egress ポリシーによりブロックされており、当初は WebSearch の抄録レベル要約に依存していたが、**利用者からのPDF提供により、13件中10件（L1, L2, L3, L5, L6, L8, L9, L10, L11, L13）を全文確認済み**とした（2026-09-22）。L6はIOPscience購読ページからは本文を取得できなかったが、NIHMS/PMC著者最終稿という別ルートで全文を取得できた。**未確認は L4（購読制、未取得）、L7（低優先度、未着手）、L12（未着手）の3件のみ**。全文確認により、当初の暫定評価は複数箇所で更新・精密化されている（特に L1 の発見により貢献A/拡張①の新規性主張を再度絞り込んだ——§3.1参照）。本表は依然として一次ドラフトの位置づけを維持するが、確定度はかなり高まった。

---

## 1. 抽出表（本研究の貢献 A・B・C・D との関連順）

| # | 引用 | 年 | 誌 | スケール | 核種 | 手法 | 主要内容 | 本研究との関係 |
|---|---|---|---|---|---|---|---|---|
| L1 | Koniar H, Miller C, Rahmim A, Schaffer P, Uribe C. "A GATE simulation study for dosimetry in cancer cell and micrometastasis from the ²²⁵Ac decay chain." *EJNMMI Phys*. 2023;10:46. doi:10.1186/s40658-023-00564-5（**全文確認済み 2026-09-22、TRIUMF/UBC**） | 2023 | EJNMMI Physics | 細胞（10µm/核8µm）〜微小転移巣（六方最密12個クラスタ） | ²²⁵Ac連鎖7核種を個別シミュレーション（²²⁵Ac, ²²¹Fr, ²¹⁷At, ²¹³Bi, ²¹³Po, ²⁰⁹Tl, ²⁰⁹Pb） | GATE 9.0（Geant4-DNA emDNAphysics）。線源位置4種：膜/細胞質/核/全細胞。**²²¹Frと²¹³Biの保持率を100%→0%まで20%刻みでスイープし、自己線量S値への影響を評価**（内部化度0-100%も20%刻みで評価） | 連鎖累積S値はMIRDcellと−4.7〜−6.9%で一致（α核種は−12.1%以内、β核種の²⁰⁹Tlは自己線量で最大−70.9%乖離）。**²²¹Fr保持率0%でS値が最大72%減少、²¹³Bi保持率0%で最大21%減少**。核内在化で自己線量S値は表面結合時の約2〜3倍に増加 | **重要な確定事項——貢献A（拡張①）の新規性を再度、大幅に絞り込む必要がある。** 本論文は既に、²²¹Frと²¹³Biの保持率を0-100%で20%刻みにスイープする感度解析を、単一細胞スケールの自己線量S値に対して実施済みである。これは本研究が「新しい貢献」として構想していた`η_i`連続変数化と**発想上ほぼ同一**（20%刻みの離散サンプリングと連続変数の差は実質的に小さい）。**本研究が依然として新規性を持ちうるのは以下の点に限定される**：(i) Koniarのモデルは保持率低下分の線量を「消失」として扱っており（遊離核種がどこへ行くかを明示的にモデル化しない）、本研究は`p_i^free(r)`という独立分布への**空間的再分配**を明示的にモデル化する点、(ii) Koniarは単一細胞〜クラスタスケールに留まり、腫瘍スケール（1mm³）のTIA分布への接続は行っていない点、(iii) そもそも²²⁵Ac単核種の解析であり、¹⁷⁷Luとの混合場（貢献B）には触れていない点。**拡張①は「η_iの導入」から「η_iの空間的再分配モデルへの拡張、および混合場文脈での感度評価」へとさらに焦点を絞る必要がある** |
| L2 | Hu Z, Qu S, Liu H, Zhang Y, Yan S, Hu A, Qiu R, Wu Z, Zhang H, Li J. "Evaluation of relative biological effectiveness of ²²⁵Ac and its decay daughters with Monte Carlo track structure simulations." *EJNMMI Phys*. 2025;12:65. doi:10.1186/s40658-025-00765-0（**全文確認済み 2026-09-22**） | 2025 | EJNMMI Physics | 細胞核（track structure、Tsinghua大学） | ²²⁵Ac 連鎖の**7核種**（²²¹Fr, ²¹⁷At, ²¹³Po, ²¹³Bi, ²⁰⁹Tl, ²⁰⁹Pb）＋²²⁵Ac 自身＋¹⁷⁷Lu（参照、計8核種） | NASIC（track structure、電子カットオフ11 eV）、V79/HSG/Renca 3細胞（核半径8.1 µm固定、domain半径0.2–0.5 µm）、mSMKM（Sato/InaniwaのSMKM拡張）でRBE算出。48条件（8核種×6分布）×10反復 | **6種の空間分布は確認の通り離散的な幾何配置カテゴリ**：①核内一様、②細胞質内一様、③全細胞一様、④細胞外、⑤細胞内外両方、⑥膜結合——連続的な保持率パラメータではない。核内均一分布でのAbstractの結果：V79細胞でAc-225のRBE_M=6.91±0.04、²²¹Fr=6.81±0.04、²¹⁷At=6.67±0.02、²¹³Po=6.43±0.05、²¹³Bi=5.91±0.09（β/α混合崩壊のため他のα核種より17%低い）、β核種²⁰⁹Tl/²⁰⁹Pbは1に近い。**分布依存性は吸収線量に対して最大80%だが、RBEに対しては最大10%**（V79細胞での`²²⁵Ac`の場合）。RBE_Mは細胞種間でも変動（V79:6.91、HSG:6.78、Renca:9.76、最大44%差）。Discussion冒頭で「To the best of our knowledge, the RBE for each individual decay daughter in the ²²⁵Ac decay chain has not been calculated」と明言し、先行研究(Rumiantcev et al.)は「全娘核種が同じ場所で崩壊すると仮定し核種別計算をしていない」と対比 | **貢献A・貢献Bの両方に強く関連する最重要先行研究、全文で確定**。核種分解自体（Table 3の核種別Gy/decay表）と離散的な幾何配置に対する感度は既に確立。**しかし、以下3点で本研究の拡張①は依然として新規性を持つ**：(i) 6分布は離散カテゴリであり、`η_i`のような0–1連続変数で束縛/遊離集団を補間するモデルではない、(ii) 単一細胞スケール（10 µm）の解析であり、本研究が想定する腫瘍スケール（1 mm³）でのTIA分布への接続は行っていない、(iii) 論文自身がDiscussionで「Zaiderらの重み付け法を用いて今後mixed radiation fieldのRBE加重線量を決定できる」と将来課題として明言しており、**本研究の混合場フレームワークが埋めるべき空白を論文自身が示唆している**。また²²⁵Acと¹⁷⁷Luは同一腫瘍に同時投与する混合場としては扱われていない（¹⁷⁷LuはRBE計算の基準線源としてのみ使用） |
| L3 | Ghaseminejad S, De Sarno D, Bauman G, Lee TY. "Framework to calculate ²²⁵Ac, ¹⁷⁷Lu, and ¹⁶¹Tb radiation dose and biological effect in metastatic castration-resistant prostate cancer treatment." *Med Phys*. 2025;52(8):e18035. doi:10.1002/mp.18035（**全文確認済み 2026-09-22**） | 2025 | Medical Physics | 細胞（2 µm分解能、Western大学） | ²²⁵Ac／¹⁷⁷Lu／¹⁶¹Tb（核種ごとに独立、患者別投与量ベース） | TOPAS-nBio（GEANT4ラッパー）で2 µm DPK生成＋DBSCAN で SSB/DSB/complex DSBスコアリング＋Biologic Effect Cell Kernel (BECK) で3D畳み込みcrossfire評価。細胞質に一様分布の単一シナリオのみ（核・細胞外は活性ゼロ） | Table1: 中心細胞核線量（crossfire込み）²²⁵Ac 0.22 Gy(crossfire寄与74%)、¹⁷⁷Lu 1.2 Gy(88%)、¹⁶¹Tb 1.69 Gy(crossfire寄与率は画像から正確な数値を読み取れず、要再確認——本文Abstractでは161Tbのcrossfire寄与は「~41%」と記載されている)。**独自RBE指標`RBE_TRT`**（complex DSB数の比、⁶⁰Coではなく核種間比較）で²²⁵Acは¹⁷⁷Luの**約1000倍**（Table3）——Hu et al. 2025の生存率ベースRBE（6–10倍程度）とは定義も数値も大きく異なり、**核医学分野でRBE定義が不統一**であることを示す | **重要な確定事項：本論文は²²⁵Ac連鎖を「Whole Chain DPK」として単一カーネルで扱っており（Fig.2キャプション "Ac-225 Whole Chain DPK"）、L2（Hu 2025）のような核種分解はしていない。** これは本研究のコア設計（Phase 1コア＝実効カーネル、拡張①＝核種分解）と整合的であり、**貢献Aへの脅威ではなくコアの妥当性を補強する**。一方、3核種は常に独立比較（投与量ベースで別個に計算）であり、2核種混合投与のf/c変数化は行っていない（貢献Bは影響なし）。また`RBE_TRT`とHu et al.のmSMKM由来RBEの数値・定義の乖離は、本研究の拡張②（生物学的効果層）で「RBE定義の標準化」も副次的論点になり得ることを示唆する |
| L4 | Yan K, Jiang Y, Wang R, Xu W, Guo J, Gao H, Chen Y, Wei S, Wang X, Zheng M, et al. "A fast convolution-based method for microdosimetric comparison of ²²⁵Ac, ²¹¹At, ¹⁷⁷Lu and ¹⁶¹Tb at the cell cluster scale." *Phys Med Biol*. **2026**;71:125005. doi:10.1088/1361-6560/ae7892（**未達：提供PDFはIOPscience購読ページのみ。公開日2026年6月18日——WebSearch要約時点の「2025年」表記は誤り、正しくは2026年に訂正**） | 2026 | Phys Med Biol | 細胞クラスター（PC-3 mesh-type単細胞モデルの格子複製） | ²²⁵Ac／²¹¹At／¹⁷⁷Lu／¹⁶¹Tb（独立比較） | PHITS で核S値計算＋FFT畳み込み＋飽和補正MKモデル、標識率・クラスタサイズ・対数正規不均一性を変化 | 畳み込み再構成とPHITS直接計算の一致（偏差<5%）。飽和補正済みdose-mean lineal energy y*は²²⁵Ac/²¹¹Atで近傍層60-70 keV/µm→遠方層4 keV/µm未満に急減（¹⁷⁷Lu/¹⁶¹Tbはより緩やか）。²²⁵AcのTCP90%必要活性が¹⁷⁷Lu/¹⁶¹Tbの約1/1000 | 本計画のPhase 2（FFT畳み込みエンジン）と方法論的にほぼ同一の先行研究。「畳み込みでマルチ核種微視的線量比較を高速に行う」という手法自体の新規性はここで大きく減じる。ただし本研究のように**2核種を`f`（活性比）で混合し`c`（空間相関）を連続変化させる**設計ではなく、単一核種ごとの独立比較である点は維持されている。**全文（購読制のため）は依然未確認** |
| L5 | Tranel J, Palm S, Graves SA, Feng FY, Hope TA. "Impact of radiopharmaceutical therapy (¹⁷⁷Lu, ²²⁵Ac) microdistribution in a cancer-associated fibroblasts model." *EJNMMI Phys*. 2022;9:67. doi:10.1186/s40658-022-00497-5（**全文確認済み 2026-09-22**） | 2022 | EJNMMI Physics | mm〜µm（3mm球状腫瘍モデル、20µm voxel） | ¹⁷⁷Lu（DVK畳み込み/重ね合わせ、GATEで事前生成）／²²⁵Ac（GATE MC、full decay chain、崩壊連鎖は一括） | 腫瘍(75%)とCAF(25%)を混在させた5モデルでクラスタサイズ（`Lmean`＝92/116/181/341/1030 µm）を変化。各核種を独立にCAF or 腫瘍を線源として計算し、DVH・efficacy ratio (ER)を評価 | ²²⁵AcのER：CAF→CAF自己線量で1.5→3.7（Lmean 92→1030µm）、腫瘍→CAFでは0.8→0.1に低下。¹⁷⁷LuのERはより緩やか（1.2→2.7、0.9→0.3）。結論：クラスタが大きいほど²²⁵Acの優位性が低下し、¹⁷⁷Luの方が有効になる | **全文で以下を確定**：(1)「**As we focused on differences...radioisotopes were modeled to be with...CAF and tumors were not considered as sources at the same time**」と明記——**²²⁵Acと¹⁷⁷Luを同一腫瘍に同時投与する設計は一切行っていない**。常にどちらか一方の核種を、どちらか一方の細胞集団を線源として計算する4通りの組合せの比較。放射能比`f`という変数も存在しない。(2) Limitationsで「**Re-distribution of the parent or the ²²⁵Ac daughters were not simulated**」と明記——**娘核種再分布は明示的に非考慮**（本研究の拡張①がこの限界を埋める）。(3) `Lmean`は**2つの異なる細胞集団（CAF vs 腫瘍）間の平均距離**であり、本研究の`c`（同一組織内の2核種分布の相関係数）とは異なる幾何定義。**貢献Bとの差別化は当初評価より一層明確になった**——Tranel 2022は「単核種を使うときの標的/線源の組合せ最適化」問題であり、「2核種混合投与の設計」問題ではない。**Paper III Introductionで最重要の対比先行研究として詳述する** |
| L6 | Tranel J, Feng FY, St. James S, Hope TA. "Effect of microdistribution of alpha and beta-emitters in targeted radionuclide therapies on delivered absorbed dose in a GATE model of bone marrow." *Phys Med Biol*. 2021;66(3):035016. doi:10.1088/1361-6560/abd3ef（**全文確認済み 2026-09-22——NIHMS/PMC著者最終稿（nihms-1661149）経由で取得。UCSF**） | 2021 | Phys Med Biol | mm〜µm（骨髄円柱モデル、10µm等方voxel） | ⁹⁰Y／¹⁷⁷Lu（β）／²¹¹At／²²⁵Ac（α）（独立比較） | GATE 9.0（Livermore物理モデル）。円柱モデル（中心血管＋海綿骨、61×61×61 voxel）で血管プール限定シナリオ、および50個のランダムvoxelに追加投与する骨髄浸潤(BMMI)シナリオの2種。各核種を個別にシミュレーション。ステップサイズ上限1µm | α線源（²¹¹At等）は血管壁から70 µm以内にエネルギーの大部分を沈着。β/α間で生物学的等価性を得るための相対投与活性比が臨床データで1.4〜770.8倍と大きく変動することを指摘し、10⁰〜10³倍の範囲でBMMI線量を比較 | 候補核種比較の先行研究（L5と同著者グループ、L5の前身研究）。**常に単一核種のみを評価しており、2核種混合投与のモデルではない**——貢献Bとの差別化に使う参照文献として全文で確定。α/β生物学的等価活性比が文献ごとに1.4〜770.8倍とばらつくという指摘は、本研究の拡張②（生物学的効果の非加算性）の動機付けにも使える |
| L7 | Delker A, Schleske M, Liubchenko G, Berg I, Zacherl MJ, Brendel M, Gildehaus FJ, Rumiantcev M, Resch S, Hürkamp K, Wenter V, Unterrainer LM, Bartenstein P, Ziegler SI, Beyer L, Böning G. "Biodistribution and dosimetry for combined [¹⁷⁷Lu]Lu-PSMA-I&T/[²²⁵Ac]Ac-PSMA-I&T therapy using multi-isotope quantitative SPECT imaging." *Eur J Nucl Med Mol Imaging*. 2023;50(5):1280–1290. doi:10.1007/s00259-022-06092-1 | 2023 | EJNMMI | 患者（臓器/voxel） | ¹⁷⁷Lu＋²²⁵Ac（同時投与患者） | Dual-isotope 定量SPECT/CT（1時間、24時間後） | 腎臓・病変への線量を核種別に算出（RBE=5でα換算） | **P2 確定**。臨床combination dosimetryは既に実施済み。ただし各核種の空間分布は**実測SPECT画像をそのまま使用**しており、`c`を明示的パラメータとして変化させる解析ではない。臓器/voxelスケール（mm）であり、µmスケールの微視的評価ではない |
| L8 | Liubchenko G, Böning G, Zacherl M, Rumiantcev M, Unterrainer LM, Gildehaus FJ, Brendel M, Resch S, Bartenstein P, Ziegler SI, Delker A.（**第一著者はLiubchenko G、Unterrainer LMは第5著者——旧記載の"Unterrainer LM et al."を訂正**）"Image-based dosimetry for [²²⁵Ac]Ac-PSMA-I&T therapy and the effect of daughter-specific pharmacokinetics." *Eur J Nucl Med Mol Imaging*. 2024;51:2504–2514. doi:10.1007/s00259-024-06681-2（**全文確認済み 2026-09-22、LMU München**） | 2024 | EJNMMI | 患者（臓器/voxel、腎臓・病変） | ²²⁵Ac＋²²¹Fr＋²¹³Bi（娘核種別PK、SPECT/CT） | 5患者、440keV(²¹³Bi)/218keV(²²¹Fr)/78keVの3光子ピークでSPECT/CT（24h・48h後）、単指数フィット、RBE=5でMIRD形式の線量計算。3手法で比較：method1(²¹³Biのみ代表)、method2(²²¹Frのみ代表)、method3(²²¹Fr+²¹³Bi個別、娘核種別PK) | 腎臓/病変の実効半減期：²¹³Bi 27±10h(腎)/38±10h(病変)、²²¹Fr 24±11h/38±11h。腎臓での²¹³Bi-to-²²¹Fr SUV比が24-48hで9±8%増加（統計的有意差 p=0.0078）。**RBE加重線量：method1=0.18±0.06、method2=0.16±0.05、method3=0.17±0.06 Sv_RBE5/MBq（腎臓）——3手法の差は最大でも8%以内** | **重要な発見——組織/臓器スケールでは、娘核種別PKを厳密に分離しても、単一光子ピークを代用した場合との線量差は8%以内に収まる。** これは本研究のL1（Koniar 2023、細胞スケールでの²¹³Bi保持率感度、最大21%）との対比で非常に重要な「スケール依存性」の実例になる——**臓器スケールでは娘核種再分布の影響が小さい（~8%）のに対し、微視的スケールでは無視できない（最大72%）**。この「スケールごとに結論が逆転しうる」事実そのものが、本研究の中心的動機（マクロ・ダシメトリの結論が微視的スケールにそのまま外挿できない）を直接裏付ける一次証拠として、Introduction/motivationで積極的に引用すべき |
| L9 | Wurzer A, Sun B, Saleh S, Brosch-Lenz J, Fischer S, Kossatz S, Hürkamp K, Li WB, Eiber M, Morgenstern A, Chilug EL, Bruchertseifer F, Weber W, D'Alessandria C. "[²²⁵Ac]Ac-PSMA I&T: A Preclinical Investigation on the Fate of Decay Nuclides and Their Influence on Dosimetry of Salivary Glands and Kidneys." *J Nucl Med*. 2025;66(12):1964–1969. doi:10.2967/jnumed.125.269744（**全文確認済み 2026-09-22、TU München/Helmholtz Zentrum München**） | 2025 | J Nucl Med | 前臨床（マウス、臓器、健常＋LNCaP腫瘍担持） | ²²⁵Ac＋²²¹Fr＋²¹³Bi（PSMA I&T、¹⁷⁷Lu類縁体と比較） | マウス群(n=5)、10分・1時間・24時間・7日時点でのγ線分光による臓器別²²¹Fr(218keV)/²¹³Bi(440keV)放射能測定、MIRDcalcで線量計算。非平衡補正2シナリオ（²²¹Fr 3α崩壊蓄積 vs ²¹³Bi 1α崩壊蓄積） | 腎臓²¹³Bi取込：平衡時比1.8倍(10分)/2倍(1時間)。唾液腺：1.7倍(10分)/8.5倍(1時間)。平衡時吸収線量：腎臓1.11、唾液腺0.20 Sv_RBE5/MBq。非平衡分を含めると腎臓線量は係数1.21-1.40、唾液腺は係数1.5(²¹³Bi)〜2.5(²²¹Fr)で増加。**腫瘍からの娘核種再分布は検出されず**（"no redistribution was found from tumor tissue"） | 娘核種再分布の**定量的な臓器レベル影響**を示す一次データ、全文確認済み。腎臓での人体データ（Kratochwil et al., 0.74 Sv_RBE5/MBq）とマウスの値(1.11)が近い一方、唾液腺は種差で10倍以上乖離——種差の存在にも留意が必要。本研究のH3・拡張①の`η_i`パラメータ範囲設定の参考データとして有用 |
| L10 | Rumiantcev M, Li WB, Lindner S, Liubchenko G, Resch S, Bartenstein P, Ziegler SI, Böning G, Delker A. "Estimation of relative biological effectiveness of ²²⁵Ac compared to ¹⁷⁷Lu during [²²⁵Ac]Ac-PSMA and [¹⁷⁷Lu]Lu-PSMA radiopharmaceutical therapy using TOPAS/TOPAS-nBio/MEDRAS." *EJNMMI Phys*. 2023;10:53. doi:10.1186/s40658-023-00567-2（**全文確認済み 2026-09-22、LMU München**） | 2023 | EJNMMI Physics | 細胞（5種の楕円体ジオメトリ、等体積）・DNA損傷（nm） | ²²⁵Ac／¹⁷⁷Lu（独立比較、¹⁷⁷Luを参照放射線として使用） | TOPAS/TOPAS-nBio（track structure、event-by-event）＋MEDRAS（DNA修復モデル）。DSB数を線量の関数としてフィット：**¹⁷⁷Luは線形二次（LQ）、²²⁵Acは線形（`a_Ac=0`）でフィットされた**（2D/3D配置、内在化2条件、細胞形状5種） | 初期損傷ベースRBEは線量非依存で一定：**1.984〜2.135（2D）、2.120〜2.206（3D）**。修復考慮後のRBEは線量依存：0 Gyで**8.04〜10.00（2D）、9.33〜10.84（3D）**、50 Gyまでに約1.5〜2まで漸減（Fig.11）。SPECTベースMIRDcell換算等でRBE=5が臨床的に使われているが、著者らはこれが特に低線量域で過小評価である可能性を指摘 | **拡張②（生物学的効果層）のLQ/RBEパラメータの直接的な文献値ソース、全文で確定。** 特に重要な点：**²²⁵Acの線量反応関係が本論文では線形（LQではない、`a_Ac≈0`）でフィットされている。** これは本計画書§5.6で仮定した「両核種にLQモデルを適用し交差項`2√(β_Ac β_Lu)D_Ac D_Lu`を導出する」という定式化の前提（`β_Ac>0`）と矛盾する可能性があり、**交差項がほぼゼロになる（高LET放射線の飽和的性質のため）可能性を示唆する。拡張②の理論的定式化（§5.6）は、この知見を踏まえて`β_Ac→0`極限での交差項の扱いを再検討する必要がある**（要修正アクション） |
| L11 | Chi WY. "Computational Pathology and Spatial Microdosimetry Guide Radiopharmaceutical Selection for TROP2-Targeted Alpha versus Beta Radionuclide Drug Conjugates (RDCs)." *bioRxiv*. 2026 Aug 25. doi:10.64898/2026.08.19.745876v1（プレプリント、査読前。**全文確認済み 2026-09-22、ただし評価を大幅下方修正**） | 2026 | bioRxiv（プレプリント、査読前） | 組織切片（2D、IHC画像ベース） | ¹⁷⁷Lu vs ²²⁵Ac（TROP2標的RDC、PSMA系ではない） | 14検体のTROP2免疫組織化学画像を色分解（HED色空間）し、2D抗原密度分布`S(x,y)`を推定。線量は**Gaussianカーネル近似**`K(r)∝exp(-r²/2σ²)`（σ=平均飛程/2.355で決定）で`S(x,y)`と畳み込み——**実際のMonte Carlo粒子輸送は一切行っていない**。Therapeutic Index (TI, 標的/非標的線量比)で¹⁷⁷Luと²²⁵Acを比較 | 14/14検体で²²⁵Ac-RDCのTIが¹⁷⁷Lu-RDCより高い（平均1.26 vs 1.01、p<0.0001）。結論：TROP2発現が焦点性・希薄な腫瘍では²²⁵Ac-RDCが優れる | **著者は1名（William Y. Chi, DPhil）、所属はWarburg Therapeutics Limited（RDC開発企業）——査読前・利益相反懸念あり、学術的重みは限定的と判断する。** 方法論的にも、(i) 実際の核種別Monte Carloカーネルではなく飛程からのGaussian近似のみ、(ii) 崩壊連鎖・LET分布・二次電子等の物理は一切モデル化せず、(iii) 2D組織切片のみで3D未対応（Limitationsで自認）——L1–L10と比べ方法論的に大きく見劣りする。**ただし重要な副産物として**：この論文は¹⁷⁷Luと²²⁵Acの線量を**常に同一の抗原密度マップ`S(x,y)`から計算**しており、両核種が完全に同一の空間分布を持つ（`c=1`）ことを検証なしに暗黙の前提としている。これは本研究の§2.1で指摘した「暗黙の仮定②：2核種が同一の微視的分布を持つ」を**2026年の実例で示す好例**であり、状況証拠というより**まさに本研究が問題視する設計そのものの具体例**として引用する方が適切 |
| L12 | Peter R, Bidkar AP, Bobba KN, Zerefa L, Dasari C, Meher N, Wadhwa A, Oskowitz A, Liu B, Miller BW, Vetter K, Flavell RR, Seo Y. "3D small-scale dosimetry and tumor control of ²²⁵Ac radiopharmaceuticals for prostate cancer." *Sci Rep*. 2024;14. doi:10.1038/s41598-024-70417-3 | 2024 | Scientific Reports | µm（オートラジオグラフィ） | ²²⁵Ac 単独 | デジタルαオートラジオグラフィ＋組織染色、cold spot と壊死領域の対応を解析 | 不均一分布でも cold spot が壊死領域と一致すれば TCP 維持 | ²²⁵Ac単独の微視的不均一性研究。Ac/Lu併用ではない。**当初「complementary carriers」と誤読していたが、実際はキレータ(Macropa)と標的分子(YS5)の組み合わせの話であり、Ac+Lu併用のことではなかった**（§11.2の記述を訂正） |
| L13 | de Kruijff RM, Raavé R, Kip A, Molkenboer-Kuenen J, Morgenstern A, Bruchertseifer F, Heskamp S, Denkova AG. "The in vivo fate of ²²⁵Ac daughter nuclides using polymersomes as a model carrier." *Sci Rep*. 2019;9:11671. doi:10.1038/s41598-019-48298-8（**全文確認済み 2026-09-22、TU Delft、Radboud大学医療センター**） | 2019 | Scientific Reports | 前臨床（マウス、静脈内／腫瘍内投与） | ²²⁵Ac（親核種）＋²¹³Bi（娘核種、自由体） | 100 nmポリマーソーム（DTPAキレートまたはInPO₄共沈殿で²²⁵Acを封入）をマウスに静注／腫瘍内投与し、²¹³Biの遊離・臓器再分布を連続ガンマカウントで追跡。Ratio = A_213Bi(t=0)/A_213Bi(t=平衡) で定量（ratio<1＝保持、>1＝他組織からの蓄積） | **静注実験（4h時点）**：血液 ratio DTPA 0.06±0.03／InPO₄ 0.14±0.07、脾臓 0.67±0.02／0.64±0.06、**腎臓 7.6±0.6／10.5±3.6（腎臓は²¹³Bi蓄積部位）**。²²⁵Ac自体の担体内保持率は**約93%**（100%ではない）。**腫瘍内投与実験（Table 2）**：腫瘍内²¹³Bi保持比（ratio、1に近いほど良保持）は 100nm/1日目 0.58±0.06、200nm/1日目 0.87±0.04、100nm/7日目 0.59±0.12、200nm/7日目 0.91±0.10。腎臓：腫瘍の²¹³Bi比は7日目で100nm粒子9.9、200nm粒子29.5 | **Phase 3bのη_Biパラメータ範囲の裏付けを、当初の「≥69%」という孫引きの単一値から、より精密な複数条件のレンジに更新できる。** 腫瘍内²¹³Bi保持比は条件（担体サイズ、時点）によって**0.58〜0.91**まで変動し、本研究のη_Bi探索範囲（0.3, 0.5, 0.7, 0.9, 1.0）はこの実測レンジをほぼ過不足なくカバーしている。**さらに重要な点**：親核種²²⁵Ac自体の保持も約93%であり100%ではない——本研究の定式化で `p_parent(r)` を「²²⁵Acの分布」として固定的に扱う仮定にも、ナノ粒子担体の場合は残余誤差があり得ることを示唆する（ただし本研究が想定するのは低分子キレート型リガンドであり、ナノ粒子担体とは薬物動態的性質が異なる点に留意） |

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

**追記（L1全文確認、2026-09-22第3回）：貢献Aの絞り込みをさらに一段進める必要がある。** Koniar et al. 2023（L1）は、²²¹Frと²¹³Biの保持率を**0%から100%まで20%刻みでスイープし**、単一細胞スケールの自己線量S値への影響を既に定量化している（²²¹Fr保持率0%でS値最大72%減少、²¹³Bi保持率0%で最大21%減少）。これは本研究が新規貢献として構想していた「`η_i`の連続変数化」と発想上ほぼ同一であり、20%刻みの離散サンプリングと連続変数の実質的な違いは小さい。**したがって、拡張①の新規性は以下の3点にさらに絞り込む**：
1. Koniarのモデルは保持率低下分の線量を系から「消失」させるだけで、遊離核種がどこへ行くかを明示的にモデル化しない。本研究は独立分布`p_i^free(r)`への**空間的再分配**を明示的に扱う（線量保存則を伴う）。
2. Koniarは単一細胞〜細胞クラスタスケールに留まり、腫瘍スケール（1 mm³）のTIA分布への接続は行っていない。
3. Koniarは²²⁵Ac単核種の解析であり、¹⁷⁷Luとの混合場（貢献B）には一切触れていない——`η_i`が混合場の最適`f`・`c`設計に与える影響は誰も評価していない。

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

## 4. 全体としての結論（13件中10件を全文確認、2026-09-22 第3回更新）

当初計画書の §2.1 で示した「先行研究の到達点」は、今回の検索でおおむね裏付けられた。ただし、**当初の想定より先行研究は密度が高く**、特に

- 核種分解カーネル（貢献A）
- µm分解能マルチ核種比較フレームワーク（Phase 2 相当）

は、想定より確立が進んでいた。一方で、**最も脅威度が高いと判断した2件（L2, L5）を全文で確認した結果、以下が確定した**：

- **貢献B（2核種同時投与・活性比fと空間共局在度cを独立変数とする混合場の一般式）は、依然として文献上未確立である。** 最も近いL5（Tranel 2022）でさえ「²²⁵Acと¹⁷⁷Luを同一腫瘍に同時投与する設計は一切行っていない」ことが本文の明記（"CAF and tumors were not considered as sources at the same time"）から確定した。**したがって、本研究のコア（§12 コア・トラック）である貢献Bを主軸に据えた設計判断は、全文確認を経てなお支持される。**
- **貢献A（娘核種保持率η_iの連続変数化）も、全文確認により新規性の範囲が確定した。** L2（Hu 2025）の「6種の空間分布」は離散的な幾何カテゴリであり、L5（Tranel 2022）は娘核種再分布を明示的に「シミュレートしていない」。したがって「η_iを0–1の連続変数として束縛/遊離集団を線形補間する」という本研究の定式化は、L1・L2という直接の先行研究が存在してもなお、狭いながら妥当な貢献として残る。
- **L13（de Kruijff et al. 2019）の全文確認により、Phase 3bのη_Biパラメータ範囲の実測アンカーが、単一の孫引き値（≥69%）から複数条件の精密なレンジ（腫瘍内保持比0.58–0.91）に格上げされた。** これはPhase 3bのパラメータ範囲設定（§6）の妥当性を一層強く補強する。
- **L3（Ghaseminejad et al. 2025）の全文確認により、貢献Aへの脅威が想定より小さいことが判明した。** 同論文は²²⁵Ac連鎖を「Whole Chain DPK」として単一カーネルで扱っており、核種分解（L2の方式）は採用していない。これは本研究のコア設計（Phase 1コアで実効カーネル、拡張①で核種分解）と整合的であり、貢献Aの新規性をむしろ補強する。
- **L11（Chi 2026, bioRxiv）の全文確認により、当初「2026年時点でも未解決と評価する独立した状況証拠」として位置づけていた評価を大幅に下方修正した。** 査読前・単著・産業界所属（RDC開発企業）で、方法論もGaussianカーネル近似にとどまりMonte Carlo輸送を行っていない。**ただし、この論文が²²⁵Acと¹⁷⁷Luの線量を常に同一の抗原密度マップから計算している（`c=1`を検証なしに仮定している）こと自体が、本研究が指摘する「暗黙の仮定」の実例として引用価値を持つ**——状況証拠としてではなく、問題の具体例として位置づけ直す。

**全体として、13件中10件の全文確認を経て、本研究計画の根幹（コア＝貢献B）は揺らいでいない一方、拡張①（貢献A）の新規性は当初の想定よりもかなり狭いことが確定した。** 特にL1（Koniar et al. 2023）の発見——²²¹Fr/²¹³Bi保持率を0–100%でスイープする感度解析が単一細胞スケールで既に実施済みという事実——により、拡張①は「η_iの導入」ではなく「η_iの**空間的再分配モデルへの拡張**、および**混合場文脈での感度評価**」という、より限定的だが依然として妥当な貢献に絞り込まれた。

さらに2つの副次的だが重要な発見があった。

- **L8（Liubchenko et al. 2024）とL1（Koniar et al. 2023）の対比**：臓器スケールでは娘核種別PKの厳密な分離が線量推定に与える影響はわずか8%（L8）だが、細胞スケールでは²²¹Fr保持率の効果だけで最大72%に達する（L1）。**このスケール依存性そのものが、本研究の核心的動機（マクロな線量評価の結論は微視的スケールにそのまま外挿できない）を直接裏付ける一次証拠であり、Introductionで積極的に活用すべきである。**
- **L10（Rumiantcev et al. 2023）の発見**：²²⁵Acの線量反応関係（DSB数 vs 線量）は本論文で線形（LQではない）としてフィットされている。本計画書§5.6で想定していたLQ交差項`2√(β_Ac β_Lu)D_Ac D_Lu`は、`β_Ac→0`の場合ほぼ消失する可能性があり、**拡張②の理論的定式化を修正する必要がある**（詳細は本研究計画書§5.6への追記を参照）。

貢献B（コア）については、L1・L6・L8・L9・L10のいずれも²²⁵Acと¹⁷⁷Luを同一腫瘍に同時投与する混合場として扱っておらず、当初の評価を覆す証拠は見つからなかった。残る未確認文献（L4, L7, L12）の全文確認が、正式なPhase 0における次の優先課題である。

---

## 5. 全文取得依頼リスト（更新：2026-09-22 第3回受領分を反映）

13件中10件が全文確認済み。**残る未達は3件のみ**。

### 未達（残り3件）
- **L4**（優先）：Yan K, Jiang Y, Wang R, et al. A fast convolution-based method for microdosimetric comparison of ²²⁵Ac, ²¹¹At, ¹⁷⁷Lu and ¹⁶¹Tb at the cell cluster scale. *Phys Med Biol*. 2026;71:125005. doi:10.1088/1361-6560/ae7892 **※2回試行したがIOPscience購読ページのみ。別ルート（機関アクセス、著者リポジトリ等）が必要**
- **L12**（中優先）：Peter R, Bidkar AP, Bobba KN, et al. 3D small-scale dosimetry and tumor control of ²²⁵Ac radiopharmaceuticals for prostate cancer. *Sci Rep*. 2024;14. doi:10.1038/s41598-024-70417-3
- **L7**（低優先）：Delker A, Schleske M, Liubchenko G, et al. Biodistribution and dosimetry for combined [¹⁷⁷Lu]Lu-PSMA-I&T/[²²⁵Ac]Ac-PSMA-I&T therapy using multi-isotope quantitative SPECT imaging. *Eur J Nucl Med Mol Imaging*. 2023;50(5):1280–1290. doi:10.1007/s00259-022-06092-1

### 完了（全文確認済み、計10件）
- ~~L1~~（Koniar et al. 2023）、~~L2~~（Hu et al. 2025）、~~L3~~（Ghaseminejad et al. 2025）、~~L5~~（Tranel et al. 2022）、~~L6~~（Tranel et al. 2021、NIHMS経由）、~~L8~~（Liubchenko et al. 2024）、~~L9~~（Wurzer et al. 2025）、~~L10~~（Rumiantcev et al. 2023）、~~L11~~（Chi 2026 bioRxiv）、~~L13~~（de Kruijff et al. 2019）

### 未発見（要追加検索）
- [P6] ²²⁵Ac 崩壊連鎖の核データ評価に関する専用文献：該当論文を未特定。ICRP Publication 107 が現状の代替根拠。
