# neurips-2025-rebuttal

## Reply to the First Reviewer 2iCx 

We thank the reviewer for their constructive feedback and positive evaluation. Below, we respond to each point of concern:

### 1. Clarification of task type (classification, detection, segmentation)

We appreciate the reviewer’s request for clarification regarding the nature of the task and how we handle images with multiple genera. Below is a detailed description:

**Task Type: Patch-Level Classification (Not Detection or Segmentation)**  
ReefNet adopts a **point-based classification** framework, where each annotation corresponds to a single coral genus at a specific spatial location—rather than to the entire image or to pixels. We extract **fixed-size patches (512×512 px)** centered on each annotation point. This patch size was selected after empirical evaluation to balance capturing both the broader coral structure and fine-grained texture, while minimizing background noise—a strategy common in reef imagery literature (e.g., *Sauder et al., Scalable semantic 3D mapping of coral reefs with deep learning*; *Shao et al., Deep learning for multi-label classification of coral conditions*; *Gómez-Ríos et al., Deep learning for multi-label classification of coral conditions*).

When an image contains multiple genera, **each point is treated independently**. This means that multiple distinct patches may be extracted from a single image, each associated with its own genus label. This setup enables fine-grained, localized classification and naturally accommodates multi-genus scenes.

We will revise Sections 3 and 5.1 to clearly state that:
- Each patch is centered on a point annotation.
- Classification is performed at the **patch level**, not over the full image or per pixel.
- Images with multiple genera yield multiple labeled patches.

### 2. Table 1 class counts

We will revise Table 1 to explicitly include the **number of annotated classes** (species, genus, or family level, where available) for each dataset. This addition will help readers contextualize ReefNet’s fine-grained taxonomic resolution—particularly its focus on **genus-level hard coral classification**, which distinguishes it from broader or coarser habitat-level datasets. We believe this update will make the dataset comparison more informative and highlight the unique contribution of ReefNet in supporting detailed coral biodiversity modeling.


### 3. Lack of domain adaptation techniques

While our main aim was to establish **baseline generalization** under domain shift, we did incorporate **strong augmentation-based strategies** that serve as a form of *implicit domain adaptation*:

* We applied a suite of augmentations—including **RandAugment**, **color jitter**, **horizontal flipping**, **Mixup**, **CutMix**, and **random erasing (reprob = 0.1)**—across all supervised and zero-shot experiments. This helped models generalize across visual variability typical of underwater reef imagery, such as lighting changes, turbidity, and structural diversity. (RandAugment: Practical automated data augmentation with a reduced search space, Cubuk et al.; CutMix: Regularization Strategy to Train Strong Classifiers with Localizable Features, Yun et al.)
* Although these choices were listed in Supplementary §S3.1, we agree they should be clearly described in the main manuscript as explicit domain generalization strategies.
* These augmentations yielded modest accuracy gains in cross-source evaluations, demonstrating their practical impact even in this initial benchmark.

We will revise the methodological sections to clearly call out these augmentation techniques as *domain-shift mitigation tools*, rather than mere preprocessing details. In the Discussion, we will outline future directions involving **explicit domain adaptation methods**—such as adversarial alignment or moment-based distribution matching—and highlight their potential applicability in coral imagery analysis.

### 4. Closed-set evaluation/lack of open-set recognition

ReefNet’s current evaluation protocol is intentionally **closed-set**, designed to benchmark generalization across geographic domains while keeping the label space fixed. This allows for rigorous and reproducible comparisons across models and training regimes.

We agree that **open-set recognition** is an important direction, particularly in ecological contexts where novel or rare taxa are frequently encountered. While our current setup does not include open-set evaluation in the formal sense (e.g., class rejection or novelty detection as discussed in *Dissecting out-of-distribution detection and open-set recognition Wang et al., 2024*), our **zero-shot vision-language experiments** (e.g., CLIP without fine-tuning) offer a step toward such generalization: models are evaluated without any ReefNet-specific supervision and rely solely on textual prompts.

That said, ReefNet includes many long-tailed or rarely sampled taxa in its metadata, making it a strong candidate for future open-set benchmarks. We will clarify these distinctions in the revised manuscript and cite Wang et al. (2024) to contextualize the importance of this direction.

### Additional Feedback Replies

**a) Macro-Recall terminology**
We appreciate the reviewer’s note. While “macro-recall” is a technically correct term reflecting the unweighted mean of per-class recall values, we acknowledge that “Balanced Accuracy” is the more widely recognized term in machine learning literature. We will revise the terminology throughout the paper and supplementary material for consistency with community standards.

**b) Highlighting inconsistencies in Table 4**
Thank you for pointing this out. We will carefully revise Table 4 to ensure that only the **best-performing values per column are highlighted**. Any inconsistencies were purely formatting oversights and will be corrected in the camera-ready version to avoid confusion.

**c) Loss function parameters for class-balanced loss**

In our loss function ablation (Table 4), we evaluated several standard and class-balanced variants. Below, we detail the exact configurations used:

* **Cross-Entropy Loss:** PyTorch’s default `nn.CrossEntropyLoss()`.
* **Focal Loss:** We follow Lin et al. (ICCV 2017), using `γ = 2`, `α = 0.25`, as proposed in their RetinaNet paper for addressing class imbalance.
* **Class-Balanced Cross-Entropy:** Based on Cui et al. (CVPR 2019), with `β = 0.9999`, using effective number-based reweighting.
* **Class-Balanced Focal Loss:** Also based on Cui et al., using `β = 0.9999`, `γ = 2`.

These settings were selected based on best practices from the literature and are now included in the Supplementary Material for full reproducibility. We appreciate the reviewer’s attention to detail, and we will make sure all loss configurations are clearly described in the final version.

**d) Confusion matrix and absent classes in Figure S9**
We agree that showing classes absent from the test set in the confusion matrix can be misleading. This occurred because the matrix was auto-generated using the full class list rather than filtered to match the test set. We will update Figure S9 to display only classes present during testing, ensuring that the matrix reflects meaningful confusion patterns and removing misleading zero-diagonal rows.

We appreciate the reviewer’s thoughtful comments and are committed to implementing these improvements in the final version.
