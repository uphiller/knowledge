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
  pp/v1/me?mode=wakeup HTTP/1.1", upstream: "http://[::1]:10080/app/v1/me?mode=wakeup", host: "api.sikdae.com" 
