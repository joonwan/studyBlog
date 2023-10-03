
---


- 크게 4가지로 나뉜다.

	- 비영속 상태 (new)
	- 영속 상태    (managed)
	- 준영속 상태(detached)
	- 삭제           (removed)

![[Pasted image 20231003205009.png]]


## 비영속 상태

![[Pasted image 20231003224318.png]]

- 엔티티를 생성만 한 상태
- 생성된 엔티티는 영속성 컨텍스트에서 관리 하지 않는다.
```Java
Member member = new Member();
```


## 영속 상태

![[Pasted image 20231003224543.png]]

- 비영속 상태인 엔티티를 em.persist() 를 이용해 영속상태로 만들 수있다.
```Java
em.persist(member);
```

- 엔티티가 영속화 되어 영속성 컨텍스트에서 관리된다.
- 이렇게 영속화 시킨다고 해서 DB에 저장되는 것은 아니다.

## 준영속 상태

- 영속성 컨텍스트에서 더이상 해당 엔티티를 관리하지 않는 상태를 말한다.
- em.detatch() 를 사용하면 된다.

## 삭제 상태

- db 에 해당 데이터를 삭제 요청한다.
- em.remove()