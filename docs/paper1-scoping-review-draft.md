# Paper I — ドラフト（第1版）

**位置づけ**：本計画書 §8.3 で定義した Paper I（"Microdosimetric dose calculation for ²²⁵Ac/¹⁷⁷Lu combination radiopharmaceutical therapy: a scoping review"）の第1稿。`docs/phase0-literature-extraction.md` で確認した13件（12件全文確認・1件抄録確認）を基盤に執筆した。

**重要な限界（正直な開示）**：本稿の「Methods」節に記す通り、この検索は大学契約データベース（PubMed, Scopus, Web of Science）への直接アクセスではなく、Web検索エンジン経由の反復的ターゲット検索と、利用者提供PDFによる全文確認を組み合わせた**簡易的（rapid）スコーピングレビュー**である。正式な PRISMA-ScR 準拠のスコーピングレビューとして投稿する前に、大学図書館経由での網羅的データベース検索（本計画書 Phase 0 正式版）による裏付けが必須である。本稿はその正式版のための骨格・叩き台と位置づける。

投稿先候補：EJNMMI Physics（Review）／Phys Med Biol。英語本文はジャーナル投稿を想定した文体で記述する。

---

## Title

**Microdosimetric dosimetry for ²²⁵Ac/¹⁷⁷Lu combination radiopharmaceutical therapy: a scoping review of nuclide-resolved kernels, spatial co-localization, and daughter redistribution**

## Abstract

*Background*: Combination therapy with the α-emitter ²²⁵Ac and the β⁻-emitter ¹⁷⁷Lu is increasingly used in prostate-specific membrane antigen (PSMA)-targeted radioligand therapy, motivated by the complementary spatial scales of α- and β-particle energy deposition. Because physical absorbed dose is linearly additive, it is sometimes assumed that no new dosimetric formalism is required for such combinations. We conducted a scoping review to map what is established and what remains open regarding microdosimetric (sub-millimetre-scale) dose calculation for ²²⁵Ac/¹⁷⁷Lu mixed fields.

*Methods*: We performed an iterative, targeted literature search (not a full database search) covering PubMed-indexed journals, EJNMMI Physics, Medical Physics, Physics in Medicine & Biology, Scientific Reports, and bioRxiv, followed by full-text verification of identified records. Thirteen records were identified as directly relevant; twelve were verified in full text and one from its abstract only. Data were extracted on nuclide scope, dosimetric scale, Monte Carlo method, treatment of daughter redistribution, spatial co-localization modelling, activity-ratio modelling, and uncertainty quantification.

*Results*: Validated Monte Carlo dose-point kernels and cellular S-values exist independently for ²²⁵Ac and ¹⁷⁷Lu, including nuclide-resolved decomposition of the ²²⁵Ac decay chain and multi-radionuclide comparative frameworks. Patient-level combined ²²⁵Ac/¹⁷⁷Lu dosimetry has been reported, including a direct measurement of inter-nuclide spatial correlation (Pearson r = 0.94–0.96 at the organ/voxel scale). Daughter-nuclide redistribution has been quantified extensively, with a striking scale dependence: its effect on absorbed dose is minor (≤8%) at the organ/voxel scale but substantial (up to 72%) at the single-cell scale. No identified study formulates a mixed dose field for two simultaneously administered radionuclides as a function of an independent activity-ratio parameter and a continuous spatial co-localization parameter. Time-dependent activity ratios and combined multi-factor uncertainty quantification for mixed fields were also not identified.

*Conclusion*: Single-radionuclide microdosimetry for ²²⁵Ac and ¹⁷⁷Lu is mature. The formulation of a two-radionuclide mixed dose field parameterized jointly by activity ratio and spatial co-localization — and its coupling to daughter-nuclide redistribution at the microdosimetric scale — remains an open problem and is the subject of ongoing doctoral research.

**Keywords**: ²²⁵Ac; ¹⁷⁷Lu; microdosimetry; combination radiopharmaceutical therapy; targeted alpha therapy; scoping review

---

## 1. Introduction

