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

- **Scalable Integration Pipeline:**
  Our proposed standardization and validation pipeline has already successfully integrated additional coral datasets beyond CoralNet (e.g., Moorea Labeled Corals, Pacific Labeled Corals, BENTHOZ-2015, Coralscapes, RSMAS Coral Texture set, Global Reef Record panoramic imagery), demonstrating scalability and robustness.

- **Future-proofing and Expansion:**
  ReefNet's framework is designed for ongoing growth, with active plans to integrate substantial institutional datasets (e.g., NOAA’s NCRMP, AIMS Long-Term Monitoring) following the same rigorous standards.

**Visualization of Contributions:**
To further clarify and demonstrate the transformative impact of ReefNet's curation process, we will enhance Section 3 with a before-and-after flow diagram and comparison table, clearly delineating the improvements and novelty introduced by ReefNet.

We believe these critical enhancements and clarifications underscore the significance of ReefNet’s contributions, transforming fragmented image resources into a cohesive, reliable, and innovative benchmark for coral reef research.

## 2. Domain‑specific text descriptions (*n = 40*) – small yet impactful

**(a) Why quality beats quantity for corals**
Unlike birds—which exhibit stable, easily verbalized field marks such as plumage color and bill shape (cf. CUB‐200 attributes)—corals pose three unique challenges:

1. *Morphological plasticity:* colony form changes with depth, light, and flow
2. *Convergent appearance:* unrelated genera can look nearly identical
3. *Taxonomic instability & synonymy:* genera are frequently split or merged as genetics outpaces morphology

Consequently, there is *no authoritative, large‐scale corpus* of non-conflicting coral descriptions—WoRMS itself provides only terse taxonomic notes.

**(b) Expert‑curated, conflict‑free prose**
Our 40 paragraphs (80 words each) were distilled from FAO regional field guides and cross-checked against recent genetic–morphology reconciliation studies to avoid contradictory traits.
Each text focuses on *growth form, corallite structure, and biogeography*—features most discriminative for vision–language alignment.

**(c) Empirical evidence of utility**
Using these descriptions as prompts boosts BioCLIP zero-shot macro-recall by **+6 pp** vs. generic ImageNet labels (Table 5).
This mirrors broader VLM work showing that even *few but domain-tuned* descriptions outperform label-only baselines and attribute lists.

*Clarification in manuscript:*
We will add two sentences to §4.1 explaining that the goal is *high-precision, domain-specific* prose rather than volume, and point to the observed zero-shot gains as evidence of effectiveness.


## 3. Why two expert–agreement splits? Balancing quantity & label fidelity

To clarify, we intentionally created two splits based on expert agreement levels to balance dataset size with label accuracy, which enables several research and benchmarking opportunities:

**(a) Realistic noisy-label benchmarking**
In ecological monitoring, expanding survey effort invariably introduces annotation noise (time pressure, varying expertise).
Robust-learning research therefore *explicitly* evaluates models on pairs of *clean* vs. *noisy* subsets—e.g., COCO-N and Cityscapes-N for instance segmentation, FNBench for federated learning, and ViT noise-robustness studies.
Our `High-conf` (92%) and `Mod-conf` (73%) splits serve the same purpose for coral imagery.

**(b) Quantified trade-off**

- `High-conf`: 118K images / 466K points — optimal for *benchmarking algorithms* where label precision is paramount.
- `Mod-conf`: 181K images / 925K points — captures rarer genera and long-tail phenotypes, valuable for *pre-training or semi-supervised* setups.

This mirrors the “clean/curated vs. large/unfiltered” dual-track adopted in many noisy-label studies.

**(c) Research opportunities enabled**

- *Noise-robust learning:* Train on `Mod-conf` and validate on `High-conf`—a standard protocol for methods like Pseudo-Loss Selection or SplitNet.
- *Active curation:* Algorithms that identify or correct mislabeled samples can be benchmarked by measuring how many `Mod-conf` points they “upgrade” to `High-conf`.

**(d) Manuscript clarification**
We will add a three-sentence paragraph for the “Quality–Quantity Splits,” citing the relevant literature and reporting the exact image/point counts for each partition.
A footnote will remind readers that `High-conf` is a proper subset of `Mod-conf`.

These additions clarify that the dual-split design is *intentional*: it gives the community a built-in, ecology-specific test bed for the rapidly growing field of label-noise-robust machine learning.

## 4. Multi-label nature of ReefNet and Fig. 4 clarification

**(a) Patch-level, single-label classification**
ReefNet follows the dominant ecology workflow: each image is overlaid with *sparse points*. A fixed-size patch (512×512 px) is cropped around every point and assigned *one* genus label.
A model performs *single-label* prediction *per patch*, not per whole image.

**(b) Why Fig. 4 looked confusing**
The figure shows example **patches**, so only one label is visible at a time.
We will:

- Revise the caption to say “Patch-level examples”
- Add a schematic in §3 (new Fig. 5) showing how multiple patches originate from one photo (mean ≈ 40 points/image)

**(c) Image-level multi-label statistics**
Each image contains many points, so ReefNet is *multi-label at the photo level*: on average five different genera per image (§3.4; new Supp. Fig. S5).
This mirrors other platforms like ReefCloud and CoralNet.

**(d) Why point-patch (not bounding boxes) matches reef ecology practice**

- *Percent cover surveys:* Random-point counts have been the standard for \~20 years.
- *Annotation cost:* Full object masks or boxes are much slower and rare in coral datasets.
- *Existing ML tooling:* CoralNet and 3D reef pipelines already classify patches this way.

**(e) Road-map to dense tasks**
We release full-image tiles and point coordinates to enable weakly-supervised segmentation or object detection in future work.

**(f) Manuscript edits**

- Rename §3.1 to “Point–patch annotation protocol” and add schematic
- Update Fig. 4 caption to “Patch-level examples (single label per patch)”
- Cite CoralNet and ReefCloud as prior art

These clarifications should remove ambiguity around Fig. 4 and explain why patch-based single-label classification is the de-facto standard for scalable reef monitoring.

## 5. Licensing of data, images, and code

We appreciate the reviewer’s suggestion and will declare licensing terms in the “Data Availability” section, `LICENSE` files, and GitHub `README`.

Our policy follows best practices for mixed-provenance datasets:

- **Images:**
  ReefNet *does not redistribute* CoralNet images. Instead, we provide:

  * A CSV with source URLs hosted on CoralNet
  * A Python script to download them
    CoralNet sources require Creative Commons licenses (e.g., CC-BY, CC-BY-NC, CC-BY-NC-SA).
    We directly host only Al-Wajh (Red Sea) photos, released under `CC-BY-NC-SA 4.0`.

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
