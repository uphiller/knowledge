### 리플리케이션 쿼리
    SET SQL_SAFE_UPDATES =0;
    CALL mysql.rds_set_external_master(host, port, id, passwd, 'mysql-bin.000001', 40890731, 0);
    CALL mysql.rds_start_replication;
    SHOW SLAVE STATUS
    CALL mysql.rds_stop_replication;
