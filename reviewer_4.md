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

We appreciate this suggestion and will update Table 1 to explicitly include the number of labeled classes in each dataset (where applicable), to better contextualize ReefNet’s fine-grained taxonomic resolution.

### 3. Lack of domain adaptation techniques

Thank you for this valuable suggestion. Our goal in this initial release was to benchmark **baseline performance** under domain shift using supervised and zero-shot models. Future work will include domain adaptation techniques (e.g., data augmentation, domain adversarial training), and we will mention this explicitly in the discussion.

We have also added augmentations (e.g., RandAugment, CutMix) in preliminary experiments, with moderate gains, which we plan to report in an extended version.

### 4. Closed-set evaluation / lack of open-set recognition

We agree that open-set recognition is an important frontier, especially in ecological settings where unseen classes are frequent. ReefNet’s current splits are **closed-set** by design to benchmark generalization across domains while holding the label set fixed. However, many rare classes are excluded from training but present in metadata. We plan to release an open-set split in future work. We thank the reviewer for referencing Wang et al. (2024), which we will cite and discuss in our revision.

### 5. Macro-Recall vs. Balanced Accuracy

Thank you — we will clarify in Section 5 that our use of "Macro Recall" is equivalent to "Balanced Accuracy" and adopt the standard terminology.

### 6. Table 4 highlighting errors

We appreciate the careful reading. We will correct any erroneous bold/underline formatting in Table 4 to ensure all highlights correctly reflect the best and second-best values per column.

### 7. Loss function parameters for class-balanced loss

We thank the reviewer for this important point. In our loss function ablation (Table 4), we evaluated several standard and class-balanced variants. Below we detail the exact configurations used:

* **Cross-Entropy Loss:** PyTorch’s default `nn.CrossEntropyLoss()`.
* **Focal Loss:** We follow Lin et al. (ICCV 2017), using `γ = 2`, `α = 0.25`, as proposed in their RetinaNet paper for addressing class imbalance.
* **Class-Balanced Cross-Entropy:** Based on Cui et al. (CVPR 2019), with `β = 0.9999`, using effective number-based reweighting.
* **Class-Balanced Focal Loss:** Also based on Cui et al., using `β = 0.9999`, `γ = 2`.

These settings were selected based on best practices from the literature and are now included in the Supplementary Material for full reproducibility. We appreciate the reviewer’s attention to detail, and we will make sure all loss configurations are clearly described in the final version.

### 8. Figure S9 confusion matrix

You're absolutely right — Figure S9 mistakenly includes classes absent from the test set. We will correct the figure to only show evaluated classes, ensuring a consistent and interpretable confusion matrix.

We appreciate the reviewer’s thoughtful comments and are committed to implementing these improvements in the final version.
