# PL / SQL (Procedual language Sql)

<hr/>

# 목차
* [1. %TYPE 데이터형 ](#타입데이터형)
* [2. %ROWTYPE 데이터형 ](#로우타입데이터형)




## 개념
- 오라클에서 제공하는 프로그래밍 언어
- 일반 프로그래밍 언어적인 요소를 다 가지고 있고, 데이터베이스 업무를 처리하기 위한 최적화된 언어


## 기본구조 : Block Structure
- PL/SQL은  프로그램을 논리적인 블록으로 나누는 구조화된 블록언어
```swift

DECLARE 
  - optional
  - Variables, Cursors, user-defined
    exceptions
    
BEGIN
  - Mandatory
  - SQL Statements
  - PL/SQL Statements
 
EXCEPTION
  - Actions to perform when errors occur
  
END
  - Mandatory

```

1. 선언부(Declare) 
- 모든 변수나 상수를 선언하는 부분
- 변수, 상수, CURSOR, USER_DEFINE Exception 선언

2. 실행부(Executable) 
- 실행부는 BEGIN으로 시작하고 END로 종료된다.
- 제어문, 반복문, 함수정의 등의 로직을 기술하는 부분 

3. 예외처리부(Exception) 
- 예외에 대한 처리
- 일반적으로 오류를 정의하고 처리하는 부분으로서 선택사항임
- 실행도중에 에러 발생시 해결하기 위한 명령들을 기술하는 부분



## 작성요령
- 한 문장이 종료할 때마다 세미콜론(;)을 사용한다.
- END 뒤에 세미콜론(;)을 사용하여 하나의 블록이 끝났다는 것을 명시한다.


## PL/SQL 블록유형

1. 익명블록(anonymous PL/SQL Block) 
- 이름이 없는 블록을 의미하며, 실행하기 위해 프로그램 안에서 선언 되고 실행시에 실행을 위해 PL/SQL 엔진으로 전달된다.
- 주로 일회성으로 사용할 경우 많이 사용된다.
- 선행 컴파일러 프로그램과 SQL * Plus 또는 서버 관리자에서 익명의 블록을 내장 할 수 있다.

2. 프로시저 : Procedure
- 특정 작업을 수행할수 있는 이름이 있는 PL/SQL 블록으로서, 매개변수를 받을수 있고, 반복적으로 사용할수 있다.
- 보통 연속 실행 또는 구현이 복잡한 트랜잭션을 수행하는 PL/SQL블록을 데이터베이스에 저장하기 위해 생성한다.

3. 함수 : Function
- 보통 값을 계산하고 결과값을 반환하기 위해서 함수를 많이 사용한다.
- 대부분 구성이 프로시저와 유사하지만 IN 파라미터만 사용할 수 있고, 반드시 반환 될 값의 데이터 타입을 RETURN문에 선언해야 한다.
- 또한 PL/SQL블록 내에서 RETURN문을 통해서 반드시 값을 반환해야 한다.

- 저장블록(stored PL/SQL Block) : 서버에 저장해 놓고 주기적으로 반복해서 사용할 경우 사용된다.


# 데이터타입

## 1. 일반변수 선언문법
```
  identifier[CONSTANT] 데이터타입 [NOT NULL]
```
- 변수의 이름은 sql의 object명과 동일한 규칙을 따른다.
- 초기값은 할당연산자 (:=)를 사용하여 정의한다.
- 초기값을 정의하지 않으면, 변수는 null값을 가지게 된다.

예제
```swift
  v_price CONSTANT NUMBER(4,2) := 12.34;
  v_name VARCHAR2(20);
  v_Bir_Type CHAR(1);
  
  v_flag BOOLEAN NOT NULL := TRUE;
  v_birthday DATE;
```

# 타입데이터형
- %Type 데이터형은 기술한 데이터베이스 테이블의 컬럼 데이터 타입을 모를 경우 사용할 수 있고, 코딩 이후 데이터베이스 컬럼의 데이터 타입이 변경될 경우 다시 수정할 필요가 없다. 
- 이미 선언된 다른 변수나 데이터베이스 컬럼의 데이터 타입을 이용하여 선언한다.
- 데이터베이스 테이블과 컬럼 그리고 이미 선언한 변수명이 %TYPE 앞에 올 수 있다.

### %TYPE 장점
- DB column definition을 정확히 알지 못하는 경우에 사용할 수있다.
- DB column definition이 변경 되어도 다시 PL/SQL을 고칠 필요가 없다.

### 예제

```swift

CREATE OR REPLACE PROCEDURE Emp_Info
  (p_empno IN emp.empno%TYPE)
   -- in parameter

IS

  -- %type 데이터형 변수 선언
  v_empno emp.empno%TYPE; emp.empno%TYPE;
  v_ename emp.ename%TYPE;
  v_sal emp.sal%TYPE;

BEGIN

  DBMS_OUTPUT.ENABLE;
  
  -- %TYPE 데이터형 변수 사용
  SELECT empno, ename, sal
  INTO v_empno, v_ename, v_sal
  FROM emp
  WHERE empno = p_empno;
  
  --결과값 출력
  
  DBMS_OUTPUT.PUT_LINE('사원번호 : ' ||v_empno);
  DBMS_OUTPUT.PUT_LINE('사원이름 : ' ||v_ename);
  DBMS_OUTPUT.PUT_LINE('사원급여 : ' ||v_sal);

  END;
 /
 

/* 결과값 출력 */

SQL > SET SERVEROUTPUT ON;
SQL > EXECUTE Emp_Info(7369);


```

# 로우타입데이터형
- 하나 이상의 데이터값을 갖는 데이터 타입으로 배열과 비슷한 역할을 하고 재사용이 가능하다. %ROWTYPE 데이터형과, PL/SQL테이블과 레코드는 복합 데이터 타입에 속함

## %ROWTYPE 
- 테이블이나 뷰 내부의 컬럼 데이터형, 크기, 속성등을 그대로 사용할 수있다.
- %ROWTYPE 앞에 오는 것은 데이터베이스 테이블 이름이다.
- 지정된 테이블의 구조와 동일한 구조를 갖는 변수를 선언할 수 있다.
- 데이터베이스 컬럼들의 수나 DATATYPE을 알지 못할 때 편리 하다.
- 테이블의 데이터 컬럼의 DATATYPE이 변경 될 경우 프로그램을 재수정할 필요가 없다.

```swift 

CREATE OR REPLACE PROCEDURE RowType_Test
(p_empno IN emp.empno%TYPE)

IS
  -- emp 테이블의 속성을 그대로 사용할 수 있다.
  v_emp emp%ROWTYPE;

BEGIN

  DBMS_OUTPUT.ENABLE;
  
  -- %ROWTYPE 데이터형 변수 사용
  SELECT empno, ename, hiredate
  INTO v_emp.v_empno, v_emp.v_ename, v_emp.hiredate
  FROM emp
  WHERE empno = p_empno;
  
  --결과값 출력
  
  DBMS_OUTPUT.PUT_LINE('사원번호 : ' ||v_emp.v_empno);
  DBMS_OUTPUT.PUT_LINE('사원이름 : ' ||v_emp.v_ename);
  DBMS_OUTPUT.PUT_LINE('입사일 : ' ||v_emp.hiredate);

  END;
 /
 

/* 결과값 출력 */

SQL > SET SERVEROUTPUT ON;
SQL > EXECUTE RowType_Test(7369);

```




