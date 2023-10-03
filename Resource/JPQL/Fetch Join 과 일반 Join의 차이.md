
--- 

## 일반조인

- 일반 조인 실행시 연관된 엔티티를 함께 조회하지 않는다.

```JQPL
select t from Team t join t.members m where t.name = '팀A'
```

- 이때 team 과 member 를 Join 하지만 오로지 team 만 sql projection 하는 것을 볼 수 있다.
- member data는 아직 초기화가 된것은 아니다.
- proxy는 아니지만 data가 없기 때문에 추가적으로 query가 나간다.

## Fetch Join

```JPQL
select t from Team t join fetch t.members as m where t.name = '팀A'
```

- 실행된 SQL을 확인해 보면 team 뿐만 아니라 member의 column 까지 projection 된 것을 확인할 수 있다.
- 또한 쿼리도 오로지 한번만 나간다.


## 정리

- JPQL은 결과를 반환할 때 연관관계를 고려하지 않는다.
- 단지 SELECT 절에 지정한 엔티티만 조회한다.
- 따라서 Fetch Join시에만 같이 조회한다.