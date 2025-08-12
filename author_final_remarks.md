# Author Final Remarks

## All the reviewes

**opening statement:**

We would like to thank the reviewers again for the discussions and detailed feedback that actually helped us in refining the paper and clarifying things that needed to be clarified at which we plan to reflect in the paper as well.

rwA4 (+)
* Valuable and well-curated resource for coral reef monitoring and marine AI research.
* Appropriate and relevant for NeurIPS Datasets & Benchmarks.
* Significant utility for ecological studies and advancing ecological data science.

rwA4 (−)
* Limited technical novelty — focus on dataset and benchmark rather than algorithmic innovation.
	* Addressed by clarifying that the paper’s contribution lies in providing a large-scale, taxonomically standardized, expert-verified dataset and broad benchmarking with multiple algorithms.
* Unclear distinction from CoralNet.
	* Clarified ReefNet’s unique role, including its harmonization, expert verification, benchmark splits, and why these necessitate a separate, centralized dataset.
* Limited domain adaptation/augmentation experiments.
	* Provided additional results in rebuttal showing effects of augmentation combinations on domain shift, to be integrated into the paper.

ue6z (+)
* Dataset relevant for ecological studies and coral classification benchmarking.
* Appreciated scale, diversity, and expert annotation verification.
* Liked inclusion of both in-distribution and cross-source splits.

ue6z (−)
* Access process complex.
	* Addressed by committing to a pip-installable CLI and HuggingFace hosting.
* Requested release of point coordinates and sampling metadata.
	* Confirmed inclusion with method details and per-image point statistics.
* Wanted clarity on expert review assignments and confidence tier derivation.
	* Clarified re-verification process by domain experts and documented confidence tier criteria.

3TBH (+)
* Recognized potential for fine-grained classification in marine ecology.
* Valued unification and standardization of hard coral datasets.

3TBH (−)
* Lack of dense annotations beyond point labels.
	* Clarified dense labeling is beyond current scope but point coordinates enable future extension.
* Questioned distinction from CoralNet.
	* Addressed by detailing unique contributions: taxonomic harmonization (WoRMS), expert re-verification, confidence tiers, new benchmark splits.
* Requested more augmentation/domain adaptation detail.
	* Provided additional results in rebuttal, to be included in paper.

2iCx (+)
* Benchmark design effective for assessing domain shift; data carefully curated with expert checks.
* Valued textual description data.
* Liked evaluation of state-of-the-art and foundation models.

2iCx (−)
* Unclear task description.
	* Clarified ReefNet as single-point patch classification, explained lack of multi-label examples, and outlined future possibilities.
* Absence of domain adaptation experiments.
	* Same as rwA4 — addressed with additional augmentation/domain shift results in rebuttal and commitment to include in paper.
* Suggested open-set recognition experiments on novel classes.
	* Provided such experiments in rebuttal, to be integrated into paper, showing practical value for deployment scenarios.

**closing statement:**

reviewers also suggested other editorial fixes, that we intent to fix in the paper and we discussed and adressed in the rebuttal


----

## Crafted Author Final Remarks

We thank all reviewers for the constructive feedback, which has helped refine the paper. rwA4 valued ReefNet as a well-curated, large-scale resource for coral reef monitoring, suitable for NeurIPS Datasets & Benchmarks, and important for ecological studies. ue6z found it relevant for ecological benchmarking, appreciating its scale, diversity, expert verification, and inclusion of both in-distribution and cross-source splits. 3TBH recognized its potential for fine-grained marine classification and for unifying fragmented datasets. 2iCx praised the benchmark design for assessing domain shift, careful expert curation, inclusion of textual descriptions, and evaluation of state-of-the-art and foundation models.

Concerns about limited technical novelty (rwA4) were addressed by clarifying our main contribution: the largest taxonomically standardized, expert-verified coral dataset (mapped to WoRMS) and broad benchmarking. Questions from rwA4 and 3TBH on distinction from CoralNet were resolved by detailing ReefNet’s taxonomic standardization, expert verification, benchmark splits, and global coverage. Requests from rwA4, 3TBH, and 2iCx for more domain adaptation/augmentation experiments led to new results in rebuttal showing augmentation effects on domain shift, to be integrated into the paper. ue6z’s access concerns will be addressed with a pip-installable CLI and HuggingFace hosting; their request for point coordinates, metadata, and per-image statistics is confirmed, with sampling methods described. 3TBH’s point on dense annotations was answered by noting this is beyond the current scope, but released coordinates enable future work. 2iCx’s task description concern was resolved by clarifying ReefNet as a single-point patch classification, explaining the absence of multi-label examples, and outlining possible future formats. Following 2iCx’s suggestion, we added open-set recognition experiments on novel classes, presented in rebuttal and to be integrated. Additionally, all the suggested editorial fixes will be incorporated into the camera-ready version.

