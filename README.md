# Quora-Insincere-Questions-Classification-Top7%-Solution

## Background
How to deal with toxic content is one of the problems among the websites today. Quora wants to tackle this problem to let users feel safe when sharing knowledge on the platform. In this competition, we are challenged to develop models that identify and flag insincere questions, which would be helpful for creating more scalable methods to detect toxic and misleading content.
<img width="1452" alt="Screen Shot 2019-06-03 at 1 17 40 AM" src="https://user-images.githubusercontent.com/40588854/58786935-88068880-859d-11e9-8b0c-dc1ec8223c90.png">

## Challenges
1. 2-hour kernel running time limitation, so how to let models converge in a short time but keep robust is the key.
2. Without further preprocessing of text, on average, just 25% vocabulary have their corresponding embeddings. Many words are not presented in the training phase.

## Solution
This is the top7% solution(0.70363) using ensemble of 4 various architectures (finished in 6416.2s)

- LSTM+GRU+CapsNet
- GRU+2Poolings(Max and Avg)
- LSTM+Attention+2Poolings(Max and Avg)
- CNN with filter_size = 3,4,5,10
