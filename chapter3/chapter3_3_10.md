# 3.3.10 LIMIT

> `LIMIT`은 출력의 행수를 정의합니다.

* [소개](#chapter3310_1)
* [결과의 일부 반환](#chapter3310_2)
* [결과의 일부를 반환하기 위해 `LIMIT`에 표현식사용하기](#chapter3310_3)

## 3.3.10.1 소개 {#chapter3310_1}

`LIMIT`에 양수를 반환하는 표현식을 사용할 수 있습니다. 단, 노드나 관계를 참조할 수 는 없습니다.

![](https://neo4j.com/docs/developer-manual/current/images/LIMIT-1.svg)

## 3.3.10.2 결과의 일부 반환 {#chapter3310_2}

첫번째 행부터 시작해서 결과의 일부를 반환하기 위해 다음과 같이 작성하시면 됩니다.

### 쿼리

```cypher
MATCH (n)
RETURN n.name
ORDER BY n.name
LIMIT 3
```

예제 쿼리에서 이름순으로 정렬한 뒤 상위 3개의 이름을 반환하게 됩니다.

### 쿼리결과

| n.name |
| :--- |
| `"A"` |
| `"B"` |
| `"C"` |
| **3 rows** |

## 3.3.10.3 결과의 일부를 반환하기 위해 `LIMIT`에 표현식사용하기 {#chapter3310_3}

`LIMIT`에 양수를 반환하는 표현식을 사용할 수 있습니다. 단, 노드나 관계를 참조할 수 는 없습니다.

### 쿼리

```cypher
MATCH (n)
RETURN n.name
ORDER BY n.name
LIMIT toInteger(3 * rand())+ 1
```

### 쿼리결과

| n.name |
| :--- |
| `"A"` |
| `"B"` |
| **2 rows** |