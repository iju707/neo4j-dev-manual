# 3.3.14 REMOVE

> `REMOVE`는 그래프 요소에서 속성이나 라벨을 제거하는 것 입니다.

* [소개](#chapter3314_1)
* [속성 제거](#chapter3314_2)
* [노드에서 라벨제거](#chapter3314_3)
* [다중 라벨제거](#chapter3314_4)

## 3.3.14.1 소개 {#chapter3314_1}

노드나 관계의 삭제를 원할 경우 [3.3.12 DELETE](chapter3/chapter3_3_12.md)를 참고하시기 바랍니다.

> 노드의 라벨을 삭제하는 것은 멱등원 연산입니다. 노드에 없는 라벨을 삭제하려고 하면 아무일도 발생하지 않습니다.

아래의 예제는 다음 그래프를 사용합니다.

![](https://neo4j.com/docs/developer-manual/current/images/cypher-remove-graph.svg)

## 3.3.14.2 속성 제거 {#chapter3314_2}

Neo4j에서는 `null`값을 속성으로 저장하지 않습니다. 대신 만약 값이 없다면 속성자체를 가지지 않습니다. 그래서 해당 노드나 관계에 대하여 속성값을 없애는 것은 `REMOVE`를 사용하는 것과 동일합니다.

### 쿼리

```cypher
MATCH (andres { name: 'Andres' })
REMOVE andres.age
RETURN andres
```

노드가 반환되면 `age`라는 속성이 제거됩니다.

### 쿼리결과

```cypher
+------------------------+
| andres                 |
+------------------------+
| Node[0]{name:"Andres"} |
+------------------------+
1 row
Properties set: 1
```

## 3.3.14.3 노드에서 라벨제거 {#chapter3314_3}

라벨을 삭제하려면 `REMOVE`를 사용하시면 됩니다.

### 쿼리

```cypher
MATCH (n { name: 'Peter' })
REMOVE n:German
RETURN n
```

### 쿼리결과

```cypher
+------------------------------+
| n                            |
+------------------------------+
| Node[2]{name:"Peter",age:34} |
+------------------------------+
1 row
Labels removed: 1
```

## 3.3.14.4 다중 라벨제거 {#chapter3314_4}

다수의 라벨을 제거할때도 `REMOVE`를 사용하시면 됩니다.

### 쿼리

```cypher
MATCH (n { name: 'Peter' })
REMOVE n:German:Swedish
RETURN n
```

### 쿼리결과

```cypher
+------------------------------+
| n                            |
+------------------------------+
| Node[2]{name:"Peter",age:34} |
+------------------------------+
1 row
Labels removed: 2
```

