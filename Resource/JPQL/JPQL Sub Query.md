-- --

- 일반적인 SQL의 서브 쿼리와 같음
- 예제 
```JPQL
나이가 평균보다 많은 회원
select m from Member m where m.age >= (
									select avg(m2.age )
									from Member m2 
									
)
```

```JPQL
%% 한건이라도 주문한 고객 %%
select m from Member as m where (select count(o) from Order o where m = o.member) > 0

```

- 메인 쿼리와 서브 쿼리가 동일 한 테이블을 사용하더라도 별칭을 따로 두어야지 성능이 잘 나옴

### 서브 쿼리 지원 함수

- EXISTS ( sub query ) : 서브 쿼리에 결과가 존재하면 참
- ALL ( sub query ) : 조건이 모두 만족하면 참
- ANY, SOME ( sub query ) : 조건을 하나라도 만족하면 참
- [NOT] IN ( sub query ) :서브 쿼리의 결과 중 하나라도 같은것이 있으면 참
- 예제
```JPQL
팀 A 소속인 회원
select m from Member m where EXISTS (
							select t from m.team where t.name = 'teamA'
)
```

```JPQL
전체 상품 각각의 제고보다 주문량이 많은 주문들

select o from Order o 
where o.orderAmount > ALL(select p.stockAmount from Product p)
```

```JQPL
어떤 팀이든 소속된 회원들
select m from Member as m where m.team ANY(select t from Team t)

```

### JPA 서브쿼리 한계

- JPA 는 WHERE, HAVING 절에서만 서브 쿼리 사용 가능
- select 절도 가능( 하이버 네이트에서 지원)
- From 절의 서브 쿼리는 현재 JPQL에서 불가능
	- 조인으로 풀 수 있으면 풀어서 해결