# 3.3.4 RETURN

> `RETURN` 절은 쿼리 결과 집합에 어떤 것을 포함하는지 정의합니다.

* [소개](#chapter334_1)
* [노드 반환](#chapter334_2)
* [관계 반환](#chapter334_3)
* [속성 반환](#chapter334_4)
* [모든 요소 반환](#chapter334_5)
* [특수문자를 사용한 변수](#chapter334_6)
* [컬럼 별칭](#chapter334_7)
* [선택적 속성](#chapter334_8)
* [기타 표현식](#chapter334_9)
* [결과 중복제거](#chapter334_10)

## 3.3.4.1 소개 {#chapter334_1}

쿼리의 `RETURN` 부분에 관심있어하는 패턴의 부분을 정의합니다. 노드나 관계 또는 그것의 속성이 될 수 있습니다.

> 만약 속성의 값만 원한다면, 노드나 관계의 전체를 반환하지 마시기 바랍니다. 성능향상에 도움이 됩니다.

아래에 사용될 그래프입니다.

![](https://neo4j.com/docs/developer-manual/current/images/RETURN-1.svg)

## 3.3.4.2 노드 반환 {#chapter334_2}

노드를 반환하려면 `RETURN`에 해당 노드를 추가하시면 됩니다.

### 쿼리

```cypher
MATCH (n { name: 'B' })
RETURN n
```

다음 예제는 노드를 반환할 것 입니다.

### 쿼리결과

| n |
| :--- |
| `Node[1]{name:"B"}` |
| **1 row** |

## 3.3.4.3 관계 반환 {#chapter334_3}

관계를 반환하려면 `RETURN`에 해당 관계를 추가하시면 됩니다.

### 쿼리

```cypher
MATCH (n { name: 'A' })-[r:KNOWS]->(c)
RETURN r
```

다음 예제는 관계를 반환할 것 입니다.

### 쿼리결과

| r |
| :--- |
| `:KNOWS[0]{}` |
| **1 row** |

## 3.3.4.4 속성 반환 {#chapter334_4}

속성을 반환하려면 점\(.\)을 사용하시면 됩니다.

### 쿼리

```cypher
MATCH (n { name: 'A' })
RETURN n.name
```

`name` 속성의 값이 반환됩니다.

### 쿼리결과

| n.name |
| :--- |
| `"A"` |
| **1 row** |

## 3.3.4.5 모든 요소 반환 {#chapter334_5}

쿼리에서 찾은 모든 노드, 관계, 경로를 반환할 경우에는 `*`을 사용하시면 됩니다.

### 쿼리

```cypher
MATCH p =(a { name: 'A' })-[r]->(b)
RETURN *
```

쿼리에서 사용된 2개의 노드와 관계, 경로가 반환됩니다.

### 쿼리결과

| a | b | p | r |
| :--- | :--- | :--- | :--- |
| `Node[0]{name:"A",happy:"Yes!",age:55}` | `Node[1]{name:"B"}` | `[Node[0]{name:"A",happy:"Yes!",age:55},:BLOCKS[1]{},Node[1]{name:"B"}]` | `:BLOCKS[1]{}` |
| `Node[0]{name:"A",happy:"Yes!",age:55}` | `Node[1]{name:"B"}` | `[Node[0]{name:"A",happy:"Yes!",age:55},:KNOWS[0]{},Node[1]{name:"B"}]` | `:KNOWS[0]{}` |
| **2 rows||||

## 3.3.4.6 특수문자를 사용한 변수 {#chapter334_6}

영문 알파벳이 아닌 문자를 포함하여 변수명을 사용할 경우에는 변수명에 ` ` `으로 감싸시면 됩니다.

### 쿼리

```cypher
MATCH (`This isn\'t a common variable`)
WHERE `This isn\'t a common variable`.name = 'A'
RETURN `This isn\'t a common variable`.happy
```

노드의 이름이 "A"인 결과가 반환됩니다.

### 쿼리결과

| \`This isn\'t a common variable\`.happy |
| :--- |
| `"Yes!"` |
| **1 row** |

## 3.3.4.7 컬럼 별칭 {#chapter334_7}

표현식과 다르게 컬럼의 이름을 사용하고 싶은 경우에는 `AS <new name>` 방식으로 이름을 변경하시면 됩니다.

### 쿼리

```cypher
MATCH (a { name: 'A' })
RETURN a.age AS SomethingTotallyDifferent
```

노드의 나이 속성을 반환하지만 이름이 바뀌었습니다.

### 쿼리결과

| SomethingTotallyDifferent |
| :--- |
| `55` |
| **1 row** |

## 3.3.4.8 선택적 속성 {#chapter334_8}

## 3.3.4.9 기타 표현식 {#chapter334_9}

## 3.3.4.10 결과 중복제거 {#chapter334_10}



