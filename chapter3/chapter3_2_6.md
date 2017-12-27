# 3.2.6 주석

쿼리에 주석을 추가할 경우 Slash를 두개 사용하시면 됩니다.

```cypher
MATCH (n) RETURN n //This is an end of line comment
```

```cypher
MATCH (n)
//This is a whole line comment
RETURN n
```

```cypher
MATCH (n) WHERE n.property = '//This is NOT a comment' RETURN n
```
