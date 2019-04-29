<hr/>

# 목차
* [1. WITH 구문](#WITH)





# WITH

- WITH구문내의 쿼리 결과(SUB쿼리)가 여러번 사용될때(호출될때) 유용하다.
- 서브쿼리 블럭에 이름을 지정할 수 있도록 해줌.
- 오라클 옵티마이저는 쿼리를 인라인뷰나 임시 테이블로 여긴다.


## 문법

```swift
  WITH ALIAS명 AS (SUB쿼리)
  SELECT 컬럼명 FROM ALIAS명;

```

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





