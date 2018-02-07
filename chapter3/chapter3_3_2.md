# 3.3.2 OPTIONAL MATCH

> `OPTIONAL MATCH` 절은 패턴에 일치하지 않는 것을 NULL로 사용해서 조회할 때 사용됩니다.

* [소개](#chapter332_1)
* [선택적 관계](#chapter332_2)
* [선택적 요소의 속성](#chapter332_3)
* [관계의 선택적 타입과 이름](#chapter332_4)

## 3.3.2.1 소개 {#chapter332_1}

`OPTIONAL MATCH` 절은 데이터베이스에서 매칭되는 데이터를 조회하는 것으로 `MATCH`와 유사합니다. 차이점은 매칭된 결과가 없는 경우 `OPTIONAL MATCH`에서는 `null`을 사용하여 결과를 반환합니다. Cypher에서 `OPTIONAL MATCH` 절은 SQL에서 아웃터 조인과 동일하다고 보시면 됩니다.

전체 패턴이 일치하거나 아무것도 일치하지 않습니다. 이 패턴에 `WHERE` 절이 있으면, 매칭이 끝나고 적용되는 것이 아닌 매칭할때 술어부를 고려하게 됩니다. 다수의 (`OPTIONAL`)`MATCH` 절이 있는 경우 `MATCH`에 적용할 `WHERE`절을 함께 놓는 것이 중요합니다.

> `OPTIONAL MATCH`에서 사용되는 패턴에 관련되서는 [3.2.9 패턴](/chapter3/chapter3_2_9.md)를 참고하시기 바랍니다.

아래의 예제에서 다음 그래프를 사용하겠습니다.

![](https://neo4j.com/docs/developer-manual/current/images/OPTIONAL%20MATCH-1.svg)

## 3.3.2.2 선택적 관계 {#chapter332_2}

관계가 선택적이면, `OPTIONAL MATCH` 절을 사용하면 됩니다. SQL에서 아웃터 조인과 동일하다고 보시면 됩니다. 만약 관계가 존재하면 관계정보를 반환하고, 없으면 `null`을 반환합니다.

### 쿼리

```cypher
MATCH (a:Movie { title: 'Wall Street' })
OPTIONAL MATCH (a)-->(x)
RETURN x
```

해당 노드에 나가는 관계가 없기 때문에 `null`을 반환합니다.

### 쿼리결과

| x |
| :--- |
| `<null>` |
| **1 row** |

## 3.3.2.3 선택적 요소의 속성 {#chapter332_3}

선택적 요소가 `null`일 경우 그것의 속성 또한 `null`을 반환합니다.

### 쿼리

```cypher
MATCH (a:Movie { title: 'Wall Street' })
OPTIONAL MATCH (a)-->(x)
RETURN x, x.name
```

### 쿼리결과

| x | x.name |
| :--- | :--- |
| `<null>` | `<null>` |
| **1 row** ||

## 3.3.2.4 관계의 선택적 타입과 이름 {#chapter332_4}

일반적인 관계와 동일하게, 관계의 방향이 어디인지 관계의 타입이 무엇인지 정의하시면 됩니다.

### 쿼리

```cypher
MATCH (a:Movie { title: 'Wall Street' })
OPTIONAL MATCH (a)-[r:ACTS_IN]->()
RETURN a.title, r
```

**Wall Street**가 제목인 노드를 반환하고, `ACT_IN` 관계로 나가는 노드가 없기 때문에 `r`로 정의된 관계는 `null`값으로 반환됩니다.

### 쿼리결과

| a.title | r |
| :--- | :--- |
| `"Wall Street"` | `<null>` |
| **1 row** ||