
---

## 기본 키 값

- JPQL에서 Entity를 직접 사용하면 SQL에서 해당 Entity의 기본 키 값을 사용

```JQPL
select count(m.id) from Member m // 엔티티의 아이디를 사용
select count(m) from Member m    // 엔티티를 직접 사용
```

- 이때 나가는 SQL은 다음과 같다

```SQL
select count(m.id) as cnt from member m
```

- 둘다 SQL 쿼리가 똑같이 나가는 것을 확인할 수 있다.


## 외래키 값

- 연관된 객체를 조회시에도 마찬가지고 외래 키 값을 이용하여 SQL이 나가는 것을 확인할 수 있다.

```Java
String sql = "select m from Member m where m.team = :team";
em.createQuery(sql,Member.class)
			.setParameter("team", team)
			.getResultList();
```

```SQL
select m from member m where m.team_id = ?
```



