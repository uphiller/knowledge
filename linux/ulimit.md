# 계정 별 프로세스 file descriptor(소켓 등 포함) 제한 확인

## Soft limit(기본값) 확인
    ulimit -a, ulimit -n
    core file size          (blocks, -c) 0 
    data seg size           (kbytes, -d) unlimited 
    file size               (blocks, -f) unlimited 
    pending signals                 (-i) 1024 
    max locked memory       (kbytes, -l) 32 
    max memory size         (kbytes, -m) unlimited 
    open files                      (-n) 1024 
    pipe size            (512 bytes, -p) 8 
    POSIX message queues     (bytes, -q) 819200 
    stack size              (kbytes, -s) 10240 
    cpu time               (seconds, -t) unlimited 
    max user processes              (-u) 69632 
    virtual memory          (kbytes, -v) unlimited 
    file locks                      (-x) unlimited 
  
## Hard limit 확인
     ulimit -Ha
     core file size          (blocks, -c) 0 
     data seg size           (kbytes, -d) unlimited 
     file size               (blocks, -f) unlimited 
     pending signals                 (-i) 1024 
     max locked memory       (kbytes, -l) 32 
     max memory size         (kbytes, -m) unlimited 
     open files                      (-n) 2048 
     pipe size            (512 bytes, -p) 8 
     POSIX message queues     (bytes, -q) 819200 
     stack size              (kbytes, -s) 10240 
     cpu time               (seconds, -t) unlimited 
     max user processes              (-u) 69632 
     virtual memory          (kbytes, -v) unlimited 
     file locks                      (-x) unlimited 

## 관련 에러 상황
  nginx 로그
  
        2017/10/10 12:19:14 [alert] 1433#0: *74007788 socket() failed (24: Too many open files) while connecting to upstream, client: 39.7.19.46, server: api.sikdae.com, request: "GET /a
        pp/v1/me?mode=wakeup HTTP/1.1", upstream: "http://[::1]:10080/app/v1/me?mode=wakeup"

## 해결
  nginx 프로세스에서 처리할 수 있는 파일 사이즈가 linux의 기본으로 1024로 설정되어있어서 갑자스럽게 많은 요청을 처리할때 발생할 수 있다
  
  
  - 임시
        
        ulimit -n 10240 으로 처리량을 늘려줌
        
  - 영구
        
        root 계정으로 수행해야 함
        soft 수치가 hard 수치보다 크면 안됨 
        vim /etc/security/limits.conf 
        [user id]         soft    nofile          10240 -> soft limit 
        [user id]         hard    nofile          10240 -> hard limit 
        
        root         soft    nofile          10240
        root         hard    nofile          10240
        nginx        soft    nofile          10240
        nginx        hard    nofile          10240   
        
 - 해당 프로세스도 바꿔야함 
 
        prlimit --nofile --output RESOURCE,SOFT,HARD --pid 1234
        prlimit --nofile=500000 --pid=1234
 
 - nginx 옵션 추가
        
        worker_rlimit_nofile 8192;
  
  
