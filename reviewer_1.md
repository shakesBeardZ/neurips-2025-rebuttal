# neurips-2025-rebuttal

## Reply to the First Reviewer rrwA4 

We appreciate that the reviewer finds ReefNet to be a valuable resource in terms of both quantity and quality, the comprehensive benchmark design, and the broad evaluation of models. We are committed to publishing reproducible materials when we accept them.

We also thank the reviewer for the critical feedback. Below, we address each of the limitations and weaknesses raised:

### **Limited Technical Novelty**

We acknowledge that this work does not introduce new algorithms, and we agree that it is not the focus of our submission. This paper is submitted to the *Datasets and Benchmarks* track, which explicitly values high-quality resources and carefully designed evaluation protocols that facilitate future algorithmic progress.
ReefNet addresses long-standing challenges in coral classification—particularly domain shift, label inconsistency, and fine-grained recognition—by providing a large-scale, taxonomically standardized, and expert-verified benchmark. Our within-source and cross-source evaluation protocols simulate real-world deployment scenarios and reveal clear gaps in current methods, thereby motivating new research on domain-adaptive and fine-grained classification models.
Rather than proposing yet another method, our goal is to establish a robust foundation upon which future methods can be developed, tested, and compared. We believe this form of contribution is especially crucial in underrepresented domains like marine ecology, where high-quality, well-structured benchmarks are scarce but urgently needed

### **Strong Dependence on CoralNet**

Despite the strong dependence on CoralNet, ReefNet addresses key limitations that make it a valuable resource for scientific exploration and development. We believe CoralNet presents challenges that hinder its use as a training and evaluation dataset for algorithmic development in ecological studies. These limitations include:

1. **Lack of a unified label set across different data sources**: CoralNet includes a mix of incompatible label sets, making data integration infeasible.
2. **Absence of taxonomically verified hard coral labels**: Many sources use alternative, outdated, or generic labels instead of scientifically recognized names (e.g., those in WoRMS).
3. **No standardized quality control**: The absence of quality checks makes it difficult to identify reliable samples, which is essential for training and especially for evaluation.
4. **No large-scale benchmark setting**: There is no established framework for standardized evaluation or model comparison.

To address these challenges, we propose **ReefNet** with the following key contributions:

1. **WoRMS-based unified taxonomic labeling**:  
   ReefNet adopts a unified labeling scheme based on WoRMS to integrate data from multiple sources. This also enabled the inclusion of a Red Sea–focused dataset. We hope this labeling scheme encourages future efforts to adopt taxonomically sound annotations.

2. **Rigorous re-verification**:  
   Using our custom web tool, we re-verified 8,962 annotations and retained only those with high agreement scores. This rigorous process ensures benchmark quality and consistency.

3. **Standardized benchmarks**:  
   We developed reproducible benchmarks with defined splits. For instance:
   - The *within-source* split evaluates performance on data with similar distribution between train and test.
   - The *cross-source* split assesses robustness to possible domain shifts due to differences in imagery tools, weather conditions, depth, and other environmental or capture-related factors.
   - The *expert agreement* splits allow for controlled comparisons between training on larger datasets with potentially noisy labels versus smaller, cleaner datasets.
   The benchmarks also investigate the tradeoff between training data quantity and quality.

4. **Red Sea–focused benchmark**:  
A dedicated Red Sea split (Al-Wajh) enables testing model generalization on an ecologically important but underrepresented region.


### **Red Sea Benchmark Underdeveloped**

The Al-Wajh (Red Sea) benchmark extends our evaluation by introducing a focused, ecologically significant source from an understudied region. This inclusion was motivated by marine scientists studying Red Sea hard corals, who emphasized the lack of high-quality labeled data in this area. Although smaller in size, this benchmark plays a distinct role in testing how models trained on broader datasets generalize to previously unseen Red Sea imagery—a task marine scientists themselves consider challenging due to regional visual differences in hard corals. In *Table 3*, the final two columns (*Test-W*) evaluates the same models trained in *Train-S3* and *Train-S4* and tested on *Test-S3&S4* (columns three and four), but the metric scale is not directly comparable due to differences in test class composition between *Test-S3&S4* and *Test-W*. *Test-W* therefore serves as a focused probe into model performance on Red Sea–specific classes, highlighting generalization in a region of ecological importance.

### **Sparse Annotations**
Point annotations have been the standard in coral research—used in datasets like CoralNet and BenthicNet—due to the difficulty of obtaining dense labels, with annotators typically labeling randomly placed points. 
While datasets like CoralSCOP, CoralVOS, MosaicsUCSD, and Coralscapes offer segmentation masks, each lacks at least one of the following: genus-level classification of hard corals, global coverage and diversity, or sufficient scale.
We agree that dense masks are valuable, especially for fine-grained ecological tasks where morphological details matter. However, in this work, we prioritized global coverage, scalability, and annotation quality. We did explore dense labeling methods such as SAM and RITM (with interactive guidance), but found SAM alone unreliable—often missing key objects or over-segmenting—while point guidance improves quality but requires significant manual effort, which we defer to future work. We also note a strong synergy between obtaining masks and point annotations in data preparation: they can be treated as independent tasks, then sparse annotations can be projected onto masks. This is particularly useful since mask creation can be done by trained non-experts, whereas accurate coral identification requires expert annotators. As future work, we plan to provide masks for the Al-Wajh dataset to complement its high-quality point labels. Despite that, the rigorous benchmarks we provide using high-quality sparse annotations establish a solid foundation against which future algorithmic developments—or models using dense labels—can be compared and built upon.
