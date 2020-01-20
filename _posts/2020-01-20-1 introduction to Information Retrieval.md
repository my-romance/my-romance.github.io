---
layout: post
title:  "1. introduction to Information Retrieval"
date:   2020-01-20
author: Romance
categories: IR
---
# 1. Introduction to Information Retrieval

### Information Retrieval

Information Retrieval (IR) : **finding material** (usually documents) of an unstructured nature (usually text) that **satisfies an information need** from within **large collections** (usually stored on computers)

<br>

### Basic assumptions of Information Retrieval

- Collection : A set of documents 
- Goal of IR : Retrieve documents with information that is **relevant** to user's **information need** and helps the user complete a **task**

<br>

### The classic search model

<img src="/assets/image/the classic search model.PNG">

classic search model 설명 : info need를 위한 적절한  `Query`를 `Search engine`에 날리면 `Collection`에서 `Query`와 연관이 있는 정보 즉, 문서를 output으로 제공하여 `Results`를 만들어 낸다. `Results`가 마음에 들지 않는다면 `Query`를 다시 정제(`Query refinement`)하여 새로운 `Query`를 `Search engine`에 날린다.

`misconception`은 사용자가  행하고 싶은 task(user task)와 사용자가 행하고 싶은 task를 하기 위해 필요한 정보(info need)의 불일치를 의미한다.

`misformulation`은 필요한 정보를 찾기위해 query를 날리는데 이때 query의 단어를 잘 못 선택한 것을 의미한다.  

ex)

user task : get rid of mice in a politically correct way

info need : info about removing mice without killing them

query : how trap mice alive

misconception: not **whether or not i kill them** but **whether  i do it humanely**

misformulation : how trap mice alive → how trap mice without killing





출처 : https://www.youtube.com/watch?v=kNkCfaH2rxc&list=PLaZQkZp6WhWwoDuD6pQCmgVyDbUWl_ZUi
