
---

## 일반 Bulk 연산이란?

- SQL의 Update랑 Delete 문이라고 보면 된다.
- 특정 PK 를 찍어 한건을 Update, Delete  를 제외한 모든 연산

## 예제

- 재고가 10개 미만인 모든 상품의 가격을 10%를 상승 하려면?
- JPA 변경 감지 기능으로 실행하려면 너무 많은 SQL을 실행하게 된다.
	- 1. 재고가 10개 미만인 상품을 리스트로 조회
	- 2.상품 엔티티의 가격을 10% 증가
	- 3.트랜잭션 커밋 시점에 변경 감지가 동작
- 변경된 Data가 100건이라면 100번의 Update 문이 실행된다.

## JPA 에서의 벌크 연산

- JPA의 벌크 연산은 쿼리 한번으로 여러 테이블의 row 를 변경시킬 수 있다.
- .excuteUpdate() 를 실행할 경우 영향을 받은 Entity 의 수를 반환한다.

```Java

em.createQuery("update Member m set m.age = 20")
				.excuteUpdate();
```

- Update, Delete 를 지원한다.
- JPA 의 구현체로 Hibernate 를 사용한다.
- 따라서 insert into ... select 를 지원한다.

## Bulk 연산의 주의점

- Bulk 연산은 영속성 컨텍스트를 무시하고 데이터 베이스에 직접 쿼리가 날라간다.
- 잘못하면 꼬이는 상황이 발생
- 이를 해결하기 위한 해결책 2가지
	- 1.벌크 연산을 먼저 실행
	- 2.벌크 연산수행 이후 영속성 컨텍스트를 초기화
		- 벌크 연산 수행시 flush()  된다.
		- 이후 clear()
	- 두가지 중 하나를 사용하자

- 참고 
	- flush 가 되는 시점
		- 1. transaction commit 시점
		- 2. query 가 DB로 나가는 시점
		- 3. flush() 호출 시점
		- 

### 예시

```Java
Member member1 = new Member("member1",10);
Member member2 = new Member("member2",15);

em.persist(member1);
em.persist(member2);

// 자동 flush
em.createQuery("update Member m set m.age = 20").excuteUpdate();

List<Member> resultList = em.createQuery("select m from Member m", Member.class)  
.getResultList();  
  
for (Member m : resultList) {  
System.out.println(m.getAge());  
}
```

- 이때 실행 결과는 각각 10, 15 이다.
- 이는 DB에는 update Query 가 날라가 정상 반영 되었지만 영속성 컨텍스트 내부에는 초기화가 되지 않았기 때문이다.
- 따라사서 이런 상황에는 영속성 컨텍스트를 bulk 연산 이후 초기화를 해주어야 한다.

```Java
Member member1 = new Member("member1",10);
Member member2 = new Member("member2",15);

em.persist(member1);
em.persist(member2);

// 자동 flush
em.createQuery("update Member m set m.age = 20").excuteUpdate();

//영속성 컨텍스트 초기화
em.clear();

List<Member> resultList = em.createQuery("select m from Member m", Member.class)  
.getResultList();  
  
for (Member m : resultList) {  
System.out.println(m.getAge());  
}
```