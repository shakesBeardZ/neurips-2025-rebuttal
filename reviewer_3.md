We thank the reviewer for the detailed critique and address each weakness or editorial note in turn.

### 1. Clarifying Annotation Type and Dataset Scope in Abstract and Title

We agree the abstract should more clearly reflect both the **annotation format** and the **taxonomic scope** of ReefNet.

#### (i) Annotation Type

We will revise the abstract to explicitly state that annotations are:

- **Sparse point-level labels**, used for
- **Patch-based classification** (not segmentation or detection)

We propose changing:

> “...∼925K genus-level hard-coral annotations...”

to:

> “...∼925K **point-level genus annotations**, supporting **localized patch classification**...”

We hope This clarifies both task type and annotation granularity.

#### (ii) Hard Coral–Only Scope

ReefNet focuses **exclusively on hard corals (Order: Scleractinia)**. Other benthic taxa (e.g., soft corals, algae) are visible but deliberately excluded from the label set, reflecting the dataset’s ecological focus and the expertise of our verification team.

We will revise the abstract to:

> “…∼925K point-level annotations of **hard coral genera**, excluding other benthic taxa…”

We will clarify this scope in **Sections 1 and 3**, and are open to adding “hard corals” to the title.

### 2. Clarifying Patch Extraction Protocol (§5.1)

Each ReefNet annotation is centered in a **512×512 pixel patch**, used for **patch-level classification**. This size was chosen after testing 224×224, 448×448, and 512×512 options to balance local detail and spatial context.

224×224 is standard in CoralNet and prior studies (Beijbom et al.; Williams et al.; Vega et al.), but 512×512 improved macro recall by **+8%**.

To improve clarity:
- We will move this detail from §S3.1 to **§3.1** (main paper)
- Mention patch extraction in **§5.1**
- Update **Figure 4 caption** to state that patches are centered 512×512 patches

These edits ensure transparency in our patch-based pipeline and evaluation.

### 3. Clarifying ReefNet vs. BenthicNet (Lines 90–94)

We agree the distinction between ReefNet and BenthicNet should be clearer.

**BenthicNet** aggregates diverse seafloor imagery annotated at the **image/frame level**, primarily using the **CATAMI scheme**. It lacks **centralized quality control**, and its use of **WoRMS taxonomy is inconsistent**. Annotations vary in format and taxonomic granularity across sources.

In contrast, **ReefNet is purpose-built** for **point-based, genus-level hard coral classification**, with over **925K annotations**:
- Consistently **mapped to WoRMS** taxonomy,
- Backed by **expert verification** (raising agreement from 73% to 92%),
- Structured for **patch-based benchmarking** with standardized within- and cross-source splits.

While BenthicNet offers broad habitat coverage, **ReefNet addresses the need for fine-grained, taxonomically standardized, and ML-ready coral benchmarks**.

We will revise the manuscript to clarify this distinction and note that we are exploring the future integration of hard coral subsets from BenthicNet—pending annotation verification and quality alignment.

### 4. Why WoRMS Instead of CATAMI

Our use of **WoRMS** (World Register of Marine Species) over **CATAMI** is a deliberate decision aligned with ReefNet’s goal of **fine-grained, taxonomically consistent genus-level classification of hard corals**.

#### Why WoRMS?

- WoRMS is the **global standard for marine species nomenclature**, widely adopted across marine biology, ecology, and biodiversity informatics (*Costello et al.*).
- It provides persistent genus/species identifiers (**AphiaIDs**) that support **long-term traceability, scientific reproducibility**, and integration with repositories like **OBIS, GenBank, and FathomNet**.
- Unlike image-only schemes, WoRMS integrates taxonomy based on **morphological and genetic analysis of physical specimens**, ensuring accuracy and scientific validity.

#### Why not CATAMI?

- **CATAMI** is a **hybrid morphological-taxonomic classification scheme** designed primarily for **coarse-grained habitat annotation** (e.g., macroalgae, sponge, sand) (*Monk et al.*).
- Its categories are standardized but often broad, and the **decision tree is purely morphology-driven**, with no formal taxonomic hierarchy embedded.
- It does not support **taxonomic resolution**, making it unsuitable for hard coral studies.

In short, while CATAMI is useful for habitat-level annotation, **WoRMS is uniquely suited for ReefNet’s goals** of a reproducible taxonomic hard coral benchmark.

We will revise the manuscript to include this rationale and cite the relevant references to clarify our design choice.

### 5. Consistency of Point Placement, Annotation Density, and Bias Transparency

