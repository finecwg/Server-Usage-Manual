# MySQL

# 5.1. MySQL

이 문서에서는 Ubuntu 서버에서 MySQL 데이터베이스를 설치, 설정 및 관리하는 방법에 대해 자세히 설명합니다. MySQL은 가장 널리 사용되는 오픈소스 관계형 데이터베이스 관리 시스템(RDBMS) 중 하나로, 웹 애플리케이션 및 데이터 중심 프로젝트에 많이 사용됩니다. 이 문서에서는 MySQL 서비스 관리, 사용자 및 데이터베이스 관리, 백업 및 복원 등 데이터베이스 관리자가 알아야 할 주요 내용을 다룹니다.

## 5.1.1. MySQL 설치

### 5.1.1.1. MySQL 서버 설치

- MySQL 서버 패키지 설치:
    
    ```bash
    sudo apt update
    sudo apt install mysql-server
    
    ```
    

`apt` 명령어를 사용하여 MySQL 서버 패키지를 설치할 수 있습니다. 설치 과정에서 루트 사용자의 비밀번호를 설정하라는 메시지가 표시될 수 있습니다.

### 5.1.1.2. 보안 설정

- MySQL 보안 설정 스크립트 실행:
    
    ```bash
    sudo mysql_secure_installation
    
    ```
    

`mysql_secure_installation` 스크립트를 실행하여 MySQL 설치의 보안을 강화할 수 있습니다. 이 스크립트는 루트 사용자의 비밀번호 설정, 익명 사용자 제거, 원격 루트 로그인 비활성화 등의 작업을 수행합니다.

## 5.1.2. MySQL 서비스 관리

### 5.1.2.1. 서비스 시작, 중지 및 재시작

- MySQL 서비스 시작:
    
    ```bash
    sudo systemctl start mysql
    
    ```
    
- MySQL 서비스 중지:
    
    ```bash
    sudo systemctl stop mysql
    
    ```
    
- MySQL 서비스 재시작:
    
    ```bash
    sudo systemctl restart mysql
    
    ```
    

`systemctl` 명령어를 사용하여 MySQL 서비스를 시작, 중지 및 재시작할 수 있습니다.

### 5.1.2.2. 서비스 상태 확인

- MySQL 서비스 상태 확인:
    
    ```bash
    sudo systemctl status mysql
    
    ```
    

`systemctl status` 명령어를 사용하여 MySQL 서비스의 현재 상태를 확인할 수 있습니다. 서비스가 실행 중인지, 오류가 발생했는지 등의 정보를 확인할 수 있습니다.

### 5.1.2.3. 부팅 시 자동 시작 설정

- MySQL 서비스 자동 시작 설정:
    
    ```bash
    sudo systemctl enable mysql
    
    ```
    

`systemctl enable` 명령어를 사용하여 시스템 부팅 시 MySQL 서비스가 자동으로 시작되도록 설정할 수 있습니다.

## 5.1.3. MySQL 사용자 및 권한 관리

### 5.1.3.1. 사용자 생성

- MySQL 루트 사용자로 로그인:
    
    ```bash
    sudo mysql -u root -p
    
    ```
    
- 새 사용자 생성 및 권한 부여:
    
    ```sql
    CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
    GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost';
    FLUSH PRIVILEGES;
    
    ```
    

MySQL 루트 사용자로 로그인한 후, `CREATE USER` 문을 사용하여 새 사용자를 생성하고 `GRANT` 문을 사용하여 권한을 부여할 수 있습니다. 위의 예시에서는 `newuser`라는 사용자를 생성하고 모든 권한을 부여하였습니다.

### 5.1.3.2. 사용자 비밀번호 변경

- 사용자 비밀번호 변경:
    
    ```sql
    ALTER USER 'username'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new_password';
    FLUSH PRIVILEGES;
    
    ```
    

