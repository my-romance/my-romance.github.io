---
layout: post
title:  "11. TF IDF Weighting"
date:   2020-02-18
author: Romance
categories: IR
---
# TF IDF Weighting

### TF-IDF Weighting

- The tf-idf weight of a term $t$ is the product of $t$ 's tf weight and $t$ 's idf weight
  $$
  W_{t,d} = (1+logtf_{t,d}) \times log_{10}(N/df_t)
  $$

- **Best Known weighting scheme** in information retrieval

- Increases with **the number of occurrences within a document**  → TF

- Increases with **the rarity of the term in the collection**  → IDF

<br>

### Final Ranking of Documents for a Query

$$
Score(q,d) = \sum_{t \in q\cap d}tf.idf_{t,d}
$$

<br>

### Binary  → Count  → Weight Matrix

<img src="/assets/image/weight_matrix.PNG">

- Each document is now represented by a real-valued vector of **tf-idf weights**  $\in R^{|V|}$

<br>

### 출처

- stanford IR 강의(https://www.youtube.com/watch?v=4-P3ckZprBk&list=PLaZQkZp6WhWwoDuD6pQCmgVyDbUWl_ZUi&index=11)

