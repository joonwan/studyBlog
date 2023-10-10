
---

## DB의 물리적 저장

 우리가 SQL을 작성하고 실행하면 DBMS에 의해 처리방법이 결정되고 운영체제를 통해 각 장치에 명령이 내려저 작업이 처리된다. 최종적으로 저장된 data는 운영체제에 종속적인 data base file로 저장된다.

## Data의 저장 위치

 실제 query에 의해 저장되는 data는 보조 기억장치에 저장된다. 즉 HDD, SSD, USB 등에 저장된다. 주로 HDD에 저장된다.

## HDD 의 Data 접근 원리

 하드 디스크에 대해 알아보자. 하드디스크는 아래와 같은 구성요소로 이루어 져 있다.

- plate
	- track
		- sector
- arm
- header

 플레이트는 논리적으로 트랙으로 나뉘며 트랙은 다시 몇개의 섹터로 나뉜다.

  원형 플레이트는 빠른 속도로 회전하며 회전하는 플레이트를 하드디스크의 암과 헤더가 접근하여 원하는 섹터에서 데이터를 가져온다. 

## Data Access Time

하드 디스크의 입출력 시간을 Access Time 이라고 한다. Access time은 아래의 요소로 구성되어 있다.

- seek time
	- 엑세스 헤드를 트랙에 이동시키는 시간
- rotational latency time 
	- 섹터가 엑세스 헤드에 접근하는 시간
- data transfer time
	- 데이터를 주기억장치로 불러오는 시간