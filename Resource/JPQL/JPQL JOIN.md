---

---
---

### JPQL - Inner Join

```JPQL
selet m from member m [inner] join m.team t
```

- 회원은 존재하고 Team이 존재하지 않을 경우 투플이 반환이 되지 않음


### JPQL - Outer Join

```JPQL
select m from Member as m LEFT [OUTER] JOIN m.team as t
```

- 회원은 존재하고 팀은 존재하지 않을 경우 team에 null이 담겨 반환


### JPQL -  Theta Join

``` JPQL
select count(m) from Member as m , Team as t where m.username = t.name
```

- Member as m, Team as t
	- JPQL에서 이 문장은 두 엔티티 테이블을 카티전 프로덕트 시킴

- 이후 조건문을 따라 팀이름과 사용자의 이름이 같은 투플을 반환시킴
- 연관관계가 없는 두 엔티티를 비교하고 싶을 때 사용한다.


### JPQL - ON 

- jpa 2.1 부터 지원
- 기능
	- 조인 대상 필터링
	- 연관관계가 없는 엔티티를 외부 조인 가능 (하이버네이트 5.1)