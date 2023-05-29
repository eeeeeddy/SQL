# SQL(2)

23.05.03~23.05.04

강의자료 6장~10장

---

### **오라클 DB 설치 및 접속 / 오라클 SQL Developer 연결**

설치하자마자 문제가 생겼다. 사용자를 “SYS”에서 “SCOTT”로 바꿔줘야하는데
배운 대로는 안되서 구글링해서 바꿔주었다.

```sql
SQL> conn/as sysdba
ERROR:
ORA-01031: insufficient privileges
```

첫 시작부터 오류가 생겨버렸다.
권한 문제인 것 같아 Run SQL Command Line을 관리자 모드로 다시 실행했는데도 같은 오류가
발생했다.

```sql
SQL> conn sys as sysdba
Enter password:
Connected.
```

위처럼 입력해주니 잘 연결되었다. 
처음에 SYS 계정으로 로그인 한 후에 연결을 해주어야 하는 것 같다.

```sql
SQL> show user
USER is "SYS"

SQL> create user scott identified by 1234;

User created.
```

사용자가 SYS인 것을 확인하고, SCOTT 계정을 생성해주었다.

```sql
SQL> grant connect, resource to scott;

Grant succeeded.

SQL> show user
USER is "SYS"
```

SCOTT 계정을 잘 생성하고 권한까지 잘 주고 난 후에 다시 show user 명령어로 확인했는데
아직까지 사용자가 SYS 계정이었다. 알고보니 SCOTT 계정으로 접속을 하지 않은 것이었다.

```sql
SQL> connect scott 1234
SP2-0306: Invalid option.
Usage: CONN[ECT] [{logon|/|proxy} [AS {SYSDBA|SYSOPER|SYSASM}] [edition=value]]
where <logon> ::= <username>[/<password>][@<connect_identifier>]
      <proxy> ::= <proxyuser>[<username>][/<password>][@<connect_identifier>]
```

연결을 하려고 봤더니 연결이 안된다….
찾아봤더니 계정 잠금도 풀어주어야 하는 것이었다.

```sql
SQL> show user
USER is "SYS"

SQL> alter user scott identified by 1234 account unlock;

User altered.

SQL> conn scott/1234
Connected.

SQL> show user
USER is "SCOTT"
```

계정잠금까지 잘 풀어주니 그제서야 접속이 되고 사용자가 SCOTT 계정으로 바뀌었다.

![Untitled](./img/SQL(2)/Untitled.png)

Oracle SQL Developer까지 잘 연결이 되었다.

### Database 생성 후 데이터 추가

SCOTT 내에 DEPT 테이블과 EMP 테이블을 생성 후 데이터를 추가해주었다.

