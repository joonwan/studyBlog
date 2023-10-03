
---


## 페이징 API

- JPA는 페이징을 다음 두 API로 추상화 한다.

	-  setFirstResult(int startPosition) :  조회 시작 위치 ( 0 부터 시작함 )
	- setMaxResults(int maxResult) : 조회할 데이터 수
	- JPA 는 JPQL을 DB 방언 SQL 으로 변환해 DB에 쿼리를 날린다.

```Java
// 예시 코드

List<Member> resultList = em.createQuery(
	"select m from Member as m order by m.age", Member.class)
	.setFirstResult(0). // 시작 index
	.setMaxResult(2).   // 반환 갯수
	.getResultList();

```

