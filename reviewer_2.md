# Reply to the second reviewer 3TBH

We sincerely thank the reviewer for their thorough assessment and constructive comments.
In the following, we address each concern in turn. For clarity, we organize our reply into numbered headings that mirror the points of the reviewer.

## 1. Novelty Beyond *CoralNet*

**Clarification on CoralNet's Nature:**
CoralNet is primarily an annotation platform hosting thousands of independent image sets ("sources") contributed by different research groups, each with its own labeling conventions and quality standards. It does not constitute a standardized or harmonized dataset suitable for benchmarking.

**Novel Contributions of ReefNet:**

- **Taxonomic Harmonization:**
  We mapped and standardized all ~925K point labels to the *World Register of Marine Species (WoRMS)*, establishing the first genus-level, WoRMS-consistent dataset specifically designed for hard coral reef analysis using machine learning.

- **Expert-Driven Quality Assurance:**
  We conducted expert re-verification of nearly 9K randomly sampled labels, improving validation agreement from 73% to 92%. This ensures significantly higher reliability compared to the raw CoralNet data, which lacks clear quality indicators per source.

- **Innovative Benchmark Structure:**
  ReefNet introduces structured *in-distribution (ID)* and *out-of-distribution (OOD)* splits, enabling evaluation in line with contemporary ML benchmarking practices—an essential feature missing in CoralNet.

- **Geographical Novelty:**
  ReefNet uniquely includes 1.3K expert-verified images from Al-Wajh in the Red Sea, addressing a critical gap since this region is both ecologically important and understudied.

- **Future-proofing and Expansion:**
  ReefNet's framework is designed for ongoing growth, with active plans to integrate substantial institutional datasets (e.g., NOAA’s NCRMP, AIMS Long-Term Monitoring) following the same rigorous standards.

We believe these critical enhancements and clarifications underscore the significance of ReefNet’s contributions, transforming fragmented image resources into a cohesive and reliable benchmark for coral reef research.

## 2. The Limited Number of Text Descriptions

For the *Within-source Test-S2* and *Cross-source Test-S3&S4* splits, we have up to 39 unique hard coral genera. Each genus is associated with two distinct textual descriptions ("GPT" and "Book"), generated using the pipeline described in S6 and Section 5.3. While additional descriptions can be generated using the same pipeline—for example, for instruction fine-tuning—we restrict ourselves to these 39 for each of the "Qwen-GPT" and "Qwen-Book" experiments. This is because our goal is to evaluate inference-time performance without any additional training, where a single description per genus is sufficient. This setup allows us to demonstrate how such descriptions support the zero-shot performance of Qwen2.5-VL, as evidenced by the results discussed in Section 5.3.

## 3. Motivation for Expert Agreement Splits: Balancing Label Quality and Coverage

We agree the rationale for expert-agreement splits needs clearer explanation and will revise Section 4 accordingly.  
As detailed in Section 3.4 and Table 2, ReefNet defines two quality tiers based on expert agreement during re-verification:

- **Moderate-confidence (81% agreement):** Includes source–genus pairs with ≥50% agreement, offering broader coverage and diversity (803K annotations).  
- **High-confidence (92% agreement):** Stricter subset with pairs ≥70% agreement, yielding 446K annotations with higher reliability.

Each tier has both **within-source** and **cross-source** splits for flexible benchmarking. In the **cross-source** setup, both training sets (Train-S3, Train-S4) are tested on the same high-confidence test set (Test-S3&S4), enabling controlled comparison of **more data vs. cleaner labels**. This supports studying dataset size vs. annotation quality tradeoffs in a real ecological context.

As Table 3 shows, the larger moderate-confidence split often yields better results, a trend also seen on Al-Wajh data (Test-W). 

## 4. Single-label Nature of ReefNet and Fig. 4 Clarification
**(a) Patch-level, single-label classification**
ReefNet follows the standard ecology workflow: each image is overlaid with *sparse points*. A fixed-size patch (512×512 px) is cropped around every point and assigned *one* genus label.
A model performs *single-label* prediction *per patch*, not at the image level.

**(b) Fig. 4 confusion**
The figure presents example **patches**, each labeled based only on the **center point**, which is where the annotation was made. However, multiple objects may appear within the same patch — as seen in the rightmost example. However, the dataset does not support multi-label annotations, a limitation we highlight in the next point.

