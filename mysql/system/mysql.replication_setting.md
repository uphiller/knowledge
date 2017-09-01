
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

