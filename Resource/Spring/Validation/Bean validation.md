
---

## What is Bean Validation?

 Bean Validation이란 특정 구현체가 아닌 Bean Validation 2.0이라는 기술 표준이다. 즉 검증 annotation 과 여러 인터페이스의 모음이다. 이에대한 구현체는 하이버네이트 Validator 이다.


## 검증 Annotation

### @NotBlank 

 빈값 과 공백만 있는 경우를 허용하지 않는 Annotation

## @NotNull

 null 값을 허용하지 않는 Annotation

## Range(min,max)

min <= x <= max 이내의 값만 허용하는 Annotation

## Max(x)

 최대 x 까지만 허용하는 Annotation
