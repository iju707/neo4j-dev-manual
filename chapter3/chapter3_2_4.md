# 3.2.4 파라미터

* [소개](#chapter3241)
* [문자 literal](#chapter3242)
* [정규식](#chapter3243)
* [대소문자구분 문자열 비교](#chapter3244)
* [속성포함 노드 생성](#chapter3245)
* [속성포함 다수 노드 생성](#chapter3246)
* [노드의 모든 속성 설정](#chapter3247)
* [`SKIP`과 `LIMIT`](#chapter3248)
* [노드 Id](#chapter3249)
* [다수의 노드 Id](#chapter32410)
* [프로시져 호출](#chapter32411)
* [인덱스 값(특정 인덱스 지칭)](#chapter32412)
* [인덱스 쿼리(특정 인덱스 지칭)](#chapter32413)

## 3.2.4.1 소개 {#chapter3241}

Cypher는 쿼리에 파라미터를 지원합니다. 이 의미는 개발자가 원하는 쿼리를 만들기 위해 문자열생성 작업을 하지 않아도 됩니다. 또한, 파라미터는 Cypher에게 실행 계획을 캐싱할 수 있도록 도와줘서 쿼리 실행시간이 더 빨라지도록 합니다.

파라미터를 다음에 이용할 수 있습니다.

* literal과 표현식
* 노드와 관계 Id
* 특정 인덱스 지칭 (인덱스 값 또는 쿼리)

파라미터는 쿼리 계획에 사용될 쿼리 일부분으로 사용될 수 는 없습니다.

* 속성키 : 예로, `MATCH (n) WHERE n.$param = 'something'`은 부적절합니다.
* 관계 유형
* 라벨

파라미터의 이름은 문자나 숫자 또는 그 조합으로 사용할 수 있지만 숫자나 통화기호로 시작할 순 없습니다.

Neo4j REST API에서의 파라미터 사용법은 [5.1 트랜잭션 Cypher HTTP 종점](/chapter5/chapter5_1.md)를 참고하시기 바랍니다.

> 파라미터를 사용할 때 기존 문법(추후에 삭제될 예정)인 `{param}`이 아닌 신규 문법인 `$param` 방식을 사용하길 권장합니다.

## 3.2.4.2 문자 literal {#chapter3242}

### 파라미터

```json
{
  "name" : "Johan"
}
```

### 쿼리

```cypher
MATCH (n:Person)
WHERE n.name = $name
RETURN n
```

파라미터를 다음 문법에서도 사용 가능합니다.

### 파라미터

```json
{
  "name" : "Johan"
}
```

### 쿼리

```cypher
MATCH (n:Person { name: $name })
RETURN n
```

## 3.2.4.3 정규식 {#chapter3243}

### 파라미터

```json
{
  "regex" : ".*h.*"
}
```

### 쿼리

```cypher
MATCH (n:Person)
WHERE n.name =~ $regex
RETURN n.name
```

## 3.2.4.4 대소문자구분 문자열 비교 {#chapter3244}

### 파라미터

```json
{
  "name" : "Michael"
}
```

### 쿼리

```cypher
MATCH (n:Person)
WHERE n.name STARTS WITH $name
RETURN n.name
```

## 3.2.4.5 속성포함 노드 생성 {#chapter3245}

### 파라미터

```json
{
  "props" : {
    "name" : "Andres",
    "position" : "Developer"
  }
}
```

### 쿼리

```
CREATE ($props)
```

## 3.2.4.6 속성포함 다수 노드 생성 {#chapter3246}

### 파라미터

```json
{
  "props" : [ {
    "awesome" : true,
    "name" : "Andres",
    "position" : "Developer"
  }, {
    "children" : 3,
    "name" : "Michael",
    "position" : "Developer"
  } ]
}
```

### 쿼리

```cypher
UNWIND $props AS properties
CREATE (n:Person)
SET n = properties
RETURN n
```

## 3.2.4.7 노드의 모든 속성 설정 {#chapter3247}

이것은 현재 속성을 모두 대체할 것 입니다.

### 파라미터

```json
{
  "props" : {
    "name" : "Andres",
    "position" : "Developer"
  }
}
```

### 쿼리

```cypher
MATCH (n:Person)
WHERE n.name='Michaela'
SET n = $props
```

## 3.2.4.8 `SKIP`과 `LIMIT` {#chapter3248}

### 파라미터

```json
{
  "s" : 1,
  "l" : 1
}
```

### 쿼리

```cypher
MATCH (n:Person)
RETURN n.name
SKIP $s
LIMIT $l
```

## 3.2.4.9 노드 Id {#chapter3249}

### 파라미터

```json
{
  "id" : 0
}
```

### 쿼리 

```cypher
MATCH (n)
WHERE id(n)= $id
RETURN n.name
```

## 3.2.4.10 다수의 노드 Id {#chapter32410}

### 파라미터

```json
{
  "ids" : [ 0, 1, 2 ]
}
```

### 쿼리 

```cypher
MATCH (n)
WHERE id(n) IN $ids
RETURN n.name
```

## 3.2.4.11 프로시져 호출 {#chapter32411}

### 파라미터

```json
{
  "indexname" : ":Person(name)"
}
```

### 쿼리

```cypher
CALL db.resampleIndex($indexname)
```

## 3.2.4.12 인덱스 값 (특정 인덱스 지칭) {#chapter32412}

### 파라미터

```json
{
  "value" : "Michaela"
}
```

### 쿼리

```cypher
START n=node:people(name = $value)
RETURN n
```

## 3.2.4.13 인덱스 쿼리 (특정 인덱스 지칭) {#chapter32413}

### 파라미터

```json
{
  "query" : "name:Andreas"
}
```

### 쿼리

```cypher
START n=node:people($query)
RETURN n
```