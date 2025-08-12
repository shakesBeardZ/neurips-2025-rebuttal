# Author Final Remarks

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
