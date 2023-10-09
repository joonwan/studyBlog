
---

#### 🎯 실무에서 너무나도 중요

## Fetch Join 이란?

- SQL 조인 종류가 아니다
- JPQL에서 성능 최적화를 위해 제공하는 전용 기능
- 연관된 엔티티나 컬렉션을 SQL 한번에 함께 조회하는 기능
- join fetch 명령어를 사용
- 쿼리로 어떤 객체들을 한번에 불러올지 직접 명시적으로 동적인 타이밍에 정할 수 있음 

## 예제

- 회원을 조회하면서 연관된 팀도 함께 조회 ( 한번에 )
- SQL을 보면 회원 뿐만아니라 팀도 함께 select 문에 나간다.

```JQPL
select m from Member m join fetch m.team
```

``` SQL
select M.*, T.* from Member M inner join Team t on m.tea_id = t.id;
```

- M.* : M 의 데이터를 축약해서 적어놓은 것
- T.* 또한 마찬가지이다.

![[Pasted image 20231003002926.png]]
- 회원 1과 회원 2는 Team A 에 소속
- 회원 3 은 Team B, 회원 4는 무소속이다.
- 나는 회원 1,2,3을 뽑고 싶다
-  화면에 회원 정보를 뿌릴 떄 한번에 데이터를 가지고 오고 싶은 상황이다.

![[Pasted image 20231003003007.png]]
- 기본적인 조인을 하면 위 릴레이션 처럼 된다.
- inner join이기 때문에 회원 4는 나오지 않는다.

![[Pasted image 20231003003034.png]]

- Result List에는 위 그림과 같은 구조로 Collection이 구성되어 있다.

## 실행 결과

### 지연로딩

```Java
List<MemberJPQL> resultList = em.createQuery("select m from MemberJPQL as m ", MemberJPQL.class).getResultList();
```

- @ManyToOne (fetch = FetchType.LAZY) 가 걸려있기 때문에 Team Entity는 Proxy 로 들어오게 된다.
- 실제 member.getTeam.getName을 호출 시 DB에 쿼리가 날라감
- member1 이 팀의 이름 요청시 진짜 객체가 존재하지 않기 때문에 DB에 쿼리가 한번 날라감
- member2 는 member1과 같은 TeamA 의 객체가 필요한데 이미 영속성 컨텍스트 내부에 TeamA가 관리되고 있기 때문에 쿼리가 날라가지 않는다.
- 하지만 member3은 TeamB의 이름을 요청하는데 이때 영속성 컨텍스트에는 TeamB 가 관리되고 있지 않기 때문에 DB에 쿼리가 날라간다.
- member1,2,3 의 팀 조회시 쿼리가 총 3번 나간다.
	- 멤버 조회 쿼리
	- teamA 찾는 쿼리
	- teamB 찾는 쿼리
- 만약 모든 멤버의 팀이 달랐다면 4번의 쿼리가 발생했을 것이다.
- 만약 회원이 100명이고 각각 팀이 다른 경우 쿼리는 101번 나간다.
- 이를 N+1 문제라고 한다.
	- 1 -> 회원을 찾는 쿼리
	- N  -> 첫번째 반환받는 Result 크기
	- 즉 첫번째 쿼리를 통해 결과를 받은만큼 추가적으로 쿼리(select)가 나가는 문제를 N+1 문제라고 한다.
	- 즉시로딩, 지연로딩 두경우 모두 발생
	- fetch join으로 해결해야한다.


### fetch join 적용

```JPQL
select m from Member m join fetch m.team
```

- fetch join을 사용할 경우 query가 한번만 나가는 것을 확인할 수 있다.
- fetch join을 사용하지 않은 경우 핋요할 때 마다 DB에 쿼리를 날리기 때문에 select 문이 추가적으로 나간다.
- 하지만 fetch join을 사용할 경우 member 조회시 한번에 team을 불러오기 때문에 쿼리가 한번 나간다.

## 컬렉션 Fetch Join

- 일대다 관계, 컬렉션 페치 조인

```JQPL
select t from Team t join fetch t.members
where t.name = '팀A'
```

```SQL
select T.*, M.* from Team T 
inner join member M on T.id = M.team_id
where t.name = '팀A'
```

- 즉 이전 예시와 반대로 조인하는 것

### 예제

```Java
List<TeamJPQL> resultList = em.createQuery("select t from TeamJPQL as t join fetch t.members", TeamJPQL.class).getResultList();
```

- 컬렉션 Fetch Join 시 주의사항
	- 일대다 조인시 Data 뻥튀기가 됨
	- 참고 : hibernate 6 부터 자동 distinct 됨!!

![[Pasted image 20231003013348.png]]

-  팀 A 입장에서 멤버1,2가 속하기 때문에 릴레이션을 두번째 그림과 같이 만들어 냄
- 즉 Row 가 2개가 됨

- 해결 책
	- SQL 의 중복을 제거하는 DISTINCT 사용 -> 이것만으로는 모든 중복을 제거 불가
	- 따라서 JPQL의 DISTINCT의 기능을 사용
	- 제공하는 기능
		- SQL에 DISTINCT를 추가하는 기능
		- 애플리케이션에서 엔티티 중복 제거

## Fetch join과 DISTINCT

- distinct 가 추가로 애플리케이션에서 중복 제거 시도
- 같은 식별자를 가진 Team Entity 제거

![[Pasted image 20231003014351.png]]

- 참고 : 다대일은 데이터 뻥튀기가 안됨