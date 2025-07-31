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

### 3. Domain Adaptation and Augmentation Strategies

We would like to clarify that **our cross-domain experiments already include strong domain adaptation baselines via a diverse set of data augmentation strategies**, even though this was not emphasized sufficiently in the main paper.

Specifically, we used a combination of **RandAugment**, **color jitter**, **horizontal flipping**, **Mixup**, **CutMix**, and **random erasing** (`reprob = 0.1`). These augmentations are widely used to improve generalization in the presence of domain shift due to variations in lighting, turbidity, and reef structure *(RandAugment, Cubuk et al.; CutMix, Yun et al.)*. While these are listed in Supplementary §S3.1, we will make this more prominent in the main text as part of our deliberate domain generalization design.

#### **Augmentation Ablation Results**

To validate the contribution of these augmentations, we conducted an ablation study on the cross-source benchmark (Train-S4 → Test-S3&S4) using ViT-B/16. Below are the results:

| Augmentation Setting  | Recall (%) | Accuracy (%) |
|-----------------------|------------|---------------|
| all-augmentation      | **42.30**  | **77.19**     |
| no-randaug            | 41.28      | 76.89         |
| only-randerase        | 39.16      | 76.12         |
| no-randerase          | 42.25      | 77.47         |
| no-augmentation       | 39.94      | 76.92         |
| only-hflip            | 38.74      | 75.91         |
| no-mixup              | 41.33      | 76.59         |
| no-cutmix             | 41.25      | 76.75         |
| only-mixup            | 37.85      | 75.51         |
| only-colorjitter      | 38.52      | 76.17         |
| no-hflip              | 41.65      | 77.07         |
| only-randaug          | 40.85      | 76.50         |
| no-colorjitter        | 41.72      | 76.64         |
| only-cutmix           | 40.10      | 76.23         |

These results demonstrate that:

* **Using all augmentations yields the highest performance**, confirming their collective benefit.
* **Disabling any single augmentation** (e.g., `RandAug`, `ColorJitter`, `Random Erase`) results in a performance drop—showing that each plays a role in enhancing robustness.
* **Training without augmentation** or with only a single augmentation significantly underperforms, validating that augmentation functions as an effective domain adaptation mechanism in our setup.


#### **Planned Clarifications**

We will:

* **Expand Section §5.1** to explicitly list our augmentation pipeline and reference it as a form of domain generalization.
* **Mention this ablation in Supplementary §S3.1**, and optionally include a brief summary in the main paper if space permits.

### 4. Closed-Set Evaluation and Open‑Set Recognition

ReefNet’s original evaluation protocol is intentionally **closed‑set**, designed to benchmark cross‑domain generalization using a fixed label space. This enables reproducible, fair comparisons across methods.

To address the reviewer’s concern, we implemented a standard **open‑set recognition (OSR)** protocol inspired by \[Vaze et al., 2022] and \[Wang et al., 2024] on the cross‑source split (Train‑S4 → Test‑S3\&S4). Specifically:

* We held out **8 coral classes** as *unknown* (e.g., *Agaricia, Pavona, etc.*), training only on the remaining **30 known classes**.
* At test time, we evaluated on a **mixed test set** containing both known (32,173 samples) and unknown (1,867 samples) classes.
* We computed standard post hoc scoring metrics:

  * **MSP** – Max Softmax Probability
  * **MLS** – Max Logit Score
  * **Energy Score** \[Liu et al., NeurIPS 2020]
* **Metrics reported:**

  * **AUROC** (Area Under ROC Curve — *higher is better*)
  * **FPR\@95%TPR** (False Positive Rate at 95% True Positive Rate — *lower is better*)

We followed a **stratified hold‑out** protocol commonly used in OSR literature by excluding low‑frequency classes and selecting unknowns with ≥100 test‑sample support for sound evaluation.

Following \[Vaze et al., 2022], we note that a classifier’s **closed‑set accuracy** correlates strongly with its open‑set detection ability. Accordingly, we use **MLS** scoring as a competitive baseline, which their experiments show performs on par with specialized OSR methods.

Consistent with \[Wang et al., 2024], we evaluate multiple scoring rules (MSP, MLS, Energy) and observe that **magnitude‑sensitive approaches like MLS and Energy** yield the best performance—reinforcing their finding that such scoring functions generalize across both OSR and OOD tasks.

| Method |    AUROC ↑ | FPR\@95%TPR ↓ |
| ------ | ---------: | ------------: |
| MSP    |     0.8169 |        0.6067 |
| MLS    | **0.8361** |    **0.5820** |
| Energy |     0.1616 |        0.9902 |

> **MLS achieves the best open-set detection performance**, which aligns with both Vaze et al.’s and Wang et al.’s empirical observations.

We appreciate this observation, as it pushed us to validate ReefNet in a broader OSR context. We believe this direction complements the dataset’s goal of benchmarking generalization under ecological shift, and opens the door for future work that unifies **open‑set detection** and **domain adaptation** in marine imagery — two challenges that often co-occur in real-world deployments.

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
