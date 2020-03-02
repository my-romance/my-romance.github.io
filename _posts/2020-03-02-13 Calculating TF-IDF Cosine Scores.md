---
layout: post
title:  "13. Calculating TF-IDF Cosine Scores"
date:   2020-03-02
author: Romance
categories: IR
---
# Calculating TF-IDF Cosine Scores

### tf-idf weighting has may variants

<img src="/assets/image/tfidf.PNG">

- 빨간색의 수식이 가장 잘 사용되는 방법
- 다른 방법들은 어떻게 normalizing을 할 것이냐에 따라 조금씩 다름
  - a(augmented)  $\rightarrow$ 0.5~1의 값이 나오도록
  - p(prob idf) $\rightarrow$ term이 전체 문서에서 반 이상나온다면 가중치 0을 주어 불용어 취급



### Weighting may differ in queries VS. documents

- Many search engines allow for different weightings for queries VS. documents
- SMART Notation : denotes the combination in user in an engine, with the notation ddd.qqq, using the acronyms from the previous table
- A very standard weighting scheme is : lnc(document).ltc(query) or **lnc.ltn**
- Document : logarithmic tf, no idf and cosine normalization
  -  no idf를 사용하는 이유 : you've already put in an IDF factor for the same words in the query.
- query : logarithmic tf, idf, cosine normalization



### tf-idf exmaple : lnc.ltc

<img src="/assets/image/ltnlnc.PNG">

- prod = query's n'lize * document's n'lize



### Computing Cosine Scores

<img src="/assets/image/cosinescores.PNG">

- **length normalization of the query is actually unnecessary** because the query vector has some length and for whatever it is, the effective length normalization would just be a rescaling that applies to all query document calculation and wouldn't change the final result.
- 효율적인 알고리즘은 아니지만 가장 general idea 알고리즘



### Summary - Vector Space Ranking

- Represent the query as a weighted tf-idf vector
- Represent each document as a weighted tf-idf vector
- Compute the cosine similarity score for the query vector and each document vector
- Rank documents with respect to the query by score
- return the top K to the user



### 출처 

- 스탠포드 IR 강의 (https://www.youtube.com/watch?v=k1tD7pYKWuM&list=PLaZQkZp6WhWwoDuD6pQCmgVyDbUWl_ZUi&index=13)
