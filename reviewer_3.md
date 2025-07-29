# neurips-2025-rebuttal

## Reply to the Third Reviewer ue6z 

We thank the reviewer for the detailed critique and address each weakness or editorial note in turn.

### 1. Clarifying Annotation Type and Dataset Scope in Abstract and Title

We agree that the original abstract could more explicitly describe both the **annotation format** and the **taxonomic focus** of the ReefNet dataset.

#### (i) Annotation Type (Point-Based Classification)

While the current abstract mentions “genus-level hard-coral annotations,” it does not specify that these annotations are:

* **Sparse point-level labels**, and
* Used for **localized patch-based classification** rather than whole-image or per-pixel labeling.

To improve clarity, we will revise the sentence:

> “...totaling ∼925K genus-level hard-coral annotations with expert-verified labels.”

to:

> “...totaling ∼925K **sparse point-level genus annotations**, supporting localized **patch-level classification** with expert-verified labels.”

This update removes ambiguity about the nature of the task (classification) and annotation granularity (point-based), helping readers distinguish ReefNet from detection or segmentation datasets.

#### (ii) Scope: Hard Corals Only

ReefNet focuses **exclusively on hard corals (Order: Scleractinia)** — the most ecologically foundational and taxonomically structured group in global reef surveys. Although other benthic organisms (e.g., algae, soft corals, sponges) may appear in the imagery, they are **explicitly excluded** from ReefNet’s label set.

This reflects both:

* The dataset’s **targeted ecological scope**, and
* The taxonomic expertise of our verification team, which specializes in hard coral identification and labeling.

To avoid confusion, we will revise the abstract to:

> “…totaling ∼925K **point-level annotations of hard coral genera**, excluding other benthic taxa…”

We will also clarify ReefNet’s scope in **Section 1 (Introduction)** and **Section 3 (Dataset)**, and if permitted, we are open to **updating the paper’s title** to explicitly include “hard corals.”

These changes ensure that readers — whether from machine learning or ecology backgrounds — clearly understand the dataset’s annotation type and taxonomic boundaries from the outset.

### 2. Clarifying Patch Extraction Protocol (§5.1)

Each annotated point in ReefNet corresponds to a **centered square image patch of size 512×512 pixels**. This patch size was selected after pilot testing to balance local texture fidelity with sufficient spatial context for taxonomic classification.

To improve clarity:
- We will move this detail from §S3.1 to **§3.1 (Dataset Construction)** in the main paper.
- We will explicitly reference the patch extraction process in **§5.1 (Benchmark Setup)**.
- We will also update the **caption of Figure 4** to clarify that each sample shown corresponds to a centered patch around a labeled point.

These updates will ensure the patch-based classification protocol is fully transparent and accessible to readers unfamiliar with CoralNet-derived workflows.

### 3. Clarifying ReefNet vs. BenthicNet (Lines 90–94)

We appreciate the reviewer’s feedback and agree that the comparison with BenthicNet (Lines 90–94) should be stated more clearly.

**BenthicNet** is an important aggregation of diverse seafloor imagery sources, annotated using the **CATAMI substrate scheme**. However, its annotations are typically **image- or frame-level**, and label consistency varies across contributing datasets. Importantly, **no centralized quality control** or expert verification pipeline has been applied across the corpus. Its use of **WoRMS taxonomy is limited** and inconsistent across sources.

In contrast, **ReefNet was purpose-built** for **genus-level, point-based hard coral classification**, with over **925K annotations** curated from expert-reviewed sources and **systematically mapped to WoRMS taxonomy**. ReefNet also includes:
- A consistent **patch-level classification protocol**,
- Rigorous **label filtering and expert verification** for ambiguous or inconsistent classes,
- And two benchmarking splits (within-source, cross-source) for evaluating **domain generalization**—a critical challenge in ecological ML.

While BenthicNet provides broader benthic coverage, **ReefNet fills a distinct and essential role** by offering **fine-grained, taxonomically standardized, and spatially localized coral classification benchmarks**.

We view ReefNet as **complementary to BenthicNet** and are currently exploring the integration of **relevant hard-coral subsets from BenthicNet** into future versions of ReefNet, after applying **rigorous vetting and annotation verification** to ensure consistency with our taxonomic and quality standards.

We will revise the manuscript to clearly reflect these distinctions and plans.

### 4. Why WoRMS Instead of CATAMI

We thank the reviewer for this important question. Our choice of WoRMS (World Register of Marine Species) over the CATAMI classification scheme is deliberate and aligned with our goal of **fine-grained, taxonomically accurate hard coral genus classification**.

**Why WoRMS:**

* WoRMS is the global taxonomic authority for marine species, offering **curated genus- and species-level identifiers (AphiaIDs)** that support taxonomic consistency and traceability across datasets. It enables **scientific reproducibility**, biodiversity modeling, and integration with major biological repositories such as OBIS, GenBank, and FathomNet (World Register of Marine Species Costello et al.).
* The use of AphiaIDs ensures **persistent taxonomic mapping**, which is essential in ecological monitoring pipelines where names and classifications evolve.

**Why CATAMI is not sufficient:**

* The CATAMI classification scheme was designed for **coarse-grained habitat annotation**, organizing benthic imagery into broad biotic or substrate types (e.g., macroalgae, sand, generic coral). It emphasizes morphology over taxonomy and lacks genus-level resolution (CATAMI: The Collaborative and Automated Tools for Analysis of Marine Imagery Monk et al.).
* It is widely used in habitat mapping but does not support the **granularity and taxonomic structure** needed for genus-level classification of Scleractinian corals.

ReefNet’s design explicitly prioritizes **taxonomically grounded, point-level genus classification**, which is **not feasible with CATAMI**. While CATAMI remains valuable for general habitat mapping, **WoRMS uniquely supports the scientific objectives of ReefNet**.