Radioligand therapy targeting the prostate-specific membrane antigen (PSMA) has become a standard option for metastatic castration-resistant prostate cancer, first with the β⁻-emitter ¹⁷⁷Lu [1,2] and, for patients refractory to β-emitter therapy, with the α-emitter ²²⁵Ac [3]. ²²⁵Ac decays through a chain of four α-emissions (via ²²¹Fr, ²¹⁷At, ²¹³Bi/²¹³Po, ²⁰⁹Tl/²⁰⁹Pb) to stable ²⁰⁹Bi, depositing high linear energy transfer (LET, 60–100 keV/µm) radiation over a range of tens of micrometres — roughly two orders of magnitude shorter than the millimetre-scale range of ¹⁷⁷Lu β⁻-particles [4,5].

This scale difference motivates combination ("tandem") regimens using both radionuclides concurrently or sequentially: ²²⁵Ac delivers highly localized, high-LET damage to well-perfused, antigen-positive cells, while ¹⁷⁷Lu's longer-range β-particles provide cross-fire to antigen-negative or poorly vascularized regions that ²²⁵Ac alone would under-dose [6,7]. Clinical experience with such combinations is accumulating [8,9].

For a physical absorbed-dose field arising from two co-administered radionuclides, linear superposition holds trivially:

D(**r**) = D_Ac(**r**) + D_Lu(**r**) = (Ã_Ac ⊗ K_Ac)(**r**) + (Ã_Lu ⊗ K_Lu)(**r**)

where Ã_i is the time-integrated activity density of nuclide *i* and K_i its dose-point kernel. Because this superposition is mathematically self-evident, it would be incorrect to claim that "mixed-field dosimetry for ²²⁵Ac/¹⁷⁷Lu has not been formulated." The open question is instead **which implicit assumptions this superposition relies on, whether those assumptions hold at the microdosimetric (sub-cellular to sub-millimetre) scale relevant to α-particle radiobiology, and how failures of those assumptions should be parameterized and quantified**. Four such assumptions are commonly implicit: (i) that the ²²⁵Ac decay chain completes *in situ*, ignoring daughter-nuclide recoil and redistribution; (ii) that the two radionuclides share an identical microscopic spatial distribution; (iii) that the activity ratio between the two nuclides is time-invariant; and (iv) that the biological effect of the combined field is a simple function of the summed physical dose, ignoring radiation-quality-dependent interactions.

This scoping review maps the current literature against these four assumptions, to establish — before a planned doctoral research programme addresses the gap — precisely which parts of the problem are already solved and which remain open.

## 2. Methods

### 2.1 Search strategy

Because this manuscript was prepared as a rapid, preliminary scoping review to scope a doctoral research plan (rather than as a stand-alone systematic review), we did not have direct access to a subscription bibliographic database (PubMed, Scopus, Web of Science) at the time of drafting. Instead, we conducted iterative, keyword-targeted searches using a general-purpose web search interface that indexes PubMed-listed abstracts and publisher pages, using search terms combining radionuclide names (²²⁵Ac, actinium-225, ¹⁷⁷Lu, lutetium-177, and decay-chain daughters ²²¹Fr, ²¹³Bi), dosimetry terms (dosimetry, microdosimetry, dose point kernel, S-value, voxel S-value), methodology terms (Monte Carlo, GATE, Geant4, PHITS, TOPAS), and combination-specific terms (combination, tandem, co-administration, activity ratio, daughter redistribution, microdistribution, co-localization). Citation snowballing (forward and backward) was applied to records returned by the initial searches. This process yielded 13 directly relevant records (Table 1).

Twelve of the 13 records were subsequently obtained as full-text PDFs (via author, institutional, or preprint-server copies) and read in full; one record (Yan et al. [16]) remained accessible only via its published abstract owing to a subscription paywall, and is reported at abstract-level detail accordingly.

### 2.2 Inclusion criteria and data extraction

