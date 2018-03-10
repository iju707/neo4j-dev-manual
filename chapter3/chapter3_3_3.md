# 3.3.3 START

> 명시적 인덱스로 시작점을 찾습니다.

`START` 절은 Cypher 3.2에서 제외되었습니다. `MATCH` ([3.3.1 `MATCH` 참조](/chapter3/chapter3_3_1.md))를 대신 사용하길 권장합니다. 그러나, 명시적 인덱스 사용이 필요한 경우에는 [Built-in Procedures](/chapter3/chatper3_5_1.md#chapter351_14)에서 관리 및 사용을 가능하게 합니다. 해당 프로시저에서 `START` 절과 동일한 기능을 제공합니다. 추가로 이 프로시져를 사용한 쿼리가 `START`를 사용한 쿼리보다 [Cost Planner](/chapter3/chapter3_6_1.md#chapter361_2)와 새로운 Cypher 3.2 컴파일러를 사용하기에 더 좋은 성능을 보입니다.

> `START` 절을 사용한 쿼리는 Cypher 3.1로 동작하게 됩니다.