ReefNet fills a long-standing gap in coral reef monitoring: unlike birds, insects, or fish—where research starts from well-curated datasets—hard-coral data has been fragmented, inconsistently labeled, and inaccessible. It is intended as a starting point of an “ImageNet for hard corals,” enabling AI and marine science communities to progress without months of data collection, cleaning, and standardization.

----

## Version 1

We thank the reviewers for their constructive feedback and acknowledge the productive discussion that strengthened this submission. All major concerns have been addressed:
	•	Dataset structure and task definition: clarified as multi-label, patch-based classification, with extraction methodology and patch sizes detailed.
	•	Red Sea benchmark: its purpose and distinction from the global benchmark were clarified, highlighting its value in testing models on an underrepresented region.
	•	Licensing: we will explicitly state all licensing terms for images, data, and code in the camera-ready, following best practices for mixed-provenance datasets.
	•	Technical novelty: as clarified in rebuttal, our focus aligns with the Datasets & Benchmarks track’s mission — delivering high-quality resources and robust evaluation protocols, not new algorithms.

ReefNet fills a long-standing gap in coral reef monitoring. In fields such as birds or insects, research begins from well-curated datasets; in corals, data has been fragmented, inconsistently labeled, and inaccessible. ReefNet aggregates and standardizes the largest-ever hard-coral dataset (∼925K genus-level hard-coral annotations), mapped to the World Register of Marine Species, and pairs it with cross-source and within-source benchmarks designed to reflect real-world deployment challenges.

From our own experience as AI researchers in biodiversity, the absence of such a “starting point” dataset has been a major bottleneck. ReefNet is intended to be that starting point — an “ImageNet for corals” — enabling both AI and marine science communities to accelerate progress without months of data collection and cleaning.

We believe the final submission is robust, impactful, and fully aligned with the goals of this track, and we hope the AC will consider its strong potential to catalyze research in an area of urgent global importance.



## Version 2

We thank the reviewers for their constructive input and summarize below their key points and our responses for the AC’s consideration.

rwA4 valued the expert verification process, clear within-/cross-source benchmarks, and metrics. They questioned differentiation from CoralNet and data accessibility. We explained that ReefNet unifies multiple sources under WoRMS taxonomy, applies multi-stage expert re-verification (~9k patches, 92% correctness), defines confidence tiers, and introduces reproducible splits including the Red Sea subset. We will provide a pip-installable CLI and HuggingFace access to simplify use.

ue6z highlighted the benchmarks’ relevance and annotation rigor. They requested point coordinates, sampling metadata, and clarity on expert assignments. We confirmed these will be released, explained the review protocol, and described confidence tier derivation. Access complexity was addressed via the same CLI plan.

2iCx praised dataset scope and evaluation design, but found the task definition unclear. We clarified that ReefNet is single-label at patch level (multi-label across points) and will make this explicit in the abstract and §3. They requested clearer class counts in Table 1 and more augmentation/domain-shift results; both will be included.

3TBH noted the dataset’s potential for fine-grained classification, but raised points on lack of dense labels, CoralNet overlap, and augmentation details. We clarified dense labels are beyond current scope, detailed the unique steps beyond CoralNet, and committed to surfacing augmentation/domain-adaptation results in the main text.

In closing, ReefNet fills a long-standing gap in coral reef monitoring. In fields such as birds, insects or fish, research begins from well-curated datasets; in corals, data has been fragmented, inconsistently labeled, and inaccessible. ReefNet aggregates and standardizes the largest-ever hard-coral dataset (∼925K genus-level hard-coral annotations), mapped to the World Register of Marine Species, and pairs it with cross-source and within-source benchmarks designed to reflect real-world deployment challenges.

From our own experience as AI researchers in biodiversity, the absence of such a “starting point” dataset has been a major bottleneck. ReefNet is intended to be that starting point — an “ImageNet for corals” — enabling both AI and marine science communities to accelerate progress without months of data collection and cleaning.

⸻

**Notes**

As researcher In AI whose has tried to find solutions for coral monitoring, we couldn't find a one place to go to find the proper data ready to be used to train AI models to solve this field problems, this dataset that we provided came from a real experience where we tried to find solutions for known problems in other biological fields like Birds, Insects ...etc. Still, we struggled to even start because of the lack of such data; we hope that the final consideration of the paper takes into consideration that this dataset comes from the process of trying to find a pain.
I'm writing a report about this idea, and I need to elaborate on it 

we wanted reefnet to be the starting point of something like ImageNet for hard corals, a one place that an AI or Coral researcher can go to and start digging from there, rather than spending days and days into downlaoding data from differenct source across the web, and checking their quality and running an extensive EDA, for fields like birds and insects there is starting points and for fish too but for corals there is always a probem due to many reasons like the domain specifity as corals are not like any other living objects 