Sampling bias from point-placement strategies is a known consideration in benthic ecology, often driven by monitoring goals (e.g., focusing on reef zones or coral types). While ReefNet models treat each annotation point as an **independent patch-level sample**, we designed the dataset to maximize **transparency and reproducibility** regarding how points were originally placed.

#### (i) Established methodology and metadata exposure

ReefNet point annotations originate from CoralNet, which uses automated methods like:

- `simple-random` (51 sources)
- `stratified-random` (23 sources)
- `uniform-grid` (2 sources)

Fully manual point-clicking is **not supported** in CoralNet, ensuring baseline procedural consistency.

Importantly, **random point count methods are a well-established ecological standard**, as shown in benthic surveys (Carleton & Done 1995; Kohler & Gill 2006), and are also widely applied in terrestrial studies (e.g., canopy cover, bird populations, vegetation structure). This reinforces that ReefNet’s point sampling is **scientifically grounded**.

#### (ii) Standardized manual refinement

Annotators may adjust points to avoid artifacts or improve label quality—following accepted protocols in marine annotation. While this may introduce slight subjectivity, it improves taxonomic accuracy and remains ecologically valid.

#### (iii) Annotation density statistics

ReefNet includes 925,322 points across 181,046 images:

- **Mean**: 40 points/image
- **Median**: 22
- **Range**: 25–180

We report this in Section 3.3 and Suppl. Fig. S1.

#### (iv) Auditability via metadata

While ReefNet does **not model** point density or sampling method during training, we expose these fields so users can:

- Filter by `point_method`
- Restrict to consistent densities
- Explore correlations between sampling and performance

#### (v) Limitation acknowledgment

We will expand Section 6 to note that while upstream sampling variability exists, ReefNet’s strength lies in making such factors **auditable and filterable**, unlike many ecological datasets. We thank the reviewer for encouraging this clarification.

### 6. Clarifying Expert Verification Process and Team Qualifications (§3.1)

We agree Section 3.1 should better summarize the expert verification process. Full details and a workflow diagram appear in **Supplementary §S1 and Fig. S1**, and we will revise the main text to highlight them more clearly.

#### (i) Why re-verify CoralNet labels?

We do **not claim our experts are inherently superior** to the original CoralNet annotators. However, our audit revealed:

- Taxonomic inconsistencies across sources,
- Mislabeling (e.g., Acropora genus errors),
- Lack of standardization in label conventions.

More importantly, **each CoralNet source is labeled by a different group**, often with varying taxonomic expertise. While this is acceptable within a source, it **introduces noise and bias when merging across sources**. ReefNet addresses this by applying **centralized expert verification across all sources**, establishing a consistent cross-source quality baseline that CoralNet alone cannot provide.

#### (ii) Who are the reviewers?

Our team included:

- **1 internationally recognized coral taxonomist**,
- **3 senior coral ecologists**, and
- **6 PhD-level experts** in coral systematics and reef monitoring.

Reviewer names and ORCID IDs will be added in the acknowledgments.

#### (iii) Review protocol

We used **stratified random sampling** to select **8,962 patches** (10 per genus per source). Each reviewer:

- Annotated patches independently,
- Flagged uncertain samples,
- Relied on **majority vote**; unresolved cases were excluded or flagged low-confidence.

This process improved inter-expert agreement from **73% to 92%**.

#### (iv) Goals of expert filtering

Rather than override CoralNet, our review process:

- Standardizes labels using **WoRMS AphiaIDs**,
- Resolves cross-source inconsistency,
- Produces a **reproducible, high-confidence benchmark** suitable for ML research.

We will revise **§3.1** to summarize this and explicitly cite the consistency benefits of centralized review.

### 7. Simplifying Data Access (Scripts + HuggingFace)

We acknowledge that reproducing the results from ReefNet currently involves multiple manual steps, and we appreciate the reviewer highlighting this.

To address this, we are preparing a **`pip`-installable CLI tool** that will streamline dataset access and preparation. Specifically, it will:

1. Download public CoralNet source images used in ReefNet, with user control over which sources to include.
2. Download all ReefNet annotation CSVs.
3. Generate both benchmark splits (`high_conf` and `mod_conf`) using expert agreement levels.
4. Include a Jupyter notebook demonstrating end-to-end setup of the splits.
5. Provide a minimal API for loading ReefNet through HuggingFace Datasets.

This enables easy dataset access and preparation.

In addition, the **Al-Wajh Red Sea dataset** will be made available directly on HuggingFace:

```python
from datasets import load_dataset
load_dataset("reefnet/al-wajh")
```

**The minor wording, formatting, and reference corrections noted by the reviewer will be addressed in the camera-ready version.**