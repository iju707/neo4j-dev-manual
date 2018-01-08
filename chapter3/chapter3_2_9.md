# 3.2.9 맵

> Cypher는 맵구조를 완벽하게 지원합니다.

* [맵 문법](#chapter3291)
* [맵 투영](#chapter3292)
  * [맵 투영 예제](#chapter32921)
  
아래의 예제에서 사용할 그래프입니다.

![](https://neo4j.com/docs/developer-manual/current/images/Maps-1.svg)
  
## 3.2.9.1 맵 문법 {#chapter3291}

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

## 3.2.9.2 맵 투영 {#chapter3292}

Cypher는 "맵 투영(map projections)"라는 개념을 지원합니다. 이것은 노드, 관계, 그리고 다른 맵을 가지고 쉽게 맵을 구성할 수 있게 합니다.

맵 투영은 투영될 그래프 엔티티에 반영할 변수명으로 시작하며, `{`와 `}`로 둘러쌓인 쉼표로 구분되는 맵 항목으로 구성됩니다.

```cypher
map_variable {map_element, [, …​n]}
```

맵 요소는 키-값 쌍으로 하나 이상 투영됩니다. 서로다른 4가지 유형의 맵 투영 항목이 있습니다.

* 속성 선택자 - 키는 속성의 이름, 값은 투영에 사용되는 `map_variable`의 속성 값
* 문법 엔트리 - `key: <expression>`의 방식으로 구성된 단순 키-값 쌍
* 변수 선택자 - 키는 변수 이름, 값은 실제 변수가 가리키는 항목. 구문은 변수만 쓰면 됩니다.
*  All-properties selector - projects all key-value pairs from the map_variable value.

### 맵 투영 예제 {#chapter32921}