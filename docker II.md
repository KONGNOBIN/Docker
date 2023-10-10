# 시작하세요! 도커/쿠버네티스

### 친절한 설명으로 쉽게 이해하는 컨테이너 관리

> 컨테이너 삭제
``` docker
docker rm containerID or containerName  (정지된 container 삭제)
docker rm -f containerID or containerName  (실행중인 container 삭제)
docker container prune (모든 컨테이너 삭제)

EX) 
    docker rm 207e70515e01      (containerID)
    docker rm eager_borg        (containerName)
	docker rm -f eager_borg     (containerName)	
```

> 옵션 1 [컨테이너 ID]

``` docker
docker ps -a -q
-a : 모든 컨테이너
-q : 컨테이너의 ID만 출력

EX)     
    docker stop $(docker ps -a -q)   - 실행상태와 상관없이 모든 컨테이너 정지
    docker rm $(docker ps -a -q)     - 실행상태와 상관없이 모든 컨테이너 삭제    
```

> 옵션 2 [포트연결]
``` docker
-p : 컨테이너의 포트를 호스트의 포트와 바인딩, 연결할 수 있게 하는 옵션 
	 ex) -p (호스트 포트번호):(컨테이너 포트번호)

EX) 
    docker run -i -t --name mywebserver -p 80:80 ubuntu
	
	ex) 아파치 웹 서버를 예시로 들 경우
		아파치 웹 서버 기본 포트 : 80
		
		현재 아파치 웹 서버는 컨테이너의 IP와 80번 포트로 서비스 중
		호스트 IP로 웹 서버 접근을 하려면 컨테이너IP:80 주소로 접근해야 함
		도커의 포트 포워딩옵션인 -p를 사용하여 호스트와 컨테이너를 연결하였으므로
		호스트의 IP와 포트를 통해 컨테이너IP:80으로 접근이 가능.		
	
	* 여러 개의 포트를 외부에 개방하려면 여러 번 써서 설정
    docker run -i -t -p 3306:3306 -p 192.168.0.100:80:80 ubuntu		
	
	* -p 80과 같이 입력 시 컨테이너의 80번 포트를 쓸 수 있는 포스트의 포트 중 하나와 연결한다.
	단 어느포트와 연결됐는지 알 수 없으므로 docker ps 명령어를 입력에 PORTS 항목을 확인해야 한다.
    
```

>간단한 에플리케이션 구축 (+옵션 정보 추가)
``` docker
-d : Detached모드로 컨테이너 실행 (백그라운드에서 동작하는 어플리케이션으로써 실행하도록 설정)
   : 입출력이 없는 상태로 컨테이너 실행.
   : 컨테이너 내부에서 프로그램이 터미널을 차지하는 포그라운드로 실행돼 사용자의 입력을 받지 않음.
   : 반드시 컨테이너에서 프로그램이 실행되어야 하며, 포그라운드 프로그램이 실행되지 않으면 컨테이너는 종료됨.
   : mysql - mysqld / 워드프레스 - apache2-foreground (하나의 터미널을 차지)
   
   ex) docker run -d --name detach_test ubuntu
       → 컨테이너 내부에 터미널을 차지하는 포그라운드로써 동작하는 프로그램이 없으므로 컨테이너는 시작X
	   
	   docker run -i -t \
	   --name mysql_attach_test \
	   -e MYSQL_ROOT_PASSWORD=gk \
	   -e MYSQL_DATABASE=wordpress \
	   mysql:5.7

-e : 컨테이너 내부의 환경변수 설정.

--link : 컨테이너 별명으로 접근하도록 설정.


\(역슬래시) : 각 옵션을 구분지어 가독성을 높힐 수 있음.

EX) 
	데이터베이스와 웹서버 컨테이너 연동
	
	1. mysql image를 사용해 DB컨테이너 생성
    docker run -d \
    --name wordpressdb \
    -e MYSQL_ROOT_PASSWORD=gk \
	-e MYSQL_DATABASE=wordpress \
	mysql:5.7
	
	2. wordpress image를 사용해 웹 서버 컨테이너 생성
	   -p 옵션에서 80을 입력했으므로 호스트의 포트 중 하나와 컨테이너의 80번 포트가 연결됨
	docker run -d \    
    -e WORDPRESS_DB_PASSWORD=gk \
	--name wordpress \
	--link wordpressdb:mysql \    -- wordpressdb 컨테이너를 mysql라는 이름으로 설정
	-p 80 \
	wordpress
	
	3. 호스트의 연결포트 확인
	docker ps
	* 호스트와 바인딩 된 포트만 확인하려면 docker port 명령 사용
	
	4. 호스트의IP:연결포트로 웹서버 접근되는지 CHECK	

```


>환경변수 확인하는 방법