Records were included if they reported Monte Carlo or analytic dosimetry, at organ, voxel, or sub-cellular scale, for ²²⁵Ac and/or ¹⁷⁷Lu (alone or in combination), or reported directly relevant nuclear decay data or radiobiological (RBE) modelling for these nuclides. For each included record we extracted: nuclide scope (single or combined; chain-lumped or nuclide-resolved); spatial scale (organ/voxel, cell cluster, single cell, sub-cellular); Monte Carlo code and physics list; treatment of the ²²⁵Ac decay chain (lumped "effective" kernel vs individually simulated daughters); whether daughter-nuclide redistribution/retention was modelled, and if so, whether as a discrete geometric scenario or a continuous retention-fraction parameter; whether two radionuclides were combined in a single dose field, and if so, whether an activity-ratio and/or spatial-correlation parameter was varied independently; and any reported quantitative uncertainty analysis.

### 2.3 Limitations of this search

This is explicitly **not** a PRISMA-ScR-compliant systematic scoping review: no formal database search string was run against indexed databases, no dual independent screening was performed, no PRISMA flow diagram was generated, and publication bias / grey literature coverage cannot be assessed. Several searched dimensions (in particular, time-dependent activity-ratio modelling and combined multi-factor uncertainty quantification for mixed fields, see §4.4) returned no directly competing record, but given the informal search strategy this should be read as "not found by this search" rather than "does not exist." A formal, database-indexed scoping review (planned as part of the doctoral research programme's own Phase 0) is required before any claim of an established gap is submitted for publication or grant review.

## 3. Results

Table 1 summarizes the 13 records. Below, findings are grouped thematically.

### 3.1 Validated single-radionuclide microdosimetric kernels

Independent groups have generated and cross-validated Monte Carlo dose-point kernels (DPKs) and cellular S-values for both ²²⁵Ac and ¹⁷⁷Lu. Koniar et al. [10] used GATE (with Geant4-DNA physics) to compute S-values for each member of the ²²⁵Ac decay chain individually (²²¹Fr, ²¹⁷At, ²¹³Bi, ²¹³Po, ²⁰⁹Tl, ²⁰⁹Pb) at single-cell and micrometastasis-cluster scale, for four discrete source geometries (membrane, cytoplasm, nucleus, whole cell), validating against MIRDcell within −4.7% to −6.9% for the summed chain (individual β-emitting daughters, e.g. ²⁰⁹Tl, showed larger relative deviations up to −70.9%, reflecting known limitations of the continuous-slowing-down approximation used by MIRDcell for low-energy electrons). Hu et al. [11] independently simulated all eight relevant nuclides (the seven ²²⁵Ac-chain members plus ¹⁷⁷Lu as a reference) with track-structure Monte Carlo (NASIC) across six discrete subcellular source distributions, reporting that subcellular distribution changes absorbed dose to the nucleus by up to 80% but relative biological effectiveness (RBE) by only up to 10%.

### 3.2 Multi-radionuclide comparative frameworks at micrometre resolution

Two independent groups have built micrometre-resolution, multi-radionuclide comparative dosimetry frameworks. Ghaseminejad et al. [12] developed 2-µm-resolution dose-point kernels and a "Biologic Effect Cell Kernel" convolution method to compare ²²⁵Ac, ¹⁷⁷Lu, and ¹⁶¹Tb at clinically relevant administered activities; notably, the ²²⁵Ac decay chain was treated as a single lumped "whole-chain" kernel rather than decomposed by individual daughter. Yan et al. [16] (abstract-level only) reported a fast Fourier-transform convolution method combined with a saturation-corrected microdosimetric-kinetic model to compare ²²⁵Ac, ²¹¹At, ¹⁷⁷Lu, and ¹⁶¹Tb at the cell-cluster scale, agreeing with direct PHITS simulation within 5%. In both cases, the multiple radionuclides were compared as **independent candidates** for a single administered therapy, never combined as a simultaneous mixed field.

### 3.3 Clinical dosimetry of combined ²²⁵Ac/¹⁷⁷Lu administration

Delker et al. [13] performed simultaneous dual-isotope quantitative SPECT/CT in eight patients receiving concurrent [¹⁷⁷Lu]Lu-PSMA-I&T and [²²⁵Ac]Ac-PSMA-I&T, imaging the 440 keV (²¹³Bi) and 208 keV (¹⁷⁷Lu) photopeaks in a single acquisition. Kidney and lesion standardized uptake values (SUVs) for the two radionuclides were strongly correlated (Pearson r = 0.94 and 0.96, respectively), with kidney absorbed dose (RBE-weighted, RBE = 5) on average 32% higher for ²²⁵Ac than ¹⁷⁷Lu (up to 106% in individual patients), attributed partly to renal accumulation of unbound ²¹³Bi. This is, to our knowledge, the only report of a directly measured spatial correlation between two simultaneously administered therapeutic radionuclides, providing an empirical anchor (r ≈ 0.94–0.96) for what "near-complete but imperfect co-localization" looks like in a real PSMA-targeted combination regimen — albeit at organ/voxel (millimetre) resolution, not the microdosimetric scale.

### 3.4 Daughter-nuclide redistribution: a striking scale dependence

Multiple independent lines of evidence establish that ²²⁵Ac daughter-nuclide redistribution is dosimetrically important, but its quantitative importance depends strongly on spatial scale. At the **organ/voxel scale**, Liubchenko et al. [14] compared three dosimetry methods in five patients — using ²¹³Bi alone, ²²¹Fr alone, or both daughters with separately fitted pharmacokinetics — and found the resulting kidney and lesion RBE-weighted absorbed doses differed by at most 8% between methods. Wurzer et al. [15] and Peter et al. [17] independently confirmed, in mouse models, that free ²¹³Bi contributes substantially to off-target organ dose (kidney dose increased by a factor of 1.2–1.4 in [15]; free ²¹³Bi accounted for 70–80% of total kidney dose in [17]) while tumour-retained progeny showed negligible redistribution in both studies. At the **single-cell scale**, however, Koniar et al. [10] swept ²²¹Fr and ²¹³Bi retention from 100% to 0% in 20% increments and found that self-dose S-values decreased by up to 72% (for ²²¹Fr) and 21% (for ²¹³Bi) as retention decreased — an order of magnitude larger relative effect than the ≤8% found at the organ scale for the same underlying phenomenon. de Kruijff et al. [18], using ²²⁵Ac-loaded polymersomes as a model carrier, measured tumour-retained ²¹³Bi fractions of 58–91% depending on carrier size and time point, and parent ²²⁵Ac retention of only ~93% (not 100%) even within a nanocarrier. None of the daughter-retention studies parameterized retention as a continuous variable coupled to an explicit, independently distributed "free" spatial population; all used discrete geometric scenarios (subcellular compartment choice) or a coarse (20%) sweep without an explicit spatial redistribution model for the released fraction.

### 3.5 Spatial microdistribution and single-radionuclide selection

Tranel et al. [19,20] used GATE Monte Carlo to compare ⁹⁰Y, ¹⁷⁷Lu, ²¹¹At, and ²²⁵Ac in bone-marrow and tumour-stroma geometries, varying the spatial separation between source and target cell populations (e.g. cancer-associated fibroblasts vs tumour cells, at mean separations of 92–1030 µm). These studies consistently frame the question as "which single radionuclide is best suited to this tissue microarchitecture," never combining two radionuclides in the same dose field with an independent activity-ratio variable. A related non-peer-reviewed preprint [21] applies a similar "radionuclide selection under heterogeneous spatial distribution" framing to TROP2-targeted constructs using a simplified (non-Monte-Carlo, Gaussian-kernel) dose approximation; notably, that work computes doses for both a α- and a β-emitting construct from the *same* underlying antigen-density map, i.e. implicitly assuming perfect co-localization (equivalent to *c* = 1 in the notation of our planned framework) without testing that assumption — an instructive real-world illustration of the implicit assumption this review, and the planned doctoral work, aims to make explicit and test.

### 3.6 Relative biological effectiveness and dose-response modelling

Rumiantcev et al. [22] used TOPAS/TOPAS-nBio track-structure simulation coupled to the MEDRAS DNA-repair model to estimate RBE of ²²⁵Ac relative to ¹⁷⁷Lu, fitting the ¹⁷⁷Lu dose–DSB relationship as linear-quadratic but the ²²⁵Ac relationship as **linear** (quadratic coefficient ≈ 0), with RBE ranging from ≈2.1 (initial damage) to 8–11 (post-repair, at 0 Gy) decreasing toward ≈1.5–2 by 50 Gy. Peter et al. [17] independently modelled ²²⁵Ac tumour-control probability using a linear-quadratic survival model with the quadratic term likewise set to zero (α = 1.8 Gy⁻¹, β ≈ 0). These two independent studies both support a near-linear dose-response for ²²⁵Ac, which has implications for how a combined-field biological-effect model should treat any α-β cross-term (see §4.3).

### 3.7 Nuclear decay data

Huang et al. [23] provide the standard ENSDF-based evaluation of ²²⁵Ac decay-chain nuclear data (half-life 10.0 ± 0.1 d; γ-ray emission probabilities), which together with ICRP Publication 107 [24] forms the basis for Monte Carlo source definitions in the studies reviewed here. A more recent half-life and γ-ray intensity remeasurement was also identified in preliminary search but not yet obtained in full text.

## 4. Discussion

### 4.1 What is established

Contrary to a naïve reading of the literature, single-radionuclide microdosimetry for ²²⁵Ac and ¹⁷⁷Lu is mature: validated Monte Carlo kernels exist at multiple spatial scales, multi-nuclide comparative frameworks at micrometre resolution have been built and cross-validated, patient-level combined-administration dosimetry has been performed (including a direct measurement of inter-nuclide spatial correlation), and daughter-nuclide redistribution has been extensively characterized at the organ scale. Any research proposal that frames its contribution as "the first dosimetry for ²²⁵Ac/¹⁷⁷Lu combination therapy" would not survive review against this literature.

### 4.2 The gap: a jointly parameterized mixed dose field

No record identified in this review formulates the combined ²²⁵Ac/¹⁷⁷Lu (or any two-radionuclide) dose field as an explicit function of two independent design variables — an activity ratio *f* and a continuous spatial co-localization parameter *c* — nor evaluates a resulting *D*(**r**; *f*, *c*) phase diagram. The closest studies [13,19,20] either measure the two nuclides' realized (not varied) co-localization in real patient images, or vary a spatial-separation parameter for one radionuclide at a time when *choosing between* candidates rather than *combining* them. This is not a matter of degree: the underlying experimental and modelling designs in every identified study exclude, by construction, the joint (*f*, *c*) parameterization proposed here.

### 4.3 A narrower, but still open, question for daughter-nuclide redistribution

The single-cell-scale daughter-retention sweep already performed by Koniar et al. [10] substantially narrows what remains novel regarding daughter redistribution. What is not yet addressed is (i) an explicit spatial redistribution model in which the released fraction is assigned to an independently distributed population *p*_free(**r**) — rather than being discounted from the dose budget without a spatial destination — allowing tissue- (not single-cell-) scale calculations; and (ii) how daughter retention interacts with the *combination-specific* design variables *f* and *c*, which no daughter-redistribution study has considered, since all such studies to date examine ²²⁵Ac in isolation.

The scale-dependence finding in §3.4 (≤8% effect at organ scale vs up to 72% at cellular scale, for the *same* physical phenomenon) is, in our view, the single most important empirical motivation surfaced by this review for pursuing microdosimetric — rather than organ-averaged — analysis of daughter redistribution in combination therapy.

### 4.4 Time-dependent activity ratio and multi-factor uncertainty quantification

We found no record modelling a time-dependent activity ratio *f*(*t*) arising from the differing physical half-lives (9.92 d for ²²⁵Ac vs 6.65 d for ¹⁷⁷Lu) and pharmacokinetics of a combination regimen, nor any combined, multi-factor uncertainty quantification (propagating spatial co-localization, daughter retention, activity ratio, and numerical/resolution uncertainty jointly) for a mixed radionuclide field. Given the informal nature of this search (§2.3), these should be read as provisional negative findings pending a formal database search.

### 4.5 An unresolved methodological inconsistency: RBE definitions

We note, without resolving it here, that the two studies computing RBE-like quantities for ²²⁵Ac relative to ¹⁷⁷Lu use markedly different definitions and obtain very different numerical ranges (survival/DSB-repair-based RBE ≈ 2–11 in [22] vs a raw complex-DSB-count ratio-based "RBE_TRT" of order 10³ in the biological-effect-kernel framework of [12]). This inconsistency is a secondary but non-trivial finding for any planned biological-effect extension of a mixed-field model, and should be addressed explicitly rather than by silently adopting one convention.

### 4.6 Limitations

This is a rapid, non-systematic review conducted without direct database access (§2.3); its negative findings (§4.2, §4.4) are provisional. A single record [16] is reported at abstract level only. Grey literature and non-English-language sources were not searched. A formal PRISMA-ScR-compliant scoping review, conducted through institutional database access, is planned as the next step of the associated doctoral research programme and should be completed before any gap statement in this manuscript is relied upon for a funding or publication decision.

## 5. Conclusion

Microdosimetry for ²²⁵Ac and ¹⁷⁷Lu individually, and organ-scale dosimetry for their combined clinical administration, are well established. What remains unaddressed is a microdosimetric mixed-field formalism that treats activity ratio and spatial co-localization as independent, jointly varied design parameters, coupled to an explicit spatial (not merely discrete-retention) model of daughter-nuclide redistribution. These form the basis of the accompanying doctoral research plan.

---

## Table 1. Summary of records included in this scoping review

| # | Reference | Scale | Nuclides | Two-nuclide mixed field? | Key contribution |
|---|---|---|---|---|---|
| [10] | Koniar et al. 2023, EJNMMI Phys | Single cell / cluster | ²²⁵Ac chain (7 nuclides individually) | No | Nuclide-resolved S-values; discrete 20%-step daughter-retention sweep |
| [11] | Hu et al. 2025, EJNMMI Phys | Cell nucleus (track structure) | ²²⁵Ac chain (8 nuclides) + ¹⁷⁷Lu (reference) | No | 6 discrete subcellular distributions; dose vs RBE sensitivity |
| [12] | Ghaseminejad et al. 2025, Med Phys | Cell (2 µm) | ²²⁵Ac (lumped chain), ¹⁷⁷Lu, ¹⁶¹Tb | No (independent comparison) | µm-resolution multi-nuclide DPK + biological effect kernel |
| [13] | Delker et al. 2023, EJNMMI | Organ/voxel (patient) | ²²⁵Ac + ¹⁷⁷Lu (8 patients, simultaneous) | Imaged together, not dose-summed | Empirical co-localization ρ = 0.94–0.96 |
| [14] | Liubchenko et al. 2024, EJNMMI | Organ/voxel (patient) | ²²⁵Ac + daughters (²²¹Fr, ²¹³Bi) | N/A (single-radionuclide PK) | Daughter-specific PK changes dose by ≤8% |
| [15] | Wurzer et al. 2025, J Nucl Med | Organ (mouse) | ²²⁵Ac + daughters | N/A | Non-equilibrium daughter dose factor 1.2–1.4× |
| [16] | Yan et al. 2026, Phys Med Biol (abstract only) | Cell cluster | ²²⁵Ac, ²¹¹At, ¹⁷⁷Lu, ¹⁶¹Tb | No (independent comparison) | FFT convolution + saturation-corrected MK model |
| [17] | Peter et al. 2024, Sci Rep | µm (autoradiography, mouse) | ²²⁵Ac + daughters | No | Free ²¹³Bi = 70–80% of kidney dose; linear TCP model |
| [18] | de Kruijff et al. 2019, Sci Rep | Organ (mouse) | ²²⁵Ac + ²¹³Bi | N/A | Tumour ²¹³Bi retention 58–91% (carrier-dependent) |
| [19] | Tranel et al. 2022, EJNMMI Phys | mm–µm (tumour model) | ¹⁷⁷Lu or ²²⁵Ac (independent) | No | Spatial-separation parameter, single-nuclide selection |
| [20] | Tranel et al. 2021, Phys Med Biol | mm–µm (bone marrow) | ⁹⁰Y, ¹⁷⁷Lu, ²¹¹At, ²²⁵Ac (independent) | No | Candidate-nuclide comparison; α dose does not scale linearly with activity |
| [21] | Chi 2026, bioRxiv (preprint) | 2D (pathology image) | ¹⁷⁷Lu-like vs ²²⁵Ac-like (TROP2) | Implicitly assumes c=1 | Illustrates the untested co-localization assumption |
| [22] | Rumiantcev et al. 2023, EJNMMI Phys | Cell / DNA damage | ²²⁵Ac vs ¹⁷⁷Lu (independent) | No | RBE via TOPAS-nBio/MEDRAS; ²²⁵Ac dose-response linear |
| [23] | Huang et al. 2007, Appl Radiat Isot | Nuclear data | ²²⁵Ac chain | — | ENSDF-based decay data evaluation |

---

## References（EJNMMI Physics形式、暫定）

1. Strosberg J, El-Haddad G, Wolin E, et al. Phase 3 trial of ¹⁷⁷Lu-Dotatate for midgut neuroendocrine tumors. N Engl J Med. 2017;376(2):125–35.
2. Sartor O, de Bono J, Chi KN, et al. Lutetium-177–PSMA-617 for metastatic castration-resistant prostate cancer. N Engl J Med. 2021;385(12):1091–103.
3. Kratochwil C, Bruchertseifer F, Giesel FL, et al. ²²⁵Ac-PSMA-617 for PSMA-targeted α-radiation therapy of metastatic castration-resistant prostate cancer. J Nucl Med. 2016;57(12):1941–4.
4. Sgouros G, Roeske JC, McDevitt MR, et al. MIRD Pamphlet No. 22: radiobiology and dosimetry of α-particle emitters for targeted radionuclide therapy. J Nucl Med. 2010;51(2):311–28.
5. Bolch WE, Eckerman KF, Sgouros G, Thomas SR. MIRD Pamphlet No. 21: a generalized schema for radiopharmaceutical dosimetry. J Nucl Med. 2009;50(3):477–84.
6. Kratochwil C, Bruchertseifer F, Rathke H, et al. Targeted α-therapy of metastatic castration-resistant prostate cancer with ²²⁵Ac-PSMA-617: swimmer-plot analysis. J Nucl Med. 2018;59(5):795–802.
7. Rosar F, Hau F, Bartholomä M, et al. Molecular imaging and biochemical response assessment after a single cycle of [²²⁵Ac]Ac-PSMA-617/[¹⁷⁷Lu]Lu-PSMA-617 tandem therapy. Theranostics. 2021;11(9):4050–60.
8. Kheirf F, et al. ²²⁵Ac-PSMA-617/¹⁷⁷Lu-PSMA-617 tandem therapy of metastatic castration-resistant prostate cancer: pilot experience. Eur J Nucl Med Mol Imaging. 2020;47(3):721–8.
9. [追加確認予定：正式Phase 0で臨床combinationの症例数を更新]
10. Koniar H, Miller C, Rahmim A, Schaffer P, Uribe C. A GATE simulation study for dosimetry in cancer cell and micrometastasis from the ²²⁵Ac decay chain. EJNMMI Phys. 2023;10:46.
11. Hu Z, Qu S, Liu H, et al. Evaluation of relative biological effectiveness of ²²⁵Ac and its decay daughters with Monte Carlo track structure simulations. EJNMMI Phys. 2025;12:65.
12. Ghaseminejad S, De Sarno D, Bauman G, Lee TY. Framework to calculate ²²⁵Ac, ¹⁷⁷Lu, and ¹⁶¹Tb radiation dose and biological effect in metastatic castration-resistant prostate cancer treatment. Med Phys. 2025;52(8):e18035.
13. Delker A, Schleske M, Liubchenko G, et al. Biodistribution and dosimetry for combined [¹⁷⁷Lu]Lu-PSMA-I&T/[²²⁵Ac]Ac-PSMA-I&T therapy using multi-isotope quantitative SPECT imaging. Eur J Nucl Med Mol Imaging. 2023;50(5):1280–90.
14. Liubchenko G, Böning G, Zacherl M, et al. Image-based dosimetry for [²²⁵Ac]Ac-PSMA-I&T therapy and the effect of daughter-specific pharmacokinetics. Eur J Nucl Med Mol Imaging. 2024;51:2504–14.
15. Wurzer A, Sun B, Saleh S, et al. [²²⁵Ac]Ac-PSMA I&T: a preclinical investigation on the fate of decay nuclides and their influence on dosimetry of salivary glands and kidneys. J Nucl Med. 2025;66(12):1964–9.
16. Yan K, Jiang Y, Wang R, et al. A fast convolution-based method for microdosimetric comparison of ²²⁵Ac, ²¹¹At, ¹⁷⁷Lu and ¹⁶¹Tb at the cell cluster scale. Phys Med Biol. 2026;71:125005.
17. Peter R, Bidkar AP, Bobba KN, et al. 3D small-scale dosimetry and tumor control of ²²⁵Ac radiopharmaceuticals for prostate cancer. Sci Rep. 2024;14:19938.
18. de Kruijff RM, Raavé R, Kip A, et al. The in vivo fate of ²²⁵Ac daughter nuclides using polymersomes as a model carrier. Sci Rep. 2019;9:11671.
19. Tranel J, Palm S, Graves SA, Feng FY, Hope TA. Impact of radiopharmaceutical therapy (¹⁷⁷Lu, ²²⁵Ac) microdistribution in a cancer-associated fibroblasts model. EJNMMI Phys. 2022;9:67.
20. Tranel J, Feng FY, St James S, Hope TA. Effect of microdistribution of alpha and beta-emitters in targeted radionuclide therapies on delivered absorbed dose in a GATE model of bone marrow. Phys Med Biol. 2021;66(3):035016.
21. Chi WY. Computational pathology and spatial microdosimetry guide radiopharmaceutical selection for TROP2-targeted alpha versus beta radionuclide drug conjugates (RDCs). bioRxiv [Preprint]. 2026.
22. Rumiantcev M, Li WB, Lindner S, et al. Estimation of relative biological effectiveness of ²²⁵Ac compared to ¹⁷⁷Lu during [²²⁵Ac]Ac-PSMA and [¹⁷⁷Lu]Lu-PSMA radiopharmaceutical therapy using TOPAS/TOPAS-nBio/MEDRAS. EJNMMI Phys. 2023;10:53.
23. Huang X, et al. Evaluation of ²²⁵Ac decay data. Appl Radiat Isot. 2007;65(6):712–23.
24. ICRP. Publication 107: nuclear decay data for dosimetric calculations. Ann ICRP. 2008;38(3).

---

## 執筆メモ（次にやるべきこと）

1. **参考文献[8]の症例数・[9]の追加文献**：正式なPhase 0実施後、臨床combination療法の報告数を最新化する。
2. **PRISMA-ScRフロー図**：正式なデータベース検索を実施した時点で追加する（現状は検索式のみ記述し、フロー図は意図的に省略している——存在しないものを描かないという誠実性のため）。
3. **Table 1の`c`列**：可能であれば、各研究が暗黙に仮定している共局在度（`c=1`固定 / 実測 / 未定義）を明示する列を追加すると、Discussion §4.2の主張がより視覚的に伝わる。
4. **L4（Yan et al. 2026）の全文確認**：入手できれば、Table 1・Results §3.2の記述を全文ベースに格上げする。
5. **投稿前の必須作業**：共著者（指導教員）によるレビュー、正式Phase 0の実施、及びこの草稿が「暫定的」であることの明記を投稿時にも残すか、正式版に格上げしてから明記を外すかの判断。
