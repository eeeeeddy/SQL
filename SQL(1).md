# SQL(1)

23.05.02

---

- **DIKW 피라미드**
    - Data
    - Information
    - Knowledge
    - Wisdom

---

1. **오라클 타입**
    1. **CHAR()** : 일정 길이가 정해져 있을 때 데이터 타입 (n자리)
    2. **VARCHAR2()** : 일정 길이가 정해져 있지 않을 때 데이터 타입
    3. **Number(전체자릿수, 소수 자릿수)**
    4. **Date()** : YYYY-MM-DD
    5. **TimeStamp()** : YYYY-MM-DD HH:MM:SS
    
2. **SQL (Structured Query Language)**
    1. **DQL (Data Query Language, 데이터 조회 언어)**
        
        RDBMS에 저장된 데이터를 원하는 방식으로 조회하는 명령어
        
        >> **SELECT**
        
    2. **DML (Data Manipulate Language, 데이터 조작 언어)**
        
        RDBMS 내 테이블의 데이터를 삽입/수정/삭제하는 명령어
        
        >> **INSERT, UPDATE, DELETE**
        
    3. **DDL (Data Definition Language, 데이터 정의 언어)**
        
        RDBMS 내 데이터 관리를 위해 테이블을 포함한 여러 객체를 생성/수정/삭제하는 명령어
        
        데이터베이스 스키마, 테이블, 뷰, 인덱스 등을 정의하고 조작하는데 사용
        
        >> **CREATE, ALTER, DROP**
        
    4. **TCL (Transaction Control Language, 트랜잭션 제어 언어)**
        
        트랜잭션 데이터의 영구저장/취소 등과 관련된 명령어
        
        >> **COMMIT, ROLLBACK, SAVEPOINT**
        
    5. **DCL (Data Control Language, 데이터 제어 언어)**
        
        데이터 사용, 접근 권한과 관련된 명령어
        
        >> **GRANT, REVOKE**
        
3. **RDBMS의 구성요소 중 Key의 종류**
    
    적절한 Key를 선택하고 관리하면 데이터베이스에서 데이터의 무결성을 보장하고, 효율적인
    
    데이터 검색과 처리가 가능해진다.
    
    1. **Primary Key (기본키)**
        
        테이블의 각 레코드(row)를 고유하게 식별할 수 있는 유일한 값.
        
        한 테이블에서 하나의 기본키만 존재할 수 있으며, NULL 값을 가질 수 없다.
        
    2. **Foreign Key (외래키)**
        
        다른 테이블의 기본키와 연결된 열(Column).
        
        외래키는 한 테이블의 데이터와 다른 테이블의 데이터를 연결하는데 사용.
        
    3. **Candidate Key (후보키)**
        
        테이블에서 기본키가 될 수 있는 열(Column) 또는 열의 조합.
        
        테이블에서 유일한 값을 가지며, NULL 값을 가질 수 없다.
        
    4. **Alternate Key (대체키)**
        
        후보키 중에서 기본키가 선택되지 않은 나머지 후보키들
        
    5. **Composite Key (복합키)**
        
        두 개 이상의 열(Column)을 조합하여 기본키로 사용하는 것.
        
    6. **Unique Key (고유키)**
        
        RDBMS에서 테이블의 특정 열(Column)이나 열의 조합에 대해 중복된 값을 허용하지
        
        않도록 제약을 거는 것을 말함. 즉, 테이블 내에서 유일한 값을 가져야하는 열이나
        
        열의 조합을 지정하여 중복 데이터를 방지하는 역할
        
