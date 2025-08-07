# Reviewer 3 Follow-Up Response

## 1.i  — Point-coordinates & label-propagation use-cases

**Reviewer’s concern**: “1.i) I note you state that the data is in the form of point labels, that are used for patch classification. I assume the user can access the coordinates of the points relative to the original whole image... this should be made clear in the dataset description because many practitioners will use the point labels for propagation or weakly supervised approaches instead of patch classification. Including mention of the task of point label propagation could also strengthen the justification for a dataset of this type.”

**(1.i) Point coordinates & propagation/weak supervision**

Many users will indeed use point labels beyond patch classification. In ReefNet we provide the exact pixel coordinates of every annotation; this is already used internally for curation (duplicate removal by **point-level coordinates**) and for training **512×512** crops around points. We realize the paper did not **explicitly** state that these coordinates are released to users. We will make this explicit in §3 and the dataset card.

**What we added/clarified (paper + README):**

* We now state that each entry ships **(Column, Row)** in the original image coordinate frame.
* We explicitly mention supported tasks beyond patch classification: **point-label propagation**, sparse-to-dense pseudo-labeling, and weakly supervised segmentation.

We appreciate the suggestion, making this information more clear and explicitly mentioning propagation/weak supervision makes ReefNet more useful and reproducible for a wide range of workflows.

## 5.i  — Release of sampling-method metadata

**Reviewer’s concern**: “5.i) Will this metadata be made available to the user? It is important for tasks like point label propagation that the labelling scheme is known for each data subset.”

**Labelling Scheme**
Absolutely. The CSV file will includes few new columns like the `point_method` (simple-random/stratified-random/grid), `points_per_image`. This transparency lets users filter subsets or analyze bias in propagation experiments. We will reference these fields in §3.3 and show a usage snippet in the dataset card.
Unfortunately we can't update the dataset released on HuggingFace at it is against the policy of NeurIPS, but we are commited to update the dataset card to clarify this.


## 6.iii  — Expert-review protocol, agreement metrics, “patch” vs “point”

**Reviewer’s concern**: “6.iii) I find this point confusing - how many reviewers annotated each point, and what was the inter-observer agreement? Was the level of skill of the annotator considered in this assessment? Can you please explain this claim: "This process improved expert agreement from 73% to 92% by selecting source–class pairs that received high-confidence scores from verifiers." What percentage of patches was corrected to result in this increase in agreement? Why are you talking about labelling patches? The original point labels would have been labelled with the benefit of the whole image's context. It is a different task to label a patch vs labelling a sparse random point within an image.”

**Proposed reply**

1. how many reviewers annotated each point, and what was the inter-observer agreement?
    In our verification process, each reviewed sample (a point with its imagery) was assessed by an expert; the paper’s “expert agreement percentage” is the proportion of reviewed samples marked Correct (i.e., correctness of the original annotation), not a multi-rater κ statistic. We will make this definition explicit in §3.4 and add a short glossary (“agreement = correctness rate”). 
    For quality auditing we drew a stratified sample of 8,962 points (10 per genus per source). Each sample was reviewed by three independent experts chosen from our 10-member panel (1 coral taxonomist, 3 senior ecologists, 6 PhD-level specialists). 

2. Was the level of skill of the annotator considered in this assessment?

    the answer is yes, we considered the annotators' expertise and experience in coral taxonomy and ecology when assigning each set of labels to a verifier because in hard corals taxonomy people are specialized in different genera and families, and we wanted to ensure that the most qualified individuals were reviewing each sample. so we trust a judgement of a specific verifier because they have the relevant expertise and experience to make informed decisions about the labels.

3. explain this claim: "This process improved expert agreement from 73% to 92% by selecting source–class pairs that received high-confidence scores from verifiers." What percentage of patches was corrected to result in this increase in agreement?


    The increase reflects filtering, not mass relabeling. We first computed the overall correctness on 8,962 reviewed samples (73%). We then filtered out low-quality sources/labels (post-filter 78%), and finally retained only source–genus pairs with ≥70% correctness in the reviewed subset, producing a high-confidence split at 92% correctness on its verification set. The underlying mechanism and counts per step are reported in §3.4 and Table 2; we will emphasize that this is selection-based quality control. 

4. “What percentage of patches was corrected?”


    We did not globally “correct” point labels; rather, we excluded classes/sources/source–genus pairs that failed the expert-agreement thresholds. After source/label filtering, 860,463 annotations remained (78% agreement). After source–genus filtering, the high-confidence subset retained 479,027 annotations at 92% agreement. We will add a line in §3.4 explicitly stating that the jump in agreement is due to exclusion, not bulk relabeling.

5. Why are you talking about labelling patches? The original point labels would have been labelled with the benefit of the whole image's context. It is a different task to label a patch vs labelling a sparse random point within an image.


    The ground truth is always the original point label. “Patch” refers only to (i) the 512×512 crop centered at the point used for model input, and (ii) the verification UI’s view for efficiency. Crucially, reviewers saw a zoomable patch and the full image context while deciding Correct/Incorrect—so the review preserved whole-image context. We will clarify this in §3.4 and Supp. §S1.2.1. 

6. What we will add to fully resolve this point (camera-ready + dataset card):


    Terminology box in §3.4 defining “agreement = correctness rate” vs. “inter-observer agreement (κ)”. 

    Panel description (roles/experience bands) in Supp. §S1.2 and the dataset card. 

    Explicit statement that improvements come from selection thresholds, not mass relabeling, with the exact counts already reported (860,463 → 479,027).
    
    Clarification sentence that verification showed a patch plus the full image and that the released labels remain point labels.


## Access workflow & suggestion to use *pixi*

**Reviewer’s concern**: “The dataset access protocol still has a lot of steps for the user. Instead of a jupyter notebook, have you considered using tools e.g. pixi to encapsulate tasks and simplify the user experience?

**Proposed**
Thank you for the suggestion. We have packaged the downloader as

```bash
pip install reefnet-cli
reefnet download --sources all --split high_conf
```

and added a **pixi** environment so a single command—`pixi run download-reefnet`—recreates all splits on Linux, macOS, or Windows without Jupyter. The quick-start in the README now shows a 60-second end-to-end example.

**Note:** We cannot update the code or the dataset on HuggingFace at the moment because we don't want to violate the policy of NeurIPS as it states that it is strictly prohibited to update both of them.
