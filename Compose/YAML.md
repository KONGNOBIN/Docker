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
ports: 서비스의 컨테이너를 개방할 포트 설정
       단, 단일 호스트 환경에서 80:80과 같이 호스트의 특정포트를 서비스의 컨테이너에 연결하면
       docker compose scale 명령어로 서비스의 컨테이너 수를 늘릴수 없다.
build: build 항목에 정의된 dockerfile에서 이미지를 빌드해 서비스의 컨테이너를 생성하도록 설정
extends: 다른 YAML 파일이나 현재 YAML 파일에서 서비스 속성을 상속받게 설정

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

    ports:
      - "8080"
      - "8081-8085"
      - "80:80"

    build: ./composetest
    
    extends: 
      file: extends_compose.yml
      service: extend_web

  mysql:
    image: mysql


  [다른 YAML 파일이거나 현재 YAML파일]
  extend_web:  
    image: ubuntn:14.04
    ports"
      - "80:80"
```
> 네트워크 정의
``` docker
driver: 서비스의 컨테이너가 브리지 네크워크가 아닌 다른 네트워크를 사용할 수 있도록 설정
ipam: IPAM(IP Address Manager)를 위해 사용할 수 있는 옵션
external: YAML 파일을 통해 프로젝트를 생성할 때마다 네트워크를 생성하는 것이 아닌
          기존의 네트워크를 사용하도록 설정

[script]
services:
  myservice:
    image: nginx    
    networks:  [ex] driver
     - mynetwork

    networks:  [ex] external
     - external_network

EX) driver
networks:
  mynetwork:
    driver: overlay
    driver_opts:
      subnet: "255.255.255.0"
      IPAdress: "10.0.0.2"

EX) ipam
networks:
  ipam:
    driver: mydriver
    config:
      subnet: 255.255.255.0           
      ip_range: 172.20.5.0/24
      gateway: 172.20.5.1
      
EX) external
networks:
  external_network:
    external: true
```
> 볼륨 정의
``` docker
driver: 볼륨을 생성할 때 사용될 드라이버를 설정 [어떠한 설정도 하지않으면 local로 설정됨]
external: 프로젝트를 생성할 때마다 생성되는 볼륨을 생성하지 않고 기존 볼륨을 사용하도록 설정

[script]
services:
  web:
    image: alicek106/composetest:web
    volumes:
      - myvolume:/var/www/html


EX) driver
volumes:
  driver: flocker
    driver_opts:
      opt: "1"
      opt2: 2

EX) external
volumes:
  myvolume
    external: true      
```

> YAML 파일 검증
``` docker
명령어 : docker-compose -f(yml 파일 경로) config
오타 검사나 파일 포맷이 적절한지 등 검사하기위한 명령어
```