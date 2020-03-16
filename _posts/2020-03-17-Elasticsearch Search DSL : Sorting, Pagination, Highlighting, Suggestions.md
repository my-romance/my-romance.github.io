---
layout: post
title:  "Elasticsearch Search DSL : Sorting, Pagination, Highlighting, Suggestions"
date:   2020-03-17
author: Romance
categories: elasticsearch
---  
# Elasticsearch Search DSL : Sorting, Pagination, Highlighting, Suggestions

### Sorting

sort 쿼리를 이용하여 특정 필드에 대한 정렬 가능

```python
from elasticsearch import Elasticsearch
from elasticsearch_dsl import Search
client = Elasticsearch()

if __name__ == '__main__':
    index_name = "mywords"
    s = Search(index = index_name).using(client).sort({'personal_score':{"order":"desc"}})
    response = s.execute()

    for hit in s:
        print(hit)
```

```python
<Hit(mywords/2): {'personal_score': 6.3, 'category': '의류'}>
<Hit(mywords/4): {'personal_score': 4.5, 'category': '음식'}>
<Hit(mywords/3): {'personal_score': 4.2, 'category': '필기구'}>
<Hit(mywords/1): {'personal_score': 1.2, 'category': '의류'}>
```

personal_score을 내림차순으로 정렬한 결과가 반환됨

<br>

### Pagination

from, size parameter를 설정하기 위해서는 python slicing을 사용하면 된다.

```python
from elasticsearch import Elasticsearch
from elasticsearch_dsl import Search
client = Elasticsearch()

if __name__ == '__main__':
    index_name = "mywords"
    s = Search(index = index_name).using(client)[0:2]
    response = s.execute()

    for hit in s:
        print(hit)
```

```python
<Hit(mywords/1): {'personal_score': 1.2, 'category': '의류'}>
<Hit(mywords/2): {'personal_score': 6.3, 'category': '의류'}>
```

query와 매치되는 모든 documents를 반환하고 싶을 때는 scan 또는 scroll을 이용할 수 있음.

```python
for hit in s.scroll():
	print(hit)
```

```python 
<Hit(mywords/1): {'personal_score': 1.2, 'category': '의류'}>
<Hit(mywords/2): {'personal_score': 6.3, 'category': '의류'}>
<Hit(mywords/3): {'personal_score': 4.2, 'category': '필기구'}>
<Hit(mywords/4): {'personal_score': 4.5, 'category': '음식'}>
```

하지만 이 경우의 결과값은 정렬되지 않는다고 함

<br>

### Highlighting

highlight를 이용하여 match된 단어를 강조 가능. query가 어느 필드에 어느 단어에 매칭되어 있는지 분석하기 위해 사용할 수 있음.

```python
from elasticsearch import Elasticsearch
from elasticsearch_dsl import Search
client = Elasticsearch()

if __name__ == '__main__':
    index_name = "mywords"
    s = Search(index = index_name).using(client).query('match',word="원피스").highlight('word',pre_tags='<em>',post_tags='</em>')

    response = s.execute()

    for hit in response:
        print(hit)
        for fragment in hit.meta.highlight.word:
            print(fragment)
```

```python
<Hit(mywords/1): {'word': '하늘원피스', 'personal_score': 1.2, 'color': '빨강', 'cat...}>
하늘<em>원피스</em>
<Hit(mywords/2): {'word': '빨강원피스', 'personal_score': 6.3, 'color': '주황', 'cat...}>
빨강<em>원피스</em>
```

\<em\>과 \</em\>을 이용하여 매치된 단어를 구분함. 매치된 단어는 'hit.meta.highlight.해당필드이름'을 통해 확인 가능. 또한, 아래와 같은 코드를 이용하여 score 점수대로 정렬 가능.

```python
s = s.highlight_options(order='score')
```

<br>

### Suggestions

사용자가 키워드를 잘 못 입력하였거나 검색한 결과가 없을 경우, 도큐먼트 내에 존재하는 단어를 대상으로 비슷한 키워드를 변경해서 제시하는 **교정 기능** 제공

- Term suggest API : 추천단어 제안
  잘못된 철자에 대해 해당 인덱스의 특정 필드에서 가장 유사한 단어를 추천해주는 오타 교정 방법

- Completion suggest API : 자동완성 제안

  사용자가 입력을 완료하기 전에 자동완성을 사용해 검색어를 예측해서 보여줌. 

- Phrase suggest API : 추천문장 제안

- Context suggest  API : 추천 문맥 제안

Term suggest API 예시

```python
text = "삘강"
suggDoc = {
	"entity-suggest" : {
		'text' : text,
		"term" : {
			"field" : "color"
		}
	}
}
query_dic = {"suggest" : suggDoc}
result = client.search(body = query_dic, index = index_name)
print(result['suggest'])
```

```python
s = Search(index = index_name).using(client).suggest('my_suggestion', '삘강', term={'field': 'color'})
response = s.execute()

for x in response.suggest['my_suggestion']:
	for y in x['text']: print(y)
```

문법은 첫번째와 두번째 코드처럼 사용할 수 있지만 영어만 잘되는 거 같음. **한글은 ICU 분석기**를 이용하여 오타를 교정하는 것이 가능하다고 함.

<br>

### 출처

- https://elasticsearch-dsl.readthedocs.io/en/latest/search_dsl.html
- https://stackoverflow.com/questions/24602347/how-to-use-suggest-in-elasticsearch-pyes
- https://deviscreen.tistory.com/29

