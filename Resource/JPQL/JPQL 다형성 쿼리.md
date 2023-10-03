
---
![[Pasted image 20231003171840.png]]

- Item을 상송받은 Album, Movie, Book 객체가 존재한다.
- JPA 는 특수한 기능을 제공한다.

---

## JPA 가 제공하는 특수한 기능들


### 1. 조회 대상을 특정 자식으로 지정

- 예를 들어 Item 중에 Book 과 Movie 를 조회하라

```JPQL
select i from Iteam I where type(i) in (Book, Movie)
```

- 이런식으로 특정 자식만 조회가 가능하다.

```SQL
select i from i where i.DTYPE in ('B','M')
```
-  위 쿼리는 실제 DB에 나가는 쿼리이다.
- type(i) 는 DTYPE 으로 바뀌어서 나간다.

###  2. TREAT (JPA 2.1)

- 자바의 타입 캐스팅과 유사
- 상속 구조에서 부모타입을 특정 자식 타입으로 다룰 때 사용
- From , Where, Select 에서 사용한다.
	- 하이버 네이트에서 지원
- 예시 
```JQPL
select i from Item i where treate(i as Book).author = 'Kim'
```
