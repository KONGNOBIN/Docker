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


참고 : https://senticoding.tistory.com/95