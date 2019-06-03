# Quora-Insincere-Questions-Classification-Top7%-Solution

## Background
How to deal with toxic content is one of the problems among the websites today. Quora wants to tackle this problem to let users feel safe when sharing knowledge on the platform. In this competition, we are challenged to develop models that identify and flag insincere questions, which would be helpful for creating more scalable methods to detect toxic and misleading content.
<img width="1452" alt="Screen Shot 2019-06-03 at 1 17 40 AM" src="https://user-images.githubusercontent.com/40588854/58786935-88068880-859d-11e9-8b0c-dc1ec8223c90.png">

## Challenges
1. 2-hour kernel running time limitation, so how to let models converge in a short time but keep robust is the key.
2. Without further preprocessing of text, on average, just 25% of vocabulary has their corresponding embeddings. Many words are not presented in the training phase.
3. "Real" test samples are 6 times larger, I cannot fully trust the public lb score and should focus more on local performance.

## My Solution (finished in 6416.2s)

### Data
Training Samples: 1,306,122 
- Sincere questions: 1,225,312 (93.81%)
- Insincere questions: 80,810 (6.19%)

Test Samples: 375,806 (28.77% of train samples)
- Public Test: 56,000 (13.30%)
- Private Test: 325,806 (86.70%)

### Evaluation
<img width="1259" alt="Screen Shot 2019-06-03 at 1 47 08 AM" src="https://user-images.githubusercontent.com/40588854/58788833-9656a380-85a1-11e9-8521-5d2162e1feb1.png">

Prediction was evaluated on F1-score. There were 56,000 test samples selected for generating public lb scores, but 325,806 samples for calculating private lb scores. Therefore, how to avoid a huge shakeup is one of the problems that needs to be considered.

### Preprocess

#### Text Cleaning
- Lower Case Letters and Remove Punctuations
- Clean Numbers
- Process Misspells 
- Clean Contractions

After the work above, the proportion of embedding found for vocabulary increased from 25% to 64%.

#### Feature Engineer
- Sentence Length
- Number of Capital Letters
- Number of Capital Letters/Sentence Length
- Number of Words
- Number of Unique Words
- Number of Unique Words/Number of Words
- Number of Sensitive Words
- Number of Toxic Words
- Standardize Features

#### Sentence Preparation
- Tokenize Sentences+Paddings
- Shuffling

### Embeddings

**4 types of embeddings provided:**
* GoogleNews-vectors-negative300
* glove.840B.300d
* paragram_300_sl999
* wiki-news-300d-1M

*Note: For those words that have no pretrained embeddings, their embeddings would be randomly initialized with the same mean and standard deviation in that matrix.*

**After several experiments, I found the combinations of GLOVE and PARAGRAM achieved the best performance. 2 kinds of combination I used in this competition:**
- Weighted Average 

(Ensemble of embeddings is a feasible way for improvement: https://arxiv.org/pdf/1804.07983.pdf)
- Concatenation 

(There are 600 dimensions and correspondingly, the time used for training is much longer, but it's good for training diverse models)

### Models

**4 various of architectures are adopted:**
- LSTM+GRU+CapsNet
- GRU+2Poolings(Max and Avg)
- LSTM+Attention+2Poolings(Max and Avg)
- CNN with filter_size 3,4,5,10
