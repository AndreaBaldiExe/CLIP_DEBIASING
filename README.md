*CLIP DEBIASING*
The aim of this project is offering a valuable debiasing pipeline on Associasion Bias gender/profession related.

Latest Dataset and Output: **https://drive.google.com/drive/folders/1TblbHdSxLqcl1XzBISvNd3QhANB4AO4B?usp=sharing**

Issue to Address-> As stated in many paper, Clip shows several forms of Biases. The two main biases cosidered in this project are Representation and Association ones. Particularly, throught several
attempts of debias, I focused at the end on Associassion bias gender/profession. My goal was to reduce the mentiond bias without spoiling zero-shot capabilities or representation bias. 

Chosen Model: Clip with Vit/32

Dataset -> Throughout the whole process I used 5 different dataset: 
1. Fairface : useful for Representation Bias over gender and race
2. OxfordIIITPet : useful for zero-shot evaluation
3. COCO: One of the most generic dataset which provides some image context (not as FairFace) and good annotations
4. pexels_evaluation_dataset: this is a self-made scraped dataset, it has no annotation corpus but gender and professions values. Images from this dataset are scraped from Pexels platform.
5. challenge_dataset: this is my own hand-crafted dataset of 180 elements. it has the same structure of the one above, I personally did annotation and image selection. This differs from the pexels
   one for the higher difficulty of image compression.

Key Enhacemnts Attempts:
1. Prompt Banking: starting from some of the dataset above, which do not have corpus but just gender/professions structure, prompt banking creates neutral prompt which are very close to the 
distribution used to train Clip, this is essential to lower data perplexity
2. FineTuning on Clip: Once frozen the visual encoder (ViT 32), a lightweight LoRa was applied. Here's the main reasons for this choice:
   i. As stated in many articles, the most of the AS bias lies in Clip Textual encoder, the idea was of getting results by meitigating this bias. That means that, throught FT, the goal is to
   get two gender words such as "male" and "female" closer to the profession word considered
   ii. the shortage of dataset to FT the whole Clip was a limitating factor
   iii. due to computational resources it was not possible to FT the visual encoder
3. The Loss was redefined with a combination of two different losses:
   i. Bias Loss: it tries to minimize the gender/profession embedding distance mentioned
   ii. Anchor Loss: In orther to keep a good zero-shot capability, this loss avoids an excessive embedding drift

Key aspects:
1. The use of differnt dataset was to challenge bias and ViT capabilities.
2. Prompt Baking has played a crucial role during finetuning

Results: 
Performance Zero-Shot (Pets):
  - Baseline Top-1: 0.8765
  - Post-Finetune Top-1: 0.8689
  - Difference: -0.0076 

Association Bias (Pexels - Easy Context Images):
  - Baseline Bias: 0.0197
  - Post-Finetune Bias: 0.0173
  - Bias Reduction: 11.83%

Bias di Associazione (Challenge Set - Hard Context Images):
  - Baseline Bias: 0.0172
  - Post-Finetune Bias: 0.0147
  - Bias reduction: 14.18%

Generalizzation (Pexels Hold-out):
  - Bias Post (Standard): 0.0173
  - Bias Post (Hold-out): 0.0189
  - Success 

Representation Bias(FairFace):
  - Baseline Acc. Genere: 0.9383
  - Post-Finetune Acc. Genere: 0.9384
  - Difference: +0.0001
