# 3.3.15 FOREACH

> `FOREACH` 절은 목록이나 경로, 집계의 결과 데이터를 업데이트하는데 사용됩니다.

* [소개](#chapter3315_1)
* [경로의 모든 노드에 표시하기](#chapter3315_2)

## 3.3.15.1 소개 {#chapter3315_1}

Cypher에서 목록과 경로는 중요한 요소입니다. `FOREACH`는 경로나 집계로 생성된 목록 등에 업데이트를 수행하는 명령으로 사용됩니다.

`FOREACH` 괄호안의 변수정보는 외부와 분리되어있습니다. 그 의미는 `FOREACH` 절에서 `CREATE`를 사용하여 노드변수를 생성하였다고 하더라도 외부에서 해당 값을 사용할 수 없으며 다시 매핑을 해야합니다.

`UNWIND`([3.3.6 UNWIND](/chapter3/chapter3_3_6.md) 참조)한 목록의 각각 요소에 대하여 추가적인 `MATCH` 수행하는데 적절한 명령입니다.

![](https://neo4j.com/docs/developer-manual/current/images/FOREACH-1.svg)

## 3.3.15.2 경로의 모든 노드에 표시하기 {#chapter3315_2}

아래 쿼리는 경로의 모든 노드에 `marked`라는 속성을 `true`로 설정하는 것 입니다.

### 쿼리

```cypher
MATCH p =(begin)-[*]->(END )
WHERE begin.name = 'A' AND END .name = 'D'
FOREACH (n IN nodes(p)| SET n.marked = TRUE )
```

결과가 아무것도 없지만 4개의 노드에 속성이 설정됩니다.

### 쿼리결과

| `(empty result)` |
| :--- |
| **0 rows Properties set: 4** |