To remove ambiguity in Fig. 4 we will:
- Revise the caption to say: *“Qualitative patch-level examples from the test set.”*
- Clarify in the caption that the label corresponds to the object at the patch’s center point.
- Add a schematic in the supplementary showing how multiple patches originate from a single image (mean ≈ 40 annotated points per image).

**(c) Image- vs. Patch-level Multi-label Support**  
Each image contains many annotated points, making ReefNet *multi-label at the image level* (avg. five genera/image). However, at the patch level (512×512 px), fewer than 1.7% contain multiple labels, dropping to 0.4% and 0.1% in the in-source and cross-source test splits. This rarity limits the feasibility of multi-label classification at the patch level.


**(d) Why Point-Patch Annotation (vs. Boxes or Dense Masks)**

- *Percent cover surveys:* Random-point annotation has been the reef ecology standard for ~20 years and CoralNet datasets are based on this approach.
- *Annotation cost:* Full masks or boxes are time-consuming and labor-intensive. Automatic tools also struggle with the domain's visual complexity.

**(e) Road-map to dense masks**
We release full-image tiles and point coordinates to enable weakly-supervised segmentation or object detection in future work.


**(f) Hierarchical Classification as a Multi-label Experiment**

Instead of single-label genus classification, we used both genus- and family-level labels per point (all hard corals share the same order taxonomy). We trained a ViT with two heads—*ViT-2Head*—for genus and family on the cross-source benchmark (Train-S4 → Test-S3&S4). The single-head baseline is *ViT-Genus*. *ViT-2Head* improved genus accuracy by over **2%**.

Because genus maps one-to-one to family, family-level classification was evaluated via:
- **ViT-2Head-Family**: direct family prediction
- **ViT-Genus & ViT-2Head-Genus**: indirect family prediction by genus-to-family mapping

| Model            | Genus Recall | Family Recall |
|------------------|--------------|---------------|
| ViT-Genus        | 42.30        | 52.82         |
| ViT-2Head-Genus  | 44.70        | 54.33         |
| ViT-2Head-Family | N/A          | 53.74         |

To analyze class-wise behavior, we examined three genera:

- **Astreopora** (*Acroporidae*), relatively common  
- **Echinopora** (*Merulinidae*), less common  
- **Echinophyllia** (*Lobophylliidae*), less common

While ViT-2Head improved overall performance, benefits varied by genus; rare genera showed no consistent gains from the hierarchical setup.

| Genus         | Family         | ViT-Genus (Genus) | ViT-Genus (Family) | ViT-2Head (Genus) | ViT-2Head (Family) |
|---------------|----------------|-------------------|--------------------|-------------------|--------------------|
| Astreopora    | Acroporidae    | 23.53             | 98.54              | 5.88              | 98.39              |
| Echinopora    | Merulinidae    | 4.44              | 45.71              | 26.67             | 89.42              |
| Echinophyllia | Lobophylliidae | 15.63             | 45.58              | 10.42             | 46.05              |


## 5. Licensing of Data, Images, and Code

We appreciate the reviewer’s suggestion and will declare licensing terms in the “Data Availability” section, `LICENSE` files, and GitHub `README`.

Our policy follows best practices for mixed-provenance datasets:

- **Images:**
  ReefNet *does not redistribute* CoralNet images. Instead, we provide:

  * A CSV with source URLs hosted on CoralNet
  * A Python script to download them, CoralNet sources require Creative Commons licenses (e.g., CC-BY, CC-BY-NC, CC-BY-NC-SA).We directly host only Al-Wajh (Red Sea) photos on HuggingFace, released under `CC-BY-NC-SA 4.0`.

- **Annotations & metadata:**
  `CC-BY-NC-SA 4.0` — allows free use for non-commercial purposes, with share-alike terms.

- **Source code:**
  `GPL-3.0` — compatible with the dataset license, ensuring reciprocal sharing of derivative software.

These choices ensure:

1. No copyright infringement
2. Open scientific use of annotations and code
3. Clear, consistent separation between data and software

## 6. Editorial Fixes

We thank the reviewer for the additional feedback and will incorporate all suggested fixes in the camera-ready version. In Figure 3, the markers near India and Hawaii reflect distinct sampling efforts and ecological contexts. Though geographically close, combining them would obscure meaningful differences in coral morphology, genetics, and environment. We will also consider the other suggestions to further improve the presentation.

## **Concluding Remark**

With these clarifications, we believe ReefNet provides a transparent, license-compliant, and rigorous benchmark for single-label coral classification across in- and out-of-distribution settings. We again thank the reviewer for their valuable feedback.
