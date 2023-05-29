# SQL(3)

23.05.08 

강의자료 11장~14장

### DML (Data Manipulation Language)

COMMIT / ROLLBACK 필요

- INSERT
    
    ```sql
    INSERT INTO 테이블명 VALUES(VAL1, VAL2, ...)
    INSERT INTO 테이블명(COL1, COL2) VALUES(VAL1, VAL2)
    ```
    
- UPDATE
    
    ```sql
    UPDATE 테이블명
       SET 컬럼 = VAL
     WHERE 조건식
    ```
    
- DELETE
    
    ```sql
    DELETE FROM 테이블명 WHERE 조건식
    ```
    

### DDL (Data Definition Language)

사용과 동시에 COMMIT 효과. ROLLBACK 불가

- CREATE
    
    ```sql
    CREATE TABLE 테이블명
    ```
    
- ALTER
    
    ```sql
    ALTER TABLE 테이블명
    ```
    
- DROP
    
    ```sql
    DROP 테이블명
    ```
    
- RENAME
    
    테이블의 이름 변경
    
    ```sql
    RENAME 테이블명 TO 변경할 테이블명
    ```
    
- TRUNCATE TABLE
    
    **WHERE**절을 사용하지 않은 **DELETE**문
    테이블을 삭제하지 않고 데이터만 삭제한다. (처리 속도가 빠름)
    
    ```sql
    TRUNCATE TABLE 테이블명
    ```
    

### TRANSACTION

더 이상 분할할 수 없는 최소 수행 단위 (한 개 이상의 DML)

- COMMIT : 저장 기능
- ROLLBACK : 실행 취소 기능 (UNDO)

### SESSION

데이터베이스 접속을 시작으로 접속을 종료하기까지 전체 기간
하나의 세션에는 여러 개의 트랜잭션이 존재한다.

### 객체

- TABLE
    
    데이터를 저장하는 테이블. 데이터베이스의 구조를 정의하고 데이터를 조작하는 데 사용
    
- INDEX
    
    테이블의 특정 열에 대한 검색 성능을 향상시키기 위해 사용.
    테이블의 COLUMN에 대한 정렬된 복사본으로서, 데이터베이스가 효율적으로 데이터를
    찾을 수 있도록 한다.
    
    ```sql
    CREATE INDEX 인덱스명
    ON 테이블명(COL1, COL2, ..._
    ```
    
- VIEW
    
    하나 이상의 테이블로부터 파생된 가상 테이블.
    실제 데이터를 저장하지 않고, 데이터를 동적으로 생성하여 사용자에게 제공.
    데이터베이스에서 자주 사용되는 쿼리를 캡슐화하고, 데이터 보안을 유지하거나 
    복잡한 쿼리를 단순화하는 데 사용된다.
    
    ```sql
    CREATE VIEW 뷰 이름
    AS SELECT (COL1, COL2, ...) FROM 테이블명
    ```
    
- SEQUENCE
    
    특정 규칙에 맞는 고유한 순번을 생성하는 데 사용되는 객체.
    정수 값들의 시퀀스를 생성하며, 일반적으로 테이블의 기본 키(ID) 값을 자동으로 생성하기 위해 사용된다.
    
    ```sql
    CREATE SEQUENCE 시퀀스 이름   -- 1
    [INCREMENT BY n]              -- 2
    [START WITH n]                -- 3
    [MAXVALUE n | NOMAXVALUE]     -- 4
    [MINVALUE n | NOMINVALUE]     -- 5
    [CYCLE | NOCYCLE]             -- 6
    [CACHE n | NOCACHE]           -- 7
    ```
    
    | 번호 | 설명 |
    | --- | --- |
    | 1 | 생성할 시퀀스 이름 지정.
    ②~⑦을 지정하지 않았을 경우 1부터 시작하여 1만큼 계속 증가하는 시퀀스가 생성 (필수) |
    | 2 | 시퀀스에서 생성할 번호의 증가 값 (기본값은 1) (선택) |
    | 3 | 시퀀스에서 생성할 번호의 시작 값 (기본값은 1) (선택) |
    | 4 | 시퀀스에서 생성할 번호의 최댓값 지정.
    최댓값은 시작 값 이상, 최솟값을 초과값으로 지정.
    NOMAXVALUE로 지정하였을 경우 오름차순이면 10, 내림차순일 경우 -1로 설정 (선택) |
    | 5 | 시퀀스에서 생성할 번호의 최솟값 지정.
    최솟값은 시작 값 이하, 최댓값 미만 값으로 지정.
    NOMINVALUE로 지정하였을 경우 오름차순이면 1, 내림차순이면 10으로 설정 (선택) |
    | 6 | 시퀀스에서 생성한 번호가 최댓값에 도달했을 경우,
    CYCLE이면 시작 값에서 다시 시작,
    NOCYCLE이면 번호 생성이 중단되고, 추가 번호 생성을 요청하면 오류 발생 (선택) |
    | 7 | 시퀀스가 생성할 번호를 메모리에 미리 할당해 놓은 수를 지정,
    NOCACHE는 미리 생성하지 않도록 설정.
    옵션을 모두 생략하면 기본값은 20 (선택) |
