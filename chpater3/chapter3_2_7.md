# 3.2.7 패턴

* [소개](#chapter3271)
* [노드의 패턴](#chapter3272)
* [관련된 노드들의 패턴](#chapter3273)
* [라벨의 패턴](#chapter3274)
* [속성 지정](#chapter3275)
* [관계 패턴](#chapter3276)
* [가변길이 패턴 매칭](#chapter3277)
* [경로 변수 지정](#chapter3278)

## 소개 {#chapter3271}
  
## 노드의 패턴 {#chapter3272}

가장 단순한 형태로 표현되는 패턴이 노드입니다. 노드는 괄호를 사용하여 표현할 수 있으며 원하는 이름을 지정할 수 있습니다.

```cypher
(a)
```

## 관련된 노드들의 패턴 {#chapter3273}

보다 강력한 구조를 가진 패턴은 다수의 노드와 관계를 표현하는 것 입니다. Cypher 패턴에서 관계는 두개의 노드 간에 화살표를 가지고 표현합니다.

```cypher
(a)-->(b)
```

위 패턴은 가장 단순한 데이터를 표현합니다. 2개 노드, 하나에서 다른것으로 연결되는 1개의 관계. 이 예제에서 두개의 노드는 `a`와 `b`의 이름을 가지고 있으며, 관계는 `a`에서 `b`로 연결되는 방향성을 가지게 됩니다.

이러한 노드와 관계에 대한 표현은 임의의 숫자에 해당하는 노드와 관계에 대한 표현으로 확장이 가능합니다.

```cypher
(a)-->(b)<--(c)
```

이러한 연결된 노드와 관계를 가지고 "경로(path)"라고 부릅니다.

노드에 대한 이름은 동일한 노드에 대하여 Cypher 쿼리 다른 부분에서 참조가 있을 때만 이름을 지정하시면 됩니다. 만약 불필요할 경우에는 이름을 다음과 같이 생략하시면 됩니다.

```cypher
(a)-->()<--(c)
```

## 라벨의 패턴 {#chapter3274}

패턴중 가장 단순한 형태의 노드 이외에 속성도 간단히 표현할 수 있습니다. 그중 노드에 필수적인 라벨에 대한 것을 보겠습니다.

```cypher
(a:user)-->(b)
```

여러개의 라벨을 가지고 있으면 다음과 같이 표현됩니다.

```cypher
(a:user:Admin)-->(b)
```

## 속성 지정 {#chapter3275}

노드와 관계는 그래프의 기본 구조입니다. Neo4j에서는 좀더 풍부한 모델을 제공하기 위하여 두가지 모두 속성정보를 제공합니다.

속성은 맵과 같은 구조의 패턴으로 표현될 수 있습니다. 중괄호로 둘러싸인 다수의 키-값 쌍을 쉼표로 구분하여 표현합니다. 만약 노드가 2가지의 속성을 가지고 있으면 다음과 같습니다.

```cypher
(a {name: 'Andres', sport: 'Brazilian Ju-Jitsu'})
```

관계가 속성을 가질 경우에는 다음과 같습니다.

```cypher
(a)-[{blocked: false}]->(b)
```

패턴에 속성들이 보이게 되면, 데이터에 제약조건을 추가하는 것 입니다. CREATE 절에서는 새로 생성되는 노드나 관계에 설정할 속성이 됩니다. MERGE 절에서는 기존 데이터가 가지고 있어야될 추가적인 제약조건(그래프의 존재하는 데이터 중 정확히 일치할 특정 속성)으로 표현됩니다. 만약 일치하는 데이터가 없을 경우 MERGE는 CREATE절과 동일하게 새로 생성하는 노드나 관계에 설정될 속성으로 사용합니다.

CREATE절에서는 특정한 속성들을 한개의 파라미터로 사용할 수 있습니다. (예 : `CREATE (node $paramName)`) 이 패턴을 다른 절에서는 사용할 수 없습니다. Cypher는 매칭을 좀더 효율적으로 하기 위하여 쿼리를 컴파일할 때 속성에 대한 이름을 알아야 하기 때문입니다.

## 관계 패턴 {#chapter3276}

관계를 가장 단순하게 표현하는 것은 이전 예제에서 소개한 것과 같이 2개의 노드간에 화살표로 표시하는 것 입니다. 이 방법을 이용하면 관계가 있는지, 어떤 방향성을 가지고 있는지 표현할 수 있습니다. 만약 관계에 대한 방향을 고려하지 않는다면, 화살표 방향을 생략하시면 됩니다.

```cypher
(a)--(b)
```

노드처럼 관계도 이름을 지정할 수 있습니다. 이 경우에는 화살표 가운데에 대괄호를 추가하고 이름을 지정하시면 됩니다.

```cypher
(a)-[r]->(b)
```

노드의 라벨과 유사하게 관계도 타입을 가지게 됩니다. 관계에 대한 타입을 표현할 경우에는 아래와 같이 하시면 됩니다.

```cypher
(a)-[r:REL_TYPE]->(b)
```

Unlike labels, relationships can only have one type. But if we’d like to describe some data such that the relationship could have any one of a set of types, then they can all be listed in the pattern, separating them with the pipe symbol | like this:

```cypher
(a)-[r:TYPE1|TYPE2]->(b)
```

Note that this form of pattern can only be used to describe existing data (ie. when using a pattern with MATCH or as an expression). It will not work with CREATE or MERGE, since it’s not possible to create a relationship with multiple types.

As with nodes, the name of the relationship can always be omitted, as exemplified by:

```cypher
(a)-[:REL_TYPE]->(b)
```

## 가변길이 패턴 매칭 {#chapter3277}

## 경로 변수 지정 {#chapter3278}

노드와 관계에 대한 연결된 정보를 "경로(path)"라고 부릅니다. Cypher에서는 경로에 대한 이름을 지정할 수 있게 합니다.

```cypher
p = (a)-[*3..5]->(b)
```

`MATCH`, `CREATE`, `MERGE`에서 사용할 수 있지만, 표현식으로는 사용할 수 없습니다.