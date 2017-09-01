## Slave DB에서 show slave status; 로 상태를 확인한다.
    에러 발생시 Read_Master_Log_Pos 값과 Exec_masterlog_pos 값이 차이가 나며 더이상 올라가지 않는다. 
    해당 에러 사항은 Last Errono와 Last_error를 참조한다.

## master 에서 쿼리 확인
    show binlog events in 'replication.030' from 101319330 limit 3;
    
## slave 에서 리플리케이션 로그 위치 수정후 다시 replication 실행
    mysql> slave stop;   
    mysql> change master to master_log_file='replication.030', master_log_pos=10139456;
    mysql> slave start; 
    
    상당히 많은 양이 에러로 인해 손실되었을 경우는 마스터와 슬레이브간의 데이터 동기화를 새로 하고 리플리케이션을 각각 구동해주는것이 좋다.
