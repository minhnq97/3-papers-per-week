### 1. End-to-End Spelling Correction Conditioned on Acoustic Feature for Code-switching Speech Recognition
[Interspeech2021] [Link](https://www.isca-speech.org/archive/pdfs/interspeech_2021/zhang21d_interspeech.pdf)  
19-May-2022  
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
19-May-2022  
**_Purpose_**:  
SSL ASR architectures usually large and include transformer. Multi head attention (MHA) has to look at all the input before computing, therefore this architecture is hard to use as streaming.  
W2v2 for ASR has very good quality. How to turn it into streaming architecture for better speed of inferring? Will the performance be kept or only small reduction of accuracy?  
**_Solution_**:  
<img src="../../img/cao21b_interspeech_fig1.png" width="400"  />  
Left context is not limited. Right context includes 1 frame. The upper layers, the more right context it can capture because it will not affect much to the time latency. Can use `attention mask` to control the context that attention will capture.  
=> Can apply attention mask to non streaming model P and finetune it on laballed data L to get streaming model S.  

### 3. Robust wav2vec 2.0: Analyzing Domain Shift in Self-Supervised Pre-Training
[Interspeech2021] [Link](https://www.isca-speech.org/archive/pdfs/interspeech_2021/hsu21_interspeech.pdf)
23-May-2022  
_**Purpose**_:  
What if domain of unlabelled data trained for W2v2 differ from labelled data for finetuning ASR.  
=> Experiments show that using target domain data during pre-training leads to large performance improvements across a variety of setups.  
=> Pre-training on multiple domains improves generalization performance on domains not seen during training.  
**_Result_**:  
1. Does adding in-domain pre-training data help?  yes
2. Does adding pre-training data help if out-of-domain? Mix, does not always help
3. Does pre-training on diverse data improve robustness? yes
4. Is it still effective and robust with more labeled data? Yes, even if the labeled data is out of domain
5. Effect of pre-training data similarity to target domain? "If there is perfect domain match for the unlabeled data, labeled data and the target domain, then more
unlabeled data leads consistently to better performance" -> "unlabeled data leads consistently to better performance"
6. Effect of in-domain pre-training data size? Will improve performance both by *jointly training* and *continual training* two domains corpus.
7. Larger model, more pre-training and fine-tuning data: In general, bigger size brings better performance

=> This paper has good analysis about strategy to train/fine-tune ASR with in/out domain

### 4. Wav2vec-C: A Self-supervised Model for Speech Representation Learning
[Interspeech2021] [Link](https://www.isca-speech.org/archive/pdfs/interspeech_2021/sadhu21_interspeech.pdf)  
23-May-2022  
_**Purpose**_: 
- Introduce wav2vec-C to enhance the quality of learning code book and focus on modifying learnt representation layer.
- Limit model size to facilitate low-latency production level ASR models.
- Compare different vector quantization frameworks.  

_**Content**_:  
<img src="../../img/sadhu21_interspeech_fig1.png" width="400"  />  
- Encoder network _f()_ consists of 3 layers LSTM
- Vector quantization module _q_ is the same as original w2v2
- Additional masking by SpecAug
- Context network _g()_: 5 transformers layers
- Consistency network _r()_ will reconstruct the quantized encoding Z_hat: 3 layers LSTM that maps the Z^ to S one by one. Target loss is to minimize L2 normed distance between S and X.  

_**Result**_:
This method seems work better with Gumbel Softmax than K-Mean.

=> Does not see clearly the improvement on WER of any testset. Does not mention to the low latency model by benchmarking speed.

### 5. Super-Human Performance in Online Low-latency Recognition of Conversational Speech  
[Interspeech2021] [Link](https://www.isca-speech.org/archive/pdfs/interspeech_2021/nguyen21c_interspeech.pdf)  
24-May-2022  
**_Purpose_**:  
Creat an ASR model which address both accuracy and latency problem. WER around 5% on SWB (Switchboard) conversational benchmark, 1s behind speaker's speech.  
Propose a novel low latency incremental inference approach.  

**_Content_**:  
<img src="../../img/nguyen21c_interspeech_fig1.png" width="400"  />  
Stability detection is the key to make the system work in the incremental manner and to produce low latency output. 
Taking advantage of ensembling inference models. The hypothesis will go into stability detection. This module check if the token predicted is stable enough and set it as a result. Then, the other tokens in hypothesises go back into Ensemble Inference with more input. 

### 6. Online Compressive Transformer for End-to-End Speech Recognition
[Interspeech2021] [Link](https://www.isca-speech.org/archive/pdfs/interspeech_2021/leong21_interspeech.pdf)  
26-May-2022  
**_Purpose_**:  
An online compressive transformer (OCT) aims to generate immediate transcription for each audio chunk while the comparable performance with offline ASR can still be achieved.  

**_Note_**:  
- [RNN-T](https://arxiv.org/pdf/1211.3711.pdf): Basically, there will be two networks: the encoder and the prediction network.  
<img src="../../img/leong21_interspeech_note1.png" width="250"  />  
- [Compressive Transformer](https://arxiv.org/pdf/1911.05507.pdf): an attentive sequence model which
compresses past memories for long-range sequence learning.  
- [Implementation](https://github.com/lucidrains/compressive-transformer-pytorch)  

**_Content_**:  
<img src="../../img/leong21_interspeech_fig1.png" width="400"  />  
OCT has a performance relatively as equal as Transformer vanilla.  
<img src="../../img/leong21_interspeech_fig2.png" width="400"  />  
The result seems better than [Sync-transformer](https://arxiv.org/abs/1912.02958) at beam search = 5.

### 7. Transformer-based ASR Incorporating Time-reduction Layer and Fine-tuning with Self-Knowledge Distillation
[Interspeech2021][Link](https://www.isca-speech.org/archive/pdfs/interspeech_2021/haidar21_interspeech.pdf)  
02-June-2022  
**_Purpose_**:  
- Propose a Transformer-based ASR model with the time-reduction layer: incorporate time-reduction layer inside Encoders in addition to traditional sub-sampling methods to reduce input features.  
- Can both improve the performance and reduce time training/inferring.
- Achieved SOTA result on Librispeech (w/o external data) with only 30M params model.  

**_Method_**:  
- Time reduction (TR) layer can be set after the first Encoder before go to second Encoder.
-  Basically, TR will concatenate two adjacent time frames

### 8. Toward a realistic model of speech processing in the brain with self-supervised learning  
[Preprint][Link](https://arxiv.org/pdf/2206.01685.pdf)  
09-June-2022  
**_Purpose_**:  
- Prove that Wav2vec2.0 learns brain-like representation
- Its functional hierarchy aligns with the cortical hierarchy of speech processing
- Wav2Vec 2.0 learns sound-generic, speech-specific and language-specific representations similar
to those of the prefrontal and temporal cortices.  
- Confirm the similarity of this specialization with the behavior of 386 additional participants.  
=> In summary, this research demonstrates that how similar the W2V2 to the human brain.  

_**Content**_:  
<img src="../../img/2206.01685_fig1.png" width="500" />  
They assess the similarity between the activations of the model X and brain activity Y with a standard encoding model W. Then compare W(X) with Y by Pearson correlation R.  
_**C**_. illustrates the comparison between X and Y.  

### 9. A Comparative Study on Neural Architectures and Training Methods for Japanese Speech Recognition  
[ArXiv][Link](https://arxiv.org/pdf/2106.05111.pdf)  
**_Purpose_**:  
- Investigate some E2E SOTA methods on training ASR for Japanese
- The best result belongs to Conformer transducer architecture.  
Experiment:  
- Use CSJ corpuse (581 hours), separated data (train/dev) by protocol of ESPnet

### 10. Tiny Transformers for Environmental Sound Classification at the Edge  
[ArXiv][Link](https://arxiv.org/pdf/2103.12157.pdf)  
**_Purpose_**:  
- Environmental Sound Classificatioln (ESC) on the edge
- What type of feature extraction techniques can work (and how effectively is it) with Transformer?  
- Results show that a BERTbased Transformer, trained on Mel spectrograms, can outperform  a CNN using 99.85% fewer parameters

**_Content_**:
Feature extraction for making input before go into transformer:
- Speech signal is continous variable, unlike text, which can be turned into discrete variable by using embedding layer. Transformer's input data requires 2 dimensional input, but raw speech signal is only 1d data. Therefore, this paper demonstrates different ways for extracting speech signal feature:
1. Amplitude reshaping: naive way, just reshape 1d speech signal into 2d.
2. Curve tokenization: based on the intuition that there may exists a small number of "curves" that can describe short sequences of audio signals.
3. VQ-VAE: to produce compressed "codes" from codebooks to represent audio during training process.
4. MFCC: Config = 128 mels, a hop length of 512, a window length of 1024, and number of FFTs of 1024.
5. Concat MFCC, GFCC, CQT, Chromagram
6. Mel spectrogram: is able to downsampling without affect to the accuracy

The paper demonstrated that MFCC/Mel spectrogram can be feature extractors to work with Transformer. However this problem is classification. I am personally looking for an architecture that make MFCC/Mel + Transformer -> ASR.  
In conclusion, MFCC + CNN in this paper is the most lightweight model version with high accuracy that can work with edge device.