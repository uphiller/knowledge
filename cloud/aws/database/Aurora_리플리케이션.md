**SET SQL_SAFE_UPDATES =0;

**CALL mysql.rds_set_external_master('home.mealc.co.kr', 3306, 'repl', 'tnavhdlsxm', 'mysql-bin.000001', 40890731, 0);

**CALL mysql.rds_start_replication;

**SHOW SLAVE STATUS

**CALL mysql.rds_stop_replication;
