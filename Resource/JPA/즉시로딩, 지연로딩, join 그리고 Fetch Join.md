
---

## 즉시 로딩과 Fetch Join은 같은 기능인가?

- 즉시 로딩 (EARGR) 과 Fetch Join은 다른 기능이다.
- em.find() 를 이용해 엔티티를 하나만 조회시 즉시 로딩으로 설정하면 연관된 엔티티도 한 쿼리로 가져오도록 최적화 된다.
- 하지만 JPQL을 사용하면 이야기가 달라진다.
- JPQL은 연관관계를 즉시 로딩으로 설정하는 것과 상관 없이 JPQL 자체만으로 SQL로 그대로 번역된다.

### 즉시로딩 설정의 경우

- 멤버 전체 조회하기 위해 JPQL 실행
```JQPL
select m from Member m
```

- JQPL은 즉시로딩 설정돠 무관하게 SQL로 번역된다.
```SQL
select * from member;
```

- JQPL의 결과는 member만 조회하고 team은 조회하지 않는다.
- 이때 member와 team이 즉시 로딩으로 설정되어 있기 때문에 연관된 팀을 각각 쿼리를 날려 추가조회 가일어난다.
- 즉 최악의 경우 쿼리는 N+1번이 일어난다.

### 지연로딩 설정의 경우

- 멤버 전체를 조회하기 위해 JPQL 실행
```JQPL
select m from Member m
```

- JPQL은 Lazy와 무관하게 SQL  그대로 번역
```SQL
select * from member;
```

- JPQL 결과가 member 만 조회하고 연관된 팀은 조회하지 않는다.
- member 와 team 은 지연로딩으로 설정되어 있기 때문에 팀에 proxy 객체를 넣어두고 실제 팀을 사용하는 시점에 team을 DB에 쿼리를 날려서 조회한다.
- 이때도 마찬가지로 최악의 경우 쿼리가 N+1번 나간다.

### 일반 Join

- 일반 조인시 기준 엔티티만 프로젝션 된다.
```JQPL
select t from Team t join t.members as m
```

- 이전 [[Fetch Join 과 일반 Join의 차이]] 에서 말햇듯 프록시가 아닌 컬렉션에 값이 초기화 되지 않았기 때문에 초기화를 위한 추가적인 쿼리가 발생한다.
### Fetch Join 사용
```JPQL
select m from Member m join fetch m.team as t
```
- fetch join은 지연로딩과 즉시로딩 둘다와 상관이 없다.
- JPQL 내부에서 fetch join을 사용하였으므로 SQL은 멤버와 Team을 한 쿼리로 조회한다.
- 즉 JQPL은 멤버와 팀을 한번에 조회하여 반환한다.
- 따라서 추가적으로 쿼리가 발생하지 않기 때문에 N+1문제가 해결된다.


## 정리 (기준  : User,  대상 : Team)

### 즉시 로딩

- User 호출 시점
	- user의 쿼리 (n 개 반환, 1번의 쿼리)
	- team 의 쿼리 (n 번의 쿼리)
- 총 쿼리 수
	- n+1

### 지연 로딩

- User 호출 시정
	- User의 쿼리 (n 개 반환, 1번의 쿼리)
- Team 사용 시점
	- Team 의 쿼리 (n 번의 쿼리)
- 총 쿼리수 
	- n+1

### 일반 조인

- User 호출 시점
	- User 쿼리 (n개 반환, 1번의 쿼리)
- Team 호출 시점
	- Team 쿼리 (n번의 쿼리)
- 총 쿼리수 
	- n+1

### Fetch Join

- User 호출 시점에 User와 Team 모두 조회
- 따라서 총 쿼리수는 1번