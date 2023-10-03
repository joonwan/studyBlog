
---

## Persistence Context란?

- 엔티티를 영구저장 할  수 있는 환경을 의미한다.
- 이는 논리적인 개념으로 눈에 보이지는 않는다.
- Entity Manager를  통해 영속성 Context에 접근이 가능한다.
	- [[Entity Manager Factory and Entity Manager]] 참고


---

## entityManager.persistence()

- DB에 바로 엔티티를 저장한다는 의미가 아니다.
- 이는 엔티티를 영속성 컨텍스트에서 관리하게끔 만든다 라는 의미이다.
- 실제 DB에 반영되는 시점은 flush 되는 시점이다.

___
## Persistence Context의 장점

- 1차 캐시
	- 영속성 컨텍스트 내부에 1차 캐시를 가지고 있다.
	- ![[Pasted image 20231003230044.png]]
	- 영속성 컨텍스트에서 관리되고 있는 엔티티를 조회할 경우 DB에 쿼리를 날리지 않고 1차 캐시 에서 id 값을 조회하여 해당되는 엔티티를 반환한다.
	- 만약 1차 캐시에서는 존재하지 않고 DB에만 존재할 경우 
		- 1. 1차 캐시 확인
		- 2. DB 조회
		- 3. 조회 결과를 1차 캐시에 저장
		- 4. 반환
	- Entity Manager는 DB Transaction 단위로 만들어지고 종료되기 때문에 큰 성능의 이점은 없다.

- 영속 Entity의 동일성 보장
	- 