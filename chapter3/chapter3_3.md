# 3.3. 절

> 이번 섹션은 Cypher 쿼리 언어에서 사용되는 모든 절에 대한 정보를 담고 있습니다.

* [일기 절](#chapter33_1)
* [투영 절](#chapter33_2)
* [읽기 하위절](#chapter33_3)
* [읽기 힌트](#chapter33_4)
* [쓰기 절](#chapter33_5)
* [읽기/쓰기 절](#chapter33_6)
* [집합 연산자](#chapter33_7)
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

| 하위절 | 설명 |
| :--- | :--- |
| [`WHERE`](/chapter3/chapter3_3_7.md) | `MATCH`나 `OPTIONAL MATCH`절의 제약조건 또는 `WITH`절의 필터를 추가 |
| [`ORDER BY [ASC[ENDING] | DESC[ENDING]]`](/chapter3/chapter3_3_8.md) | `RETURN` 또는 `WITH`에 연결되는 하위절. 정의된 오름차순(기본) 또는 내림차순으로 결과를 정렬 |
| [`SKIP`](/chapter3/chapter3_3_9.md) | 결과에 포함될 행의 시작 행번호 |
| [`LIMIT`](/chapter3/chapter3_3_10.md) | 결과에 포함될 행의 개수 |

## 읽기 힌트 {#chapter33_4}

쿼리를 튜닝할 때 사용되는 실행계획 힌트입니다. 이것의 사용법과 쿼리튜닝에 대한 더 자세한 내용은 [3.6.2 실행계획 힌트와 `USING`](/chapter3/chapter3_6_2.md)를 참고하시기 바랍니다.

| 힌트 | 설명 |
| :--- | :--- |
| [`USING INDEX`](/chapter3/chapter3_6_4.md#chapter364_2) | 실행계획에서 시작점으로 사용될 인덱스를 지정하는 힌트 |
| [`USING SCAN`](/chapter3/chapter3_6_4.md#chapter364_3) | 인덱스 대신 라벨을 스캔하도록 강제하는 힌트 |
| [`USING JOIN`](/chapter3/chapter3_6_4.md#chapter364_4) | 특정 지점에서 조인 연산을 수행하도록 강제하는 힌트 |

## 쓰기 절 {#chapter33_5}

데이터베이스에 데이터를 쓰는 절 입니다.

| 절 | 설명 |
| :--- | :--- |
| [`CREATE`](/chapter3/chapter3_3_11.md) | 노드와 관계 생성 |
| [`DELETE`](/chapter3/chapter3_3_12.md) | 그래프 항목(노드, 관계, 경로) 삭제. 삭제될 노드는 연관된 모든 관계가 삭제되어 있어야 합니다. |
| [`DETACH DELETE`](/chapter3/chapter3_3_12.md) | 노드나 노드들 삭제. 연관된 모든 관계는 자동으로 삭제됩니다. |
| [`SET`](/chapter3/chapter3_3_13.md) | 노드의 라벨과 노드 또는 관계의 속성 갱신 |
| [`REMOVE`](/chapter3/chapter3_3_14.md) | 노드와 관계에서 속성과 라벨 제거 |
| [`FOREACH`](/chapter3/chapter3_3_15.md) | 경로의 항목 또는 집계결과같은 목록에서 데이터 갱신 |

## 읽기/쓰기 절 {#chapter33_6}

데이터베이스에서 읽기와 쓰기를 함께하는 절 입니다.

| 절 | 설명 |
| :--- | :--- |
| [`MERGE`](/chapter3/chapter3_3_16.md) | 그래프에 존재하는지 확인합니다. 기존 정보를 사용하거나 생성합니다. |
| [`--- ON CREATE`](/chapter3/chapter3_3_16.md#chapter3316_3) | `MERGE`에 사용되는 접속사로 생성할 필요가 생길 때 처리해야할 하위절을 정의합니다. |
| [`--- ON MATCH`](/chapter3/chapter3_3_16.md#chapter3316_3) | `MERGE`에 사용되는 접속사로 기존 정보를 사용할 때 처리해야할 하위절을 정의합니다. |
| [`CALL […​YIELD]`](/chapter3/chapter3_3_17.md) | 데이터베이스에 반영된 프로시져를 실행하고 결과를 반환합니다. |
| [`CREATE UNIQUE`](/chapter3/chapter3_3_18.md) | `MATCH`와 `CREATE`의 조합으로 매칭하고 없는 것을 생성합니다. **미사용** |

## 집합 연산자 {#chapter33_7}

| 절 | 설명 |
| :--- | :--- |
| [`UNION`](/chapter3/chapter3_3_19.md) | 다수의 쿼리 결과를 한개의 결과로 합칩니다. 중복된 정보는 제외됩니다. |
| [`UNION ALL`](/chapter3/chapter3_3_19.md) | 다수의 쿼리 결과를 한개의 결과로 합칩니다. 중복된 정보도 포함됩니다. |

## 데이터 가져오기 {#chapter33_8}

| 절 | 설명 |
| :--- | :--- |
| [`LOAD CSV`](/chapter3/chapter3_3_20.md) | CSV 파일로부터 데이터 가져오기 |
| [`--- USING PERIODIC COMMIT`](/chapter3/chapter3_6_4.md#chapter364_5) | `LOAD CSV`를 사용하여 큰 규모의 데이터를 가져올 때 out-of-memory 오류를 피하기 위한 쿼리 힌트 |

## 스키마 절 {#chapter33_9}

스키마 관리를 위한 절 입니다. 자세한 내용은 [3.5 스키마](/chapter3/chapter3_5.md)에서 확인할 수 있습니다.

| 절 | 설명 |
| :--- | :--- |
| [`CREATE | DROP INDEX`](/chapter3/chapter3_5_1.md) | 모든 노드의 특정 라벨 또는 속성에 인덱스를 생성 또는 삭제 |
| [`CREATE | DROP CONSTRAINT`](/chapter3/chapter3_5_2.md) | 노드 라벨 또는 관계 타입 또는 속성에 관련된 제약조건을 생성 또는 삭제 |