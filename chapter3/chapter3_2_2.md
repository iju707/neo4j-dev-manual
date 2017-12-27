# 3.2.2 표현식

* [일반적인 표현식](#chapter3221)
* 문자 literal 참고
* `CASE` 표현식
  * 단순 CASE 유형 : 다양한 값을 비교하는 표현식
  * 일반 CASE 유형 : 다수의 조건식을 표현
  * 단순 및 일반 CASE 유형의 사용시기 구분

## 3.2.2.1 일반적인 표현식 {#chapter3221}

Cypher에서 가능한 표현식 입니다.

* 10진법 literal \(정수 또는 실수\) : `13`, `-40000`, `3.14`, `6.022E23`
* 16진법 literal \(`0x`로 시작\) : `0x13af`, `0xFC3A9`, `-0x66eff`
* 8진법 literal \(`0`으로 시작\) : `01372`, `02127`, `-05671`
* 문자 literal : `'Hello'`, `"World"`
* Boolean literal : `true`, `false`, `TRUE`, `FALSE`
* 변수 : `n`, `x`, `rel`, `myFancyVariable`, \`A name with weird stuff in it\[\]!\`
* 속성 : `n.prop`, `x.prop`, `rel.thisproperty`, myFancyVariable.\`(weird property name)\`
* 동적 변수 : `n["prop"]`, `rel[n.city + n.zip]`, `map[coll[0]]`
* 파라미터 : `$param`, `$0`
* 배열 : `['a', 'b']`, `[1, 2, 3]`, `['a', 2, n.property, $param]`, `[ ]`
* 함수 호출 : `length(p)`, `nodes(p)`
* 집계 함수 : `avg(x.prop)`, `count(*)`
* 경로 패턴 : `(a)-->()<--(b)`
* 연산식 : `1 + 2`, `3 < 4`
* 