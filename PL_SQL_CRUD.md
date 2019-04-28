# PL/SQL INSERT, UPDATE, DELETE

<hr/>

# 목차
* [1. INSERT ](#INSERT)
* [2. UPDATE ](#UPDATE)




# INSERT

- 사원 등록 예제

```swift

CREATE OR REPLACE PROCEDURE Insert_Test
  (
    v_empno IN emp.empno%TYPE,
    v_ename IN emp.ename%TYPE,
    v_deptno IN emp.deptno%TYPE
  )

IS

BEGIN
   DBMS_OUTPUT.ENABLE;
   
   INSERT INTO emp(empno, ename, hiredate, deptno)
   VALUES(v_empno, v_ename, sysdate, v_deptno);
   
   COMMIT;

END;
/

```




# UPDATE

- 특정 사원의 급여를 인상 / 인하 하는 예제 

```swift

CREATE OR REPLACE PROCEDURE Update_Test
(
   v_empno IN emp.empno%TYPE,   -- 급여 수정 사번
   v_rate IN NUMBER  -- 급여 인상/인하율
)


IS
-- 수정 데이터를 확인하기 위한 변수 선언

  v_emp emp%ROWTYPE;

BEGIN

  DBMS_OUTPUT.ENABLE;
  
  UPDATE emp
  SET sal = sal + (sal * (v_rate/100)) -- 급여 인상률 계산
  WHERE empno = v_empno;
  
  COMMIT;
  
  DBMS_OUTPUT.PUT_LINE('데이터 수정 성공');
  
  SELECT empno, ename, sal
  INTO v_emp.empno, v_emp.ename, v_emp.sal
  FROM emp
  WHERE empno = v_empno;
  
  DBMS_OUTPUT.PUT_LINE(' ***** 수정 확인 ***** ');
  DBMS_OUTPUT.PUT_LINE('사원번호 : ' || v_emp.empno);
  DBMS_OUTPUT.PUT_LINE('사원이름 : ' || v_emp.ename);
  DBMS_OUTPUT.PUT_LINE('사원급여 : ' || v_emp.sal);

END;
/

 -- 7900 번 사원의 급여 10% 인하
SQL > EXECUTE Update_Test(7900, -10);

```
