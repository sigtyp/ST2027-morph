## SIGTYP Shared Task on Morph Segmentation and Root Identification Across Typologically Diverse Languages

## Table of Contents

1. [Introduction](#user-content-1-introduction)
2. [Important Dates](#user-content-2-important-dates)
3. [Task Description](#user-content-3-task-description)
4. [Data](#user-content-4-data)
5. [Evaluation Metrics](#user-content-5-evaluation-metrics)
6. [Baselines](#user-content-6-baselines)
7. [Registration](#user-content-7-registration)
8. [CodaBench Submission Instructions](#user-content-8-codabench-submission-instructions)
9. [Submission of System Description Papers](#user-content-9-submission-of-system-description-papers)
10. [Ethical and Other Remarks](#user-content-10-ethical-and-other-remarks)
11. [Task Organisers](#user-content-11-task-organisers)
12. [Acknowledgments](#user-content-12-acknowledgments)
13. [Quick Links](#user-content-13-quick-links)

## 1. Introduction

In 2027, <font color="red">S</font>IG<font color="red">T</font>YP is hosting a **Shared Task on Morph Segmentation and Root Identification Across Typologically Diverse Languages.** 
Morphological segmentation, the decomposition of word forms into meaningful units, is an important task in computational linguistics, with applications in lexicography, language documentation, and language modeling. However, existing resources and benchmarks cover relatively few languages, use incompatible annotation conventions, and often treat word forms in isolation.

The current CodaBench-powered shared task draws on earlier efforts, such as Morpho Challenge 2005–2010 and SIGMORPHON 2022, but introduces several crucial extensions and improvements:

1. **Diversity of included languages.** The task covers more than 30 typologically, genealogically, and geographically diverse languages, with both large and small speaker populations, exposed to varying degrees of language contact.
2. **Harmonised evaluation data.** Newly annotated development and test data follow a common sampling method and unified annotation guidelines, enabling meaningful comparisons across languages.
3. **Heterogeneous training data.** Training data come from already existing resources and vary considerably in size, quality, lexical material selection, and annotation schema, reflecting the present-day reality of lexicographic resources.
4. **Linking to Universal Dependencies (UD).** Word forms to be segmented are provided with sentence contexts drawn from UD treebanks as well as with morphological categories following the Universal Features schema.
5. **Written as well as spoken form of language.** For most languages, the shared task uses written word forms. For a few languages, however, phonetic transcriptions are used, because the existing writing system is not well suited to morphological segmentation.
6. **Variety of writing systems.** The shared task covers all major types of writing systems, including alphabetic, abjad, abugida, syllabic, and logographic systems.
7. **Handling non-concatenative morphology.** The shared task includes languages with highly non-concatenative morphology.
8. **Support for multilingual solutions.**  The data used in the shared task are provided in the same simple file format across all languages, which should facilitate the development of cross-lingual transfer-based approaches as well as the application of zero-shot and few-shot LLM methods.


## 2. Important Dates

The shared task timeline is organized as follows:

* **Registration opens:** November 21, 2026
* **Development phase begins:** January 15, 2027
  + Training and development data released
  + Baselines released
  + Evaluation script released
  + CodaBench development phase opens
* **Evaluation phase begins:** February 15, 2027
  + (Unannotated) evaluation data released
  + CodaBench evaluation phase opens
* **Evaluation phase ends:** March 1, 2027
  + CodaBench evaluation phase closes
* **System description paper submission deadline:** March 15, 2027
* **Notification of paper acceptance:** March 22, 2027
* **Camera-ready:** March 29, 2027
* **SIGTYP Workshop:** May 13, 2027



## 3. Task Description

For each language, the participants will be given a list of word forms to be segmented.
Then, given a word form in its sentential context, participating systems are expected to 

1. **segment the word form into surface morphs,** i.e., the overt realizations of minimal meaningful units, and 
2. **identify the root morph(s)** among the resulting segments.

A root is understood as the morph carrying the primary lexical meaning of a word. While most word forms contain one or more roots, certain mono-morphemic function words, particularly primary prepositions and conjunctions, are considered rootless. By contrast, pronouns and articles are assigned a root and may also be morphologically complex. Whenever a word form contains multiple roots, all of them must be identified.

The task follows two central principles. First, it adopts a surface-based approach: the identified morphs must concatenate to reconstruct the original word form, rather than being represented through abstract or canonical morphological forms. Second, it requires maximum (exhaustive) segmentation: systems are expected to identify all constituent morphs, rather than merely separating a stem from an inflectional ending or identifying only the most recently added derivational affix. The aim is thus to capture the complete morphological structure of each word form as it is realized on the surface.

Each word form is accompanied by its sentence context and, where available, morphological features encoded according to the Universal Dependencies (UD) feature schema. The contextual information is intended to help systems resolve ambiguities that cannot be reliably addressed when word forms are considered in isolation, including homonymous forms that may have different morphological analyses.

The expected output consists of a segmentation of each word form and the identification of its root morph(s). The annotation format uses the plus sign (+) to indicate boundaries between morphs and the hash sign (#) to mark the beginning of a root morph. A special boundary marker ($) is used when a morphological boundary falls within a single grapheme, as may occur in syllabic and abugida writing systems.

All word forms are represented in lowercase in the annotation, including proper nouns, abbreviations, and other forms conventionally written with an initial capital letter. No additional white spaces are inserted around morph boundaries. However, word forms in some languages may themselves contain white spaces; in such cases, the plus sign marking a morph boundary is placed immediately after the existing white space.

The following examples illustrate the expected annotation:


<pre>
Word form          Annotation                 Language 
------------------------------------------------------
talked             #talk+ed                   English 
including          in+#clud+ing               English 
to                 to                         English 
Jahresabschlüsse   #jahr+es+ab+#schlüss+e     German 
твоей              #т+вое$й                   Russian
देखो                #देखो$                       Hindi 
行け               #行け$                     Japanese 
đàn ông           #đàn +#ông                  Vietnamese
</pre>

For languages with non-concatenative morphology, the annotation is based on IPA transcriptions. Discontinuous parts of a morpheme are enclosed in brackets to indicate that they belong together. Different bracket types are assigned in order of their first occurrence in the word: () for the first discontinuous morph, [] for the second, {} for the third, and <> for the fourth; see the following example
 with the transcription of the Arabic expression 
<span dir="rtl">لجَنَاح</span>

<pre>
IPA transcription      Annotation                 Language
----------------------------------------------------------
ʔal(#dʒ)anaː(ħ)i       ʔal+(#dʒ)[a](n)[aː](ħ)+i   Arabic
</pre>

Further details on morph segmentation and root identification are provided in the annotation guidelines used to prepare the development and evaluation data: [Guidelines for manual annotation of morph segmentation and root identification](guidelines/morph-segmentation-guidelines.pdf)


## 4. Data

For each language included in the shared task, we provide three datasets: training, development, and evaluation.

The development and evaluation datasets were created by selecting a sample of 2,000 word forms for each language using a uniform stratified sampling procedure designed to cover both frequent and rare words in the corpus, with the intermediate frequency range divided into bins using exponentially increasing intervals. Each word form was supplemented with information from the UD treebanks, including its lemma, Universal Part-of-Speech (UPOS) tag, Universal Features, and an example of its use in a sentence.

For each language, all 2,000 word forms were then manually annotated by a language expert, following the annotation guidelines mentioned above and paying particular attention to annotation consistency. The annotated items were subsequently randomly divided into two equally sized subsets: 1,000 word forms for the development phase, in which participants will have access to the manual annotations, and 1,000 word forms for the evaluation phase, in which the manual annotations will be kept hidden and used internally by CodaBench solely to evaluate the participants' performance.

The situation is quite different when it comes to the training data. For training purposes, we use diverse data from existing morphological segmentation resources, including both traditional lexicographic works originally published in print as well as morphological datasets developed for NLP. These resources differ in several respects, such as 

* the number of segmented entries,
* the criteria used to select entries (e.g., whether they include only citation forms or also other inflected forms, and whether they cover all or only selected parts of speech),
* the completeness and granularity of the annotations (in some cases, only the inflectional ending is separated from the rest of the word, while in others, the segmentation is more fine-grained),
* whether the segmentation is surface-based or so-called canonical segmentation,
* whether the segments are assigned any morphological or other labels,
* whether the original writing system is used, or a romanized transliteration, or a transcription,
* and, last but not least, the linguistic quality of the segmentation.

For the purposes of this shared task, data from all these sources have been technically converted into a common file format, drawing in particular on our previous expertise gained through the UniSegments project. However, we have not been able to eliminate all the differences in approach among the individual primary sources, and, more importantly, the discrepancies between their segmentation conventions and our own approach to morphological segmentation petrified in the development and evaluation data. Hence, participants should keep this heterogeneity in mind, particularly if they develop a trainable segmentation system, as e.g. the optimal training method and/or optimal hyperparameter settings may substantially vary for different types of training datasets.

More technical information, such as file format desctription, will appear here soon.

References:

* Zdeněk Žabokrtský et al. 2022. Towards Universal Segmentations: UniSegments 1.0. In Proceedings of the Thirteenth Language Resources and Evaluation Conference. https://aclanthology.org/2022.lrec-1.122/
* Vojtěch John et al. 2026. Universal Segmentations 1.5 (UniSegments 1.5). LINDAT/CLARIAH-CZ digital library at the Institute of Formal and Applied Linguistics, Faculty of Mathematics and Physics, Charles University. http://hdl.handle.net/11234/1-6130


## 5. Evaluation Metrics

Each system participating in the shared task will be evaluated using a primary metric and several secondary metrics.

The primary evaluation metric upon which the CodaBench leaderboard will be based will be F1-score of joint segmentation and classification of morphs. For a morph to be considered a true positive, its span within the segmented word must be retrieved correctly, and it must be classified correctly as a root or non-root. If a morph boundary is inside a grapheme, the $ delimiter must be used directly after the grapheme in question. If the morph is discontinuous and uses brackets to mark its separated parts, these must be correctly applied.

Secondary evaluations will also be available, probably including morph-level F1 without root vs. non-root classification, boundary-level F1 (based on the correct placement of token boundaries), edit-distance of the recovered morph sequence and the ground truth, and character-level cluster-based metrics such as Dice coefficient.

A Python script will be provided for the evaluation (to appear here by the beginning of the development phase).


## 6. Baselines

Before the beginning of the development phrase, we will release two baseline systems and their results. The first baseline will consist of a fine-tuned encoder-decoder sequence-to-sequence model as in Ginn et al. (2026). The second baseline will consist of zero/few-shot prompting using a decoder-only large language model as in John and Žabokrtský (2026). Both baselines will be implemented monolingually (one instance per language). A more detailed description will be released alongside the final models.

References:

* Michael Ginn et al. 2026. Massively multi-lingual joint segmentation and glossing. In Proceedings of the 64th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers). https://aclanthology.org/2026.acl-long.1657/
* Vojtěch John and Zdeněk Žabokrtský. 2026. LLMs as linguistic experts: The case of morphological segmentation. In Text, Speech, and Dialogue. Springer. https://link.springer.com/chapter/10.1007/978-3-032-37249-9_8


## 7. Registration


If you are potentially interested in participating in this shared task, please fill out the [**registration form**](https://docs.google.com/forms/d/e/1FAIpQLSdHc1G7ttdoITBjntyM13b2I7ASr9BRnavjIBvcLmdErFYSRw/viewform?usp=dialog) as soon as possible. We strongly recommend that at least one person from your participating team registers so that you don't miss any updates regarding the shared task.

For any questions about the shared task, please contact [Zdeněk Žabokrtský](mailto:zabokrtsky@ufal.mff.cuni.cz).


## 8. CodaBench Submission Instructions

The shared task competition will be conducted using the CodaBench platform and will consist of two phases.

During the development phase, participants will be able to submit their predictions for the development word lists. Since the corresponding manual annotations will also be publicly available, participants will be able to evaluate their systems locally. Nevertheless, we encourage them to familiarize themselves with the CodaBench environment already during this phase.

The actual competition will take place during the evaluation phase. Participants will submit their predicted segmentations for the evaluation word lists to CodaBench, which will automatically compute their scores by comparing the predictions against the (unreleased) manual annotations and continuously publish the results on a leaderboard.

The final ranking of participants will be determined by their performance in terms of the primary evaluation metric at the time the evaluation phase closes.

More technical instructions will be added here soon.


## 9. Submission of System Description Papers

All shared task participants are invited to submit system description papers to the SIGTYP 2027 Workshop. Please note that the deadline for the system description paper is later than the deadline for regular workshop papers (see Important Dates above). 

Paper formatting instructions will be the same as for the [workshop](https://sigtyp.github.io/workshop.html). Each team participating in the shared task is expected to submit a system description paper of 4 to 5 pages, plus additional pages for references, formatted according to the workshop guidelines. The paper should describe the system and the resources used along with the libraries. The methodology/strategy should be documented in such a way that the readers and other researchers are able to replicate the work from the system description in the paper. Identity of the authors of the participating systems will be known to organisers in some cases, and thus it is not required to make the submitted papers anonymous as the reviewing process can be guaranteed to be only single-blind anyway.

If accepted, the system description papers will be published in the SIGTYP workshop proceedings.  The task organisers will write an overview paper describing the task, summarising the approaches taken, and analyse their results.


## 10. Ethical and Other Remarks

Researchers who were involved in the manual annotation of the evaluation data may also participate in the shared task, either as individual participants or as members of competing teams. However, they are expected not to submit predictions for the language they contributed to annotating, as this could give them an unfair advantage.

If you notice any issues in training datasets, please bear in mind that the datasets were produced from already existing data sources using automatic converters, and 
the observed issues may have been introduced during the conversion process, possibly due to our implementation errors or an insufficient understanding of the source formats. Therefore, they may not reflect the quality of the original data sources. If anyone is interested in exploring this further, please contact [Zdeněk Žabokrtský](mailto:zabokrtsky@ufal.mff.cuni.cz). We will be happy to share our data conversion pipelines and use your feedback for improving them.

The training and development data used in this shared task will remain publicly available after the shared task has concluded. We kindly ask future users to respect the accompanying licenses and cite the reference that will be made available here after the shared task.  If you use only a subset of our data package, please also consider citing the individual primary data sources.


## 11. Task Organisers
* **Magda Ševčíková**, Charles University
* **Zdeněk Žabokrtský**, Charles University
* **Priya Rani**, Charles University
* **Andrew Dyer**, Charles University
* **Hana Hledíková**, Charles University
* **Vojtěch John**, Charles University
* **Anja Nedoluzhko**, Charles University
* **Aleš Papáček**, Charles University
* **Enora Rice**, University of Colorado

Please contact [Zdeněk Žabokrtský](mailto:zabokrtsky@ufal.mff.cuni.cz) if you have any questions regarding the shared task.


## 12. Acknowledgments

This shared task is supported by the [CrossLing project](https://ufal.mff.cuni.cz/grants/crossling), Cross-Linguistic Analysis of the Internal Structure of Words, LL2615, funded by the Ministry of Education, Youth and Sports of the Czech Republic.

We would also like to thank all the language experts who contributed to the annotation of the new evaluation data, as well as the authors of previously published segmentation datasets, without which we would not have had sufficient training data.

<!-- TODO: thank also the crossling as well as authors of datasets integrated in UniSegments -->

## 13. Quick Links

A summary of links from the above sections:

* [The SIGTYP 2027 workshop page](https://sigtyp.github.io/workshop.html)
* [This shared task's intro page](https://sigtyp.github.io/st2027-lingseg.html)
* [Registration form for participants](https://docs.google.com/forms/d/e/1FAIpQLSdHc1G7ttdoITBjntyM13b2I7ASr9BRnavjIBvcLmdErFYSRw/viewform?usp=dialog)
* [Guidelines for manual annotation of morph segmentation and root identification](guidelines/morph-segmentation-guidelines.pdf)
* Training data package - a link to appear here
* Development data package -- a link to appear here
* Evaluation data package -- a link to appear here
* Evaluation script  -- a link to appear here
* Baseline systems -- a link to appear here
* CodaBench competition page -- a link to appear here
* OpenReview page for system description papers -- a link to appear here





