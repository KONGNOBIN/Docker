# Docker API

### Postman API Test


> docker 데몬

``` docker
	데몬 실행 
		: sudo dockerd -H tcp://127.0.0.1:2375
	데몬 실행여부 
		: ps -ef | grep dockerd

	docker 종료 
	   : systemctl stop docker.service
	파일 삭제 
	   : cat [경로]
		 ex) cat /var/run/docker.pid


	1. docker.service파일의 TCP소켓 접속허용 설정
		: vi /lib/systemd/system/docker.service

	    아래와 같이 수정 [그림 1 참고]

		ExecStart=/usr/bin/dockerd -H unix:///var/run/docker.sock -H tcp://0.0.0.0:2376

		-H unix:///var/run/docker.sock : 로컬 client접속을 허용해주는 유닉스 소켓 설정

		-H tcp://0.0.0.0:2376 : 어느 ip에 있는 client들이 모두(0.0.0.0) 2376포트로 접근할 수 있도록 허용해주는 tcp설정  (보안을 위해 비권장)
```
> 그림 1
![alt text](./images/vi.jpg)
