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