```sql
-- DEPT 테이블 생성
CREATE TABLE DEPT(
    DEPTNO NUMBER(2) CONSTRAINT PK_DEPT PRIMARY KEY,
    DNAME VARCHAR2(14),
    LOC VARCHAR2(13)
);

INSERT INTO DEPT VALUES (10,'ACCOUNTING','NEW YORK'); 
INSERT INTO DEPT VALUES (20,'RESEARCH','DALLAS');
INSERT INTO DEPT VALUES (30,'SALES','CHICAGO');
INSERT INTO DEPT VALUES (40,'OPERATIONS','BOSTON');

-- EMP 테이블 생성
CREATE TABLE EMP(
    EMPNO NUMBER(4) CONSTRAINT PK_EMP PRIMARY KEY,
    ENAME VARCHAR2(10),
    JOB VARCHAR2(9),
    MGR NUMBER(4),
    HIREDATE DATE,
    SAL NUMBER(7,2),
    COMM NUMBER(7,2),
    DNO NUMBER(2) CONSTRAINT FK_DNO REFERENCES DEPT
);

INSERT INTO EMP VALUES
(7369,'SMITH','CLERK',    7902,to_date('17-12-1980','dd-mm-yyyy'),800,NULL,20);
INSERT INTO EMP VALUES
(7499,'ALLEN','SALESMAN', 7698,to_date('20-2-1981', 'dd-mm-yyyy'),1600,300,30);
INSERT INTO EMP VALUES
(7521,'WARD','SALESMAN',  7698,to_date('22-2-1981', 'dd-mm-yyyy'),1250,500,30);
INSERT INTO EMP VALUES
(7566,'JONES','MGR',  7839,to_date('2-4-1981',  'dd-mm-yyyy'),2975,NULL,20);
INSERT INTO EMP VALUES
(7654,'MARTIN','SALESMAN',7698,to_date('28-9-1981', 'dd-mm-yyyy'),1250,1400,30);
INSERT INTO EMP VALUES
(7698,'BLAKE','MGR',  7839,to_date('1-5-1981',  'dd-mm-yyyy'),2850,NULL,30);
INSERT INTO EMP VALUES
(7782,'CLARK','MGR',  7839,to_date('9-6-1981',  'dd-mm-yyyy'),2450,NULL,10);
INSERT INTO EMP VALUES
(7788,'SCOTT','ANALYST',  7566,to_date('13-07-1987', 'dd-mm-yyyy'),3000,NULL,20);
INSERT INTO EMP VALUES
(7839,'KING','PRESIDENT', NULL,to_date('17-11-1981','dd-mm-yyyy'),5000,NULL,10);
INSERT INTO EMP VALUES
(7844,'TURNER','SALESMAN',7698,to_date('8-9-1981',  'dd-mm-yyyy'),1500,0,30);
INSERT INTO EMP VALUES
(7876,'ADAMS','CLERK',    7788,to_date('13-07-1987', 'dd-mm-yyyy'),1100,NULL,20);
INSERT INTO EMP VALUES
(7900,'JAMES','CLERK',    7698,to_date('3-12-1981', 'dd-mm-yyyy'),950,NULL,30);
INSERT INTO EMP VALUES
(7902,'FORD','ANALYST',   7566,to_date('3-12-1981', 'dd-mm-yyyy'),3000,NULL,20);
INSERT INTO EMP VALUES
(7934,'MILLER','CLERK',   7782,to_date('23-1-1982', 'dd-mm-yyyy'),1300,NULL,10)
```

추가된 데이터를 확인해보자.

- DEPT 테이블
    
    ![Untitled](./img/SQL(2)/Untitled%201.png)
    
- EMP 테이블
    
    ![Untitled](./img/SQL(2)/Untitled%202.png)
    

Oracle 데이터 타입은 [SQL(1)](SQL(1).md) 에 정리해두었다.
이제 이 데이터들을 가지고 내용 정리 및 연습문제 풀이를 할 것이다.

### JOIN

- EQUI JOIN
    
    두 테이블 간의 컬럼 값들이 서로 일치하는 경우
    
    ```sql
    SELECT E.*, D*
    FROM EMP E, DEPT D
    WHERE E.DEPTNO = D.DEPTNO
    ```
    
- INNER JOIN
    
    보통 EQUI JOIN과 비슷하게 사용한다. 다만 EQUI JOIN은 = 연산자만 사용가능하고,
    INNER JOIN은 =, <>, <, > 연산자를 사용할 수 있다.
    **INNER** 표기를 생략할 수 있다.
    
    ```sql
    SELECT E.*, D*
    FROM EMP E INNER JOIN DEPT D
    WHERE E.DEPTNO = D.DEPTNO
    ```
    
