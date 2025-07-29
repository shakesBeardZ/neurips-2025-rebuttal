# neurips-2025-rebuttal

## Reply to the Third Reviewer ue6z 

We thank the reviewer for the detailed critique and address each weakness or editorial note in turn.

### 1. Abstract omits annotation type (“classification/point labels”)

The Abstract will be updated to read:
*“… **sparse point‑level hard‑coral labels** that support patch **classification** of 925K points across 181K in‑situ images.”*

### 2. Only hard‑coral annotations – clarify title and abstract

We agree and will add “*(hard corals)*” in both the title and the first sentence of the Abstract, plus a footnote in §3.1 noting that soft corals, algae, and invertebrates—although visible—are not annotated in this version of the dataset.

### 3. Patch extraction protocol unclear (§5.1)

Patches are square crops of size `512×512 px` centered on each point. We will move this detail (with schematic) to §3.1 and reference it in §5.1 and the caption of Fig. 4.

### 4. Lines 90–94: clarify gap and ReefNet vs BenthicNet

While BenthicNet is a valuable global collection of seafloor imagery, its annotations are primarily image- or frame-level and use the CATAMI scheme, which categorizes broad substrate types (e.g., sand, algae, generic coral). Though some of its source datasets reference WoRMS, this is limited and not systematically applied.

In contrast, ReefNet was purpose-built for genus-level classification of hard corals, with nearly one million point-level annotations, each rigorously curated and standardized via WoRMS taxonomy. Furthermore, ReefNet includes expert-reviewed subsets, fine-grained class filtering, and multiple benchmarking splits explicitly designed to evaluate domain generalization, a core challenge in ecological ML.

ReefNet fills a critical gap in the ecosystem by enabling taxonomically grounded, high-resolution, cross-site coral classification, which BenthicNet does not currently support. We will revise the paper to make this distinction unambiguous.

### 5. Why WoRMS instead of CATAMI?

We deliberately chose WoRMS because our goal is fine-grained, taxonomically valid classification of hard coral genera—a level of specificity not supported by CATAMI, which is designed for broader habitat categories. WoRMS provides globally curated genus-level identifiers (AphiaIDs), enabling ecological consistency, cross-dataset interoperability, and scientific traceability critical for biodiversity monitoring and taxonomic grounding.

We will revise the paper to clarify this motivation.

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
