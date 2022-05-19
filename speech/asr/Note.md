### 1. End-to-End Spelling Correction Conditioned on Acoustic Feature for Code-switching Speech Recognition
[Interspeech2021] [Link](https://www.isca-speech.org/archive/pdfs/interspeech_2021/zhang21d_interspeech.pdf)  
19/05/2022  
**_Note_**:  
Code switching (CS) definition: " In code-switching, multiple languages are freely interchanged within a single sentence or between sentences."  
**_Dataset_**: ASRU 2019 Mandarin-English Code-Switching Speech Recognition Challenge at [link](https://arxiv.org/pdf/2007.05916.pdf).  
Example samples:

| CS transcription | EN transcription |
| ---------------- | ---------------- |
| “我今天要去买一个 iPhone.” | asdfsdf |

_**Purpose**_: Vietnamese ASR usually faces this problem. Transcription comes from conversational speech usually include English word. Solving CS can enhance the accuracy for Vietnamese ASR.  
_**Content**_: Create a E2E spelling correction module, which is the post processing module for output of ASR. Use acoustic features append with LM features to compose the input for spelling correction model.

### 2. Improving Streaming Transformer Based ASR Under a Framework of Self-supervised Learning
[Interspeech2021] [Link](https://www.isca-speech.org/archive/pdfs/interspeech_2021/cao21b_interspeech.pdf)  
19/05/2022  
Purpose:  
SSL ASR architectures usually large and include transformer. Multi head attention (MHA) has to look at all the input before computing, therefore this architecture is hard to use as streaming.  
W2v2 for ASR has very good quality. How to turn it into streaming architecture for better speed of inferring? Will the performance be kept or only small reduction of accuracy?  
Solution:  
<img src="../../img/cao21b_interspeech_fig1.png" width="400"  />  
Left context is not limited. Right context includes 1 frame. The upper layers, the more right context it can capture because it will not affect much to the time latency. Can use `attention mask` to control the context that attention will capture.  
=> Can apply attention mask to non streaming model P and finetune it on laballed data L to get streaming model S.  