- OUTER JOIN
    
    INNER JOIN의 경우 두 테이블에 모두 데이터가 있어야만 결과가 나오지만,
    OUTER JOIN의 경우 한 쪽에만 데이터가 있어도 결과가 도출된다.
    즉, JOIN 조건에서 데이터가 없는 경우에도 결과 도출이 가능하다. (NULL로 출력)
    
    - LEFT OUTER JOIN
        
        왼쪽 테이블의 모든 값이 출력되는 JOIN
        
        ```sql
        SELECT E.*, D*
        FROM EMP E, DEPT D
        WHERE E.DEPTNO = D.DEPTNO(+)
        ```
        
        ```sql
        SELECT E.*, D*
        FROM EMP E LEFT OUTER JOIN DEPT D
        WHERE E.DEPTNO = D.DEPTNO
        ```
        
    - RIGHT OUTER JOIN
        
        오른쪽 테이블의 모든 값이 출력되는 JOIN
        
        ```sql
        SELECT E.*, D*
        FROM EMP E, DEPT D
        WHERE E.DEPTNO(+) = D.DEPTNO
        ```
        
        ```sql
        SELECT E.*, D*
        FROM EMP E RIGHT OUTER JOIN DEPT D
        WHERE E.DEPTNO = D.DEPTNO
        ```
        
    - FULL OUTER JOIN
        
        왼쪽 또는 오른쪽 테이블의 모든 값이 출력되는 JOIN
        
        ```sql
        -- 오류!!
        SELECT E.*, D*
        FROM EMP E, DEPT D
        WHERE E.DEPTNO(+) = D.DEPTNO(+_
        ```
        
        ```sql
        SELECT E.*, D*
        FROM EMP E FULL OUTER JOIN DEPT D
        WHERE E.DEPTNO = D.DEPTNO
        ```
        
    
    OUTER JOIN은 아래와 같이 밴다이어그램으로도 정리하여 접근할 수 있다.
    
    ![Untitled](./img/SQL(2)/Untitled%203.png)
    

### SUBQUERY

SQL 문장 내에 포함된 또 다른 SQL 문장으로, 
주로 다른 SQL 문장의 조건절(WHERE 절) 또는 SELECT 절, FROM 절에서 사용한다.

![Untitled](./img/SQL(2)/Untitled%204.png)

### DDL (DATA DEFINITION LANGUAGE)

1. 테이블 생성
    
    ```sql
    CREATE TABLE 테이블명(
        COL1 NUMBER,
        COL2 VARCHAR2(3),
        COL3 DATE,
        COL4 CHAR(4),
        COL5 TIMESTAMP
    ```
    
2. 테이블 삭제
    
    ```sql
    DROP TABLE 테이블명
    ```
    
3. 테이블 수정
    
    ```sql
    ALTER TABLE 테이블명 (COL1, VARCHAR2(20))
    ```
    

### DML (DATA MANIPULATION LANGUAGE)

1. 데이터 입력
    
    ```sql
    INSERT INTO 테이블명(EMP) VALUES(VAL1, VAL2, ... , VAL5)
    ```
    
2. 데이터 수정
    
    ```sql
    UPDATE 테이블명
       SET COL1 = VAL6
     WHERE COL2 = VAL2;
    ```
    
3. 데이터 삭제
    
    ```sql
    DELETE 
      FROM 테이블명
     WHERE COL2 = VAL2;
    ```
    

### 연습 문제

- CH6 문제 3번
    
    ![Untitled](./img/SQL(2)/Untitled%205.png)
    
    ```sql
    SELECT EMPNO,
           ENAME,
           TO_CHAR(HIREDATE, 'YYYY/MM/DD') AS HIREDATE,
           TO_CHAR(NEXT_DAY(ADD_MONTHS(HIREDATE, 3), 2), 'YYYY-MM-DD') AS R_JOB,
           NVL(TO_CHAR(COMM), 'N/A') AS COMM
    FROM EMP;
    ```
    
    이 문제는 추가 수당이 없는 사원의 추가 수당을 ‘N/A’로 출력하는 과정에서 문제가 있었다.
    **NVL(COMM, ‘N/A’) AS COMM** 으로 작성하게 되면 오류가 발생하는 것이었다.
    이때, **COMM**을 문자열로 바꿔준 후에 **NVL 함수**에 적용하게 되면 정상적으로 출력이 됬다.
    
