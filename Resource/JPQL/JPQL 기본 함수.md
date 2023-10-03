
---


## CONCAT

- 두 문자를 더하는 함수
- 예시
```JQPL
select concat('a','b') from Member as m
```


## SUBSTRING

- 문자열의 일부를 추출하는 함수
- 예시
```JPQL
select substring(m.username,1,3) from Member as m
```

- subString 함수의 매개 변수
	- 1. 문자열
	- 2. 시작 인덱스 (1 부터 시작)
	- 3. 반환 문자 갯수

## Trim 

- 공백 제거 기능


## LOWER, UPPER

- 각각 대소문자 변환 함수


## Length

- 문자의 길이

## Locate

- Locate( 찾는 문자열, 문자열) 으로 사용
- 문자열 내부에서 찾는 문자열의 시작점을 반환한다.
- 찾는 문자열이 문자열 내부에 존재하지 않을 경우 0 을 반환
- 예시
```JPQL
select locate('de','abcdef') from Member as m
```

## SIZE, INDEX(JPA 용도)

### SIZE

- size 함수는 양방향 연관관계가 걸린 엔티티 내부의  컬렉션의 크기를 반환해줌
```JPQL
select size(t.members) from Team as t
```

### INDEX

- 값타입 컬렉션 내부의 값의 위치를 반환할 때 사용
- 사용하지 않는 것을 추천