# 챕터 3. Cypher

> 이번 챕터는 Cypher 쿼리 언어에 대한 전체적인 내용을 다룹니다.

## 3.1 소개

짧은 소개는 [섹션 3.1.1, "Cypher란 무엇인가?"](#311-cypher란-무엇인가)를 보시면 됩니다. Cypher를 사용하기 위한 첫단계는 [섹션 2.2, "Cypher로 시작하기"](/chapter2/chapter2_2.md)를 보시면 됩니다. 관련 용어는 [참조 B, 용어](/appendixB.md)를 확인하시기 바랍니다.

## 3.1.1 Cypher란 무엇인가?

### 구조

Cypher는 SQL로부터 구조를 차용하였습니다 - 쿼리는 다양한 Clause\(이하 절\)들을 가지고 구성됩니다.

절는 서로 묶여있으며, 각각의 중간결과를 서로 공유합니다. 예로들어, 하나의 `MATCH` 절로부터의 결과값은 다음 절에 존재하게 됩니다.

쿼리 언어는 몇 가지의 절로 구성이 되며, 각각의 자세한 내용은 다음 절에서 확인할 수 있습니다.

그래프로부터 읽는 몇몇의 절를 보겠습니다.

* `MATCH` : 일치하는 값을 찾는 그래프 패턴. 그래프로부터 데이터를 가져오는 가장 일반적인 방법입니다.
* `WHERE` : 단독으로 사용할 수 는 없고 `MATCH`, `OPTIONAL MATCH`, `WITH` 구문과 함께 사용됩니다. 패턴에 대한 제약조건을 추가하거나 `WITH` 절을 통하여 생성된 결과를 필터하는데 사용됩니다.
* `RETURN` : 반환하고자 하는 것을 정의

실제 `MATCH`와 `RETURN`을 살펴보겠습니다.

예로 사용할 그래프는 다음과 같습니다.

![](https://neo4j.com/docs/developer-manual/3.3/images/Example-Graph-cypher-intro.svg)

예로, John이라고 불리는 사용자와 John의 친구들\(단, 직접적인 친구관계 제외\)를 찾고 John과 그의 친구의 친구\(friends-of-friends\)를 반환하는 것 입니다.

```
MATCH (john {name: 'John'})-[:friend]->()-[:friend]->(fof)
RETURN john.name, fof.name
```

결과는 다음과 같습니다.

```
+----------------------+
| john.name | fof.name |
+----------------------+
| "John"    | "Maria"  |
| "John"    | "Steve"  |
+----------------------+
2 rows
```

다음은 더 다양하게 하기 위하여 필터를 추가해보겠습니다.

목록에 해당하는 이름을 가지고, 그의 친구에 대한 이름이 "S."으로 시작하는 사용자에 대한 사용자 이름과 친구의 이름을 반환하는 예제입니다.

```
MATCH (user)-[:friend]->(follower)
WHERE user.name IN ['Joe', 'John', 'Sara', 'Maria', 'Steve'] AND follower.name =~ 'S.*'
RETURN user.name, follower.name
```

결과는 다음과 같습니다.

```
+---------------------------+
| user.name | follower.name |
+---------------------------+
| "Joe"     | "Steve"       |
| "John"    | "Sara"        |
+---------------------------+
2 rows
```

추가로 그래프에 대한 정보를 업데이트하는 몇 가지 절은 다음과 같습니다.

* `CREATE` \(와 `DELETE`\) : 노드와 관계를 생성\(또는 삭제\)
* `SET` \(와 `REMOVE`\) : `SET`을 활용하여 노드에 대한 속성값을 설정과 라벨을 추가. 또는 `REMOVE`를 이용하여 그것을 삭제.
* `MERGE` : 노드와 패턴에 대하여 기존 것을 조회하거나 신규로 생성. 유일성에 대한 제약조건을 활용할 때 유용합니다.



