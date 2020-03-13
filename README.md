DDL
===   
- 데이터 정의 언어
- 객체를 만들고, 수정하고 삭제하는 구문,
- CREATE(생성), ALTER(수정),DROP(삭제)
   
   
SET OPERATOR
=====

## SET OPERATOR(집합연산자) H2   
   - 두개 이상의 테이블에서 조인을 사용하지 않고, 연관된 데이터를 조회하는    방법
   - 여러 개의 결과를 하나로 결합하는 방식
   - 집합연산자는 ROW를 추가하여 데이텨를 표현    
   
- 집합 연산자의 성립 조건   

   1. SELECT 절의 컬럼수가 동일해야하고
   2. SELECT 절의 동일위치에 존재하는 컬럼 데이터가 상호 호환 가능해야함.
   
- 두 개 쿼리문을 각 집합연산자를 이용해 처리    
```
SELECT EMP_ID,EMP_NAME,
DEPT_CODE, SALARY FROM
EMPLOYEE
WHERE DEPT_CODE = 'D5’;    

   
SELECT
EMP_ID,EMP_NAME,DEPT_CODE,
SALARY FROM EMPLOYEE
WHERE SALARY > 3000000;
```   
   1. UNION(중복된 영역 제외하고 합)      
   ```
   SELECT EMP_ID,EMP_NAME,
   DEPT_CODE, SALARY FROM
   EMPLOYEE
   WHERE DEPT_CODE = 'D5'
   UNION
   SELECT
   EMP_ID,EMP_NAME,DEPT_CODE,
   SALARY FROM EMPLOYEE
   WHERE SALARY > 3000000;   
   ```   
   
   - __심봉선, 대북혼 중복데이터 1 개만출력__
   - __EMP_ID로 로 오름 차순 정렬__   
      
   2. UNION ALL(중복데이터도 포함해 합)   
      
   ```
   SELECT EMP_ID,EMP_NAME,
   DEPT_CODE, SALARY FROM
   EMPLOYEE
   WHERE DEPT_CODE = 'D5'
   UNION ALL
   SELECT
   EMP_ID,EMP_NAME,DEPT_CODE,
   SALARY FROM EMPLOYEE
   WHERE SALARY > 3000000;
   ```   
   
   - __심봉선, 대북혼 중복데이터 모두 출력__   
      
      
   3. INTERSECT(합집합)     
      
   ```
   SELECT EMP_ID,EMP_NAME,
   DEPT_CODE, SALARY FROM
   EMPLOYEE
   WHERE DEPT_CODE = 'D5’
   INTERSECT
   SELECT
   EMP_ID,EMP_NAME,DEPT_CODE,
   SALARY FROM EMPLOYEE
   WHERE SALARY > 3000000;
   ```   
  
      
   - __심봉선, 대북혼 중복데이터만 출력__   
   
      
   4. MINUS(차집합)     
   
   ```
   SELECT EMP_ID,EMP_NAME,
   DEPT_CODE, SALARY FROM
   EMPLOYEE
   WHERE DEPT_CODE = 'D5’
   MINUS
   SELECT
   EMP_ID,EMP_NAME,DEPT_CODE,
   SALARY FROM EMPLOYEE
   WHERE SALARY > 3000000;
   ```   
   
   - __첫번째 SELECT 문에서 심봉선, 대북혼 중복데이터 제외하고 출력__   
   
   
