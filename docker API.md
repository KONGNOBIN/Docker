# Docker API


> Rest API 통신방법

``` docker

	원격으로 도커를 제어하는 방법

	Docker client (docker) : Docker deamon 에 서비스를 요청하며, REST API, UNIX socket, network interface를 이용하여, 통신.

	Docker deamon(dockerd) : image, container, network, volume과 같은 Docker object들을 관리하고 Docker client로 부터 들어오는 요청을 수행하는 역할을 한다. 또한 다른 Docker deamon과 통신 가능

	따라서, API 요청을 받기 위해
	Docker deamon은 Socket 방식의 3가지 타입을 가진다. (unix, tcp, fd)

	1. tcp 활성화		
    [명령어]
		dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock

	-H tcp://0.0.0.0:2375 : 어떤 ip에 있는 client들이 모두(0.0.0.0) 2375포트로 접근할 수 있도록 허용해주는 tcp설정
	-H unix:///var/run/docker.sock : 로컬 client접속을 허용해주는 유닉스 소켓 설정

	docker는 기본적으로 2375 port
	deamon과 암호화된 통신에서는 2376 port	

	* 만약 Error 발생할 경우 데몬 종료 후 실행

	docker 종료 (데몬 종료)
	[명령어]
		sudo systemctl stop docker.service

	docker 시작
	[명령어]
		sudo systemctl start docker.service

	데몬 실행여부 
	[명령어]
		ps -ef | grep dockerd

	2. HTTP Request 통해 API 확인
	하단 참고
	
```
> docker API documentation (1.4v)

[docker API documentation](https://docs.docker.com/engine/api/v1.40/)	
		

> Container List 확인

	IP주소:2375/containers/json (=docker ps)
![alt text](./images/docker%20API%20Test%20I.png)


> Container 생성

	IP주소:2375/containers/containerID/start (=docker start containerID)
![alt text](./images/docker%20API%20Test%20II.png)


> 기타 다른 방법 (안됨..)
```docker
	0. docker.service파일의 TCP소켓 접속허용 설정
	[명령어]
		vi /lib/systemd/system/docker.service

	그림 1 처럼 수정	
		ExecStart=/usr/bin/dockerd -H unix:///var/run/docker.sock -H tcp://0.0.0.0:2376
```
> 그림 1
![alt text](./images/docker%20remote%20vi.png)