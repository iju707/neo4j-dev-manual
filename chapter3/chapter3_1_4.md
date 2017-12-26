# 3.1.4 고유성

패턴 일치에서 Neo4j는 동일한 그래프 관계에서 한가지 패턴으로 반복적으로 찾지 않기 위하여 확실하게 해야합니다. 일반적인 대부분의 경우 확실하게 할 수 있습니다.

예로, 사용자를 제외한 사용자의 친구의 친구를 검색하는 것 입니다.

예시에 사용할 노드와 관계를 생성하겠습니다.

```
CREATE (adam:User { name: 'Adam' }),(pernilla:User { name: 'Pernilla' }),(david:User { name: 'David' }),
(adam)-[:FRIEND]->(pernilla),(pernilla)-[:FRIEND]->(david)
```

입력한 정보는 다음 그림과 같습니다.

![](https://neo4j.com/docs/developer-manual/3.3/images/cypherdoc--13303421.svg)

Adam의 친구의 친구를 찾아보겠습니다.

```
MATCH (user:User { name: 'Adam' })-[r1:FRIEND]-()-[r2:FRIEND]-(friend_of_a_friend)
RETURN friend_of_a_friend.name AS fofName
```

```
+---------+
| fofName |
+---------+
| "David" |
+---------+
1 row
```

위 쿼리에서 관계를 `r1`, `r2`로 모두 표시하여 동일한 그래프 관계가 반환되지 않도록 하였습니다.

그렇지만 항상 모든 쿼리가 원하는대로 동작하지는 않습니다. 만약 사용자를 반환할 경우 MATCH 절을 다수개 사용할 수도 있습니다.

```
MATCH (user:User { name: 'Adam' })-[r1:FRIEND]-(friend)
MATCH (friend)-[r2:FRIEND]-(friend_of_a_friend)
RETURN friend_of_a_friend.name AS fofName
```

```
+---------+
| fofName |
+---------+
| "David" |
| "Adam"  |
+---------+
2 rows
```

다음 쿼리는 직전 쿼리와 유사하지만, 결과는 첫번째와 동일하게 반환하게 됩니다.

```
MATCH (user:User { name: 'Adam' })-[r1:FRIEND]-(friend),(friend)-[r2:FRIEND]-(friend_of_a_friend)
RETURN friend_of_a_friend.name AS fofName
```

MATCH 절에서 직전의 쿼리는 두개의 분리된 패턴을 사용했다면 여기는 두개의 경로를 하나의 패턴안에 사용하였습니다.

```
+---------+
| fofName |
+---------+
| "David" |
+---------+
1 row
```



