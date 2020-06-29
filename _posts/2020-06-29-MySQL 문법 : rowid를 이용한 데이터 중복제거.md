---
layout: post
title:  "MySQL 문법 : UPDATE"
date:   2020-02-19
author: Romance
categories: MySQL
---
# MySQL 문법 : rowid를 이용한 데이터 중복제거

```sql
select * from 테이블명
where rowid in (select max(rowid) from 테이블명 group by 중복제거할 기준되는 컬럼)
```

```sql
# 예제
select date, name, cate1, cate2, cate3,img as img_url
From shopping.public.entity
where date > 20170000 and date < 20200000 and id in (select max(id) from shopping.public.entity group by name)
order by date, name
```

### 출처 

- https://kdarkdev.tistory.com/82
