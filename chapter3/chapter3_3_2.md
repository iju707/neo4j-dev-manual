# 3.3.2 OPTIONAL MATCH

> `OPTIONAL MATCH` 절은 패턴에 일치하지 않는 것을 NULL로 사용해서 조회할 때 사용됩니다.

* [소개](#chapter332_1)
* [선택적 관계](#chapter332_2)
* [선택적 요소의 속성](#chapter332_3)
* [관계의 선택적 타입과 이름](#chapter332_4)

## 3.3.2.1 소개 {#chapter332_1}

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