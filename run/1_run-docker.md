# [Option 1] Docker 실행

```note
Pre-Built 된 FOSSology Docker 이미지가 [Docker Hub](https://hub.docker.com/r/fossology/fossology)에서 배포됩니다.
```

## 필요 사항


- Docker 환경 설정

## 실행 방법

- 아래 명령 실행
```
docker run -p 8081:80 fossology/fossology
```
 
## 접속정보

- 접속 주소 : http://IP_OF_DOCKER_HOST:8081/repo

- ID : fossy

- PW : fossy

## 기타
```warning
Docker Instance 종료 시 모든  정보가 삭제됩니다.

분석 결과, 생성된 계정 등 정보를 유지하기 위해서는 1) [Source Install](http://10.178.85.79:8091/run/2_install-from-source.html)을 하거나, 2) [외부 PostgreSQL DB와 연동](http://10.178.85.79:8091/run/2_install-from-source.html)해야 합니다.
```
