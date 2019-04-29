<hr/>

# 목차
* [1. WITH 구문](#WITH)





# WITH

- WITH구문내의 쿼리 결과(SUB쿼리)가 여러번 사용될때(호출될때) 유용하다.
- 서브쿼리 블럭에 이름을 지정할 수 있도록 해줌.
- 오라클 옵티마이저는 쿼리를 인라인뷰나 임시 테이블로 여긴다.
- 복잡한 SQL에서 동일 블록에 대해 반복적으로 사용하는 경우, 그 블록을 재사용 할 수 있게 함으로서 성능향상을 높일 수 있는데 이때 with절을 이용하여 미리 Query Block을 만들 수 있다. with에 의해 만들어 지는 것은 메모리에 존재하는 가상 테이블 정도이다.
- 개발시에, UNION을 이용하여 데이터를 가져오는 경우 QUERY가 조건절 등에서 반복되는 경우가 있다. 이때 with로 빼냈을때와 안빼냈을때는 큰 속도 차이를 보임


## WITH 특징
- 자주 사용되는 query를 사용하기 전에 with 절로 미리 query block으로 정의한 후 사용한다.
- 서브쿼리문은 sub query에 의해 실행된 결과에 의해 main query가 실행되기 때문에 서브쿼리문은 성능이 저하된다.
- with절을 통해 서브쿼리를 보다 쉽고 간편하게 사용할 수 있게 한다.
- with절은 여러개의 sub query가 하나의 main query에서 사용될때 생기는 복잡성을 간결하게 정의함 으로서, 서브쿼리에서 발생할 수 있는 성능 저하 현상을 방지할 수 있다.
- 먼저, 하나의 main query에 정의될 sub query를 with절과 함께 선언하고, 각각의 sub query가 정의될 때 sub query를 대신할 인라인 뷰의 이름을 사용자가 적절히 정의한다. 여러개의 sub query가 사용된다면 순서대로 선언하면 된다.
- sub query의 선언이 끝나면, 실제로 실행 될 main query절을 작성하는데, 필요한 sub query는 인라인 뷰의 이름으로 새로운 sub query를 작성하여 사용한다.



## 문법

```swift
  WITH ALIAS명1 AS (SUB쿼리1),
       ALIAS명2 AS (SUB쿼리2),
       ...
  SELECT 컬럼명 FROM ALIAS명;

```

## 문법 유의사항

1. with절 속에 반드시 select문이 있어야 한다.
2. query명과 기존의 테이블명이 동일하게 사용되는 경우, 쿼리 블럭명이 우선함
3. 하나의 with절에 여러개의 query block이 사용 가능하다.
4. with절을 불러서 사용하는 body영역에서는 block명이 우선되므로 테이블명은 사용할 수 없다.
5. with절 내에 또 다른 with절을 포함할 수 없다.
6. set operator를 사용한 쿼리에서는 사용할 수 없다.



## 예시

```swift

WITH with_Test AS
(
   SELECT ROWNUM, 'test1', SYSDATE FROM DUAL
   UNION ALL
   SELECT ROWNUM, 'test2', SYSDATE FROM DUAL
   UNION ALL
   SELECT ROWNUM, 'test3', SYSDATE FROM DUAL
)
SELECT * FROM with_Test
;

```

```
결과 

ROWNUM     'TEST1'     SYSDATE
1           test1       sysdate      
1           test2       sysdate
1           test3       sysdate

```

## 뷰 처럼 사용할 수도 있다.

```swift
 WITH with_Test1 AS
(   
   select * from emp
),
with_Test2 AS
(
   select * from emp
),
with_Test3 AS
(
  select T1.empno, T2.ename, T2.sal
  from with_Test1 T1, with_Test2 T2
  where T1.empno = T2.empno
    and T1.ENAME LIKE 'S%'
)
select * 
from with_Test3
;


```

```
  결과
  EMPNO    ENAME    SAL
  7369	   SMITH	  800
  7788	   SCOTT	  3000
  
  
  위와 같이 WITH문으로 임시테이블이나 View처럼 사용할 수도 있다.
 
```