## SUBQUERY H2   
   - 하나의 SELECT 문장 안에 포함된 또 하나의 SELECT 문장
   - 서브쿼리는 메인쿼리 실행 전 한번만 실행   
   
   *서브쿼리조건*   
      
      1. 서브쿼리는 반드시 소괄호..
      2. 서브쿼리는 연산자의 오른쪽에 위치
      3. 서브쿼리 내에서 ORDER BY 문법은 지원 X(예외O)
      4. 서브쿼리와 비교할 항목은 서브쿼리의 SELECT한 학목의 개수와    자료형이 일치해야함.
      

 - SUBQUERY 유형   
   1. 단일행 SUBQUERY   
   ```
   SELECT EMP_ID,
   EMP_NAME,
   DEPT_CODE,
   SALARY
   FROM EMPLOYEE
   WHERE
   SALARY >=
   (SELECT AVG(SALARY)
   FROM EMPLOYEE);
   ```   
   
   2. 
      

  
   
   
   


   
  ## **CREATE**   
  
    : DDL의 한 종류로 테이블이나 인덱스,유저 등 다양한 데이터베이스 객체를 생성하는 구문

  1. 관리자 계정과 사용자 계정   
  
  : 관리자는 DB의 생성과 관리를 담당하는 계정으로, 모든 권한과 책임을 가지고, 사용자 계정은 DB에 대해      질의,갱신,보고서 작성등을 수행할 수 있는 계정

  2. **사용자 생성**   
    - CREATE USER 사용자 이름 IDENTIFIED BY 비밀번호;
    - 생성 하더라도  아직 권한이 없기에 관리자 계정에서 권한을 부여해 줘야한다.
    
  3. **GRANT(권한부여)**   
    - CONNECT: 사용자가 DB에 접속 가능하도록 하기위한 CREATE와 SESSION 권한이 있는 ROLE
    - RESOURCE: 객체를 생성할 수 있는 권한과 ISERT,UPDATE,DELETE의 권한을 가진 ROLE
    - EX) GRANT RESOURCE,CONNECT TO KH;
    
  4. **테이블 생성**   
    ```
    - CREATE TABLE 테이블명 (   
        ID VARCHAR2(20),   
        PW VARCHAR2(20),   
        NAME VARCHAR2(40)   
      );
    ```  
    - 테이블에 주석 달기   
        - ```
          COMMENT ON COLUMNN 테이블명.컬럼명 IS '주석내용';
          ```

    5. **제약 조건**         
      - 테이블 작성 시 각 컬럼에 대한 기록에 대해 제약 조건 설정 가능
      - 데이터 무결성을 지키기 위한 제한된 조건   

        제약조건 | 설명
        ---|:---:|
        'NOT NULL | 데이터에 NULL을 허용하지 않는다.
        'UNIQUE' | 중복된 값을 허용하지 않는다.
        'PRIMARY KEY' | NOT NULL + UNIQUE, 컬럼의 고유식별자로 사용됨.
        'FOREIGN KEY' | 참조되는 테이블의 컬럼 값이 존재하면 허용한다.
        'CHECK' | 저장 가능한데이터 값의 범위나 조건을 지정하여 설정한 값만 허용한다.


        1. **NOT NULL**  
        
            ```
            CREATE TABLE USER_NOCONS(
            USER_NO NUMBER NOT NULL
            );
            ```     
          **NOT NULL은 데이터레벨에서는 사용 X**   
          
           
        2. **UNIQUE**   
        
          ```
          CREATE TABLE USER_NO(
          USER_ID VARCHAR2(20) UNIQUE
          );
          ```   
          
          또는   
          
          ```
          CREATE TABLE USER_NO(
          USER_ID VARCHAR2(20),
          UNIQUE(USER_ID
          );
          ```         
          
        3. **PRIMARY KEY**   
        
          ```
          CREATE TABLE NO(
            USER_NO NUMBER PRIMARY KEY
          );
          ```      
          
          또는   
          
          ```
          CREATE TABLE NO(
            USER_NO NUMBER
            PRIMARY KEY(USER_NO)
          );
          ```      
          
        4. __FOREIGN KEY__      
        
           - 참조 무결성을 유지하기 위한 제약조건
           - 참조된 다른 테이블이 제공하는 값만 사용할 수 있도록 제한하는 것   
           
           ```
           CREATE TABLE NO(
            USER_NO NUMBER
            PRIMARY KEY(USER_NO)
           );
           ```      
           
           - 참조하여 테이블 작성   
           
            ```   
            CREATE TABLE NO2(
              PRODOCTOR_NO NUMBER REFERENCSE NO(USER_NO)
            );
            ```      
            
            또는   
            
            ```
            CREATE TABLE NO2(
              PRODOCTOR_NO NUMBER ,
              FOREIGN KEY (PRODOCTOR_NO) REFERENCSE NO(USER_NO)
            );
            ```  
            
            - **ON DELETE SET NULL과 ON DELETE CASCADE**
               - 둘 모두 FOREIGN 뒤에 쓰게되면 참조하는 테이블이 삭제가 가능하다.
               - ON DELETE SET NULL은 참조된 컬럼의 데이터 삭제시 해당 컬럼만 삭제하고 나머지 데이터 유지   
               - ON DELETE CASCASE는 참조된 컬럼의 데이터 삭제시 데이터 전부 소거
   
         5. **CHECK**    
         
         
            - 해당 컬럼에 입력되거나 수정되는 값을 체크, 설정된 값 이외의 값이면 에러 발생   
            
            ```
            CREATE TABLE USER(
              GENDER CHAR(6) CHECK (GENDER IN('남','여','중성'))
            );
            ```   
            
            
       **SUBQUERY를 이용해 테이블 작성**   
       
          ```
          CREATE TABLE EMPLOYEE_COPY
          AS
          SELECT EMP_ID,EMP_NAME,DEPT_TITLE,JOB_NAME FROM EMPLOYEE   
          LEFT JOIN DEPARTMENT ON (DEPT_CODE = DEPT_ID)
          LEFT JOIN JOB USING(JOB_CODE);
          ```      
          
 ## ALTER H2   
 
  - DDL의 한 종류로 CREATE로 정의된 내용을 수정할 때 사용
  - <U>컬럼의 추가 삭제, 제약조건의 추가 삭제, 컬럼의 자료형 변경, 테이블명 컬   럼명 제약조건 이름 변경 등이 가능</U>      
  
  
  1. **컬럼 추가**   
  
    - ```
      ALTER TABLE DEPT_COPY 
      ADD(KNAME VARCHAR2(20));
      ```   
    - ```
      ALTER TABLE DEPT_COPY
      ADD (HNAME VARCHAR2(20) DEFAULT ‘kh’);
      ```   

  2. **컬럼 수정**   
  
    - '''
      ALTER TABLE DEPT_COPY  
      MODIFY DEPT_ID CHAR(3)    
      MODIFY DEPT_TITLE VARCHAR2(30)
      '''   
    
    
  3. **제약조건 확인**   
  
    - '''
    SELECT UC.CONSTRAINT_NAME, -- 제약조건 이름
           UC.CONSTRAINT_TYPE, -- 제약조건 타입
           UC.TABLE_NAME, -- 테이블이름
           UCC.COLUMN_NAME, -- 컬럼이름
           UC.SEARCH_CONDITION -- 제약조건 설명
           FROM USER_CONSTRAINTS UC
           JOIN USER_CONS_COLUMNS UCC ON (UC.CONSTRAINT_NAME =
           UCC.CONSTRAINT_NAME)
           WHERE UC.TABLE_NAME = ‘DEPT_COPY’; -- 테이블명(반드시 대문자로 기입)
       '''   
       
        
  4. **제약조건 추가**   
  
    - '''
      ALTER TABLE DEPT_COPY
      ADD CONSTRAINT DCOPY_ID_PK PRIMARY KEY(DEPT_ID)  --CONSTRAINT:  제약조건
      ADD CONSTRAINT DCOPY_TITLE_UNQ UNIQUE(DEPT_TITLE)
      MODIFY HNAME(컬럼명) CONSTRAINT DCOPY_HNAME_NN(제약이름) NOT NULL;  
      '''   
 
      **NOT NULL은 은 MODIFY로 로 추가**   
      
        
  5. **컬럼 삭제**   
  
    - '''
      ALTER TABLE DEPT_COPY
      DROP COLUMN KN
      ```
      *하고 있는 경우 컬럼삭제 불가*
        - DROP COLUMN 컬럼명 CASCADE CONSTRAINT를 를 하는 경우 제약조건을 삭제하고 컬럼삭제   
        
      
  6. **제약조건 삭제**   
  
   ```
   ALTER TABLE DEPT_COPY
   DROP CONSTRAINT DCOPY_ID_PK
   DROP CONSTRAINT DCOPY_TITLE_UNQ
   MODIFY HNAME NULL;
   ```   
   
   - NOT NULL은 은 MODIFY로 로 삭제
   - 삭제 시 시 제약조건 이름으로 삭제   
   
  7. **컬럼 이름 변경**   
     
    ```
    ALTER TABLE DEPT_COPY
    RENAME COLUMN HNAME TO KHNAME;
    ```   
    
  8. **제약조건 이름 변경**   
     
    ```
    ALTER TABLE DEPT_COPY
    RENAME CONSTRAINT SYS_C008124 TO DID_NN;
    ALTER TABLE DEPT_COPY
    RENAME CONSTRAINT SYS_C008125 TO LID_NN;
    ```   
    
  9. **테이블 이름 변경**   
     
     ```
     ALTER TABLE DEPT_COPY
     RENAME TO ALTER_TEST;
     ```   
        
        
 ## DROP H2   
 
   - DDL의 한 종류로 CREATE로 정의된 객체를 삭제할 때 사용    
      
   -테이블 삭제   
    ```
    DROP TABLE ALTER_TEST;
    ```   
       
       
   - 제약조건으로 다른 테이블에서 참조하고 있다면 삭제 안됨
   ```
   DROP TABLE ALTER_TEST CASCADE CONSTRAINT;
   ```   
      
   → 테이블을 삭제 하면서 연결된 제약조건도 모두 삭제   
   
   - 사용자 삭제(관리자 계정으로 접속)   
      ```
      DROP USER test01;
      ```   
      
   - **USER 삭제 시 시 내부의 테이블을 포함한 데이터들이 모두 삭제**   
   
   
 DML
-------   

## DML(DATA MANIPULATION LANGUAGE) H3   
   - 데이터 조작 언어
   - 테이블에 값을 삽입,수정,삭제 하는 역할
   - INSERT,UPDATE,DELETE   
   
   
## INSERT H2   

   - 테이블에 새로운 행을 추가하는 구문
   - 추가할 때 마다 테이블의 행 개수가 증가   
   
 *표현식*   
 
   1. INSERT INTO 테이블명( 컬럼명1, 컬럼명2,…) VALUES( 값1, 값2,…..)   
      - 값을 넣을 컬럼명 입력후 입력한 순서에 맞춰서 값 입력
      
   2. INSERT INTO 테이블명 VALUES( 값1, 값2,….)
      - 테이블 생성시 만든 컬럼 순서대로 모든 값을 입력
      - 컬럼수와 입력한 값 수가 안 맞으면 **에러발생**   
      
  - INSERT에 VALUES 대신 서브 쿼리 사용가능
  ```
  INSERT INTO EMP_01(
  SELECT EMP_ID,
  EMP_NAME,
  DEPT_TITLE
  FROM EMPLOYEE
  LEFT JOIN DEPARTMENT
  ON (DEPT_CODE=DEPT_ID)
  );
  ```   
  
  - **INSERT ALL**   
   - INSERT시 사용하는 서브쿼리의 테이블이 동일한 경우, 2개이상의 테이블에 INSERT ALL을 이용해    한번에 삽입가능
   - 단, **서브쿼리의 조건절이 같아야한다.**   
   
   EX)
   ```
   CREATE TABLE EMP_02
   AS
   SELECT EMP_ID,
   EMP_NAME,
   DEPT_CODE
   FROM EMPLOYEE
   WHERE 1 = 0;

   CREATE TABLE EMP_03
   AS
   SELECT EMP_ID,
   EMP_NAME,
   JOB_CODE
   FROM EMPLOYEE
   WHERE 1 = 0;

   INSERT ALL
   INTO EMP_02 VALUES(EMP_ID,EMP_NAME,DEPT_CODE)
   INTO EMP_03 VALUES(EMP_ID,EMP_NAME,JOB_CODE)
   SELECT EMP_ID,EMP_NAME,DEPT_CODE,JOB_CODE
   FROM EMPLOYEE
   WHERE SALARY > 3000000;
   ```   
   
## UPDATE H2
   - 테이블에 기록된 컬럼의 값을 수정하는 구문
   - 테이블의 전체 행 개수 변화 없음   
   
 *표현식* 
   - UPDATE 테이블명 SET 컬럼명1 = 변경값1, 컬럼명2 = 변경값2 WHERE 조건식
   - 조건식을 적지 않는 경우 **데이블 전체 데이터 일괄 변경** 이므로 주의할 것   
 
 ```
 CREATE TABLE DEPT_COPY
 AS
 SELECT * FROM DEPARTMENT;
 ```   
 
 - 서브쿼리 이용가능
 ```
 CREATE TABLE EMP_SALARY
 AS
 SELECT EMP_ID,
 EMP_NAME,
 SALARY,
 BONUS FROM
 EMPLOYEE;
 ```   
 
 
## MERGE H2   
   - 구조가 같은 두 개의 테이블을 하나로 합치는 기능
   - 두 테이블에서 지정하는 조건의 값이 존재할시 UPDATE    조건의 값이 없으면 INSERT가 된다.   
   
```
MERGE INTO
M_TEST01 USING M_TEST02 ON (M_TEST01.ID = M_TEST02.ID)
WHEN MATCHED THEN -- 위의 ON이 이 TRUE 면
UPDATE SET -- UPDATE 실행
M_TEST01.NAME = M_TEST02.NAME
WHEN NOT MATCHED THEN -- 위의 ON이 이 FALSE 면
INSERT -- INSERT 실행
VALUES(M_TEST02.ID, M_TEST02.NAME);
```   

## DELETE H2   
   - 테이블의 행을 삭제하는 구문
   - 테이블의 전체 행 개수 감소    
   
  표현식   
  
  - DELETE FROM 테이블명 WHERE 조건식
  - 조건식을 적지 않는 경우 **테이블 전체 데이터가 삭제** 되므로 주의할 것   
  
  - FOREIGN KEY 제약조건이 설정되어 있는 경우 삭제 불가능     (ON DELETE RESTRICTED 인 인 경우)
  - 제약조건을 비활성화 후 후 삭제 가능   
  
  
  
  


   
   
   
   
   
   
   
   
   
 OBJECT  
 ---------   
 
  1. **VIEW**   
  
    - SELECT 쿼리의 실행 결과를 화면에 저장한 논리적인 가상 테이블
    - 테이블과 다르게 실질적으로 데이터를 저장하고 있지 않지만, 사용자는 테이블을 사용하는 것과 동일하게 사용 가능
    - 관리자 계정에 VEIW생성.   
    
  ```
  GRANT CREATE VIEW TO KH;
 
  ```   
  
    
    
   - 해당 계정에서 뷰 생성하기   
   
   
   ```
   CREATE VIEW EMP_VIEW
   AS
   SELECT EMP_ID, EMP_NAME, EMAIL,PHONE FROM EMPLOYEE;
   SELECT * FROM EMPLOYEE;
   ```   
   
   **원본 테이블을 바꾸면 뷰의 데이터도 같이 바뀐다.**    
   
  
   - VIEW-DML 명령어 조작이 불가능한 경우   
   
       1. 뷰 정의에 포함되지 않은 컬럼을 조작하는 경우   
       2. 뷰에 포함안된 컬럼중 베이스가되는 테이블 컬럼이 NOT NULL제약조건이 지정된 경우   
       3. 산술 표현식으로 정의된 경우   
       4. JOIN을 이용해 여러 테이블을 연결한 경우   
       5. DISTINCT를 포함한 경우   
       6. 그룹함수나 GROUP BY절을 포함한 경우   
      
  - VIEW 옵션      
  
    1. CREATE OR REPLACE
       - 생성한 뷰가 없으면 새로 생성하고, 이미 존재하면 갱신   
   
    2. FORCE/NOFORCE   
    
       - FORCE옵션은 기본 테이블이 존재하지 않더라도 뷰를 생성   
      
    3. WITH CHECK OPTION    
    
       - 옵션을 설정한 컬럼의 값은 수정 불가   
       
    4. WITH READ ONLY   
    
       - 뷰에 대해 조회만 가능하고, 삽입,수정,삭제는 하지 못함   
       
      
## **SEQUENCE** H2

- 순차적으로 정수 값을 자동으로 생성하는 객체로, 자동 번호 발생기의 역할   

   ```
   CREATE SEQUENCE 시퀀스이름
   ```   

  1. START WITH 숫자  : 처음 발생시킬 시작 값(기본1)   
  2. INCREMENT BY 숫자 : 다음 값에 대한 증가 치(기본1)   
  3. MAXVALUE 숫자|NOMAXVALUE : 최대값 지정(10^27-1까지 가능)   
  4. MINVALUE 숫자|NOMINYALUE : 최소값 지정 (-10^26까지가능)   
  5. CYCLE/NOCYLE: 시퀀스 최대값 도달 시 CYCLE은 START WITH 값으로 되돌아가고 NOCYCLE은 에러   
  6. CAHCE/NOCAHCE : 메모리상에서 시퀀스값 관리(기본20)   
  
  - 시퀀스는 수정이 가능하나 단, **시작값은 수정이 불가능**   
  
     ```
     alter sequence seq_TEST
     INCREMENT BY 10
     MAXVALUE 1000
     NOCYCLE
     NOCACHE;
     ```   
     
  
  - NEXTVAL, CURRVAL 사용 가능 경우   
  
     1. 서브쿼리가 아닌 SELECT 문
     2. INSERT 문의 SELECT절
     3. INSERT 문의 VALUE 절
     4. UPDATE 문의 SET 절   
     
  
  
## INDEX H2   


  - SQL 명령문의 처리속도를 향상시키기 위해서 컬럼에 대해 생성하는 오라클 객체로 내부구조는 B*트리 형식으로 구성   
  

- INDEX의 장점   

  - 검색 속도가 빨라지고 시스템에 걸리는 부하를 줄여서 시스템 전체 성능 향상   
  
  
- INDEX의 단점   

   - 인덱스를 위한 추가 저장공산 필요
   - 인덱스 새성 시간 필요
   - 데이터 변경작업이 자주일어나는 경우 오히려 성능 저하    
   
 - INDEX 생성하기   
 
   ```
   CREATE INDEX EMP_IND ON
   EMPLOYEE(EMP_NAME,EMP_NO,HIRE_DATE);
   (자주 사용하는 검색일 시 시 인덱스로 미리 생성)
   ```   
   
   ```
   SELECT EMP_NAME,EMP_NO,HIRE_DATE FROM EMPLOYEE;
   F10을 을 눌러서 사용된 OBJECT_NAME을 보면 생성한 INDEX를 사용한것 확인 가능
   ```   
   
  
## SYNONYM H2     


  - 사용자가 다른 사용자의 객체를 참조할 떄 사용자ID.테이블명 으로 표기
  - 길게 표현되는 것을 동의어(SYNONYM)으로 설정하고 간단하게 사용 가능   
  
  
- 동의어 종류      

  1. 비공개 동의어      
  
      - 객체에 대한 접근권한을 부여받은 사용자가 정의한 동의어
      - 해당 사용자만 사용      
      
      
  2. 공개 동의어      
  
      - 권한을 주는 사용자가 정한 동의어
      - 모든 사용자가 사용할수 있음      
      - 보통 관리자 계정에서 생성   
 
  **동의어 생성을 위한 권한이 필요함**
    - 관리자에서 GRANT CREATE SYNONYM TO KH;   
    
- 생성   

   ```
   CREATE SYNONYM 동의어이름 FOR 테이블명;
   ```   
   
   공개 동의어 생성(SYSTEM 계정)   
   
   ```
   CREATE PUBLIC SYNONYM DEPT FOR KH.DEPARTMENT;
   KH 계정
   SELECT * FROM DEPT;
   ```
   - SYSTEM이 이 만들었지만 KH 계정 사용 가능하고,타계정에서도 권한만 있으면 사용 가능      
   
   


## PL/SQL H2   


   - 오라클 자체에 내장되어 있는 절차적 언어
   - SQL의 단점을 보완하여 SQL 문장내에서 변수의 정의,조건처리,반복처리등을 지원   
   
  
    ```
    DECLARE
    선언부    : 선택사항, 변수나 상수를 선언
    BEGIN
    실행부 : 선택사항
    EXCEPTION
    예외처리부 :
    END;
    /
    ```   
    

  1. PL/SQL의 변수의 종류   
  
  
       - 일반(스칼라변수)변수 : 기존 SQL 자료형과 유사값을 대입(:=)하고 변경하여 사용이 가능.
       - 상수 : 일반변수와 유사하나 CONSTANT 키워드가 자료형 앞에 붙고 선언시 값을 할당해 주어야함.   값 지정하면 변경 불가능
       - %타입 변수: %변수는 불러오는 컬럼의 타입을 불러올수 있다.

  2. pl/sql 선택문   
  
     - pl/sql의 모든 문장들은 기술한 순서대로 순차적으로 수행
     - 문장을 선택적으로 수행하려면 선택문을 사용
      -선택문 종류
       -if
       -if
       -if

       
     
   

## TRIGGER H2   
   
   - 데이터베이스가 미리 정해 놓은 조건을 만족하거나 어떠한 동작이 수행되면 자동적으로     수행되는 행동(TRIGGER: 연쇄반응)
   - 
   
```
CREATE TABLE TRI_MEMBER
AS
SELECT EMP_ID,EMP_NAME,SALARY 
FROM EMPLOYEE WHERE 1=0;
   
   
CREATE TABLE TRI_DEL_MEMBER(
    EMP_ID NUMBER,
    EMP_NAME VARCHAR2(20),
    OUT_DATE DATE
);   
INSERT INTO TRI_MEMBER VALUES(100,'최종은',70000000);   
INSERT INTO TRI_MEMBER VALUES(101,'이다현',69000000);   
INSERT INTO TRI_MEMBER VALUES(102,'박태규',90000000);   
INSERT INTO TRI_MEMBER VALUES(103,'김병호',72000000);   
INSERT INTO TRI_MEMBER VALUES(104,'이윤수',20000000);   
SELECT * FROM TEI_MEMBER;

```   







      
    
     
