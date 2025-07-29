# neurips-2025-rebuttal

## Reply to the Third Reviewer ue6z 

We thank the reviewer for the detailed critique and address each weakness or editorial note in turn.

### 1. Abstract omits annotation type (“classification/point labels”)

To avoid ambiguity, we will revise the abstract to **explicitly state the annotation type and task**. While the current text mentions “genus-level hard-coral annotations,” it omits the fact that these are **sparse point-based labels** used for **patch-level classification**.

To clarify this, we will update the sentence:

> “...totaling ∼925K genus-level hard-coral annotations with expert-verified labels.”

to:

> “...totaling ∼925K **sparse point-level genus annotations** with expert-verified labels, supporting localized **patch-level classification**.”

This modification accurately reflects ReefNet’s annotation format and task setup, helping readers distinguish it from image-level classification, object detection, or segmentation pipelines.
We appreciate the reviewer’s feedback and believe this clarification will strengthen the paper’s clarity for both machine learning and ecological audiences.

### 2. Clarifying Scope: Focus on Hard Corals in Abstract and Title

ReefNet indeed focuses **exclusively on hard corals (Order: Scleractinia)** — the most ecologically foundational, structurally distinct, and consistently annotated group in global reef-monitoring programs. While other benthic taxa (e.g., soft corals, algae, sponges) may be present in some imagery or initial annotations, they are **explicitly excluded** from ReefNet’s curated label set. This exclusion reflects both our study’s focus and the practical consideration that our team’s taxonomic validation expertise lies primarily in hard corals.

To eliminate ambiguity, we will:
- **Revise the abstract** to state:

> “…totaling ∼925K **point-level annotations of hard coral genera**, with other benthic taxa excluded…”

- **Clarify the dataset scope** in the Introduction and Section 3, noting that ReefNet is purpose-built for fine-grained classification of hard corals and aligned with international monitoring practices (e.g., NOAA NCRMP, AIMS LTMP, GCRMN).

- **Optionally revise the title** to mention “hard corals,” depending on editorial policy at the camera-ready stage. We agree that this change could further improve clarity if permitted.

We believe these updates will resolve any potential confusion regarding the dataset’s taxonomic scope.

### 3. Clarifying Patch Extraction Protocol (§5.1)

Each annotated point in ReefNet corresponds to a **centered square image patch of size 512×512 pixels**. This patch size was selected after pilot testing to balance local texture fidelity with sufficient spatial context for taxonomic classification.

To improve clarity:
- We will move this detail from §S3.1 to **§3.1 (Dataset Construction)** in the main paper.
- We will explicitly reference the patch extraction process in **§5.1 (Benchmark Setup)**.
- We will also update the **caption of Figure 4** to clarify that each sample shown corresponds to a centered patch around a labeled point.

These updates will ensure the patch-based classification protocol is fully transparent and accessible to readers unfamiliar with CoralNet-derived workflows.

### 4. Clarifying ReefNet vs. BenthicNet (Lines 90–94)

We appreciate the reviewer’s feedback and agree that the comparison with BenthicNet (Lines 90–94) should be stated more clearly.

**BenthicNet** is an important aggregation of diverse seafloor imagery sources, annotated using the **CATAMI substrate scheme**. However, its annotations are typically **image- or frame-level**, and label consistency varies across contributing datasets. Importantly, **no centralized quality control** or expert verification pipeline has been applied across the corpus. Its use of **WoRMS taxonomy is limited** and inconsistent across sources.

In contrast, **ReefNet was purpose-built** for **genus-level, point-based hard coral classification**, with over **925K annotations** curated from expert-reviewed sources and **systematically mapped to WoRMS taxonomy**. ReefNet also includes:
- A consistent **patch-level classification protocol**,
- Rigorous **label filtering and expert verification** for ambiguous or inconsistent classes,
- And two benchmarking splits (within-source, cross-source) for evaluating **domain generalization**—a critical challenge in ecological ML.

While BenthicNet provides broader benthic coverage, **ReefNet fills a distinct and essential role** by offering **fine-grained, taxonomically standardized, and spatially localized coral classification benchmarks**.

We view ReefNet as **complementary to BenthicNet** and are currently exploring the integration of **relevant hard-coral subsets from BenthicNet** into future versions of ReefNet, after applying **rigorous vetting and annotation verification** to ensure consistency with our taxonomic and quality standards.

We will revise the manuscript to clearly reflect these distinctions and plans.

### 5. Why WoRMS Instead of CATAMI

We thank the reviewer for this important question. Our choice of WoRMS (World Register of Marine Species) over the CATAMI classification scheme is deliberate and aligned with our goal of **fine-grained, taxonomically accurate hard coral genus classification**.

