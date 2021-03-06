# 3.3.9 SKIP

> `SKIP` 절은 결과가 어떤 행부터 시작할지에 대한 행을 정의하는 것 입니다.

* [소개](#chapter339_1)
* [최초 3개 행 생략](#chapter339_2)
* [중간 2개 행 반환](#chapter339_3)
* [`SKIP`을 사용해 행의 부분집합을 반환하기 위한 표현식](#chapter339_4)

## 3.3.9.1 소개 {#chapter339_1}

`SKIP`을 사용하면 결과를 첫행부터 생략해서 구성할 수 있습니다. 참고로, 쿼리에 `ORDER BY`를 명시했더라 하더라도 결과의 순서를 확실하게 보장하지는 않습니다. `SKIP`은 양수를 출력하는 모든 표현식으로 사용이 가능합니다. 단, 노드나 관계를 참조할 수는 없습니다.

![](https://neo4j.com/docs/developer-manual/current/images/SKIP-1.svg)

## 3.3.9.2 최초 3개 행 생략 {#chapter339_2}

4번째 결과부터 시작해서 결과의 일부분을 반환하려면 다음과 같은 쿼리를 작성하시면 됩니다.

### 쿼리

```cypher
MATCH (n)
RETURN n.name
ORDER BY n.name
SKIP 3
```

최초 3개의 노드는 생략이 되고, 남은 2개의 노드가 결과로 출력됩니다.

### 쿼리결과

| n.name |
| :--- |
| `"D"` |
| `"E"` |
| **2 rows** |

## 3.3.9.3 중간 2개 행 반환 {#chapter339_3}

중간부터 시작해서 2개의 결과를 반환하려면 다음과 같은 쿼리를 작성하시면 됩니다.

### 쿼리

```cypher
MATCH (n)
RETURN n.name
ORDER BY n.name
SKIP 1
LIMIT 2
```

중간의 2개 노드가 반환됩니다.

> **역주** 총 5개의 노드중 가운데 2개를 추출하므로 시작점은 (5 - 2)/5 = 1.xxx => 1로 해서 `SKIP 1`을 사용하게 됩니다.

### 쿼리결과

| n.name |
| :--- |
| `"B"` |
| `"C"` |
| **2 rows** |

## 3.3.9.4 `SKIP`을 사용해 행의 부분집합을 반환하기 위한 표현식 {#chapter339_4}

`SKIP` 절에 양수를 출력하는 표현식을 사용할 수 있습니다. 단, 외부변수 참조는 불가능합니다.

### 쿼리

```cypher
MATCH (n)
RETURN n.name
ORDER BY n.name
SKIP toInteger(3*rand())+ 1
```

최초 3개의 노드가 생략되고 2개의 노드가 결과에 반환됩니다.

> **역주** `rand()`가 0 ~ 1의 실수를 반환하게 되므로, `SKIP` 절에서는 `0 + 1`부터 `3 + 1`까지의 범위가 결정되어 랜덤으로 결과가 출력됩니다.

### 쿼리결과

| n.name |
| :--- |
| `"B"` |
| `"C"` |
| `"D"` |
| `"E"` |
| **4 rows** |