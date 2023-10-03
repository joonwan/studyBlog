-- --

## 문자

- 'Hello' 같이 문자를 표현시 '' 사이에 원하는 문자를 넣으면 된다.
- ' ' 사이에 ' 를 표현하고 싶을때는 '' 처럼 ' 를 두개 넣어주면 된다.
	- 'she''s' -> she's

## 숫자

- 10L -> Long
- 10D -> Double
- 10F -> Float

## Boolean

- TRUE
- FALSE


## Enum
- enum type
- jpabook.MemberType.Admin 처럼 패키지 명을 포함해서 적어야 한다.
- 하지만 Parameter Binding을 하면 복잡하지 않게 작성할 수 있다.
- 예시 코드

```Java
// no parameter binding
List<Oject[]> res = em.createQuery(
			"select m.username, 'hello', true from Member m
					  where m.memberType = jpabasic.ex1hellojpa.jpql.MemberType.ADMIN"
).getResultList();
```

```Java
// parameter binding
List<Object[]> res = em.createQuery(
					"select m.username, 'hello', true from Member as m where m.memberType = : userType"
). setParameter("userType", MemberType.ADMIN)
.getResultList();

```


## Entity Type

```JPQL

select i from Item i where type(i) = Book, Item.class
```

- type(i) = Book : d_type을 확인해주는 역할이다.
- 상속관계에서 사용한다.
- 잘 사용하지는 않는다.

