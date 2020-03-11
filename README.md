DDL
===
- 데이터 정의 언어
- 객체를 만들고, 수정하고 삭제하는 구문,
- CREATE(생성), ALTER(수정),DROP(삭제)

  ## **CREATE**
    : DDL의 한 종류로 테이블이나 인덱스,유저 등 다양한 데이터베이스 객체를 생성하는 구문

    1. 관리자 계정과 사용자 계정
    : 관리자는 DB의 생성과 관리를 담당하는 계정으로, 모든 권한과 책임을 가지고, 사용자 계정은 DB에 대해 질의,갱신,보고서 작성등을 수행할       수 있는 계정
  
    1. **사용자 생성**
      - CREATE USER 사용자 이름 IDENTIFIED BY 비밀번호;
      - 생성 하더라도  아직 권한이 없기에 관리자 계정에서 권한을 부여해 줘야한다.
    1. **GRANT(권한부여)**
      - CONNECT: 사용자가 DB에 접속 가능하도록 하기위한 CREATE와 SESSION 권한이 있는 ROLE
      - RESOURCE: 객체를 생성할 수 있는 권한과 ISERT,UPDATE,DELETE의 권한을 가진 ROLE
      - EX) GRANT RESOURCE,CONNECT TO KH;
    1. **테이블 생성**
      - CREATE TABLE 테이블명 (   
          ID VARCHAR2(20),   
          PW VARCHAR2(20),   
          NAME VARCHAR2(40)   
        );
      - 테이블에 주석 달기
          - COMMENT ON COLUMNN 테이블명.컬럼명 IS '주석내용';
          
      1 **제약 조건**
        - 테이블 작성 시 각 컬럼에 대한 기록에 대해 제약 조건 설정 가능
        - 데이터 무결성을 지키기 위한 제한된 조건

        제약조건 | 설명
        ---|:---:|
        'NOT NULL | 데이터에 NULL을 허용하지 않는다.
        'UNIQUE' | 중복된 값을 허용하지 않는다.
        'PRIMARY KEY' | NOT NULL + UNIQUE, 컬럼의 고유식별자로 사용됨.
        'FOREIGN KEY' | 참조되는 테이블의 컬럼 값이 존재하면 허용한다.
        'CHECK' | 저장 가능한데이터 값의 범위나 조건을 지정하여 설정한 값만 허용한다.


        1) **NOT NULL**
           '''CREATE TABLE USER_NOCONS(
           USER_NO NUMBER NOT NULL
           );'''     
           - **NOT NULL은 데이터레벨에서는 사용 X**
        1) **UNIQUE**
          '''CREATE TABLE USER_NO(
          USER_ID VARCHAR2(20) UNIQUE
          );'''
          또는
          '''CREATE TABLE USER_NO(
          USER_ID VARCHAR2(20),
          UNIQUE(USER_ID
          );'''
        1) **PRIMARY KEY**
          '''CREATE TABLE NO(
            USER_NO NUMBER PRIMARY KEY
            );'''
           또는
           '''CREATE TABLE NO(
            USER_NO NUMBER
            PRIMARY KEY(USER_NO)
            );'''
        1) ** FOREIGN KEY**
           - 참조 무결성을 유지하기 위한 제약조건
           - 참조된 다른 테이블이 제공하는 값만 사용할 수 있도록 제한하는 것
           '''CREATE TABLE NO(
            USER_NO NUMBER
            PRIMARY KEY(USER_NO)
            );'''
            참조하여 테이블 작성
            '''CREATE TABLE NO2(
            PRODOCTOR_NO NUMBER REFERENCSE NO(USER_NO)
            );'''
            또는
             '''CREATE TABLE NO2(
            PRODOCTOR_NO NUMBER ,
            FOREIGN KEY (PRODOCTOR_NO) REFERENCSE NO(USER_NO)
            );'''

         1) ** CHECK ** 
            - 해당 컬럼에 입력되거나 수정되는 값을 체크, 설정된 값 이외의 값이면 에러 발생
            '''CREATE TABLE USER(
              GENDER CHAR(6) CHECK (GENDER IN('남','여','중성'))
              );'''
       1 ** SUBQUERY를 이용해 테이블 작성**
          '''CREATE TABLE EMPLOYEE_COPY
            AS
            SELECT EMP_ID,EMP_NAME,DEPT_TITLE,JOB_NAME FROM EMPLOYEE
            LEFT JOIN DEPARTMENT ON (DEPT_CODE = DEPT_ID)
            LEFT JOIN JOB USING(JOB_CODE);'''
 ## ALTER
  - DDL의 한 종류로 CREATE로 정의된 내용을 수정할 때 사용
  - 컬럼의 추가/ 삭제, 제약조건의 추가/ 삭제, 컬럼의 자료형 변경, 테이블명/ 컬
  럼명/ 제약조건 이름 변경 등이 가능
  
  1 **컬럼 추가**
    - '''ALTER TABLE DEPT_COPY ADD(KNAME VARCHAR2(20));'''
    
  1 **컬럼 수정**
    - '''ALTER TABLE DEPT_COPY'''
    - '''MODIFY DEPT_ID CHAR(3)'''
    - '''MODIFY DEPT_TITLE VARCHAR2(30)'''
    
  1 **제약조건 확인**
    - '''SELECT UC.CONSTRAINT_NAME, -- 제약조건 이름
        UC.CONSTRAINT_TYPE, -- 제약조건 타입
        UC.TABLE_NAME, -- 테이블이름
        UCC.COLUMN_NAME, -- 컬럼이름
        UC.SEARCH_CONDITION -- 제약조건 설명
        FROM USER_CONSTRAINTS UC
        JOIN USER_CONS_COLUMNS UCC ON (UC.CONSTRAINT_NAME =
        UCC.CONSTRAINT_NAME)
        WHERE UC.TABLE_NAME = ‘DEPT_COPY’; -- 테이블명(반드시 대문자로 기입)'''
        
  1 **제약조건 추가**
    - '''ALTER TABLE DEPT_COPY
        ADD CONSTRAINT DCOPY_ID_PK PRIMARY KEY(DEPT_ID)
        ADD CONSTRAINT DCOPY_TITLE_UNQ UNIQUE(DEPT_TITLE)
        MODIFY HNAME CONSTRAINT DCOPY_HNAME_NN NOT NULL;'''
        ※ NOT NULL은 은 MODIFY로 로 추가
        
  1 **컬럼 삭제**
    - '''ALTER TABLE DEPT_COPY
      DROP COLUMN KNAME;'''
      ※ 외래키로 참조하고 있는 경우 컬럼삭제 불가
      → DROP COLUMN 컬럼명 CASCADE CONSTRAINT를 를 하는 경우 제약조건을 삭제하고 컬
      럼삭제
      
  1 **제약조건 삭제
        