`ALTER USER` 문을 사용하여 기존 사용자의 비밀번호를 변경할 수 있습니다. `FLUSH PRIVILEGES` 문을 사용하여 변경 사항을 적용합니다.

### 5.1.3.3. 사용자 삭제

- 사용자 삭제:
    
    ```sql
    DROP USER 'username'@'localhost';
    
    ```
    

`DROP USER` 문을 사용하여 사용자를 삭제할 수 있습니다.

### 5.1.3.4. 사용자 권한 관리

- 사용자 권한 부여:
    
    ```sql
    GRANT SELECT, INSERT, UPDATE ON database_name.* TO 'username'@'localhost';
    
    ```
    
- 사용자 권한 제거:
    
    ```sql
    REVOKE DELETE ON database_name.* FROM 'username'@'localhost';
    
    ```
    
- 사용자 권한 확인:
    
    ```sql
    SHOW GRANTS FOR 'username'@'localhost';
    
    ```
    

`GRANT` 문을 사용하여 사용자에게 특정 권한을 부여하고, `REVOKE` 문을 사용하여 권한을 제거할 수 있습니다. `SHOW GRANTS` 문을 사용하여 사용자의 현재 권한을 확인할 수 있습니다.

## 5.1.4. MySQL 데이터베이스 관리

### 5.1.4.1. 데이터베이스 생성

- 데이터베이스 생성:
    
    ```sql
    CREATE DATABASE database_name;
    
    ```
    

`CREATE DATABASE` 문을 사용하여 새 데이터베이스를 생성할 수 있습니다.

### 5.1.4.2. 데이터베이스 삭제

- 데이터베이스 삭제:
    
    ```sql
    DROP DATABASE database_name;
    
    ```
    

`DROP DATABASE` 문을 사용하여 데이터베이스를 삭제할 수 있습니다. 주의: 이 작업은 데이터베이스와 관련된 모든 데이터를 영구적으로 삭제합니다.

### 5.1.4.3. 데이터베이스 선택

- 데이터베이스 선택:
    
    ```sql
    USE database_name;
    
    ```
    

`USE` 문을 사용하여 작업할 데이터베이스를 선택할 수 있습니다.

### 5.1.4.4. 데이터베이스 목록 확인

- 데이터베이스 목록 확인:
    
    ```sql
    SHOW DATABASES;
    
    ```
    

`SHOW DATABASES` 문을 사용하여 서버에 존재하는 모든 데이터베이스의 목록을 확인할 수 있습니다.

## 5.1.5. MySQL 백업 및 복원

### 5.1.5.1. 데이터베이스 백업

- `mysqldump`를 사용한 데이터베이스 백업:
    
    ```bash
    mysqldump -u username -p database_name > backup_file.sql
    
    ```
    

`mysqldump` 명령어를 사용하여 데이터베이스를 SQL 형식의 백업 파일로 내보낼 수 있습니다. 사용자 이름과 데이터베이스 이름을 지정해야 하며, 백업 파일의 이름과 경로를 지정할 수 있습니다.

### 5.1.5.2. 데이터베이스 복원

- SQL 백업 파일에서 데이터베이스 복원:
    
    ```bash
    mysql -u username -p database_name < backup_file.sql
    
    ```
    

SQL 백업 파일에서 데이터베이스를 복원하려면 `mysql` 명령어를 사용하여 백업 파일을 데이터베이스로 가져올 수 있습니다. 사용자 이름과 데이터베이스 이름을 지정해야 하며, 백업 파일의 경로를 제공해야 합니다.

### 5.1.5.3. 자동 백업 스크립트 작성

