# 3.2.5 연산자

* [연산자 개요](#chapter3251)
* [일반적 연산자](#chapter3252)
  * [`DISTINCT` 연산자 사용하기](#chapter3252_1)
  * [맵구조에서 `.`을 사용하여 속성접근하기](#chapter3252_2)
  * [`[]`연산자를 사용하여 동적으로 속성키를 필터링하기](#chapter3252_3)
* [수학적 연산자](#chapter3253)
  * [누승 연산자 `^` 사용하기](#chapter3253_1)
  * [단항 음수 연산자 `-` 사용하기](#chapter3253_2)
* [비교 연산자](#chatper3254)
  * [두개의 숫자 비교](#chapter3254_2)
  * [이름 필터를 위한 `START WITH` 연산자](#chapter3254_3)
* [Boolean 연산자](#chapter3255)
  * [숫자 필터를 위한 Boolean 연산자](#chapter3255_1)
* [문자열 연산자](#chapter3256)
  * [단어 필터를 위한 정규식 `=~` 연산자](#chapter3256_1)
* [목록 연산자](#chapter3257)
  * [`+` 연산자를 사용하여 목록 연결하기](#chapter3257_1)
  * [`IN` 연산자를 사용하여 목록에 숫자 존재여부 확인](#chapter3257_2)
  * [`[]` 연산자를 사용하여 목록의 항목에 접근하기](#chapter3257_3)
* [속성 연산자](#chapter3258)
* [값의 동등 비교](#chapter3259)
* [값의 순서 비교](#chapter32510)
* [비교 연산자 연결](#chapter32511)

## 3.2.5.1 연산자 개요 {#chapter3251}

| 연산자 | 종류 |
| :--- | :--- |
| [일반적 연산자](#chapter3252) | `DISTINCT`, 속성접근을 위한 `.`, 동적 속성접근을 위한 `[]` |
| [수학적 연산자](#chapter3253) | `+`, `-`, `*`, `/`, `%`, `^` |
| [비교 연산자](#chatper3254) | `=`, `<>`, `<`, `>`, `<=`, `>=`, `IS NULL`, `IS NOT NULL` |
| [문자특화 비교 연산자](#chapter3254_1) | `START WITH`, `ENDS WITH`, `CONTAINS` |
| [Boolean 연산자](#chapter3255) | `AND`, `OR`, `XOR`, `NOT` |
| [문자열 연산자](#chapter3256) | 연결을 위한 `+`, 정규식을 위한 `=~` |
| [목록 연산자](#chapter3257) | 연결을 위한 `+`, 항목 존재여부 확인을 위한 `IN`, 항목접근을 위한 `[]` |

## 3.2.5.2 일반적 연산자 {#chapter3252}

일반적 연산자는 다음과 같습니다.

* 값의 중복제거 : `DISTINCT`
* 노드, 관계, 맵구조체의 속성에 접근하는 `.` 연산자
* 동적 속성접근에 사용되는 `[]` 연산자

### `DISTINCT` 연산자 사용하기 {#chapter3252_1}

`Person` 노드의 눈동자 색의 종류를 찾아보겠습니다.

#### 쿼리

```cypher
CREATE (a:Person { name: 'Anne', eyeColor: 'blue' }),(b:Person { name: 'Bill', eyeColor: 'brown' }),(c:Person { name: 'Carol', eyeColor: 'blue' })
WITH [a, b, c] AS ps
UNWIND ps AS p
RETURN DISTINCT p.eyeColor
```

**Anne**와 **Carol**이 똑같은 파란색 눈을 가지고 있기 때문에 **blue**는 한번만 반환됩니다.

#### 쿼리결과

| p.eyeColor |
| :--- |
| `"blue"` |
| `"brown"` |
| **2 rows Nodes created: 3 Properties set: 6 Labels added: 3** |

`DISTINCT` 연산자는 일반적으로 [집계 함수](/chapter3/chapter3_4_3.md)와 함께 사용됩니다.

### 맵구조에서 `.`을 사용하여 속성접근하기 {#chapter3252_2}

#### 쿼리

```cypher
WITH { person: { name: 'Anne', age: 25 }} AS p
RETURN p.person.name
```

#### 쿼리결과

| p.person.name |
| :--- |
| `"Anne"` |
| ** 1 row ** |

### `[]`연산자를 사용하여 동적으로 속성키를 필터링하기 {#chapter3252_3}

#### 쿼리

```cypher
CREATE (a:Restaurant { name: 'Hungry Jo', rating_hygiene: 10, rating_food: 7 }),(b:Restaurant { name: 'Buttercup Tea Rooms', rating_hygiene: 5, rating_food: 6 }),(c1:Category { name: 'hygiene' }),(c2:Category { name: 'food' })
WITH a, b, c1, c2
MATCH (restaurant:Restaurant),(category:Category)
WHERE restaurant["rating_" + category.name]> 6
RETURN DISTINCT restaurant.name
```

#### 쿼리결과

| restaurant.name |
| :--- |
| `"Hungry Jo"` |
| **1 row Nodes created: 4 Properties set: 8 Labels added: 4** |

동적 속성접근에 대하여 자세한 내용은 [3.3.7.2 기본적 사용법](/chapter3/chapter3_3_7.md#chapter3372)를 보시기 바랍니다.

### 3.2.5.3 수학적 연산자 {#chapter3253}

수학적 연산자는 다음과 같습니다.

* 더하기 : `+`
* 빼기 또는 단항 음수 : `-`
* 곱하기 : `*`
* 나누기 : `/`
* 나머지 : `%`
* 지수화 : `^`

#### 누승 연산자 `^` 사용하기 {#chapter3253_1}

##### 쿼리

```cypher
WITH 2 AS number, 3 AS exponent
RETURN number ^ exponent AS result
```

##### 결과

| result |
| :--- |
| `8.0` |
| **1 row** |

#### 단항 음수 연산자 `-` 사용하기 {#chapter3253_2}

##### 쿼리

```cypher
WITH -3 AS a, 4 AS b
RETURN b - a AS result
```

##### 쿼리결과

| result |
| :--- |
| `7` |
| **1 row** |

### 3.2.5.4 비교 연산자 {#chapter3254}

비교 연산자는 다음과 같습니다.

* 같다 : `=`
* 다르다 : `<>`
* 작다 : `<`
* 크다 : `>`
* 작거나 같다 : `<=`
* 크거나 같다 : `>=`
* `IS NULL`
* `IS NOT NULL`

#### 문자특화 비교 연산자 {#chapter3254_1}

* `START WITH` : 문자열에서 대소문자 구분해서 시작부분부터 비교
* `ENDS WITH` : 문자열에서 대소문자 구분해서 끝부분 부터 비교
* `CONTAINS` : 문자열에서 대소문자 구분해서 포함하는지 비교

#### 두개의 숫자 비교 {#chapter3254_2}

##### 쿼리

```cypher
WITH 4 AS one, 3 AS two
RETURN one > two AS result
```

##### 쿼리결과

| result |
| :--- |
| `true` |
| **1 row** |

비교 연산자에 대한 상세한 동작은 [3.2.5.9 값의 동등 비교](#chapter3259)를 참고하시기 바랍니다. 더 많은 예시는 [3.3.7.8 범위 사용](/chapter3/chapter3_3_7.md#chapter3378)을 참고하시기 바랍니다.

#### 이름 필터를 위한 `START WITH` 연산자 {#chapter3254_3}

##### 쿼리

```cypher
WITH ['John', 'Mark', 'Jonathan', 'Bill'] AS somenames
UNWIND somenames AS names
WITH names AS candidate
WHERE candidate STARTS WITH 'Jo'
RETURN candidate
```

##### 쿼리결과

| candidate |
| :--- |
| `"John"` |
| `"Jonathan"` |
| **2 rows** |

문자열에 특화된 비교 연산자를 사용한 다양한 예제와 함께 자세한 정보는 [3.3.7.3 문자열 매칭](/chapter3/chapter3_3_7.md#chapter3373)을 참고하시기 바랍니다.

### 3.2.5.5 Boolean 연산자

논리연산자로 잘 알려진 Boolean 연산자는 다음과 같습니다.

* `AND`
* `OR`
* `XOR`
* `NOT`

`AND`, `OR`, `XOR`, `NOT`에 대한 참거짓표 입니다.

| a | b | a `AND` b | a `OR` b | a `XOR` b | `NOT` a |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `false` | `false` | `false` | `false` | `false` | `true` |
| `false` | `null` | `false` | `null` | `null` | `true` |
| `false` | `true` | `false` | `true` | `true` | `true` |
| `true` | `false` | `false` | `true` | `true` | `false` |
| `true` | `null` | `null` | `true` | `null` | `false` |
| `true` | `true` | `true` | `true` | `false` | `false` |
| `null` | `false` | `false` | `null` | `null` | `null` |
| `null` | `null` | `null` | `null` | `null` | `null` |
| `null` | `true` | `null` | `true` | `null` | `null` |

#### 숫자 필터를 위한 Boolean 연산자 {#chapter3255_1}

##### 쿼리

```cypher
WITH [2, 4, 7, 9, 12] AS numberlist
UNWIND numberlist AS number
WITH number
WHERE number = 4 OR (number > 6 AND number < 10)
RETURN number
```

##### 쿼리결과

| number |
| :--- |
| `4` |
| `7` |
| `9` |
| **9 rows** |

### 3.2.5.6 문자열 연산자 {#chapter3256}

문자열 연산자는 다음과 같습니다.

* 문자열 연결 : `+`
* 정규식 비교 : `=~`

#### 단어 필터를 위한 정규식 `=~` 연산자 {#chapter3256_1}

##### 쿼리

```cypher
WITH ['mouse', 'chair', 'door', 'house'] AS wordlist
UNWIND wordlist AS word
WITH word
WHERE word =~ '.*ous.*'
RETURN word
```

##### 쿼리결과

| word |
| :--- |
| `"mouse"` |
| `"house"` |
| **2 rows** |

정규식을 이용하여 필터링하는 방법에 대한 더 자세한 내용과 예제는 [3.3.7.4 정규식 표현](/chapter3/chapter3_3_7.md#chapter3374)를 참고 하시기 바랍니다. 추가로 문자열 비교에 관련된 내용은 [문자특화 비교 연산자](#chapter3254_1)를 참고 하시기 바랍니다.

### 3.2.5.7 목록 연산자 {chapter3257}

목록 연산자는 다음과 같습니다.

* 목록 연결하기 : `+`
* 목록에 숫자 존재여부 확인 : `IN`
* 목록의 항목에 접근하기 : `[]`

#### `+` 연산자를 사용하여 목록 연결하기 {#chapter3257_1}

##### 쿼리

```cypher
RETURN [1,2,3,4,5]+[6,7] AS myList
```

##### 쿼리결과

| myList |
| :--- |
| `[1,2,3,4,5,6,7]` |
| **1 row** |

#### `IN` 연산자를 사용하여 목록에 숫자 존재여부 확인 {#chapter3257_2}

##### 쿼리

```cypher
WITH [2, 3, 4, 5] AS numberlist
UNWIND numberlist AS number
WITH number
WHERE number IN [2, 3, 8]
RETURN number
```

##### 쿼리결과

| number |
| :--- |
| `2` |
| `3` |
| **2 rows** |

#### `[]` 연산자를 사용하여 목록의 항목에 접근하기 {#chapter3257_3}

##### 쿼리

```cypher
WITH ['Anne', 'John', 'Bill', 'Diane', 'Eve'] AS names
RETURN names[1..3] AS result
```

대괄호는 목록에서 인덱스 `1`부터 인덱스 `3`까지(제외됨) 항목을 추출하게 됩니다. 

##### 쿼리결과

| result |
| :--- |
| `["John", "Bill"]` |
| **1 row** |

### 3.2.5.8 속성 연산자 {#chapter3258}

> 버전 2.0 이후, 기존에 지원한 속성 연산자 `?`와 `!`는 삭제되었습니다. 더 이상 지원되지 않습니다. 없는 속성의 경우 `null`을 반환할 것 입니다. 만약 기존의 `?` 연산자 작업이 필요한 경우에는 `(NOT(exists(<ident>.prop)) OR <ident>.prop=<value>)`를 사용하시면 됩니다. 또한 선택적 관계에 활용된 `?` 연산자도 삭제되었으며 `OPTIONAL MATCH`절을 활용하시면 됩니다.

### 값의 동등 비교 {#chapter3259}

### 값의 순서 비교 {#chapter32510}

### 비교 연산자 연결 {#chapter32511}

