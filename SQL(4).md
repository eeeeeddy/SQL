# SQL(4)

23.05.09

CH15. 사용자, 권한, 롤 관리

- 사용자
    
    데이터베이스에 접속하여 데이터를 사용/관리하는 계정
    
- 권한
    
    접속 사용자에 따라 접근할 수 있는 데이터 영역을 지정
    
    ```sql
    -- EX
    GRANT
    RESOURCE
    DBA TO SCOTT
    ...
    ```
    
    - 유저/패스워드 생성
        
        ```sql
        CREATE USER ORCLSTUDY
        IDENTIFIED BY ORACLE;
        ```
        
    - 사용자 정보(패스워드) 변경
        
        ```sql
        ALTER USER ORCLSTUDY
        IDENTIFIED BY ORCL;
        ```
        
    - 권한 부여
        
        ```sql
        GRANT RESOURCE, CREATE SESSION, CREATE TABLE TO ORCLSTUDY;
        GRANT SELECT, INSERT ON TEMP TO ORCLSTUDY;
        ```
        
    - 권한 부여 취소
        
        ```sql
        REVOKE SELECT, INSERT ON TEMP FROM ORCLSTUDY;
        ```
        
    - 사용자 정의 ROLE 생성
        
        ```sql
        CREATE ROLE ROLESTUDY;
        
        GRANT CONNECT, RESOURCE, CREATE, VIEW, CREATE SYNONYM TO ROLESTUDY;
        ```
        
    - 생성한 사용자 정의 ROLE을 사용자에게 부여
        
        ```sql
        GRANT ROLESTUDY TO ORCLSTUDY;
        ```
        
    - 사용자에게 부여된 ROLE과 권한 확인
        
        ```sql
        SELECT * FROM USER_SYS_PRIVS;
        SELECT * FROM USER_ROLE_PRIVS;
        ```
        
    - 부여한 사용자 정의 ROLE 취소
        
        ```sql
        REVOKE ROLESTUDY FROM ORCLSTUDY;
        ```
        
    - ROLE 삭제
        
        ```sql
        DROP ROLE ROLESTUDY;
        ```