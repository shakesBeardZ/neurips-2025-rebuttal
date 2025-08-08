# Reviewer 3 — Follow-Up Response

We thank the reviewer for their constructive feedback and suggestions. Below, we address each point raised in the follow-up review.

## 1.i — Point coordinates & label-propagation/weak supervision use-cases

In ReefNet, we provide the exact pixel coordinates of every annotation in the form **(row, column)**. These coordinates are already used internally for curation (e.g., duplicate removal by **point-level coordinates**) and for training **512×512** crops around points. We acknowledge that the paper did not explicitly state that these coordinates are available to users; we will make this clear in §3 and in the dataset README on HuggingFace.

We will also explicitly mention possible downstream tasks beyond patch classification, including **point-label propagation**, sparse-to-dense pseudo-labeling, and weakly supervised segmentation.

## 5.i — Release of sampling-method metadata

Yes. The CSV file will be updated to include new columns: `point_method` (simple-random / stratified-random / grid) and `points_per_image` for each image. This will be documented in the paper and the dataset README.

## 6.iii — Expert-review protocol, agreement metrics, “patch” vs “point”

### How many reviewers annotated each point, and what was the inter-observer agreement?

In our verification process, there was no inter-observer agreement assessment, as each genus was reviewed by only one expert. Reviewers were assigned based on their specific expertise with particular genera, with the more challenging genera or samples verified by our internationally recognized taxonomist and senior coral ecologists.

### Was the level of skill of the annotator considered in this assessment?

Yes. We considered the annotators’ expertise and experience in coral taxonomy and ecology when assigning each set of genera to a verifier. In hard coral taxonomy, specialists often focus on particular genera or families; therefore, each set of genera was reviewed by a single verifier whose expertise matched that group. Assignments were made manually to ensure that the reviewer’s knowledge was directly relevant, and we trusted their judgment accordingly.

### Clarification of the 73%→92% claim

To clarify this claim, we first define the *expert agreement percentage* and then describe the two-stage filtering process that progressively removed low-confidence annotations (and their representative verified samples), thereby increasing the confidence in the filtered dataset.

**Definition of “expert agreement percentage.”**
In the verification process, for each source–genus pair, we manually reviewed up to 10 samples (or fewer if fewer than 10 were available). This yielded 8,962 verified samples, representative of the 920,017 total annotations in the dataset. The *expert agreement percentage* is the micro-averaged proportion of these verified samples judged correct by experts, relative to all verified samples.

**Initial expert agreement (73%).**
Before any filtering, the micro-average correctness across all 8,962 verified samples was 73%. This reflects the baseline quality of the raw dataset (representative of the 920,017 annotations).

**Two-stage filtering process.**
As outlined in Sec. 3.4, we applied two quality control steps:

1. **Source and Label Filtering** – Removed entire sources whose verified samples had <50% agreement (across all genera), and removed genera whose verified samples had <50% agreement (across all sources). This eliminated systematically low-quality sources and labels, increasing expert agreement on the remaining verified samples to 78% (860,463 annotations retained).

2. **Source–Genus Filtering** – Applied a stricter filter at the source–genus level: for each source–genus pair, if the verified subset for that pair did not reach 70% agreement, all annotations from that pair were excluded. This targeted removal of low-confidence local label–source combinations preserved high-quality subsets even when the overall source or genus performed well elsewhere.

**Resulting expert agreement (92%).**
After the second stage, the remaining dataset comprised 479,027 annotations, represented by 5,295 verified samples. On this verified subset, the micro-averaged correctness increased to 92%. This improvement was achieved solely by excluding low-confidence source–genus combinations; no labels were corrected or altered.

### What percentage of patches was corrected to result in this increase?

None. We did not globally “correct” point labels; rather, we excluded classes, sources, or source–genus pairs that failed the expert-agreement thresholds. After source/label filtering, 860,463 annotations remained (78% agreement). After source–genus filtering, the high-confidence subset retained 479,027 annotations (92% agreement). We will make this explicit in §3.4.

### “Patch” vs “point” labels

The ground truth is always the original point label. “Patch” refers only to (i) the 512×512 crop centered at the point used for model input, and (ii) the verification UI’s zoomable view for efficiency. Reviewers always had access to the full image context while making their judgments, ensuring that verification preserved whole-image context. We will clarify this in §3.4 and Supplement §S1.2.1.

**Note:** All the above clarifications will be added to the camera-ready version of the paper and the dataset README on HuggingFace.

## Access workflow & suggestion to use *Pixi*

We agree that the current dataset access protocol involves too many manual steps, and we appreciate the suggestion to use a task encapsulation tool such as Pixi. We evaluated Pixi and similar solutions, but decided to develop a pip-installable CLI tool to achieve the same streamlined, one-command setup while avoiding additional environment dependencies for users.

The CLI will automate the full process:

* Download public CoralNet source images used in ReefNet, with user-selectable sources.
* Retrieve all ReefNet annotation CSVs.
* Generate both benchmark splits (*high\_conf* and *mod\_conf*) based on expert agreement levels.
* Provide a minimal API for loading the Al-Wajh dataset directly via HuggingFace Datasets.
* Optionally launch a Jupyter notebook for users who want an interactive walk-through, but keep this step entirely optional.

The Al-Wajh Red Sea dataset will also be directly accessible:

```python
from datasets import load_dataset
load_dataset("reefnet/al-wajh")
```

This approach keeps the convenience of a single-step workflow while remaining platform-independent, lightweight, and reproducible.

**Note:** We cannot update the code or dataset on HuggingFace at this stage to comply with NeurIPS policy, which prohibits post-submission updates.