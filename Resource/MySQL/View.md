
---

## View 란 무엇인가?

View 란 하나 이상의 테이블을 합하여 만든 가상의 테이블을 의미한다. 여기서 합한다의 의미는 Select 문을 통해 얻은 최종 결과를 의미하며 뷰는 이러한 결과를 가상의 테이블로 정의하여 실제 테이블 처럼 사용할 수 있도록 만든 DB 개체이다.

## View 를 써야하는 이유

실제 물리적인 테이블에 열을 추가한다고 가정하자. 이는 사용중인 프로그램을 수정 해야하며 DB 의 저장 용량이 늘어나는 문제점이 발생한다. 또한 이는 특정 개체의 속성이 변경될 경우 다른 연관된 테이블의 속성을 수정 해야하는 번거로움이 생긴다.

## View 를 사용시 장점

 View 를 이용해 가상의 테이블을 만들면 이는 실제 데이터를 디스크에 저장하는것이 아닌 단지 View 를 생성할 때 사용한 Select 문의 정의 를 DBMS 에 저장한다. 저장한 View 를 사용할 경우 DBMS 는 View 의 정의를 참조하여 질의를 수행하고 그 결과를 사용자에게 반환한다.

- 편리성 및 재사용성
	- 미리 정의된 뷰를 일반 테이블 처럼 사용할 수 있어 편리
	- 자주 사용되는 질의를 뷰로 만들어 놓으면 재사용 가능하다.
- 보안성
	- 각 사용자별로 보안이 필요한 데이터를 제외하여 선별하여 보여줄 수 있다.
- 독립성
	- 논리 데이터 베이스의 원본 테이블의 구조가 변해도 응용 프로그램에 영향을 주지 않고록 하는 논리적 독립성을 제공한다.

## View 의 생성

```MySQL
create view view_name [(row_name[,....n])]
as select -- implementation your query 
```

- 예제

Book table 에서 축구가 들어간 책만 보여주는 뷰를 만들자

```MySQL
create view vw_book
select as *
from book
where bookname like "%축구%"
```


## View 의 수정

 물리적인 테이블의 수정 작업과 같이 뷰도 필요에 따라 정의된 SQL 문의 수정이 필요하다. 이는 앞서 배운 View 생성문법중 `create` 뒤에 `or replace` 만 붙여줘도 된다.

```MySQL
create or replace view yourViewName(row_name ....)
as select -- implementation your query
```

## View 의 삭제

 뷰를 삭제하고 싶으면 drop 문을 쓰면 된다.

```MySQL
drop view view_name;
```


## System View

 



참고자료 : MySQL로 배우는 데이터 베이스와 실습 - 한빛 미디어
