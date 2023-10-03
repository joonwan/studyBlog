
---

## 명시적 조인

- join 키워드를 직접 사용
```JPQL
select m from Member m join m.team t
```

## 묵시적 조인

- 경로 표현식에 의해 묵시적으로 SQL 조인 이 발생 (내부 조인만 가능)
```JQPL
select m.team From Member m
```
