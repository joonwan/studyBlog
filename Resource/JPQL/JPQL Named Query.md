
---

## Named Query 란?

- 미리 정의해서 이름을 부여해 두고 사용하는 JPQL
- 정적 쿼리
- 어노테이션, XML에 정의
- 애플리케이션  로딩 시점에 초기화(SQL parsing) 후 재사용 하는 방식
	- JPQL은 SQL로 변환할 때 코스트 발생
	- 하지만 이 방식을 이용하면 코스트 거의 없음
	- 로딩 시점에만 코스트가 발생하고 캐싱 되기 때문이다.
- 애플리케이션 로딩시점에 쿼리를 검증가능.
	- 이부분이 매우 중요
	- 아래 예시 참조


## 활용 예시

```Java
@Entity
@NamedQuery(
	name = "Member.findByUsername",
	query = "select m from Member as m where m.username = :username"
)
public class Member{
	// 멤버 구현 Part
}
```


```Java
// test code

em.createNamedQuery("Member.findByUsername",Member.class)
					.setParameter("username", username)
					.getResultList();
```

- 만약 Named Query 내부 JPQL에 오류가 있을 경우 Application Loading 시점에ParameterResolutionException 이 발생
- 즉 Application Loading 시점에 JPQL에 오류가 있을 경우 오류를 띄어준다.