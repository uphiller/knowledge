
## mysql my.cnf 환경설정(MasterServer)
    ----------------------------------------------------------------------------------------------------------    
    [mysqld]

        #log setting    
        log-bin = mysql-bin             // 로그파일명
        max_binlog_size = 100M      // 로그파일크기
        expire_logs_days = 7          // 로그보존주기

        #Replication for master server    
        server-id = 3                      // 서버 식별자(유니크)
        binlog_do_db = test1          // 리플리케이션DB명(생략시엔 전체DB를 리플리케이션함)
        binlog_do_db = test2         // 여러 개의 DB일경우, 계속 추가
    ----------------------------------------------------------------------------------------------------------

## mysql 재가동(MasterServer)

## 유저추가(MasterServer)
    GRANT REPLICATION SLAVE ON *.* TO 'username'@'%' IDENTIFIED BY 'password';
    FLUSH PRIVILEGES;

## 데이터 백업(MasterServer)

## 데이터 복사(SlaveServer)

## 체크 
    SHOW MASTER STATUS;

## 환경설정(SlaveServer)
    vi /etc/my.cnf
    ----------------------------------------------------------------------------------------------------------    
    [mysqld]

    #Replication for master server
    server-id = 4                       // 서버 식별자(유니크)
    replicate-do-db = test1        // 리플리케이션DB명(생략시엔 전체DB를 리플리케이션함)
    replicate-do-db = test2        // 여러 개의 DB일경우, 계속 추가
    ----------------------------------------------------------------------------------------------------------   

## master 설정, slave start sql 실행(SlaveServer)
    mysql> CHANGE MASTER TO
        -> MASTER_HOST='master-pri',
        -> MASTER_USER='repl',
        -> MASTER_PASSWORD='repl',
        -> MASTER_PORT=3306,
        -> MASTER_LOG_FILE='mysql-bin.000008',
        -> MASTER_LOG_POS=456730,
        -> MASTER_CONNECT_RETRY=5;
    mysql> START SLAVE;
    mysql> show slave status;

## bin log 정리
        purge master logs to 'mysql-bin.000186';

## bin log 보관 주기 설정
        set global expire_logs_days=7;
        SHOW VARIABLES LIKE '%expire%';
        
## 관련 옵션 my.cnf
        --log-slave-updates
            일반적으로, 슬레이브는 마스터 서버에서 전달 받은 업데이트에 대해서는 자신의 바이너리 로그에 기록하지 않는다. 
            이 옵션은 SQL 쓰레드가 실행한 업데이트를 자신의 바이너리 로그에 기록하도록 만든다
        -- binlog_format
            Statement: 가장 오래된 Format으로 데이터 변경에서 사용되는 모든 쿼리를 쿼리대로 저장하는 방식을 말함(5.7R까지 기본)
            Row Format: 변경 작업으로 변경된 모든 Row의 정보를 기록하는 방식
            Mixed Format: Statement 방식과 Row 방식을 혼합한 방식으로 기본은 Statement 방식이고, 몇몇의 경우에는 Row 방식으로 동작하는 방식

            * Row 와 Statement
            데이터베이스에 작은 변경을 많이 발생 시키는 쓰레드는 열 기반 로깅을 선호한다
            where 구문에 있는 많은 열과 매치가 되는 업데이트를 실행하는 쓰레드는 명령문 기반 로깅을 선호하는데,
            그 이유는 많은 열을 로깅하는 것 보다는 적은 명령문 로깅이 효과적이기 때문이다.
            마스터에서 오랜 실행 시간 동안 실행되지만 비교적 적은 수의 열만을 수정하는 명령문들이 있다.
            이러한 명령문들은 열 기반 로깅을 사용해서 복제하는 것이 보다 효과적이다.

