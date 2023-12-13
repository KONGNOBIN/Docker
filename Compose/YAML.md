### Docker YAML 작성 요령

> 버전 정의
``` docker
YAML 파일의 맨 윗부분 명시

[script]
version: '3.0'  [버전 3은 도커 스윔모드와 호환되는 버전]
```

> 서비스 정의
``` docker
도커 컴포즈로 생성할 컨테이너 옵션 정의
서비스 이름 : services 하위 항목으로 정의
컨테이너 옵션 : 서비스 이름의 하위 항목으로 정의

image: 컨테이너 생성할 떄 쓰일 이미지
links: 서비스 명만으로 접근할 수 있도록(별칭) 설정할 수 있는 옵션
environment: 컨테이너 내부에서 사용할 환경변수 지정
command: 컨테이너가 실행될 때 수행할 명령어를 설정
depends_on: 특정 컨테이너에 대한 의존관계를 나타내며, 이 항목에 명시된 컨테이너가 먼저 생성됨

[script]
services:
  my_container_1:
    image: alicek106/composetest:mysql
    link: 
      - db
      - db:database
    environment:
      - MYSQL_ROOT_PASSWORD=gk
      - MYSQL_DATABASE_NAME=mydb

    environment:
      MYSQL_ROOT_PASSWORD=gk
      MYSQL_DATABASE_NAME=mydb

    command: apachectl -DFOREGROUND   

    depends_on
      - mysql


  mysql:
    image: mysql
```
> 네트워크 정의
``` docker
```
> 볼륨 정의
``` docker
```

> YAML 파일 검증
``` docker
```