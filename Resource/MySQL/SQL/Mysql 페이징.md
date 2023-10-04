
---

## 페이징 이란?

- 페이징은 query의 결과를 가져올 때 일부만 가져오는 것을 의미

## Mysql 에서 페이징

### OffSet 방식

- 예를 들어 날짜순 상위 3개만 가져와보자.

```SQL
select * from table order by time limit 3 offset 0;
```

- limit : 가져올 행의 갯수
- offset : 시작 번호