4. **Oracle Database’s Object**
    1. **Table**
        
        행과 열로 구성된 데이터의 집합.
        
    2. **Index**
        
        테이블에서 빠른 데이터 검색을 위해 생성된 객체.
        
    3. **View**
        
        하나 이상의 테이블이나 다른 뷰의 결과를 기반으로 생성된 가상의 테이블로, 실제 데이터를
        
        저장하지 않음.
        
    4. **Sequence**
        
        일련번호 생성기로, 특정 열(Column)에 값을 저장할 때 자동으로 순차적으로 증가하는
        
        일련번호를 생성할 수 있음.
        
    5. **Synonym**
        
        오라클 객체의 별칭을 지정. 다른 객체의 대체 이름으로 사용.
        
    6. **Precedure**
        
        SQL 문장들의 집합으로, 이름을 가지고 저장되며, 데이터베이스에서 반복적으로 수행되는
        
        작업을 수행하는데 사용, 프로그래밍 연산 및 기능 수행이 가능. (반환 값 없음)
        
    7. **Function**
        
        입력값을 받아 프로그래밍 연산 및 기능 수행이 가능. (반환 값 있음)
        
    8. **Package**
        
        프로시저, 함수, 변수 등의 객체를 하나의 논리적 단위로 묶어서 관리할 수 있도록 도와줌.
        
    9. **Trigger**
        
        데이터베이스에서 발생하는 이벤트에 대해 자동으로 실행되는 프로그램.
        
        데이터 관련 작업의 연결 및 방지 관련 기능을 제공.
        
5. **SQL 기본 구조**
    
    ```sql
    SELECT *
    FROM 
    WHERE ~ AND(OR) ~
    GROUP BY
    HAVING
    ORDER BY
    ```
    
    여기서 **GROUP BY, HAVING, ORDER BY**에 대해서 조금만 더 자세히 기록하고자 한다.
    
    1. **GROUP BY**
        
        특정 열(Column)을 그룹화한다.
        
        ```sql
        -- Column 그룹화
        SELECT 컬럼 FROM 테이블 GROUP BY 그룹화할 컬럼;
        ```
        
        ```sql
        -- 조건 처리 후 Column 그룹화
        SELECT 컬럼 FROM 테이블 WHERE 조건식 GROUP BY 그룹화할 컬럼;
        ```
        
    2. **HAVING**
        
        **WHERE**와 비슷하게 조건을 설정한다.
        
        그러나 **WHERE**는 그룹화 하기 전에, **HAVING**은 그룹화 후에 사용한다.
        
        ```sql
        -- 조건 처리 후 Column 그룹화 후 조건 처리
        SELECT 컬럼 FROM 테이블 WHERE 조건식 GROUP BY 그룹화할 컬럼 HAVING 조건식;
        ```
        
    3. **ORDER BY**
        
        데이터를 정렬하는데 사용.
        
        **ASC(오름차순)**과 **DESC(내림차순)**으로 정렬 방법 지정이 가능하다. 
        
        기본적으로 **ASC(오름차순)** 정렬이다.
        
        ```sql
        SELECT 컬럼 FROM 테이블 [WHERE 조건식]
        GROUP BY 그룹화할 컬럼 [HAVING 조건식] ORDER BY 컬럼1 [, 컬럼2, 컬럼3 ...];
        ```
        
    
6. WHERE절에서 사용 가능한 연산자
    1. **AND / OR**
    2. **산술 연산자** : +, -, *, /
    3. **비교 연산자** : <, >, =, ≠
    4. **논리 부정 연산자 (NOT)**
        
        ```sql
        COL NOT A = x;
        ```
        
    5. **(NOT) IN 연산자**
        
        ```sql
        SELECT COL
        FROM TABLE
        WHERE (NOT) COL in (DATA1, DATA2, ... ,)
        ```
        
    6. **(NOT) BETWEEN A AND B 연산자**
        
        ```sql
        SELECT COL
        FROM TABLE
        WHERE COL (NOT) BETWEEN MIN AND MAX;
        ```
        
    7. **LIKE 연산자**
        - _ : 어떤 값이든 상관없이 한 개의 문자 데이터를 의미
        - % : 길이와 상관없이 (문자 없는 경우도 포함) 모든 문자 데이터를 의미
    8. **IS (NOT) NULL 연산자**
        - IS NULL : 대상 데이터가 NULL일 때 TRUE
        - IS NOT NULL : 대상 데이터가 NULL이 아닐 때 TRUE
    9. **집합 연산자**
        - UNION : 중복을 허용하지 않는 합집합
        - UNION ALL : 중복을 허용하는 합집합
        - MINUS : 차집합
        - INTERSECT : 교집합
    10. **연산자 우선순위**
        
        ![Untitled](SQL(1)%20c0e3f412bf49494989c833bdff65d56f/Untitled.png)
        