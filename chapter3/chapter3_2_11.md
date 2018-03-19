# 3.2.11 맵

> Cypher는 맵구조를 완벽하게 지원합니다.

* [맵 문법](#chapter3211_1)
* [맵 투영](#chapter3211_2)
  * [맵 투영 예제](#chapter3211_2_1)
  
아래의 예제에서 사용할 그래프입니다.

![](https://neo4j.com/docs/developer-manual/current/images/Maps-1.svg)
  
## 3.2.11.1 맵 문법 {#chapter3211_1}

Cypher에서 맵구조를 만들 수 있습니다. REST를 통해서는 JSON 객체로 가져올 수 있고, Java에서는 `java.util.Map<String,Object>`로 가져올 수 있습니다.

### 쿼리

```cypher
RETURN { key: 'Value', listKey: [{ inner: 'Map1' }, { inner: 'Map2' }]}
```

### 쿼리결과

| { key: 'Value', listKey: [{ inner: 'Map1' }, { inner: 'Map2' }]} |
| :--- |
| `{listKey -> [{inner -> "Map1"},{inner -> "Map2"}], key -> "Value"}` |
| **1 row** |

## 3.2.11.2 맵 투영 {#chapter3211_2}

Cypher는 "맵 투영(map projections)"라는 개념을 지원합니다. 이것은 노드, 관계, 그리고 다른 맵을 가지고 쉽게 맵을 구성할 수 있게 합니다.

맵 투영은 투영될 그래프 엔티티에 반영할 변수명으로 시작하며, `{`와 `}`로 둘러쌓인 쉼표로 구분되는 맵 항목으로 구성됩니다.

```cypher
map_variable {map_element, [, …​n]}
```

맵 요소는 키-값 쌍으로 하나 이상 투영됩니다. 서로다른 4가지 유형의 맵 투영 항목이 있습니다.

* 속성 선택자 - 키는 속성의 이름, 값은 투영에 사용되는 `map_variable`의 속성 값
* 문법 엔트리 - `key: <expression>`의 방식으로 구성된 단순 키-값 쌍
* 변수 선택자 - 키는 변수 이름, 값은 실제 변수가 가리키는 항목. 구문은 변수만 쓰면 됩니다.
* 모든 속성 선택자 - `map_variable` 값에서 모든 키-값 쌍을 투영합니다.

`map_variable`이 `null` 값일 경우에는 투영되는 맵 또한 `null`입니다.

### 맵 투영 예제 {#chapter3211_2_1}

**Charlie Sheen**을 찾고 그와 그가 출현했던 영화에 대한 정보를 반환하겠습니다. 이 예제는 `collect()`라는 집계함수를 통하여 맵이 투영되는 문법 엔트리를 보여줍니다.

#### 쿼리

```cypher
MATCH (actor:Person { name: 'Charlie Sheen' })-[:ACTED_IN]->(movie:Movie)
RETURN actor { .name, .realName, movies: collect(movie { .title, .year })}
```

#### 쿼리결과

| actor { .name, .realName, movies: collect(movie { .title, .year })} |
| :--- |
| `{name -> "Charlie Sheen", movies -> [{title -> "Apocalypse Now", year -> 1979},{title -> "Red Dawn", year -> 1984},{title -> "Wall Street", year -> 1987}], realName -> "Carlos Irwin Estévez"}` |
| **1 row** |

영화에 출연한 모든 사람을 찾고 출연횟수를 보여주겠습니다. 이번 예제에서는 `count`를 사용한 변수와 값을 투영하기 위한 변수 선택자를 사용하는 것을 보여줍니다.

#### 쿼리

```cypher
MATCH (actor:Person)-[:ACTED_IN]->(movie:Movie)
WITH actor, count(movie) AS nrOfMovies
RETURN actor { .name, nrOfMovies }
```

#### 쿼리결과

| actor { .name, nrOfMovies } |
| :--- |
| `{name -> "Martin Sheen", nrOfMovies -> 2}` |
| `{name -> "Charlie Sheen", nrOfMovies -> 3}` |
| **2 rows** |

다시, **Charlie Sheen**에서 노드의 모든 속성을 투영해보도록 하겠습니다. 여기서는 노드 속성 모두를 투영하기 위한 모든 속성 선택자를 사용하고, 추가로 `age`라는 속성을 투영하도록 명시하였습니다. 하지만 `age`속성은 노드에 존재하지 않기 때문에 `null`의 값이 투영될 것 입니다.

#### 쿼리

```cypher
MATCH (actor:Person { name: 'Charlie Sheen' })
RETURN actor { .*, .age }
```

#### 쿼리결과

| actor { .*, .age } |
| :--- |
| `{name -> "Charlie Sheen", realName -> "Carlos Irwin Estévez", age -> <null>}` |
| **1 row** |