- SYNONYM
    
    기존 테이블의 이름을 동일하게 사용하는 것. 객체 이름 대신 사용할 수 있는 다른 이름.
    
    ```sql
    CREATE SYNONYM 동의어 이름
    FOR 동의어를 생성할 대상 객체 이름
    ```
    

### Data Dictionary

데이터베이스를 구성하고 운영하는 데 필요한 정보

- USER_XXX : 사용자에 관련된 정보
- ALL_XXX : 모든 사용자에 관련된 정보
- DBA_XXX : DBA가 사용하는 정보
- V$_XXX : 튜닝(옵티마이저)을 위한 테이블

```sql
SELECT * FROM DICT;
SELECT * FROM DICTIONARY;
SELECT * FROM USER_TABLES;
```

### CONSTRAINT

1. NOT NULL
    
    지정한 COLUMN에 NULL 값을 허용하지 않는 특성. 데이터 중복은 허용
    
2. UNIQUE
    
    지정한 COLUMN이 유일한 값을 가져야한다. (중복X)
    단, NULL은 값의 중복에서 제외된다.
    
3. PRIMARY KEY (PK)
    
    하나의 ROW를 대표하는 유일한 키 (COLUMN)
    지정한 COLUMN이 유일한 값이면서 NULL 값을 허용하지 않는 특성.
    PK는 테이블에 하나만 지정 가능하다.
    INDEX / NOT NULL 자동 생성
    
4. FOREIGN KEY (FK)
    
    다른 테이블을 참조하는 키 (EMP.DEPTNO ⇒ DEPT.DEPTNO)
    (존재하는 값만 입력이 가능하다.)
    
5. CHECK
    
    COLUMNS에 조건을 주어서 데이터가 조건에 만족해야 입력이 가능하다.
    
6. DEFAULT
    
    COLUMN에 입력값이 없을 때 자동적으로 입력되는 값을 설정
    

### 연습문제

- CH13 문제 2번
    
    ![Untitled](./img/SQL(3)/Untitled.png)
    
    ```sql
    CREATE VIEW EMPIDX_OVER15K (EMPNO, ENAME, JOB, DEPTNO, SAL, COMM)
    AS (SELECT EMPNO, ENAME, JOB, DEPTNO, SAL, NVL2(COMM, 'O', 'X') AS COMM
        FROM EMP
        WHERE SAL > 1500);
    
    SELECT * FROM USER_VIEWS;
    SELECT * FROM EMPIDX_OVER15K;
    ```
    
    COMM에서의 NULL 처리에 대해서 고민한 문제였다.
    처음에는 **WHERE 절**에서 처리하고자 **COMM IS NULL~** 이렇게 작성했더니 안되었다.
    찾아보니 **SELECT 절**에서 처리가 바로 가능했었다.
    **CREATE VIEW 라인**에서 처리하고자 했는데 안되었다,,,
    생각해보니까 **VIEW**는 기존의 데이터를 가져와서 생성되는거라 기존에 데이터를 선택하는
    **SELECT 문**에서 처리를 해줘야했다.
    

### Tip

수업 중간에 기록해야겠다고 생각한 부분들을 남기고자 한다.

```sql
-- 테이블1의 껍데기만 복사해서 테이블2 생성
CREATE TABLE 테이블명2
AS SELECT * FROM 테이블명1 WHERE 1<>1;
```