- 자동 백업 스크립트 예시:
    
    ```bash
    #!/bin/bash
    
    # MySQL 접속 정보
    USER="username"
    PASSWORD="password"
    DATABASE="database_name"
    
    # 백업 파일 이름 및 경로
    BACKUP_DIR="/path/to/backup/directory"
    TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")
    BACKUP_FILE="${BACKUP_DIR}/${DATABASE}_${TIMESTAMP}.sql"
    
    # 백업 실행
    mysqldump -u $USER -p$PASSWORD $DATABASE > $BACKUP_FILE
    
    # 오래된 백업 파일 삭제 (선택 사항)
    find $BACKUP_DIR -type f -name "${DATABASE}_*.sql" -mtime +30 -delete
    
    ```
    

자동 백업 스크립트를 작성하여 데이터베이스를 정기적으로 백업할 수 있습니다. 위의 예시 스크립트는 MySQL 접속 정보, 백업 파일 이름 및 경로를 설정하고 `mysqldump` 명령어를 사용하여 백업을 수행합니다. 또한 오래된 백업 파일을 자동으로 삭제하는 기능도 포함되어 있습니다.

## 5.1.6. MySQL 모니터링 및 성능 최적화

### 5.1.6.1. 느린 쿼리 로그 설정

- 느린 쿼리 로그 활성화:
    
    ```sql
    SET GLOBAL slow_query_log = 'ON';
    SET GLOBAL slow_query_log_file = '/var/log/mysql/slow-query.log';
    SET GLOBAL long_query_time = 1;
    
    ```
    

느린 쿼리 로그를 활성화하여 실행 시간이 긴 쿼리를 식별하고 최적화할 수 있습니다. `slow_query_log` 변수를 `'ON'`으로 설정하고, `slow_query_log_file` 변수를 사용하여 로그 파일의 경로를 지정합니다. `long_query_time` 변수를 사용하여 느린 쿼리로 간주되는 실행 시간 임계값을 설정할 수 있습니다.

### 5.1.6.2. 인덱스 사용 확인

- 테이블의 인덱스 확인:
    
    ```sql
    SHOW INDEX FROM table_name;
    
    ```
    

`SHOW INDEX` 문을 사용하여 특정 테이블의 인덱스 정보를 확인할 수 있습니다. 인덱스를 적절하게 사용하면 쿼리 성능을 향상시킬 수 있습니다.

### 5.1.6.3. EXPLAIN 문을 사용한 쿼리 분석

- EXPLAIN 문을 사용한 쿼리 분석:
    
    ```sql
    EXPLAIN SELECT * FROM table_name WHERE condition;
    
    ```
    

`EXPLAIN` 문을 사용하여 쿼리의 실행 계획을 분석할 수 있습니다. 이를 통해 쿼리의 성능 병목 지점을 식별하고 최적화 방안을 모색할 수 있습니다.

### 5.1.6.4. 캐시 및 버퍼 설정 조정

- 캐시 및 버퍼 설정 조정:
    
    ```sql
    SET GLOBAL query_cache_size = 1024 * 1024 * 32; -- 32MB
    SET GLOBAL innodb_buffer_pool_size = 1024 * 1024 * 1024; -- 1GB
    
    ```
    

MySQL의 캐시 및 버퍼 설정을 조정하여 성능을 최적화할 수 있습니다. `query_cache_size` 변수를 사용하여 쿼리 캐시의 크기를 설정하고, `innodb_buffer_pool_size` 변수를 사용하여 InnoDB 버퍼 풀의 크기를 설정할 수 있습니다. 적절한 값은 시스템 리소스와 워크로드에 따라 달라질 수 있습니다.

이 문서에서는 MySQL 데이터베이스의 설치, 서비스 관리, 사용자 및 권한 관리, 데이터베이스 관리, 백업 및 복원, 모니터링 및 성능 최적화에 대해 자세히 다루었습니다. MySQL은 강력하고 유연한 데이터베이스 시스템으로, 이를 효과적으로 관리하고 활용하면 데이터 중심 애플리케이션 및 서비스를 안정적으로 운영할 수 있습니다. 이 문서를 참조하여 MySQL 데이터베이스 관리에 필요한 지식과 기술을 습득하고 적용할 수 있을 것입니다.