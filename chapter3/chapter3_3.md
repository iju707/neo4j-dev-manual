# 3.3. 절

> 이번 섹션은 Cypher 쿼리 언어에서 사용되는 모든 절에 대한 정보를 담고 있습니다.

* [일기 절](#chapter33_1)
* [투영 절](#chapter33_2)
* [읽기 하위절](#chapter33_3)
* [읽기 힌트](#chapter33_4)
* [쓰기 절](#chapter33_5)
* [읽기/쓰기 절](#chapter33_6)
* [설정 연산자](#chapter33_7)
* [데이터 가져오기](#chapter33_8)
* [스키마 절](#chapter33_9)

## 일기 절 {#chapter33_1}

데이터베이스에서 데이터를 읽어오는 절을 포함하고 있습니다.

Cypher 쿼리에서 데이터 흐름은 데이터베이스에서 쿼리의 변수와 값 사이에 구성이 가능한 순서가 정해지지 않은 키-값 쌍의 맵으로 구성됩니다. 이 절은 쿼리의 후속부분으로 더 구체화 됩니다.

| 절 | 설명 |
| :--- | :--- |
| [`MATCH`](/chapter3/chapter3_3_1.md) | 데이터베이스에서 검색 |
| [`OPTIONAL MATCH`](/chapter3/chatper3_3_2.md) | 데이터베이스에서 없는 부분에 대해서는 `null`을 사용하도록 하는 검색 |
| [`START`](/chapter3/chapter3_3_3.md) | 명시적인 인덱스에서 시작점 찾기 **미사용** |

## 투영 절 {#chapter33_2}

결과를 반환하기 위한 표현식을 포함합니다. 반환 표현식은 모두 `AS`를 사용하여 별칭을 지정할 수 있습니다.

| 절 | 설명 |
| :--- | :--- |
| [`RETURN ... [AS]`](/chapter3/chatper3_3_4.md) | 쿼리 결과에 포함될 것을 정의 |
| [`WITH ... [AS]`](/chapter3/chapter3_3_5.md) | 쿼리 부분이 연결될 수 있도록 하며, 결과를 다음의 시작점 또는 기준으로 사용할 수 있도록 전달 |
| [`UNWIND ... [AS]`](/chapter3/chapter3_3_6.md) | 목록을 행으로 확장 |

## 읽기 하위절 {#chapter33_3}

읽기 절의 부분으로 사용되는 하위절 입니다.

| 절 | 설명 |
| :--- | :--- |
| [`WHERE`](/chapter3/chapter3_3_7.md) | `MATCH`나 `OPTIONAL MATCH`절의 제약조건 또는 `WITH`절의 필터를 추가 |
| [`ORDER BY [ASC[ENDING] | DESC[ENDING]]`](/chapter3/chapter3_3_8.md) | `RETURN` 또는 `WITH`에 연결되는 하위절. 정의된 오름차순(기본) 또는 내림차순으로 결과를 정렬 |
| [`SKIP`](/chapter3/chapter3_3_9.md) | 결과에 포함될 행의 시작 행번호 |
| [`LIMIT`](/chapter3/chapter3_3_10.md) | 결과에 포함될 행의 개수 |

## 읽기 힌트 {#chapter33_4}

쿼리를 튜닝할 때 사용되는 실행계획 힌트입니다. 이것의 사용법과 쿼리튜닝에 대한 더 자세한 내용은 [3.6.2 실행계획 힌트와 `USING`](/chapter3/chapter3_6_2.md)를 참고하시기 바랍니다.

| 힌트 | 설명 |
| :--- | :--- |
| [`USING INDEX`](/chapter3/chapter3_6_4.md#chapter3642) | 실행계획에서 시작점으로 사용될 인덱스를 지정하는 힌트 |
| [`USING SCAN`](/chapter3/chapter3_6_4.md#chapter3643) | 인덱스 대신 라벨을 스캔하도록 강제하는 힌트 |
| [`USING JOIN`](/chapter3/chapter3_6_4.md#chapter3644) | 특정 지점에서 조인 연산을 수행하도록 강제하는 힌트 |

## 쓰기 절 {#chapter33_5}

데이터베이스에 데이터를 쓰는 절 입니다.

| 힌트 | 설명 |
| :--- | :--- |
| [`CREATE`](/chapter3/chapter3_3_11.md) | 노드와 관계 생성 |
| [`DELETE`](/chapter3/chapter3_3_12.md) | 그래프 항목(노드, 관계, 경로) 삭제. 삭제될 노드는 연관된 모든 관계가 삭제되어 있어야 합니다. |
| [`DETACH DELETE`](/chapter3/chapter3_3_12.md) | 노드나 노드들 삭제. 연관된 모든 관계는 자동으로 삭제됩니다. |
| [`SET`](/chapter3/chapter3_3_13.md) | 노드의 라벨과 노드 또는 관계의 속성 갱신 |
| [`REMOVE`](/chapter3/chapter3_3_14.md) | 노드와 관계에서 속성과 라벨 제거 |
| [`FOREACH`](/chapter3/chapter3_3_15.md) | 경로의 항목 또는 집계결과같은 목록에서 데이터 갱신 |

## 읽기/쓰기 절 {#chapter33_6}



## 설정 연산자 {#chapter33_7}

## 데이터 가져오기 {#chapter33_8}

## 스키마 절 {#chapter33_9}