We will revise the manuscript to clearly explain this motivation and cite relevant taxonomic standards to support our choice.

### 5. Consistency of Point Placement, Annotation Density, and Bias Transparency

Sampling bias from point-placement strategies is a known challenge in benthic ecology, often driven by specific monitoring goals (e.g., focusing on particular coral taxa or reef zones). While ReefNet models do **not use full-image context or sampling density directly**—since each annotation point is treated as an **independent patch-level sample**—we designed the dataset to maximize **transparency and reproducibility** in how those points were originally selected.

**(i) Explicit annotation method metadata**  
Each ReefNet image includes a `point_method` tag exported from CoralNet, specifying how annotation points were seeded:

- `simple-random` (51 sources)  
- `stratified-random` (23 sources)  
- `uniform-grid` (2 sources)  

Crucially, CoralNet **does not support fully manual, clicked-from-scratch annotations**—ensuring a baseline consistency in point generation across all sources.

**(ii) Standardized manual refinement**  
After automated point placement, annotators may adjust point positions to avoid image artifacts (e.g., edges, shadows) or target specific coral structures more accurately. While this introduces minor subjectivity, it follows **established ecological annotation protocols** and improves label precision. Some upstream surveys may still focus on certain genera (e.g., Acropora reef flats), but this reflects ecological—not procedural—bias.

**(iii) Annotation density statistics**  
ReefNet includes 925,322 point annotations across 181,046 images, with:

- **Mean**: 40 points/image  
- **Median**: 22 points/image  
- **Range**: 25 to 180 points/image  

We summarize these statistics in Section 3.3 and Supplementary Fig. S1.

**(iv) Metadata enables dataset auditing (not modeling)**  
While ReefNet does **not model** sampling pattern or density, we expose metadata such as `point_method` and point count per image to support downstream auditing. Users can optionally:

- Filter by method (e.g., only stratified-random annotations)  
- Restrict to consistent point densities  
- Study metadata correlations with performance  

This transparency supports robust secondary analyses without complicating the training pipeline.

**(v) Clear limitations and recommendations**  
We will expand Section 6 (Limitations) to explicitly discuss potential upstream bias and clarify that ReefNet’s strength lies in making such factors **auditable and filterable**. This positions ReefNet as one of the most **transparent and reproducible patch-level coral annotation datasets** currently available.

We thank the reviewer for encouraging us to clarify these important design choices.

### 6. Clarifying Expert Verification and Reviewer Qualifications (Section 3.1)

Due to space constraints, the full filtering and verification procedure was not detailed in the main paper in 3.1. However, it is presented in **Supplementary Section S1**, with an overview diagram in **Figure S1**. We will revise **Section 3.1** of the main manuscript to better summarize this pipeline and clearly refer readers to the Supplementary for methodological transparency.

#### (i) Motivation for Expert Verification

We do **not assume that our experts are inherently “better”** than the original CoralNet annotators. Rather, we performed a **manual audit of on CoralNet sources** and discovered:

- Taxonomic inconsistencies (e.g., mixed naming conventions),
- Annotation noise (e.g., coarse or ambiguous labels),
- Incorrect genus-level assignments (e.g., mislabeling of Acropora genus).

These findings led us to initiate a **systematic expert-guided filtering process**, to raise label consistency, ecological fidelity, and taxonomic precision across ReefNet.

#### (ii) Expert Team Composition

Our verification team included:

- **Two senior coral domain experts**, one serving as field lead,
- **Seven Ph.D.-level coral taxonomists** with research expertise in coral systematics and reef ecology.

All reviewers are active contributors to global coral monitoring efforts. Their names and ORCID IDs will be added in the Acknowledgments.

#### (iii) Review Protocol

We conducted **stratified random sampling** over 8,962 patches, assigning 10 patches per genus per source to each reviewer without overlap. Reviewers:

- Labeled each patch independently,
- Flagged low-quality or ambiguous samples,
- Requested escalation to a more senior expert when needed.

Labels were accepted via **majority agreement**, and patches with persistent disagreement were excluded or flagged as low-confidence. This **raised inter-expert agreement from 73% to 92%**, indicating substantial reliability gains through the review process.

#### (iv) Contribution of Expert Verification

The expert filtering did not seek to “override” CoralNet, but to:

- Normalize labels using **WoRMS AphiaIDs** for taxonomic rigor,
- Resolve inconsistencies across sources,
- Ensure a **reliable, reproducible benchmark** for ML-based coral classification.

We will revise the manuscript to clearly communicate the intent and structure of this verification process. We thank the reviewer again for the opportunity to clarify this key aspect of the ReefNet pipeline.

### 7. Data-access complexity (scripts + HuggingFace)

We agree that the steps included in the process of downloading ReefNet data and reproducing what we have on the paper can be a bit of a headache, To simplify access, we will package a `pip`-installable CLI tool that:

1. Downloads public CoralNet sources images that were included in ReefNet study, the user will have control over which sources to include or exclude
2. Downloads annotations CSV files from the ReefNet repository
3. Commands to generate the two benchmark splits (`high_conf` and `mod_conf`) with expert agreement levels
4. Includes a Jupyter Notebook to reproduce both benchmark splits with two expert agreement levels
5. Provides a simple API to load the dataset via HuggingFace
This will allow users to easily generate the splits and access the dataset without needing to manually download or process files.
The CLI tool will be documented in the repository and will include examples for common use cases.
This will allow users to easily generate the splits and access the dataset without needing to manually download or process files.

The Al-Wajh dataset will be made available on HuggingFace:

```python
datasets.load_dataset("reefnet/al-wajh")
```

**The minor wording, formatting, and reference corrections noted by the reviewer will be addressed in the camera-ready version.**