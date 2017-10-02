## 원격지 접속을 위한 지정된 경로에 host 등록
    vi /etc/ansible/hosts
    mail.domain.com
    main2.domain.com
    
## shell 스크립트 실행
    ---
    - hosts: localhost #호스트명
      tasks:
      - name: excution shell #주석
        shell: echo "test" #쉘스크립트
        
## 원격지로 파일 copy 
    ---
    - hosts: 192.168.10.10
      sudo: yes
      tasks:
      - name: copy file
        copy:
         src: ./test.conf
         dest: /etc/nginx/conf.d/
         
## 실행시 파라미터 설정 및 파일에 파라미터 세팅
    ansible-playbook test.yml --extra-vars "subUrl=test22"
    ---
    - hosts: 192.168.10.10
      sudo: yes
      tasks:
      - name: copy file
        copy:
         src: ./{{ subUrl }}.conf
         dest: /etc/nginx/conf.d/
