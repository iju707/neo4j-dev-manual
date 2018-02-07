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

`MATCH` 절은 Neo4j에서 데이터베이스 검색을 위한 패턴입니다. 이것은 바인딩된 집합에 데이터를 가져오기 위한 핵심적인 방법입니다. [3.2.7 패턴](/chapter3/chapter3_2_7.md)을 활용해서 패턴을 구체화시키면 좀더 자세히 읽어올 수 있습니다.

`MATCH`절은 좀더 특정된 데이터를 가져오기 위해 자주 `WHERE` 절을 사용해서 제한점, 술어부 등을 추가하여 사용합니다. 술어부는 패턴을 표현하는데 일부이며, 매칭이 완료된 뒤 적용되는 필터로 간주하시면 안됩니다. 이 의미는 `WHERE`절은 항상 `MATCH`절과 함께 적용되어야 합니다.

`MATCH`는 쿼리의 시작 또는 `WITH` 다음으로 사용될 수 있습니다. 처음으로 사용되면, 바인딩 된 데이터가 없이 Neo4j는 `MATCH` 절과 `WHERE`절에 정의된 술어부를 적용해서 결과를 찾도록 구성을 합니다. 여기에는 데이터베이스를 스캔하고, 특정 라벨에 관련된 노드를 검색하고, 패턴 매칭의 시작점을 찾기 위해 인덱스를 검색하기도 합니다. 탐색에 의해 찾아진 노드와 관계는 바운드 패턴 요소로 사용가능하며, 패턴 매칭의 부분 그래프로도 사용이 가능합니다. 또한 나중의 처리하는 `MATCH`절에 Neo4j는 알려진 요소로 사용하여 이것으로부터 알려지지 않은 요소를 찾기도 합니다.

Cypher는 선언적이기 때문에 항상 쿼리가 특정 알고리즘을 사용하여 검색을 수행하진 않습니다. Neo4j에서 자동으로 최적의 시작노드를 찾고 패턴매칭을 진행합니다. `WHERE`절의 술어부는 패턴매칭전, 패턴매칭중, 매칭 결과를 찾은 후에도 검증이 진행될 수 있습니다. 그러나 자동으로 수행하지 않고 쿼리 컴파일러에 특정방법으로 수행하도록 제어할 수 도 있습니다. [3.5.1 인덱스](/chapter3/chapter3_5_1.md)에서 인덱스에 관련된 정보를 보시거나, [3.6.4 실행계획 힌트 와 USING 키워드](/chapter3/chatper3_6_4.md)에서 Neo4j의 쿼리를 처리하기 위한 특정 힌트를 사용하는 방법에 대해 읽어보시기 바랍니다.

> `MATCH` 절에 사용되는 패턴에 대한 자세한 정보는 [3.2.7 패턴](/chapter3/chapter3_2_7.md)을 보시기 바랍니다.

예제에 다음 그래프를 사용하겠습니다.

![](https://neo4j.com/docs/developer-manual/current/images/MATCH-3.svg)

## 기초적인 노드 찾기 {#chapter331_2}

### 모든 노드 가져오기 {#chapter331_2_1}

단일 노드에 라벨이 없는 패턴을 사용하면 그래프의 전체 노드를 가져올 수 있습니다.

#### 쿼리

```cypher
MATCH (n)
RETURN n
```

데이터베이스의 모든 노드를 반환합니다.

#### 쿼리결과

| n |
| :--- |
| `Node[0]{name:"Charlie Sheen"}` |
| `Node[1]{name:"Martin Sheen"}` |
| `Node[2]{name:"Michael Douglas"}` |
| `Node[3]{name:"Oliver Stone"}` |
| `Node[4]{name:"Rob Reiner"}` |
| `Node[5]{title:"Wall Street"}` |
| `Node[6]{title:"The American President"}` |
| **7 rows** |

### 특정 라벨의 모든 노드 가져오기 {#chapter331_2_2}

특정 라벨에 대한 모든 노드를 가져오는 방법은 단일노드패턴에 원하는 라벨을 지정하시면 됩니다.

#### 쿼리

```cypher
MATCH (movie:Movie)
RETURN movie.title
```

데이터베이스의 모든 Moive를 반환합니다.

#### 쿼리결과

| movie.title |
| :--- |
| `"Wall Street"` |
| `"The American President"` |
| **2 rows** |

### 관련된 노드 가져오기 {#chapter331_2_3}

`--` 기호는 관련이 있다는 표시입니다. 단, 관계의 타입이나 방향은 제외합니다.

#### 쿼리

```cypher
MATCH (director { name: 'Oliver Stone' })--(movie)
RETURN movie.title
```

**Oliver Stone**이 연출한 모든 영화가 반환될 것 입니다.

#### 쿼리결과

| movie.title |
| :--- |
| `"Wall Street"` |
| **1 row** |

### 라벨을 활용하여 매칭하기 {#chapter331_2_4}

노드에 대하여 특정라벨만 처리할 경우, 라벨문법을 패턴에 추가할 수 있습니다.

#### 쿼리

```cypher
MATCH (:Person { name: 'Oliver Stone' })--(movie:Movie)
RETURN movie.title
```

`Person`라벨의 **Oliver Stone** 이름을 가진 노드에 연결된 `Movie`라벨 노드를 조회합니다.

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