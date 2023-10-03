
-- --

## 기본 CASE 식

```JQPL
select 
	case when m.age <= 10 then '학생요금'
		 when m.age >= 60 then '경로요금'
	     else '일반요금'
	end
from Member m
```

## 단순 CASE식

```JPQL
select 
	case t.name
		when '팀A' then '인센티브110%'
		when '팀B' then '인센티브120%'
		else '인센티브 105%'
	end
from Team t
		
```

## COALESCE

- 하나씩 조회해서 Null 이 아니면 반환
- 멤버의 이름이 없을 경우 '이름 없는 회원 ' 이라고 반환하는 예제
```JPQL
select coalesce(m.username, '이름 없는 회원') from Member as m
```


## NULLIF 

- 두 값이 같으면 Null 반환, 다르면 첫번째 값 반환
- 회원 이름이 관리자일때 null을 반환하는 예제

```JPQL
select nullif(m.username,'관리자') from Member m
```
