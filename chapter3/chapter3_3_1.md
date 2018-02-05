# 3.3.1 MATCH

> `MATCH`절은 표현한 패턴을 찾을 때 사용합니다.

* [소개](#chapter331_1)
* [기초적인 노드 찾기](#chapter331_2)
  * [모든 노드 가져오기](#chapter331_2_1)
  * [특정 라벨의 모든 노드 가져오기](#chapter331_2_2)
  * [관련된 노드 가져오기](#chapter331_2_3)
  * [라벨을 활용하여 매칭하기](#chapter331_2_4)
* [관계 기본](#chpater331_3)
  * [나가는 관계](#chapter331_3_1)
  * [직접관계 및 변수](#chapter331_3_2)
  * [관계타입으로 매칭](#chapter331_3_3)
  * [다중 관계타입으로 매칭](#chapter331_3_4)
  * [관계타입으로 매칭하고 변수로 사용](#chapter331_3_5)
* [깊이있는 관계](#chapter331_4)
  * [특수문자를 사용한 관계타입](#chapter331_4_1)
  * [다중 관계](#chapter331_4_2)
  * [가변길이 관계](#chapter331_4_3)
  * [가변길이 관계의 관계변수](#chapter331_4_4)
  * [가변길이 경로에서 속성매칭](#chapter331_4_5)
  * [0 길이 경로](#chapter331_4_6)
  * [명명된 경로](#chapter331_4_7)
  * [양방향 관계 매칭](#chapter331_4_8)
* [최단경로](#chapter331_5)
  * [단일 최단경로](#chapter331_5_1)
  * [조건기반 최단경로](#chapter331_5_2)
  * [모든 최단경로](#chapter331_5_3)
* [ID로 노드 또는 관계조회](#chapter331_6)
  * [ID기준 노드](#chapter331_6_1)
  * [ID기준 관계](#chapter331_6_2)
  * [ID기준 다수 노드](#chapter331_6_3)
  
## 소개 {#chapter331_1}

## 기초적인 노드 찾기 {#chapter331_2}

### 모든 노드 가져오기 {#chapter331_2_1}

### 특정 라벨의 모든 노드 가져오기 {#chapter331_2_2}

### 관련된 노드 가져오기 {#chapter331_2_3}

### 라벨을 활용하여 매칭하기 {#chapter331_2_4}

## 관계 기본 {#chpater331_3}

### 나가는 관계 {#chapter331_3_1}

### 직접관계 및 변수 {#chapter331_3_2}

### 관계타입으로 매칭 {#chapter331_3_3}

### 다중 관계타입으로 매칭 {#chapter331_3_4}

### 관계타입으로 매칭하고 변수로 사용 {#chapter331_3_5}

## 깊이있는 관계 {#chapter331_4}

### 특수문자를 사용한 관계타입 {#chapter331_4_1}

### 다중 관계 {#chapter331_4_2}

### 가변길이 관계 {#chapter331_4_3}

### 가변길이 관계의 관계변수 {#chapter331_4_4}

### 가변길이 경로에서 속성매칭 {#chapter331_4_5}

### 0 길이 경로 {#chapter331_4_6}

### 명명된 경로 {#chapter331_4_7}

### 양방향 관계 매칭 {#chapter331_4_8}

## 최단경로 {#chapter331_5}

### 단일 최단경로 {#chapter331_5_1}

두개의 노드사이에 최댄경로를 쉽게 찾으려면 `shortesPath` 함수를 사용하시면 됩니다.

#### 쿼리

```cypher
MATCH (martin:Person { name: 'Martin Sheen' }),(oliver:Person { name: 'Oliver Stone' }), p = shortestPath((martin)-[*..15]-(oliver))
RETURN p
```

위 함수 의미는 두개의 노드간 최단거리를 찾는데 최대 길이는 15로 설정하였습니다. 괄호안에 시작 노드, 연결관계, 종료노드를 가지고 단일 경로를 정의하시면 됩니다. 최단경로를 찾을 때 관계타입, 관계길이, 방향 등의 특성을 사용할 수 있습니다. `shortestPath`함수가 있는 `MATCH` 절에 `WHERE`절이 포함되면, 관련된 조건은 `shortestPath`에 적용됩니다. 경로의 관계에 `none()`이나 `all()` 조건은 성능을 향상시키기 위한 탐색을 하는데 사용됩니다. ([3.7.6 최단경로 플랜](/chpater3/chpater3_7_6.md)를 참고하시기 바랍니다)

#### 쿼리결과

| p |
| :--- |
| `[Node[1]{name:"Martin Sheen"},:ACTED_IN[1]{role:"Carl Fox"},Node[5]{title:"Wall Street"},:DIRECTED[3]{},Node[3]{name:"Oliver Stone"}]` |
| **1 row** |

### 조건기반 최단경로 {#chapter331_5_2}

### 모든 최단경로 {#chapter331_5_3}

## ID로 노드 또는 관계조회 {#chapter331_6}

### ID기준 노드 {#chapter331_6_1}

ID를 기준으로 노드를 검색할때는 `id()` 함수를 조건식으로 사용하면 됩니다.

> Neo4j에서는 노드나 관계가 삭제되면 해당 내부 ID를 재사용합니다. 이 의미는 어플리케이션에서 Neo4j 내부 ID를 가지고 처리할 경우 다루기 어렵거나 실수가 유발될 수 있습니다. 그러므로 어플리케이션에서 생성한 ID를 가지고 처리하길 권장합니다.

#### 쿼리

```cypher
MATCH (n)
WHERE id(n)= 0
RETURN n
```

조건에 맞는 노드가 반환됩니다.

#### 쿼리결과

| n |
|:--|
| `Node[0]{name:"Charlie Sheen"}` |
| **1 row** |

### ID기준 관계 {#chapter331_6_2}

ID를 기준으로 관계를 검색할때는 `id()` 함수를 조건식으로 사용하면 됩니다.

이것 또한 권장되지 않습니다. Neo4j ID를 사용하는 것에 대한 정보는 [ID기준 노드](#chapter331_6_1)를 참고하시기 바랍니다.

#### 쿼리

```cypher
MATCH ()-[r]->()
WHERE id(r)= 0
RETURN r
```

관계중 ID가 `0`인 데이터가 반환됩니다.

#### 쿼리결과

| r |
| :--- |
| `:ACTED_IN[0]{role:"Bud Fox"}` |
| **1 row** |

### ID기준 다수 노드 {#chapter331_6_3}

`IN` 절을 활용해서 다수의 노드를 ID 기준으로 선택할 수 있습니다.

#### 쿼리

```cypher
MATCH (n)
WHERE id(n) IN [0, 3, 5]
RETURN n
```

`IN` 절에 있는 모든 노드를 반환합니다.

#### 쿼리결과

| n |
| :--- |
| `Node[0]{name:"Charlie Sheen"}` |
| `Node[3]{name:"Oliver Stone"}` |
| `Node[5]{title:"Wall Street"}` |
| **3 rows** |