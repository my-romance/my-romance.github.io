---
layout: post
title:  "MaxDoubleSliceSum"
date:   2020-03-28
author: Romance
categories: Algorithm_codility
---
# [MaxDoubleSliceSum](https://app.codility.com/programmers/lessons/9-maximum_slice_problem/max_double_slice_sum/)

```python
# you can write to stdout for debugging purposes, e.g.
# print("this is a debug message")

def solution(A):
    A.pop(); A.pop(0);
    
    left_arr, max_num = [0], 0
    for l_i in range(1, len(A)):
        max_num = max(0, max_num+A[l_i-1])
        left_arr.append(max_num)
    
    A.reverse()
    right_arr, max_num = [0], 0
    for r_i in range(1, len(A)):
        max_num = max(0, max_num+A[r_i-1])
        right_arr.append(max_num)
    right_arr.reverse()
    
    max_num = 0
    for l_num, r_num in zip(left_arr, right_arr):
        max_num = max(max_num, l_num+r_num)
    return max_num
        
```

제목이 Double이 들어간 것처럼 두개의 max slice sum을 만들어서 이를 합해주어야 하는 문제.

풀이 방법 : 

<img src="/assets/image/MaxDoubleSliceSum1.PNG">

<img src="/assets/image/MaxDoubleSliceSum2.PNG">

<img src="/assets/image/MaxDoubleSliceSum3.PNG">

**느낀 점 :** **Kadane's algorithm**을 적용. [-1] 배열보단 [ ] 배열의 원소 값이 0으로 합이 최대가 되는데 이거를 고려하지 못하였음. 
