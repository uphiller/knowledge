
## 현상
idx 컬럼을 생성하고, idx컬럼을 Auto-Increment 설정을 하였다.
값을 하나 정상적으로 입력했을 때 idx는 1
두번째 값은 실패
세번째 값은 정상적으로 입력 : idx 3
두번째 입력이 실패했는데도 불구하고 Auto-Increment 값은 자동으로 증가하는 현상을 발견

## 이유
MySQL 버전 5.1.22 전후로 innodb_autoinc_lock_mode 기본값이 변경되었기 때문에 발생한 현상

5.1.22 이전까지는 innodb_autoinc_lock_mode default 값이 0으로 traditional mode를 사용
5.1.22 이후부터는 default 값이 1인 consecutive mode가 사용

1. innodb_autoinc_lock_mode=0
 - Auto-increment lock이 테이블 레벨로 동작
 - Insert into로 실제 삽입된 경우가 아닌 업데이트 경우에는 해당 테이블의 Auto-increment 값을 증가시키지 않는 옵션

2. innodb_autoinc_lock_mode=1 
 - 단순한 insert에 대해서는 lock을 걸지 않고, INSERT 되는 레코드 건수를 정확히 예측할 수 있을 때 사용해야 함
 - latch(mutex)를 이용하여 처리함

3. innodb_autoinc_lock_mode=2
 - Auto-increment lock을 사용하지 않고, 항상 latch(mutex)를 사용함
 - 대량의 INSERT 실행 도중 다른 커넥션에서 INSERT를 수행할 수 있어. 동시 처리 성능이 높음.
 - 그러나 복제를 사용하는 경우 마스터와 슬레이브의 자동 증가 값이 달라질 가능성이 있으니 사용 시 주의가 필요

## 해결 방법
결과적으로 문제를 해결하기 위해서 innodb_autoinc_lock_mode 값을 기본값이 아닌 0으로 설정하여 사용해야 함
이 경우, Concurrent insert가 매우 빈번히 발생할 경우 Auto-increment lock이 병목이 될 가능성이 있으니 주의해야 함

SET global innodb_autoinc_lock_mode = 0;
커맨드 상에서 위와 같이 입력했을 때 Variable 'innodb_autoinc_lock_mode' is a read only variable 와 같은 에러가 나올 수 있다.

이때는 /etc/mysql/mysql.conf.d/mysql.cnf 파일을 열어서 innodb_autoinc_lock_mode = 0 이라고 추가해 준다.
