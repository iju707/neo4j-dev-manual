# 3.2.3 표현식

* [일반적인 표현식](#chapter323_1)
* [문자 literal 참고](#chapter323_2)
* [`CASE` 표현식](#chapter323_3)
  * [`CASE` 단순형 : 다수의 값을 비교](#chapter323_3_1)
  * [`CASE` 일반형 : 다수의 조건식을 비교](#chapter323_3_2)
  * [`CASE`의 단순형과 일반형 사용시기 구분](#chapter323_3_3)

## 3.2.3.1 일반적인 표현식 {#chapter323_1}

Cypher에서 가능한 표현식 입니다.

* 10진법 literal \(정수 또는 실수\) : `13`, `-40000`, `3.14`, `6.022E23`
* 16진법 literal \(`0x`로 시작\) : `0x13af`, `0xFC3A9`, `-0x66eff`
* 8진법 literal \(`0`으로 시작\) : `01372`, `02127`, `-05671`
* 문자 literal : `'Hello'`, `"World"`
* Boolean literal : `true`, `false`, `TRUE`, `FALSE`
* 변수 : `n`, `x`, `rel`, `myFancyVariable`, \`A name with weird stuff in it\[\]!\`
* 속성 : `n.prop`, `x.prop`, `rel.thisproperty`, myFancyVariable.\`\(weird property name\)\`
* 동적 변수 : `n["prop"]`, `rel[n.city + n.zip]`, `map[coll[0]]`
* 파라미터 : `$param`, `$0`
* 배열 : `['a', 'b']`, `[1, 2, 3]`, `['a', 2, n.property, $param]`, `[ ]`
* 함수 호출 : `length(p)`, `nodes(p)`
* 집계 함수 : `avg(x.prop)`, `count(*)`
* 경로 패턴 : `(a)-->()<--(b)`
* 연산식 : `1 + 2`, `3 < 4`
* 술어 표현 \(true 또는 false를 반환하는 표현식\) : `a.prop = 'Hello'`, `length(p) > 10`, `exists(a.name)`
* 정규식 : `a.name =~ 'Tob.*'`
* 대소문자 구분 문자열 비교식 : `a.surename START WITH 'Sven'`, `a.surename ENDS WITH 'son'`, `a.surename CONTAINS 'son'`
* `CASE` 표현식

## 3.2.3.2 문자 literal 참고 {#chapter323_2}

문자 literal은 다음과 같은 escape 문자를 포함합니다.

| escape 문자 | 문자내용 |
| :--- | :--- |
| `\t` | 탭 |
| `\b` | Backspace |
| `\n` | Newline |
| `\r` | Carriage return |
| `\f` | Form feed |
| `\'` | Single quote |
| `\"` | Double quote |
| `\\` | Backslash |
| `\uxxxx` | Unicode UTF-16 code \(`\u` 뒤에 4자리 16진수 포함\) |
| `\Uxxxxxxxx` | Unicode UTF-32 code \(`\U` 뒤에 8자리 16진수 포함\) |

## 3.2.3.3 `CASE` 표현식 {#chapter323_3}

일반 조건식은 잘 알려진 `CASE`를 활용하여 표현됩니다. Cypher는 2가지 종류의 `CASE` 표현식이 있습니다. 한개의 변수를 다수의 값과 비교하는 단순형, 다수의 조건 표현식을 사용하는 일반형.

예를 위하여 다음 그래프를 보겠습니다.

![](https://neo4j.com/docs/developer-manual/3.3/images/%60CASE%60%20expressions-1.svg)

### `CASE` 단순형 : 다수의 값을 비교 {#chapter323_3_1}

이 표현식은 `WHEN` 절에서 일치하는 것을 찾을 때까지 순서대로 계산하고 비교합니다. 만약 일치하는 것이 없는 경우 `ELSE` 절에 해당하는 값이 반환됩니다. 그러나 `ELSE` 절이 없는 경우에는 `null`값이 반환됩니다.

#### 문법

```cypher
CASE test
 WHEN value THEN result
  [WHEN ...]
  [ELSE default]
END
```

#### 변수항목

| 이름 | 설명 |
| :--- | :--- |
| `test` | 유효한 표현식 |
| `value` | `test`와 비교할 값 표현식 |
| `result` | `test`와 `value`를 비교하여 일치할 경우 반환할 값 |
| `default` | 일하는 것이 없는 경우 `default`값이 반환 |

#### 쿼리 예제

```cypher
MATCH (n)
RETURN
CASE n.eyes
WHEN 'blue'
THEN 1
WHEN 'brown'
THEN 2
ELSE 3 END AS result
```

#### 쿼리 결과

| 결과 |
| :--- |
| `2` |
| `1` |
| `3` |
| `2` |
| `1` |
| 5 행 |

### `CASE` 일반형 : 다수의 조건식을 비교 {#chapter323_3_2}

서술부에 작성된 조건식을 순서대로 비교하여 `true` 값이 나오면 해당 결과를 반환합니다. 만약 `true`에 해당하는 조건식이 없는 경우에는 `ELSE` 절의 값이 반환됩니다. 그러나 `ELSE` 절이 없는 경우에는 `null`값이 반환됩니다.

#### 문법

```cypher
CASE
WHEN predicate THEN result
  [WHEN ...]
  [ELSE default]
END
```

#### 변수항목

| 이름 | 설명 |
| :--- | :--- |
| `predicate` | 검사하고자 하는 조건식 |
| `result` | `predicate` 조건식이 `true`일 때 반환되는 결과값 |
| `default` | `true`에 일치하는 조건식이 없는 경우 `default`값이 반환 |

#### 쿼리 예제

```cypher
MATCH (n)
RETURN
CASE
WHEN n.eyes = 'blue'
THEN 1
WHEN n.age < 40
THEN 2
ELSE 3 END AS result
```

#### 쿼리 결과

| 결과 |
| :--- |
| `2` |
| `1` |
| `3` |
| `2` |
| `1` |
| 5 행 |

### `CASE`의 단순형과 일반형 사용시기 구분 {#chapter323_3_3}

두가지 유형이 거의 유사하기 때문에 가끔은 어떤 유형을 사용하여 쿼리를 작성해야하는지 혼동스러울 때가 있습니다. `age_10_years_ago`라는 이름의 10년전 나이를 계산하되 `n.age`가 `null`일 경우 `-1`이 반환되는 시나리오를 가지고 어떤 유형을 사용해야하는지 알아보겠습니다. 

#### 쿼리 (단순형)

```cypher
MATCH (n)
RETURN n.name,
CASE n.age
WHEN n.age IS NULL THEN -1
ELSE n.age - 10 END AS age_10_years_ago
```

#### 쿼리 결과 (단순형)

| n.name | age_10_years_age |
| :--- | :--- |
| `"Alice"` | `28` |
| `"Bob"` | `15` |
| `"Charlie"` | `43` |
| `"Daniel"` | `<null>` |
| `"Eskil"` | `31` |
| 5 행 ||

여기서는 `CASE` 단순형을 가지고 쿼리를 작성하였습니다. 하지만 `Daniel`이라는 이름을 가진 노드에 대한 `age_10_years_age`가 원하는 결과인 `-1`이 아닌 `null`입니다. 그 이유는 일반형의 경우 `n.age`와 `n.age IS NULL`을 비교하기 때문에 `n.age IS NULL`은 boolean값이고 `n.age`는 숫자이기 때문에 `WHEN n.age IS NULL THEN -1`이라는 조건식은 생략됩니다. 그래서 결과는 `ELSE n.age - 10`이 되고 결과로 `null`이 반환됩니다.

원하는 결과가 나오기 위하여 `CASE` 일반형으로 변경해보겠습니다.

#### 쿼리 (일반형)

```cypher
MATCH (n)
RETURN n.name,
CASE
WHEN n.age IS NULL THEN -1
ELSE n.age - 10 END AS age_10_years_ago
```

#### 쿼리 결과 (일반형)

| n.name | age_10_years_age |
| :--- | :--- |
| `"Alice"` | `28` |
| `"Bob"` | `15` |
| `"Charlie"` | `43` |
| `"Daniel"` | `-1` |
| `"Eskil"` | `31` |
| 5 행 ||

정상적으로 `Daniel` 노드에 대한 `age_10_years_ago`가 `-1`로 나오는 것을 확인할 수 있습니다.