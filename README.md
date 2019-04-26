# PL / SQL (Procedual language Sql)
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
- 
3. 예외처리부(Exception) 
- 예외에 대한 처리
- 일반적으로 오류를 정의하고 처리하는 부분으로서 선택사항임
- 실행도중에 에러 발생시 해결하기 위한 명령들을 기술하는 부분

4. 기타
- 익명블록(anonymous PL/SQL Block) : 주로 일회성으로 사용할 경우 많이 사용된다.
- 저장블록(stored PL/SQL Block) : 서버에 저장해 놓고 주기적으로 반복해서 사용할 경우 사용된다.


## 작성요령
- 한 문장이 종료할 때마다 세미콜론(;)을 사용한다.
- END 뒤에 세미콜론(;)을 사용하여 하나의 블록이 끝났다는 것을 명시한다.



