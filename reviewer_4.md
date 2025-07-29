# neurips-2025-rebuttal

## Reply to Reviewer 2iCx

We thank the reviewer for their constructive feedback and positive evaluation. Below, we address each concern in detail:

### 1. Clarification of Task Type (Classification, Detection, Segmentation)

We appreciate the request for clarification regarding the nature of the task and how we handle images with multiple genera.

**Task Type: Patch-Level Classification (Not Detection or Segmentation)**
ReefNet adopts a **point-based classification** framework, where each annotation corresponds to a single coral genus at a specific spatial location—rather than to the entire image or to pixels. We extract **fixed-size patches (512×512 px)** centered on each annotation point. This patch size was selected after empirical evaluation to balance broader structural context and fine-grained texture, while minimizing background clutter—a strategy common in coral imagery research (e.g., *Scalable semantic 3D mapping of coral reefs with deep learning Sauder et al.*; *Deep learning for multi-label classification of coral conditions Shao et al.*; *Deep learning for multi-label classification of coral conditions Gómez-Ríos et al.*).

When an image contains multiple genera, **each point is treated independently**. This enables fine-grained, localized classification and is naturally suited to multi-genus images.

We will revise Sections 3 and 5.1 to clearly state that:

* Each patch is centered on a point annotation.
* Classification is performed at the **patch level**, not over the full image or per pixel.
* Images with multiple genera yield multiple labeled patches.

### 2. Table 1 Class Counts

We will revise Table 1 to explicitly include the **number of annotated classes** (species, genus, or family level, where available) for each dataset. This update will significantly improve the interpretability of the dataset comparison and reinforce ReefNet’s role in supporting fine-grained coral classification.

### 3. Domain Adaptation and Data Augmentation

We respectfully clarify that domain adaptation strategies—particularly via **aggressive data augmentation**—**were already applied** in our cross-domain evaluations.

Specifically, we incorporated a suite of augmentations including **RandAugment**, **color jitter**, **horizontal flipping**, **Mixup**, **CutMix**, and **random erasing** (`reprob = 0.1`). These augmentations are established tools for improving robustness under high domain variability in lighting, turbidity, and reef morphology (*RandAugment Cubuk et al.*, *CutMix Yun et al.*). While listed in Supplementary §S3.1, we will make them more prominent in the main text as **deliberate domain generalization mechanisms**.

To **validate their impact**, we ran an additional ablation **with all augmentations disabled**. Using ViT-B/16 on the cross-source split (Train-S4/Test-S3), balanced accuracy dropped from **42.30% (with augmentation)** to **39.99% (without augmentation)**—a **4.1-point decrease**, despite all other settings remaining fixed. This underscores the critical role augmentation plays in promoting cross-domain generalization in ReefNet.

In future work, we aim to explore **explicit domain adaptation techniques** (e.g., adversarial alignment, moment matching), but our current setup already reflects strong adaptation baselines through data augmentation alone.

### 4. Closed-Set Evaluation and Open-Set Recognition

ReefNet’s current evaluation protocol is intentionally **closed-set**, designed to benchmark generalization across geographic domains while keeping the label space fixed. This supports rigorous, reproducible model comparisons.

We agree that **open-set recognition** is a valuable direction, especially for ecological monitoring where novel taxa frequently arise. While our current setup does not perform explicit novelty detection (as formalized in *Dissecting out-of-distribution detection and open-set recognition Wang et al., 2024*), our **zero-shot vision-language experiments** (e.g., CLIP with natural language prompts) evaluate generalization without any ReefNet-specific training. This partially emulates open-set behavior by bypassing supervised fine-tuning, although it still uses a fixed label space.

ReefNet includes many long-tailed and infrequently sampled taxa, making it a strong candidate for future open-set extensions. We will clarify this in the revised manuscript and cite Wang et al. (2024) to highlight its relevance.

### Additional Feedback Replies

**a) Macro-Recall Terminology**
Thank you for noting this. While “macro-recall” is technically correct (the unweighted mean of per-class recalls), we acknowledge that “Balanced Accuracy” is the more widely recognized term in ML literature. We will revise the terminology for clarity and consistency.

**b) Highlighting in Table 4**
We appreciate this observation. We will correct the formatting in Table 4 to ensure that only the **best-performing values per column** are highlighted. These inconsistencies were unintentional and will be addressed in the camera-ready version.

**c) Loss Function Parameters**
In our loss function ablation (Table 4), we evaluated several class-balanced and standard variants. Below are the exact settings:

* **Cross-Entropy Loss**: PyTorch `nn.CrossEntropyLoss()`
* **Focal Loss**: `γ = 2`, `α = 0.25` (as in *Focal Loss for Dense Object Detection Lin et al.*)
* **Class-Balanced Cross-Entropy**: `β = 0.9999` with effective number reweighting (*Class-Balanced Loss Based on Effective Number of Samples Cui et al.*)
* **Class-Balanced Focal Loss**: `β = 0.9999`, `γ = 2` (Cui et al.)

These configurations are now included in the Supplementary Material for full reproducibility.

**d) Confusion Matrix and Absent Classes in Figure S9**
We agree that showing zero-diagonal classes can be misleading. This occurred due to auto-generation with the full class list. We will update Figure S9 to include only classes present in the test set, so the matrix accurately reflects true confusion patterns.
