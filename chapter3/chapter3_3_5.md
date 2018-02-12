# 3.3.5 WITH

> `WITH` 절은 하나의 결과를 다음의 시작점 또는 기준으로 연결할 수 있게 하여 쿼리의 부분을 함께 엮을 수 있게 합니다.

* [소개](#chapter335_1)
* [집계함수결과 필터링](#chapter335_2)
* [수집하기전 정렬하기](#chapter335_3)
* [경로검색의 분기제한](#chapter335_4)

## 3.3.5.1 소개 {#chapter335_1}

`WITH`를 활용하면 쿼리의 다음 부분에 결과를 전달하기 전에 잘 다룰 수 있게 합니다. 조작은 결과의 형태나 반환할 엔트리 수가 될 수 있습니다.

`WITH`의 일반적인 사용법중 하나는 다음의 `MATCH`절에 엔트리의 개수를 제한시켜 전달하는 것 입니다. `ORDER BY`와 `LIMIT`을 조합하면, 특정 기준으로부터 상위 X 개의 엔트리를 추출한 뒤 그래프에서 추가적인 데이터를 가져옵니다.

다른 활용방법은 집계결과를 필터링하는 것 입니다. `WITH`에 정의된 집계정보 다음에 `WHERE`절을 활용해서 조건절을 추가하는 것 입니다. 이 집계표현은 결과에 새로운 바인딩을 생성하게 됩니다. `WITH`는 `RETURN`과 같이 결과에 대하여 별칭을 지정할 수 있습니다.

`WITH`절은 또한 그래프에서 읽는 부분과 쓰는 부분을 분리할 수 있습니다. 쿼리의 모든 부분은 읽기전용 또는 쓰기전용입니다. 만약 쓰기부분부터 읽기부분으로 쿼리가 진행된다면, `WITH` 절을 활용하여 중간분기를 해야합니다.

![](https://neo4j.com/docs/developer-manual/current/images/WITH-1.svg)

## 3.3.5.2 집계함수결과 필터링 {#chapter335_2}

집계된 결과는 필터링을 하기 위하여 `WITH`절을 통해 전달되어야 합니다.

### 쿼리

```cypher
MATCH (david { name: 'David' })--(otherPerson)-->()
WITH otherPerson, count(*) AS foaf
WHERE foaf > 1
RETURN otherPerson.name
```

이름이 `David`인 사람에 대하여 연결된 관계가 1개 이상인 것을 조회합니다.

### 쿼리결과

| otherPerson.name |
| :--- |
| `"Anders"` |
| **1 row** |

## 3.3.5.3 수집하기전 정렬하기 {#chapter335_3}

수집하기전 결과를 정렬하여 정렬된 목록을 가져올 수 있습니다.

### 쿼리

```cypher
MATCH (n)
WITH n
ORDER BY n.name DESC LIMIT 3
RETURN collect(n.name)
```

이름을 역순으로 정렬하여 3명을 조회한 목록입니다.

### 쿼리결과

| collect(n.name) |
| :--- |
| `["Emil","David","Ceasar"]` |
| **1 row** |

## 3.3.5.4 경로검색의 분기제한 {#chapter335_4}

임의의 수로 제한검색을 위하여 경로를 매칭하고 특정수로 제한한 뒤 그 경로를 기반으로 매칭을 진행할 수 있습니다.

### 쿼리

```cypher
MATCH (n { name: 'Anders' })--(m)
WITH m
ORDER BY m.name DESC LIMIT 1
MATCH (m)--(o)
RETURN o.name
```

`Anders`부터 시작해서 연결된 모든 노드를 매칭한 뒤 이름순으로 정렬하고 그 중 가장 첫번째 결과에서 매칭된 모든 노드의 이름을 반환합니다.

### 쿼리결과

| o.name |
| :--- |
| `"Bossman"` |
| `"Anders"` |
| **2 rows** |
