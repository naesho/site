+++
date        = "2017-09-22T00:30:00+09:00"
title       = "선택도 (기수성)"
tags        = [ "mysql", "selectivity", "carldinality"]
categories = [ "Concepts"]
thumbnailImage = ""
+++

# 선택도

>**모든 인덱스 키값 가운데 유니크한 값의 수를 의미함**

전체 인덱스 키값은 100개인데, 그중에서 유니크한 값의 수는 10개라면 기수성은 10

인덱스 키값에 중복이 많다면 선택도가 낮다는 뜻임

선택도가 높다면, 검색 대상이 줄어들기 때문에 그만큼 빠르게 질의(Query) 가 가능하다.

>**선택도가 좋지 않아도, 정렬이나 그룹핑과 같은 작업을 위해 인덱스가 필요 할 수도 있다.**
>**인덱스가 검색만을 위해서 사용되는 것은 아니다.**

옵티마이저에 의해서, 선택도가 낮은 경우 (검색 결과가 테이블의 30% 이상 리턴 하는 경우)

테이블 풀 스캔이 발생할 수 있다.