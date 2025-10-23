# CLIP_DEBIASING
*Dataset Avalailable* at: https://drive.google.com/drive/folders/1TblbHdSxLqcl1XzBISvNd3QhANB4AO4B?usp=drive_link

*Context of interest*: Vision–Language Models (VLMs) such as CLIP have demonstrated impressive zero-shot recognition capabilities by jointly embedding images and natural-language prompts into a     shared semantic space. However, these models often inherit and amplify social biases present in their large-scale training data (e.g., correlations between gender, race, and professional roles). This project explores strategies to measure, analyze, and mitigate such biases while maintaining the strong zero-shot abilities that make VLMs so effective.

*Problem Statement*: Empirical studies show that standard CLIP models associate certain demographic attributes (gender, race) disproportionately with specific professions or contexts.
These spurious correlations can manifest as association bias (biased text–image similarity) and representation bias (embedding clusters aligned with demographic features).
The challenge addressed here is to reduce both forms of bias without compromising zero-shot generalization performance.

*Objectives*: 
1. Quantify existing social bias in pretrained CLIP models through both association and representation analyses.
2. Mitigate these biases via targeted fine-tuning (LoRA adapters) that preserve the model’s zero-shot alignment.
3. Evaluate whether the fine-tuned model maintains comparable visual–semantic performance while producing fairer embeddings.

So main measures here were Representation (on FairFace) and Association (on COCO) Bias reduction and zero-shot invariance. 

*Dataset*: Two complementary datasets were used to expose and correct bias across different visual contexts:
FairFace — a balanced human-face dataset labeled by gender and race, used to learn demographic directions and drive bias mitigation. COCO (Common Objects in Context) — a rich, scene-level dataset containing captions of people in diverse activities. Used to assess context-dependent bias and to support a mixed fine-tuning stage combining facial and contextual diversity.

*Analytical Methodology*: The analysis was structured in two phases:
  Vanilla Model Evaluation
	•	Computed gender- and race-specific similarity gaps across professional prompts.
	•	Applied Principal Component Analysis (PCA) to visualize demographic clustering in CLIP embeddings.
	•	Quantified both association bias (via similarity gaps) and representation bias (via embedding variance along demographic components).

  Fine-Tuned Model Evaluation
	•	Introduced LoRA adapters on top of CLIP’s image and text encoders for lightweight, bias-aware fine-tuning.
	•	Used a blended FairFace + COCO training strategy to balance demographic diversity with contextual information.
	•	Re-evaluated PCA projections and similarity gaps to measure bias reduction.
	•	Assessed zero-shot retrieval (image↔text) to confirm that semantic alignment and generalization were preserved.

*Improvements Introduced*
	•	Prompt Parity Bank: generation of balanced gender/race-conditioned prompts to reveal and control bias in text embeddings.
	•	Bias-Aware Loss Function: combined CLIP contrastive loss with an adaptive penalty on embedding projections along demographic directions (learned via PCA).
	•	Blended Fine-Tuning Regime: 70% FairFace + 30% COCO batches to couple demographic balance with real-world contextual richness.
	•	LoRA-Based Adaptation: efficient low-rank fine-tuning enabling fast training and minimal interference with pretrained zero-shot capabilities.
	•	Post-Training Analyses: comparison of similarity gaps, PCA variance ratios, and zero-shot retrieval metrics between the vanilla and fine-tuned models to verify both fairness gains and performance stability.
