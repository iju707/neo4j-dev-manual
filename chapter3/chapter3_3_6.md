# 3.3.6 UNWIND

> `UNWIND` 절은 목록의 데이터를 Row로 확장시키는 것 입니다.

* [소개](#chapter336_1)
* [목록 풀기](#chapter336_2)
* [중복방지 목록생성](#chapter336_3)
* [`UNWIND`안에 목록의 표현식 사용](#chapter336_4)
* [`UNWIND`에 이중목록 사용](#chapter336_5)
* [`UNWIND`에 공백목록 사용](#chapter336_6)
* [`UNWIND`에 목록이 아닌 표현식 사용](#chapter336_7)
* [목록 파라미터로 노드 생성](#chapter336_8)

## 3.3.6.1 소개 {#chapter336_1}

`UNWIND`로 목록을 각각의 행으로 변경할 수 있습니다. 변경하고자하는 목록은 파라미터로 전달될 수 도 있고, 직전의 `collect`로 생성된 목록 또는 다른 목록 표현식이 될 수 있습니다.

일반적인 `UNWIND`사용용도는 중복이 제거된 목록을 만들 때 사용됩니다. 다른 것은 쿼리에 제공된 파라미터 목록으로부터 데이터를 만들 때 입니다.

`UNWIND`는 내부 값을 위하여 꼭 이름을 지정해야 합니다.

## 3.3.6.2 목록 풀기 {#chapter336_2}

목록을 행으로 변경한 뒤 이름을 `x`로 해보겠습니다.

### 쿼리

```cypher
UNWIND [1, 2, 3, NULL ] AS x
RETURN x, 'val' AS y
```

`null`을 포함해서 원본 목록의 값이 각각의 행으로 변경되어 반환됩니다.

### 쿼리결과

```
+----------------+
| x      | y     |
+----------------+
| 1      | "val" |
| 2      | "val" |
| 3      | "val" |
| <null> | "val" |
+----------------+
4 rows
```

## 3.3.6.3 중복방지 목록생성 {#chapter336_3}

중복된 목록을 `DISTINCT` 절을 활용하여 변환할 수 있습니다.

### 쿼리

```cypher
WITH [1, 1, 2, 2] AS coll
UNWIND coll AS x
WITH DISTINCT x
RETURN collect(x) AS SET
```

원본 목록이 풀어진 뒤 유일한 집합으로 변경되기 위해 `DISTINCT`에 전달됩니다.

### 쿼리결과

```
+-------+
| set   |
+-------+
| [1,2] |
+-------+
1 row
```

## 3.3.6.4 `UNWIND`안에 목록의 표현식 사용 {#chapter336_4}

목록을 반환하는 표현식을 `UNWIND`에 사용할 수 있습니다.

### 쿼리

```cypher
WITH [1, 2] AS a,[3, 4] AS b
UNWIND (a + b) AS x
RETURN x
```

`a`와 `b` 목록을 새로운 한개 목록으로 합친 뒤, `UNWIND`로 처리하는 것 입니다.

### 쿼리결과

```
+---+
| x |
+---+
| 1 |
| 2 |
| 3 |
| 4 |
+---+
4 rows
```

## 3.3.6.5 `UNWIND`에 이중목록 사용 {#chapter336_5}

목록안의 목록을 풀기 위해서는 `UNWIND` 절을 연결해서 사용하시면 됩니다.

### 쿼리

```cypher
WITH [[1, 2],[3, 4], 5] AS nested
UNWIND nested AS x
UNWIND x AS y
RETURN y
```

첫번째 `UNWIND`로 2개의 목록을 포함한 원본목록이 `[1, 2]`, `[3, 4]`, `5`로 3개의 행으로 반환되며 이름은 `x`가 됩니다. 두번째 `UNWIND`로 각각의 행이 풀어져서 총 5개의 행으로 반환되며 이름은 `y`가 됩니다.

### 쿼리결과

```
+---+
| y |
+---+
| 1 |
| 2 |
| 3 |
| 4 |
| 5 |
+---+
5 rows
```

## 3.3.6.6 `UNWIND`에 공백목록 사용 {#chapter336_6}

빈 목록을 가지고 `UNWIND`를 사용하면 앞에 어떤 행이 존재하던, 다른 값을 포함하고 있던 결과가 반환되지 않습니다. 특히 `UNWIND []`의 경우 행을 0건으로 만들어 쿼리를 중단시키게 되며 결과가 반환되지 않습니다. `UNWIND v`와 같이 변수를 가지고 하는 경우 `v`가 이전 절에서 빈 목록이 되면 `MATCH`절이 아무런 결과가 없는 것 처럼 됩니다. `UNWIND`가 빈 목록을 처리하지 않도록 하려면, `CASE`를 사용해서 빈 목록을 `null`로 치환해주시면 됩니다 : `WITH [] AS list UNWIND CASE WHEN list = [] THEN [null] ELSE list END AS emptylist RETURN emptylist`

### 쿼리

```cypher
UNWIND [] AS empty
RETURN empty, 'literal_that_is_not_returned'
```

### 쿼리결과

```
+----------------------------------------+
| empty | 'literal_that_is_not_returned' |
+----------------------------------------+
+----------------------------------------+
0 row
```

## 3.3.6.7 `UNWIND`에 목록이 아닌 표현식 사용 {#chapter336_7}

`UNWIND 5`와 같이 목록이 아닌 표현식으로 `UNWIND`를 사용하게 되면 에러가 발생합니다. 예외적으로 표현식이 `null`을 반환하면 결과 행이 0건으로 되며 실행을 중단하고 결과를 반환하지 않습니다.

### 쿼리

```cypher
UNWIND NULL AS x
RETURN x, 'some_literal'
```

### 쿼리결과

```
+--------------------+
| x | 'some_literal' |
+--------------------+
+--------------------+
0 row
```

## 3.3.6.8 목록 파라미터로 노드 생성 {#chapter336_8}

목록 파라미터로부터 `FOREACH`를 사용하지 않고 노드와 관계를 생성할 수 있습니다.

### 파라미터

```json
{
  "events" : [ {
    "year" : 2014,
    "id" : 1
  }, {
    "year" : 2014,
    "id" : 2
  } ]
}
```

### 쿼리

```cypher
UNWIND $events AS event
MERGE (y:Year { year: event.year })
MERGE (y)<-[:IN]-(e:Event { id: event.id })
RETURN e.id AS x
ORDER BY x
```

원본 목록의 값이 풀리게 되며 `MERGE`에 전달되어 노드와 관계를 찾고 생성하게 됩니다.

### 쿼리결과

```
+---+
| x |
+---+
| 1 |
| 2 |
+---+
2 rows
Nodes created: 3
Relationships created: 2
Properties set: 3
Labels added: 3
```