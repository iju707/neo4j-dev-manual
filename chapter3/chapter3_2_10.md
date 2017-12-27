# 3.2.10 `null` 사용법

* [Cypher에서의 `null`](#chapter32101)
* [`null`이 포함된 논리연산](#chapter32102)
* [`null`과 `IN`연산](#chapter32103)
* [`null`을 반환하는 표현식](#chapter32104)

## Cypher에서의 `null` {#chapter32101}

Cypher에서 `null`은 없거나 정의되지 않은 값을 의미합니다. 개념상 `null`은 '잃어버린 알려지지 않은 값'이며, 다른 값과는 다르게 처리됩니다. 예로 노드에서 없는 속성을 가져오면 `null`값을 반환합니다. `null`을 입력으로 하는 대부분의 표현식은 `null`을 반환합니다. `WHERE`절에 사용되는 boolean 표현식 또한 포함됩니다. 이 경우 `true`가 아닌 경우 `false`로 처리됩니다.

`null`은 `null`과 다릅니다. 두개의 `null`이 동일한 값을 가지고 있는지 모릅니다. 그렇기 때문에 `null` = `null` 표현식은 `true`가 아닌 `null`을 반환합니다.

## `null`이 포함된 논리연산 {#chapter32102}

논리연산(`AND`, `OR`, `XOR`, `NOT`)에서 `null`은 'unknown`이라는 삼단논리로 처리됩니다. 여기에 각 연산자에 대한 비교표입니다.

| a | b | a `AND` b | a `OR` b | a `XOR` b | `NOT` a |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `false` | `false` | `false` | `false` | `false` | `true` |
| `false` | `null` | `false` | `null` | `null` | `true` |
| `false` | `true` | `false` | `true` | `true` | `true` |


## `null`과 `IN`연산 {#chapter32103}

## `null`을 반환하는 표현식 {#chapter32104}