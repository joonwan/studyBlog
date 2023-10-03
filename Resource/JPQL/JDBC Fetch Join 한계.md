
---

## Fetch join 대상에는 별칭을 줄 수 없다.

```Java
String sql = "select t from Team as t join fetch t.members as m "
```

- 원칙적으로는 t.members 에 별칭을 줄 수 없다.
- 하지만 하이버 네이트는 가능하다.
- 가급적 사용하지 말자

```JPQL
where m.name = "???"
```
- 이런식으로 하면 안된다.
- 따로 조회하는 방법을 사용해야한다.

## 둘 이상의 컬렉션은 fetch join 할 수 없다.

- Fetch join을 할 두 엔티티 내부에 각각 컬렉션이 존재할 경우 Fetch join 이 불가능하다.
- 일대다도 데이터 뻥튀기가 되는데 이 상황은 일대 다대 다 이기 때문에 데이터 정합성이 깨짐

## 컬렉션을 fetch join할 경우 페이징 API를 사용할 수 없다

- 일대일, 다대일 같은 단일값 연관 필드들은 Fetch join해도 페이징이 가능
- 하지만 일대다일 경우 데이터 뻥튀기 발생하기 때문에 페이징에 문제가 생긴다.
- 이 경우 실제 SQL에서는 페이징 쿼리가 포함되지 않는다.
- 하이버네이트는 경고 로그를 남기고 메모리에서 페이징한다.
	- 이는 매우 위험하다.

#### 첫번째 해결책

```JQPL
select m from Member m fetch join m.team as t
```

- 이처럼 일대다 관계를 뒤집어 다대일로 쿼리를 날리는 방법

#### 두번째 해결책 @BatchSize(int size)

##### 기본

- 기존 JQPL 에서 fetch join문을 뺀다
```JPQL
select t from Team t 
```

- 이때 Many 측에 지연 로딩이 걸려있기 때문에 멤버 조회시 위 JPQL에서 반환된 크기만큼의 추가적인 쿼리가 나간다.

##### @BatchSize 적용

- One 쪽에 @BatchSize 어노테이션을 적용한다.

```Java

@Entity
public class Team{
// 기본 팀 구현 멤머 ...
@BatchSize(size = 100)
@OneToMany(mappedBy = "team")
private List<Member> members = new ArrayList<>();
}
```

- 이때 나간 SQL을 살펴보면 한번에 멤버를 조회하는 것을 확인할 수 있다.

#### @BatchSize() 원리

- Team을 가져올때 Member는 지연 로딩 상태이다.
- @BatchSize() 사용시 team에 관련된 member의 쿼리가 각각 나가지 않고 in query를 이용해서 앞서 설정된 size만큼의 쿼리가 한번에 나간다.

#### @BatchSize() 설정

- 앞서 나온 코드처럼 클래스 내부에 어노테이션을 이용해서 설정이 가능하다.
- 하지만 @BatchSize는 글로벌 세팅이 가능하다.

```application.yml
jpa:
	properties:
		hibernate:
			default_batch_fetch_size : yourBatchSize
```
- 이렇게 설정하면 N+1 만큼의 쿼리가 아닌 Table 수만큼의 쿼리가 날라간다.


## Fetch join의 특징과 한계 정리

- 연관된 엔티티들을 SQL 한번으로 조회한다.
	- 성능 최적화
- 엔티티에 직접 적용하는 글로벌 로딩 전략보다 우선이다.
	- @OneToMany(fetch = FetchType.LAZY)  -> 글로벌 로딩 전략
- 실무에서 글로벌 로딩 전략은 모두 지연로딩
- 최적화가 필요한 곳은 페치 조인 적용

## Fetch Join 정리

- 모든 것을 Fetch Join으로 해결할 수는 없다.
- 페치 조인은 객체 그래프를 유지할 때 사용하면 효과적
	- 즉 m.team 처럼 찾아갈때는 효과적이다.
- 여러 테이블을 조인해서 엔티티가 가진 모양이 아닌 전혀 다른 결과를 내야한다면 Fetch join이 아닌 일반 조인을 사용하고 필요한 데이터들만 조회하여 DTO 로 반환하는 것이 효과적이다.