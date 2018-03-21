# 3.3.7 WHERE

> `WHERE`절은 `MATCH`나 `OPTIONAL MATCH` 절에 제약조건을 추가하거나, `WITH` 절의 결과를 필터링 합니다.

* [소개](#chapter337_1)
* [기본적인 사용법](#chapter337_2)
  * [부울 연산자](#chapter337_2_1)
  * [노드 라벨 필터링](#chapter337_2_2)
  * [노드 속성 필터링](#chapter337_2_3)
  * [관계 속성 필터링](#chapter337_2_4)
  * [동적 노드 속성 필터링](#chapter337_2_5)
  * [속성 존재 확인](#chapter337_2_6)
* [문자열 매칭](#chapter337_3)
  * [문자열 시작부터 매칭](#chapter337_3_1)
  * [문자열 끝부터 매칭](#chapter337_3_2)
  * [문자열 부분 매칭](#chapter337_3_3)
  * [문자열 매칭 제외](#chapter337_3_4)
* [정규표현식](#chapter337_4)
  * [정규표현식 사용하기](#chapter337_4_1)
  * [정규표현식에 특수문자 사용](#chapter337_4_2)
  * [대소문자 구분없는 정규표현식](#chapter337_4_3)
* [`WHERE`안에 경로 패턴사용](#chapter337_5)
  * [패턴으로 필터](#chapter337_5_1)
  * [`NOT`을 사용한 패턴으로 필터](#chapter337_5_2)
  * [속성을 포함한 패턴으로 필터](#chapter337_5_3)
  * [관계 타입으로 필터](#chapter337_5_4)
* [목록](#chapter337_6)
  * [`IN` 연산자](#chapter337_6_1)
* [없는 속성과 값](#chapter337_7)
  * [속성이 없을 때 `false`로 하기](#chapter337_7_1)
  * [속성이 없을 때 `true`로 하기](#chapter337_7_2)
* [범위 사용](#chapter337_8)
  * [단순 범위](#chapter337_8_1)
  * [복합 범위](#chapter337_8_2)
  
## 3.3.7.1 소개 {#chapter337_1}

`WHERE` 절은 단독으로 사용하는 것이 아닌, `MATCH`, `OPTIONAL MATCH`, `START`, `WITH` 절의 부분으로 사용됩니다.

`WITH`와 `START`의 경우 `WHERE`는 결과를 필터링 해주는 역할을 합니다.

그와 반대로 `MATCH`와 `OPTIONAL MATCH`에서 `WHERE` 절은 패턴이 표현할 제약조건을 추가하는 것입니다. 매칭이 끝난 다음에 필터링하는 것으로 보이지 않습니다.

> `MATCH`와 `OPTIONAL MATCH` 절이 다중으로 있는 경우, `WHERE`의 제약조건은 항상 바로 전에 있는 `MATCH`와 `OPTIONAL MATCH` 절의 부분으로 활용됩니다. 잘못된 `MATCH` 절에 `WHERE` 절을 추가하면 결과에 성능에 악영향을 미치게 됩니다.

다음 그래프를 예시로 진행하겠습니다.

![](https://neo4j.com/docs/developer-manual/current/images/WHERE-1.svg)

## 3.3.7.2 기본적인 사용법 {#chapter337_2}

### 부울 연산자 {#chapter337_2_1}

`AND`, `OR`, `XOR`, `NOT`과 같은 부울 연산자를 사용할 수 있습니다. `null`과 함께 처리하는 방법은 [3.2.12 null 사용법](/chapter3/chapter3_2_12.md)을 참고하시기 바랍니다.

#### 쿼리

```cypher
MATCH (n)
WHERE n.name = 'Peter' XOR (n.age < 30 AND n.name = 'Tobias') OR NOT (n.name = 'Tobias' OR n.name = 'Peter')
RETURN n.name, n.age
```

#### 쿼리결과

| n.name | n.age |
| :--- | :--- |
| `"Andres"` | `36` |
| `"Tobias"` | `25` |
| `"Peter"` | `35` |
| **3 rows** ||

### 노드 라벨 필터링 {#chapter337_2_2}

노드를 라벨로 필터링하기 위해서는 `WHERE` 절에 `WHERE n:foo`와 같은 방법으로 라벨 제약조건을 사용하시면 됩니다.

#### 쿼리

```cypher
MATCH (n)
WHERE n:Swedish
RETURN n.name, n.age
```

**Swedish**라는 라벨을 가진 노드의 이름과 나이가 반환됩니다.

#### 쿼리결과

| n.name | n.age |
| :--- | :--- |
| `"Andres"` | `36` |
| **1 row** ||

### 노드 속성 필터링 {#chapter337_2_3}

노드의 속성을 필터링하려면, `WHERE` 절 이후에 해당 조건을 사용하시면 됩니다.

#### 쿼리

```cypher
MATCH (n)
WHERE n.age < 30
RETURN n.name, n.age
```

30세 미만이기 때문에 **Tobias** 노드의 이름과 값이 반환됩니다.

#### 쿼리결과

| n.name | n.age |
| :--- | :--- |
| `"Tobias"` | `25` |
| **1 row** ||

### 관계 속성 필터링 {#chapter337_2_4}

관계의 속성을 필터링하려면, `WHERE` 절 이후에 해당 조건을 사용하시면 됩니다.

#### 쿼리

```cypher
MATCH (n)-[k:KNOWS]->(f)
WHERE k.since < 2000
RETURN f.name, f.age, f.email
```

**Andres**가 2000년 이전부터 알았기 때문에, **Peter** 노드의 이름, 나이, 이메일 정보가 반환됩니다.

#### 쿼리결과

| f.name | f.age | f.email |
| :--- | :--- | :--- |
| `"Peter"` | `35` | `"peter_n@example.com"` |
| **1 row** |||

### 동적 노드 속성 필터링 {#chapter337_2_5}

동적으로 생성된 이름으로 속성을 필터링하려면, 대괄호 문법을 사용하면 됩니다.

#### 쿼리

```cypher
WITH 'AGE' AS propname
MATCH (n)
WHERE n[toLower(propname)]< 30
RETURN n.name, n.age
```

30세 미만이기 때문에 **Tobias**의 이름 나이가 반환됩니다.

#### 쿼리결과

| n.name | n.age |
| :--- | :--- |
| `"Tobias"` | `25` |
| **1 row** ||

### 속성 존재 확인 {#chapter337_2_6}

특정 속성이 노드나 관계에 있는지 확인하려면 `exists()` 함수를 사용하면 됩니다.

#### 쿼리

```cypher
MATCH (n)
WHERE exists(n.belt)
RETURN n.name, n.belt
```

유일하게 `belt` 속성을 가지고 있는 **Andres** 노드만 이름과 벨트 정보가 반환됩니다.

> `has()` 함수는 `exists()` 함수로 대체되었습니다.

#### 쿼리결과

| n.name | n.belt |
| :--- | :--- |
| `"Andres"` | `"white"` |
| **1 row** ||

## 3.3.7.3 문자열 매칭 {#chapter337_3}

문자열의 시작과 끝은 `STARTS WITH`와 `ENDS WITH`로 매칭할 수 있습니다. 위치에 상관없는 포함여부는 `CONTAINS`를 사용합니다. 각각의 매칭은 대소문자를 구분합니다.

### 문자열 시작부터 매칭 {#chapter337_3_1}

`STARTS WITH` 연산자는 대소문자를 구분하는 문자열 시작부분 매칭방법입니다.

#### 쿼리

```cypher
MATCH (n)
WHERE n.name STARTS WITH 'Pet'
RETURN n.name, n.age
```

**Pet**로 이름이 시작하는 **Peter** 노드의 이름과 나이가 반환됩니다.

#### 쿼리결과

| n.name | n.age |
| :--- | :--- |
| `"Peter"` | `35` |
| **1 row** ||

### 문자열 끝부터 매칭 {#chapter337_3_2}

`ENDS WITH` 연산자는 대소문자를 구분하는 문자열 끝부분 매칭방법입니다.

#### 쿼리

```cypher
MATCH (n)
WHERE n.name ENDS WITH 'ter'
RETURN n.name, n.age
```

**ter**로 이름이 끝나는 **Peter** 노드의 이름과 나이가 반환됩니다.

#### 쿼리결과

| n.name | n.age |
| :--- | :--- |
| `"Peter"` | `35` |
| **1 row** ||

### 문자열 부분 매칭 {#chapter337_3_3}

`CONTAINS` 연산자는 대소문자를 구분하는 위치에 상관없는 문자열 매칭방법 입니다.

#### 쿼리

```cypher
MATCH (n)
WHERE n.name CONTAINS 'ete'
RETURN n.name, n.age
```

**ete**가 이름에 포함되는 **Peter** 노드의 이름과 나이가 반환됩니다.

#### 쿼리결과

| n.name | n.age |
| :--- | :--- |
| `"Peter"` | `35` |
| **1 row** ||

### 문자열 매칭 제외 {#chapter337_3_4}

주어진 문자열 매칭을 결과에서 제외하려면 `NOT` 키워드를 사용하면 됩니다.

#### 쿼리

```cypher
MATCH (n)
WHERE NOT n.name ENDS WITH 's'
RETURN n.name, n.age
```

**s**가 이름에 포함되지 않는 **Peter** 노드의 이름과 나이가 반환됩니다.

#### 쿼리결과

| n.name | n.age |
| :--- | :--- |
| `"Peter"` | `35` |
| **1 row** ||

## 3.3.7.4 정규표현식 {#chapter337_4}

Cypher에는 정규표현식을 사용한 필터링을 지원합니다. 정규표현식은 [the java regular expressions](https://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html)에 따릅니다. 대소문자구분안함 `(?i)`, 다중행 `(?m)`, 마침점 `(?s)`와 같은 문자열 매칭에 필요한 플래그 또한 지원합니다. 플래그는 정규표현식의 가장 앞에 사용됩니다. 예로들면, `MATCH (n) WHERE n.name =~ '(?i)Lon.*' RETURN n`은 **London**과 **LonDoN** 이름을 가진 노드 모두 반환됩니다.

### 정규표현식 사용하기 {#chapter337_4_1}

`=~ 'regexp'` 문법을 사용하여 정규표현식으로 매칭할 수 있습니다.

#### 쿼리

```cypher
MATCH (n)
WHERE n.name =~ 'Tob.*'
RETURN n.name, n.age
```

**Tob**으로 이름이 시작하기 때문에 **Tobias** 노드의 이름과 나이가 반환됩니다.

#### 쿼리결과

| n.name | n.age |
| :--- | :--- |
| `"Tobias"` | `25` |
| **1 row** |

### 정규표현식에 특수문자 사용 {#chapter337_4_2}

`.`이나 `*`와 같은 문자는 정규표현식에 특수한 의미를 가지고 있습니다. 이것을 의미없이 문자 자체로만 사용할 경우 Escaping 처리를 해야합니다.

#### 쿼리

```cypher
MATCH (n)
WHERE n.email =~ '.*\\.com'
RETURN n.name, n.age, n.email
```

이메일의 끝이 **.com**으로 끝나기 때문에 **Peter**의 이름, 나이, 이메일 정보가 반환됩니다.

#### 쿼리결과

| n.name | n.age | n.email |
| :--- | :--- | :--- |
| `"Peter"` | `35` | `"peter_n@example.com"` |
| **1 row** |||

### 대소문자 구분없는 정규표현식 {#chapter337_4_3}

정규표현식에 `(?i)`를 앞에 붙이면, 대소문자구분없이 처리하게 됩니다.

#### 쿼리

```cypher
MATCH (n)
WHERE n.name =~ '(?i)ANDR.*'
RETURN n.name, n.age
```

대소문자에 관계없이 **ANDR**로 이름이 시작하는 **Andres**의 이름과 나이가 반환됩니다.

#### 쿼리결과

| n.name | n.age |
| :--- | :--- |
| `"Andres"` | `36` |
| **1 row** ||

## 3.3.7.5 `WHERE`안에 경로 패턴사용 {#chapter337_5}

### 패턴으로 필터 {#chapter337_5_1}

### `NOT`을 사용한 패턴으로 필터 {#chapter337_5_2}

### 속성을 포함한 패턴으로 필터 {#chapter337_5_3}

### 관계 타입으로 필터 {#chapter337_5_4}

## 3.3.7.6 목록 {#chapter337_6}

### `IN` 연산자 {#chapter337_6_1}

## 3.3.7.7 없는 속성과 값 {#chapter337_7}

### 속성이 없을 때 `false`로 하기 {#chapter337_7_1}

###속성이 없을 때 `true`로 하기 {#chapter337_7_2}

## 3.3.7.8 범위 사용 {#chapter337_8}

### 단순 범위 {#chapter337_8_1}

요소가 특정 범위에 있는지 확인하기 위해서는, `<`, `<=`, `>=`, `>` 연산자를 사용하시면 됩니다.

#### 쿼리

```cypher
MATCH (a)
WHERE a.name >= 'Peter'
RETURN a.name, a.age
```

**Peter** 보다 문자열적으로 크거나 같은 속성을 가진 노드의 이름과 나이가 반환됩니다.

#### 쿼리결과

| a.name | a.age |
| :--- | :--- |
| `"Tobias"` | `25` |
| `"Peter"` | `35` |
| **2 rows** ||

### 복합 범위 {#chapter337_8_2}

서로다른 범위 조건을 동시에 여러개 사용할 수 있습니다.

#### 쿼리

```cypher
MATCH (a)
WHERE a.name > 'Andres' AND a.name < 'Tobias'
RETURN a.name, a.age
```

문자열적으로 **Andres**와 **Tobias** 사이에 있는 노드의 이름과 나이가 반환됩니다.

#### 쿼리결과

| a.name | a.age |
| :--- | :--- |
| `"Peter"` | `35` |
| **1 row** ||