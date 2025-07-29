# neurips-2025-rebuttal

## Reply to the First Reviewer 2iCx 

We thank the reviewer for their constructive feedback and positive evaluation. Below, we respond to each point of concern:

### 1. Clarification of task type (classification, detection, segmentation)

ReefNet is designed for **point-based classification** — each sparse annotation corresponds to a single coral genus at a specific image location (not whole-image or per-pixel). The task is thus not detection or segmentation, but rather patch-level classification. We extract fixed-size patches (`224×224`) centered on annotation points. In cases where an image contains multiple genera, each point is treated independently with its own label. We will clarify this in Section 3 and 5.1.

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
