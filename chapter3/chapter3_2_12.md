# 3.2.12 `null` 사용법

* [Cypher에서의 `null`](#chapter3212_1)
* [`null`이 포함된 논리연산](#chapter3212_2)
* [`null`과 `IN`연산](#chapter3212_3)
* [`null`을 반환하는 표현식](#chapter3212_4)

## Cypher에서의 `null` {#chapter3212_1}

Cypher에서 `null`은 없거나 정의되지 않은 값을 의미합니다. 개념상 `null`은 '잃어버린 알려지지 않은 값'이며, 다른 값과는 다르게 처리됩니다. 예로 노드에서 없는 속성을 가져오면 `null`값을 반환합니다. `null`을 입력으로 하는 대부분의 표현식은 `null`을 반환합니다. `WHERE`절에 사용되는 boolean 표현식 또한 포함됩니다. 이 경우 `true`가 아닌 경우 `false`로 처리됩니다.

`null`은 `null`과 다릅니다. 두개의 `null`이 동일한 값을 가지고 있는지 모릅니다. 그렇기 때문에 `null` = `null` 표현식은 `true`가 아닌 `null`을 반환합니다.

## `null`이 포함된 논리연산 {#chapter3212_2}

논리연산(`AND`, `OR`, `XOR`, `NOT`)에서 `null`은 'unknown`이라는 삼단논리로 처리됩니다. 여기에 각 연산자에 대한 비교표입니다.

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

## `null`과 `IN`연산 {#chapter3212_3}

`IN` 연산자도 비슷한 로직를 가집니다. Cypher에서 목록에 원하는 값이 존재하는 경우 `true`를 반환합니다. 만약 `null`을 포함하는 목록이고 원하는 결과가 없는 경우 `null`을 반환합니다. `null`이 없는 경우에는 `false`를 반환합니다.

| 표현식 | 결과 |
| :--- | :--- |
| 2 IN [1, 2, 3] | `true` |
| 2 IN [1, `null`, 3] | `null` |
| 2 IN [1, 2, `null`] | `true` |
| 2 IN [1] | `false` |
| 2 IN [] | `false` |
| `null` IN [1, 2, 3] | `null` |
| `null` IN [1, `null`, 3] | `null` |
| `null` IN [] | `false` |

`all`, `any`, `none`, `single` 연산자도 비슷합니다. 결과가 명확하게 계산이 되면 `true`나 `false`를 반환합니다. 그렇지 않으면 `null`이 반환됩니다.

## `null`을 반환하는 표현식 {#chapter3212_4}

* 리스트에 항목이 없는 경우 : `[][0]`, `head([])`
* 노드나 관계에 없는 속성에 접근할 경우 : `n.missingProperty`
* `null`과의 비교식 : `1 < null`
* `null`이 포함된 연산식 : `1 + null`
* `null`을 인수로 사용한 함수호출 : `sin(null)`