**Why WoRMS:**

* WoRMS is the global taxonomic authority for marine species, offering **curated genus- and species-level identifiers (AphiaIDs)** that support taxonomic consistency and traceability across datasets. It enables **scientific reproducibility**, biodiversity modeling, and integration with major biological repositories such as OBIS, GenBank, and FathomNet (World Register of Marine Species Costello et al.).
* The use of AphiaIDs ensures **persistent taxonomic mapping**, which is essential in ecological monitoring pipelines where names and classifications evolve.

**Why CATAMI is not sufficient:**

* The CATAMI classification scheme was designed for **coarse-grained habitat annotation**, organizing benthic imagery into broad biotic or substrate types (e.g., macroalgae, sand, generic coral). It emphasizes morphology over taxonomy and lacks genus-level resolution (CATAMI: The Collaborative and Automated Tools for Analysis of Marine Imagery Monk et al.).
* It is widely used in habitat mapping but does not support the **granularity and taxonomic structure** needed for genus-level classification of Scleractinian corals.

ReefNet’s design explicitly prioritizes **taxonomically grounded, point-level genus classification**, which is **not feasible with CATAMI**. While CATAMI remains valuable for general habitat mapping, **WoRMS uniquely supports the scientific objectives of ReefNet**.

We will revise the manuscript to clearly explain this motivation and cite relevant taxonomic standards to support our choice.

### 6. Consistency of point placement + points per image

We thank the reviewer for raising this important point. Sampling bias due to point placement is a well-known challenge in benthic ecology, where annotations are often influenced by the specific monitoring goals of each survey (e.g., targeting particular coral assemblages). To mitigate this, ReefNet is designed for global coverage and broad taxonomic representation, and we provide detailed metadata that supports transparent, bias-aware analysis. Specifically:

**(i) Documented point-generation method:**
Each ReefNet source includes a `point_method` field from CoralNet metadata specifying the sampling strategy: *simple-random* (51 sources), *stratified-random* (23 sources), or *uniform-grid* (2 sources). No source relies on purely manual (click-based) annotation, ensuring that point placement is both consistent and reproducible across sources.

**(ii) Manual refinement protocol:**
Following automatic seeding, annotators routinely adjust point positions to avoid image edges, blurred regions, or irrelevant objects (e.g., sand, shadows). This is part of CoralNet\'s standard annotation procedure.

**(iii) Per-image annotation density:**
ReefNet includes 925,322 annotations across 181,046 images, with a mean of 40 points/image and a median of 22. Densities range from 25 to 180 points/image—matching common survey protocols (e.g., 50-point CPCe, 25-point RLS).

**(iv) Bias mitigation:**
Because sampling method and point density are logged per image, users can:

* restrict analyses to specific methods (e.g., only stratified-random),
* normalize point counts (e.g., downsample to 50 points/image),
* incorporate point density as a covariate.

These strategies will be documented in Section 3.4 and Supplementary Fig. S5.

**(v) Transparency and guidance:**
We will expand Section 6 to explicitly discuss residual sampling bias and provide usage recommendations, following best practices from coral survey bias literature.

We hope this addresses the reviewer’s concern. ReefNet’s point-placement metadata and tools make it one of the most auditable and bias-aware benthic annotation datasets to date.

### 7. Expert filtering: protocol and qualifications (§3.1)

* **Team Composition:** Two senior domain experts (including the field leader) and seven Ph.D.-level coral taxonomists.
* **Procedure:** Stratified random sampling across 8,962 patches. Each expert independently assigned genus labels; decisions were accepted by majority vote. Patches with unanimous disagreement were marked “low-confidence” for later review. This raised inter-expert agreement from 73% to 92%.
* **Qualifications:** Two leads specialize in coral systematics, evolution, and reef-diversity mapping. Remaining team members hold doctorates in coral taxonomy. Full names and ORCID IDs will be included in Acknowledgements.

### 8. Data-access complexity (scripts + HuggingFace)

To simplify access, we will package a `pip`-installable CLI tool that:

1. Downloads public CoralNet source images we curated
2. Downloads annotations CSV
3. Includes a Jupyter Notebook to reproduce both benchmark splits with two expert agreement levels

The cross-source Al-Wajh split remains on HuggingFace for easy use via:

```python
datasets.load_dataset("reefnet/al-wajh")
```

### 9. Minor wording & formatting

* Change “coral‑reef” → “coral reef” throughout
* Delete rogue “2” at line 247
* Fix reference formatting:

  * Add venues for references 43 and 49
  * Remove boilerplate note in reference 42
