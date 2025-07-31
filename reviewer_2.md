# Reply to the second reviewer 3TBH

We sincerely thank the reviewer for their thorough assessment and constructive comments.
In the following, we address each concern in turn. For clarity, we organize our reply into numbered headings that mirror the points of the reviewer.

## 1. Novelty beyond *“just CoralNet”*

**Clarification on CoralNet's Nature:**
CoralNet is primarily an annotation platform hosting thousands of independent image sets ("sources") contributed by separate research groups, each with unique labeling conventions and quality levels. It does not constitute a standardized or harmonized dataset suitable for benchmarking.

**Novel Contributions of ReefNet:**

- **Taxonomic Harmonization:**
  We mapped and standardized all 925,000 point labels to the *World Register of Marine Species (WoRMS)*, establishing the first genus-level, WoRMS-consistent dataset specifically designed for coral reef analysis using machine learning.

- **Expert-Driven Quality Assurance:**
  We conducted rigorous validation involving expert re-verification of nearly 9,000 randomly sampled labels, significantly raising the inter-expert consensus from 73% to 92%, thus ensuring higher reliability compared to existing sources.

- **Innovative Benchmark Structure:**
  ReefNet introduces structured *in-distribution (ID)* and *out-of-distribution (OOD)* splits, enabling robust evaluation in line with contemporary ML benchmarking standards—an essential feature missing from CoralNet.

- **Geographical Novelty:**
  ReefNet uniquely includes 1,300 expert-verified images from Al-Wajh in the Red Sea, addressing a critical gap since this region is both ecologically important and understudied.

- **Future-proofing and Expansion:**
  ReefNet's framework is designed for ongoing growth, with active plans to integrate substantial institutional datasets (e.g., NOAA’s NCRMP, AIMS Long-Term Monitoring) following the same rigorous standards.

- **Visualization of Contributions:**
  To further clarify and demonstrate the transformative impact of ReefNet's curation process, we will enhance Section 3 with a before-and-after flow diagram and comparison table, clearly delineating the improvements and novelty introduced by ReefNet.

We believe these critical enhancements and clarifications underscore the significance of ReefNet’s contributions, transforming fragmented image resources into a cohesive, reliable, and innovative benchmark for coral reef research.

## 2. The Limited Number of Text Descriptions

For the *Within-source Test-S2* and *Cross-source Test-S3&S4* splits, we have up to 39 unique hard coral genera. Each genus is associated with two distinct textual descriptions ("GPT" and "Book"), generated using the pipeline described in S6 and Section 5.3. While additional descriptions can be generated using the same pipeline—for example, for instruction fine-tuning—we restrict ourselves to these 39 for each of the "Qwen-GPT" and "Qwen-Book" experiments. This is because our goal is to evaluate inference-time performance without any additional training, where a single description per genus is sufficient. This setup allows us to demonstrate how such descriptions support the zero-shot performance of Qwen2.5-VL, as evidenced by the results discussed in Section 5.3.

## 3. Motivation for Expert Agreement Splits: Balancing Label Quality and Dataset Coverage

We agree that the motivation behind the expert-agreement-based splits could be more clearly explained and will revise Section 4 accordingly.
As described in Section 3.3 and Table 2, ReefNet includes two quality tiers based on expert agreement thresholds during our re-verification procedure:

- **Moderate-confidence subset (81% agreement):** Includes all source–genus pairs with at least 50% expert agreement, yielding broader coverage and greater diversity (803K annotations).
- **High-confidence subset (92% agreement):** A stricter subset with only source–genus pairs achieving ≥70% expert agreement, resulting in 446K annotations with higher label reliability.

For each subset, we define both a **within-source** and a **cross-source** split, allowing flexible benchmarking.
In the **cross-source setup**, both training sets (Train-S3 and Train-S4) are evaluated on the same high-confidence test set (Test-S3\&S4), enabling a controlled comparison between training with **more data vs. cleaner labels**. This structure directly supports investigation into the tradeoff between dataset size and annotation fidelity in a real-world ecological setting.

