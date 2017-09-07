
## 레지스트리 docker 컨테이너 실행
  docker run --name personal-registry -d -p 5000:5000 registry
  
## Dockerfile 생성, 태그 생성
  docker build -t nacyot/hello_docker .
  docker tag nacyot/hello_docker 0.0.0.0:5000/hello_docker

## private registry에서 이미지 받아서 실행하기
  docker run 0.0.0.0:5000/hello_docker
