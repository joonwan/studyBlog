
---

## Entity Manager Factory

- Entity Manager Factory 는 Entity Manager를 생성하는 역할을 한다.
- Client에게 요청이 올 경우 Entity Manager Factory는 Client에게 각각 다른 Entity Manager를 생성해서 반환한다.

![[Pasted image 20231003200449.png]]

## Entity Manager

- Entity Manager은 Entity Manager Factory에 의해 생성이 된다.
- 생성된 Entity Manager는 Connection Pool에서 Connection을 획득하여 DB에 접근한다.
- Entity Manager가 생성될 때 1:1로 [[영속성 Context]] 가 생성된다.