As shown in Table 3 of the paper, this tradeoff is nuanced: in some cases, training on the larger moderate-confidence split yields better performance, while in others, the cleaner subset provides gains — especially for models more sensitive to label noise.

We will clarify this intent in the manuscript and explicitly note that the **high-confidence set is a filtered subset of the moderate-confidence set**. We appreciate the reviewer prompting us to strengthen the transparency of this benchmark design.

## 4. Multi-label nature of ReefNet and Fig. 4 clarification
**(a) Patch-level, single-label classification**
ReefNet follows the dominant ecology workflow: each image is overlaid with *sparse points*. A fixed-size patch (512×512 px) is cropped around every point and assigned *one* genus label.
A model performs *single-label* prediction *per patch*, not per whole image.

**(b) Why Fig. 4 looked confusing**
The figure presents example **patches**, each labeled based only on the **center point**, which is where the annotation was made. However, multiple objects may appear within the same patch — as seen in the rightmost example. The dataset, however, does not support multi-label annotations, a limitation we highlight in the next point.

To Remove ambiguity around Fig. 4 We will:
- Revise the caption to say: *“Qualitative patch-level examples from the test set.”*
- Clarify in the caption that the label corresponds to the object at the patch’s center point.
- Adding a schematic in the supplementary showing how multiple patches originate from a single image (mean ≈ 40 annotated points per image).

**(c) Image-level and patch-level multi-label support**  
Each image contains many annotated points, so ReefNet is *multi-label at the image-level*, with an average of five different genera per image. Investigating patch-level samples with a patch size of 512×512 px, we found that fewer than 1.7% of patches contain more than one label. This becomes even more scarce after applying the different splits in the proposed benchmarks, where the in-source and cross-source test samples contain only 0.4% and 0.1% of patches with multiple labels, respectively. This scarcity limits the design and evaluation of multi-label classification in ReefNet.

**(d) Why point-patch annotation (not bounding boxes or dense annotations)**

- *Percent cover surveys:* Random-point annotation within images has been the standard method in reef ecology for over 20 years, and CoralNet datasets are based on this approach.
- *Annotation cost:* Creating full object masks or bounding boxes is much slower and more labor-intensive. Furthermore, automatic annotation tools face significant challenges due to the complex visual characteristics of coral imagery.


**(e) Road-map to dense masks**
We release full-image tiles and point coordinates to enable weakly-supervised segmentation or object detection in future work.

## 5. Licensing of data, images, and code

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

## 6. Editorial tweaks (addressing Additional Feedback)

We will incorporate every suggestion in the camera-ready version:

**(a) Line 36 spacing:**
Use `Fig.~1` syntax to eliminate extra gap.

**(b) Table 1 formatting:**

- Increase font size
- Widen first column
- Add caption note: “Annotation type†” with footnote for point-patch protocol

**(c) Terminology harmonization:**
Introduce ML terms *in-distribution (ID)* and *out-of-distribution (OOD)* on first mention alongside “within-source” and “cross-source.”

**(d) Figure 3 visual polish:**

- Convert “600000” to “600 K”
- Use dot size for annotation count, dot color for genus diversity
- Preserve distinct markers for India and Hawaii to reflect different ecological contexts

**Note:** We have intentionally placed multiple source markers near India and Hawaii to reflect distinct sampling efforts in those regions. Although they are geographically close, they represent different coral collection events or ecological contexts, which could have implications for variation in coral morphology, genetics, or environmental parameters. Merging them into one would obscure this meaningful nuance.

**(e) Line 286 extra space:**
Remove doubled space before “41%”

## **Concluding Remark**

With these clarifications and edits, we believe ReefNet offers a transparent, license-compliant, and uniquely rigorous benchmark for multi-label, out-of-distribution coral reef image analysis.
We again thank the reviewer for their valuable feedback.