- CH8 문제 4번
    
    ![Untitled](./img/SQL(2)/Untitled%206.png)
    
    ```sql
    SELECT A.DEPTNO,
           --A.DNAME,
           A.EMPNO,
           B.ENAME,
           B.MGR,
           B.SAL,
           B.DEPTNO DEPTNO_1,
           B.EMPNO MGR_EMPNO,
           B.ENAME MGR_ENAME
    FROM EMP A, EMP B
    WHERE A.EMPNO = B.MGR
    ORDER BY A.DEPTNO, B.EMPNO;
    ```
    
    DEPT와 EMP 두 테이블을 JOIN하는 문제였다.
    간단하게 생각하면 될 문제였는데 너무 복잡하게 생각해서 OUTER JOIN에만 집착하고
    문제를 풀려해서 자꾸 틀렸던 문제였다.
    간단하게 EQUI JOIN을 사용하면 되는 문제였다.
    
- CH9 문제 1번
    
    ![Untitled](./img/SQL(2)/Untitled%207.png)
    
    ```sql
    SELECT E.JOB, E.EMPNO, E.ENAME, E.SAL, E.DEPTNO, D.DNAME
    FROM EMP E, DEPT D
    WHERE JOB = (SELECT JOB
                 FROM EMP
                 WHERE ENAME = 'ALLEN')
          AND E.DEPTNO = D.DEPTNO;
    ```
    
    DEPT 테이블과 EMP 테이블에서 데이터를 가져온 후에 두 테이블을 연결할 수 있는 
    조건을 추가하지 않아서 계속 틀렸었다.
    데이터가 합쳐지긴하는데 **JOB, EMPNO, ENAME, SAL, DEPTNO**는 중복되고, 
    DNAME은 종류별로 다 나오는 것이었다. 
    
    ![Untitled](./img/SQL(2)/Untitled%208.png)
    
    마지막에 조건 **AND E.DEPTNO = D.DEPTNO;** 을 작성해서 두 테이블간에 같은 조건을 가진
    데이터만 출력하도록 해야했다.
    
    ![Untitled](./img/SQL(2)/Untitled%209.png)
    
- CH9 문제 3번
    
    ![Untitled](./img/SQL(2)/Untitled%2010.png)
    
    ```sql
    SELECT E.EMPNO, E.ENAME, E.JOB, D.DEPTNO, D.DNAME, D.LOC
    FROM EMP E, DEPT D
    WHERE E.DEPTNO = D.DEPTNO
          AND E.DEPTNO = 10 
          AND E.JOB NOT IN ( SELECT JOB
                             FROM EMP
                             WHERE DEPTNO = 30 );
    ```
    
    30번 부서에 존재하지 않는 직책을 걸러내지 못해서 계속 오류가 발생했었다.
    단순하게 **E.JOB ≠** 으로 작성했는데 계속 문제가 생겼었다.
    
    ```sql
    ORA-01427: single-row subquery returns more than one row
    ```
    
    오류에 대해서 찾아보니 서브쿼리에서 조회된 행이 반드시 1개가 와야하는데 1개 이상이라
    오류가 발생한 것이었다.
    (1개 이상 조회되니 ≠ 연산자가 안먹을 수 밖에,,,)
    **NOT IN** 으로 30번 부서의 직책을 걸러주도록 했더니 잘 실행되었다.
    
    ![Untitled](./img/SQL(2)/Untitled%2011.png)
    
- CH10 문제 5번
    
    ![Untitled](./img/SQL(2)/Untitled%2012.png)
    
    ```sql
    -- 오류 코드
    DELETE
    FROM CHAP10HW_EMP E
    WHERE E.EMPNO = (SELECT E.EMPNO
                     FROM CHAP10HW_EMP E, CHAP10HW_SAL S
                     WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL
                     AND S.GRADE = 5)
    ```
    
    CH9 문제 3번과 똑같은 오류였다.
    
    ```sql
    ORA-01427: single-row subquery returns more than one row
    ```
    
    WHERE 절의 서브쿼리에서 1개 이상의 결과가 나오기 때문에 생기는 문제였다.
    =를 **IN**으로 바꾸어 주었더니 맞을 수 있었다.
    
    ```sql
    -- 정답 코드
    DELETE
    FROM CHAP10HW_EMP E
    WHERE E.EMPNO IN (SELECT E.EMPNO
                      FROM CHAP10HW_EMP E, CHAP10HW_SAL S
                      WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL
                            AND S.GRADE = 5);
    ```
    
