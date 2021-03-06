# 3.2.10 목록

> Cypher는 전반적으로 목록을 제공합니다.

* [일반적인 목록](#chapter3210_1)
* [목록으로부터 목록생성](#chapter3210_2)
* [경로로부터 목록생성](#chapter3210_3)

## 일반적인 목록 {#chapter3210_1}

일반적인 목록은 대괄호와 쉼표로 구분되는 항목을 사용하여 표현됩니다.

### 쿼리

```cypher
RETURN [0, 1, 2, 3, 4, 5, 6, 7, 8, 9] AS list
```

### 쿼리결과

| list |
| :--- |
| `[0,1,2,3,4,5,6,7,8,9]` |
| **1 row** |

예제에서 범위함수를 사용할 것 입니다. 주어진 시작/끝 숫자 사이의 모든 숫자(끝 포함)를 목록으로 반환하는 것 입니다. 

목록에서 각각의 항목에 접근할 경우 대괄호를 다시 사용하시면 됩니다. 이것은 시작인덱스부터 끝인덱스(제외됨)까지의 항목을 추출합니다.

### 쿼리

```cypher
RETURN range(0, 10)[3]
```

### 쿼리결과

| range(0, 10)[3] |
| :--- |
| `3` |
| **1 row** |

음수 또한 사용됩니다. 목록의 마지막부터 시작하게 됩니다.

### 쿼리

```cypher
RETURN range(0, 10)[-3]
```

### 쿼리결과

| range(0, 10)[-3] |
| :--- |
| `8` |
| **1 row** |

인덱스 항목을 범위로 지정할 수 도 있습니다.

### 쿼리

```cypher
RETURN range(0, 10)[0..3]
```

### 쿼리결과

| range(0, 10)[0..3]) |
| :--- |
| `[0,1,2]` |
| **1 row** |

### 쿼리

```cypher
RETURN range(0, 10)[0..-5]
```

### 쿼리결과

| range(0, 10)[0..-5] |
| :--- |
| `[0,1,2,3,4,5]` |
| **1 row** |

### 쿼리

```cypher
RETURN range(0, 10)[-5..]
```

### 쿼리결과

| range(0, 10)[-5..] |
| :--- |
| `[6,7,8,9,10]` |
| **1 row** |

### 쿼리

```cypher
RETURN range(0, 10)[..4]
```

### 쿼리결과

| range(0, 10)[..4] |
| :--- |
| `[0,1,2,3]` |
| **1 row** |

> 범위를 벗어나는 목록에 대한 부분은 자동으로 잘려나갑니다. 하지만 범위를 벗어나는 목록의 단일 항목은 `null`을 반환하게 됩니다.

### 쿼리

```cypher
RETURN range(0, 10)[15]
```

### 쿼리결과

| range(0, 10[15] |
| :--- |
| `<null>` |
| **1 row** |

### 쿼리

```cypher
RETURN range(0, 10)[5..15]
```

### 쿼리결과

| range(0, 10)[5..15] |
| :--- |
| `[5,6,7,8,9,10]` |
| **1 row** |

목록에 대한 크기는 다음으로 구할 수 있습니다.

### 쿼리

```cypher
RETURN size(range(0, 10)[0..3])
```

### 쿼리결과

| size(range(0, 10)[0..3]) |
| :--- |
| `3` |
| **1 row** |

## 3.2.10.2 목록으로부터 목록생성(List comprehension) {#chapter3210_2}

List comprehension는 Cypher에서 기존의 목록을 기반으로 새로운 목록을 생성할 수 있게 하는 구문구조 입니다. 맵이나 필터함수를 사용하지 않고 수학적 설정구성 표기법으로 구성됩니다.

### 쿼리

```cypher
RETURN [x IN range(0,10) WHERE x % 2 = 0 | x^3] AS result
```

### 쿼리결과

| result |
| :--- |
| `[0.0,8.0,64.0,216.0,512.0,1000.0]` |
| **1 row** |

필터나 맵을 각기 사용하고 싶으면, `WHERE`절이나 표현식 부분을 생략할 수 있습니다.

### 쿼리

```cypher
RETURN [x IN range(0,10) WHERE x % 2 = 0] AS result
```

### 쿼리결과

| result |
| :--- |
| `[0,2,4,6,8,10]` |
| **1 row** |

### 쿼리 

```cypher
RETURN [x IN range(0,10)| x^3] AS result
```

### 쿼리결과

| result |
| :--- |
| `[0.0,1.0,8.0,27.0,64.0,125.0,216.0,343.0,512.0,729.0,1000.0]` |
| **1 row** |

## 3.2.10.3 경로로부터 목록생성(Pattern comprehension) {#chapter3210_3}

Pattern comprehension는 Cypher에서 패턴 매칭을 통해 새로운 목록을 생성할 수 있게 하는 구문구조 입니다. 일반적인 `MATCH`절과 `WHERE`절을 사용하여 매칭을 한 뒤 원하는 방식의 결과출력문을 사용합니다.

예제를 위한 그래프는 다음과 같습니다.

![](https://neo4j.com/docs/developer-manual/current/images/Lists-1.svg)

### 쿼리

```cypher
MATCH (a:Person { name: 'Charlie Sheen' })
RETURN [(a)-->(b) WHERE b:Movie | b.year] AS years
```

### 쿼리결과

| years |
| :--- |
| `[1979, 1984, 1987]` |
| **1 row** |

`WHERE`절을 포함한 전체 술어부는 선택사항이므로 생략이 가능합니다.