# 3.1.2 그래프 질의하기 및 갱신하기

Cypher를 이용하여 그래프에 대한 질의 및 갱신을 할 수 있습니다.

## 3.1.2.1 갱신 쿼리 구조

* Cypher 쿼리 부분에서는 동시에 그래프를 매치하거나 갱신할 수 없습니다.
* 각각의 부분은 그래프를 읽고 매치하거나 그래프를 갱신할 수 있습니다.

만약 그래프를 읽고 그래프를 갱신하고 싶다면 쿼리는 2개의 부분으로 나뉘어집니다 - 먼저 읽고 그다음 쓰기를 합니다.

쿼리가 단순히 읽기만 수행할 경우 Cypher는 실행을 보류하고 결과를 요청할 때 실행합니다. 갱신 쿼리의 경우, 의미상 쓰기가 발생하기전에 모든 읽기 행동을 완료합니다.

The only pattern where the query parts are implicit is when you first read and then write — any other order and you have to be explicit about your query parts. 쿼리의 각 부분을 `WITH` 절을 활용하여 분리할 수 있습니다. `WITH`절은 실행할 것과 실행된 것 간을 차단하여 이벤트간 구분선과 같은 역할을 합니다.

집합처리된 데이터에 대하여 필터하고 싶은 경우, 두개의 쿼리 부분으로 나눠야 합니다. - 첫번째는 집합처리, 두번째는 첫번째 결과에 대한 필터

```cypher
MATCH (n {name: 'John'})-[:FRIEND]-(friend)
WITH n, count(friend) AS friendsCount
WHERE friendsCount > 3
RETURN n, friendsCount
```

WITH 절을 사용하여 원하는 집합을 정의할 수 있고, Cypher가 필터링 하기전에 집합처리가 완료됩니다.

그래프를 집합처리한 데이터로 갱신하는 예제입니다.

```cypher
MATCH (n {name: 'John'})-[:FRIEND]-(friend)
WITH n, count(friend) AS friendsCount
SET n.friendsCount = friendsCount
RETURN n.friendsCount
```

메모리가 허용하는 한, 많은 쿼리 부분을 연결하여 수행할 수 있습니다.

## 3.1.2.2 데이터 반환하기

어느 쿼리든 결과를 반환할 수 있습니다. 쿼리가 단순히 읽기만 한다면, 데이터를 무조건 반환해야 합니다. 그렇지 않은 경우에는 유효한 Cypher 쿼리가 아닙니다.  갱신 쿼리는 별도로 데이터를 반환할 필요는 없지만, 반환도 가능합니다.

쿼리의 모든 부분에 대한 끝은 `RETURN` 절입니다. `RETURN` 절은 다른 쿼리의 부분이 아니며 쿼리 전체의 끝점을 지칭합니다. `RETURN` 절은 `SKIP` / `LIMIT` / `ORDER BY` 3개의 부분절을 가지고 있습니다.

만약 삭제된 그래프 항목을 반환할 경우, 더이상 유효한 위치를 가리키고 있지 않기 때문에 해당 노드에 대한 추가적인 동작은 적용되지 않습니다.

