# 3.3.12 DELETE

> `DELETE` 절은 그래프의 항목 (노드, 관계, 경로)를 삭제할때 사용됩니다.

* [소개](#chapter3312_1)
* [단일 노드 삭제](#chapter3312_2)
* [모든 노드/관계 삭제](#chapter3312_3)
* [노드와 관계된 모든 관계 삭제](#chapter3312_4)
* [관계만 삭제](#chapter3312_5)

## 3.3.12.1 소개 {#chapter3312_1}

속성이나 라벨을 삭제할 경우에는 [3.3.14 REMOVE](/chapter3/chapter3_3_14.md)를 보시면 됩니다. 주의하실 점은 삭제하고자 하는 특정 노드에 대하여 해당 노드가 시작 또는 끝으로 있는 관계가 삭제되기 전까지는 삭제할 수 없습니다. 관계를 명시적으로 모두 삭제하거나, `DETACH DELETE` 절을 사용하여 삭제하시면 됩니다.

아래의 내용은 다음의 그래프 예제를 가지고 진행하겠습니다.

![](https://neo4j.com/docs/developer-manual/current/images/DELETE-1.svg)

## 3.3.12.2 단일 노드 삭제 {#chapter3312_2}

노드를 삭제하기 위해, `DELETE` 절을 사용하면 됩니다.

### 쿼리

```cypher
MATCH (n:Useless)
DELETE n
```

### 쿼리결과

```
+-------------------+
| No data returned. |
+-------------------+
Nodes deleted: 1
```

## 3.3.12.3 모든 노드/관계 삭제 {#chapter3312_3}

다음 쿼리는 대규모의 데이터를 삭제할 때 사용하라고 있는 쿼리는 아니지만, 작은 규모의 샘플 데이터를 가지고 진행할 때 유용할 것 입니다.

### 쿼리

```cypher
MATCH (n)
DETACH DELETE n
```

### 쿼리결과

```
+-------------------+
| No data returned. |
+-------------------+
Nodes deleted: 3
Relationships deleted: 2
```

## 3.3.12.4 노드와 관계된 모든 관계 삭제 {#chapter3312_4}

특정 노드와 그와 관계된(시작 또는 종료) 모든 관계를 포함하여 삭제할 경우 `DETACH DELETE`절을 사용하시면 됩니다.

### 쿼리

```cypher
MATCH (n { name: 'Andres' })
DETACH DELETE n
```

### 쿼리결과

```
+-------------------+
| No data returned. |
+-------------------+
Nodes deleted: 1
Relationships deleted: 2
```

## 3.3.12.5 관계만 삭제 {#chapter3312_5}

노드에 영향을 주지 않고 관계만 삭제가 가능합니다.

### 쿼리

```cypher
MATCH (n { name: 'Andres' })-[r:KNOWS]->()
DELETE r
```

이름이 **Andres**인 노드로부터 나가는 방향의 타입이 **KNOWS**인 모든 관계를 삭제합니다.

### 쿼리결과

```
+-------------------+
| No data returned. |
+-------------------+
Relationships deleted: 2
```
