em # 시작하세요! 도커/쿠버네티스

### 친절한 설명으로 쉽게 이해하는 컨테이너 관리

> 컨테이너 로깅
``` docker
docker logs
컨테이너 로스는 JSON형태로 도커 내부에 저장됨.
경로 : cat /var/lib/docker/container/${CONTAINER_ID}/${CONTAINER_ID}-json.log
--log-opt max-size 컨테이너 로그파일 크기 지정 가능
--log-opt max-file 컨테이너 로그파일 개수 지정 가능

EX) 
    docker run -d \
	> --name mysqllog
	> -e MYSQL_ROOT_PASSWORD=gk \	
	> mysql:5.7

	docker logs mysqllog

	2023-10-23 13:42:02+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.1.0-1.el8 started.
	2023-10-23 13:42:02+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
	2023-10-23 13:42:02+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.1.0-1.el8 started.
	(... ...)

    환경변수 없이 생성한경우 (mysql은 환경변수가 없으면 컨테이서 실행이 안됨)
	docker run -d \
	> --name mysqllog	
	> mysql:5.7

	docker logs mysqllog (오류확인)
	2023-10-23 13:44:27+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.1.0-1.el8 started.
	2023-10-23 13:44:28+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
	2023-10-23 13:44:28+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.1.0-1.el8 started.
	2023-10-23 13:44:28+00:00 [ERROR] [Entrypoint]: Database is uninitialized and password option is not specified
    You need to specify one of the following as an environment variable:
    - MYSQL_ROOT_PASSWORD
    - MYSQL_ALLOW_EMPTY_PASSWORD
    - MYSQL_RANDOM_ROOT_PASSWORD

	이외
	docker logs --tail 10 mysqllog [밑에서 10줄]
	docker logs --since 1313132213 mysqllog [유닉스 시간 입력, 특정시간 이후 로그 확인]
	docker logs -f -t myysqllog [실시간 출력 내용 확인]

    run -i, -t 사용시 bash 셸 등에서 입출력한 내용도 확인가능   
	
```

> syslog 로그

``` docker    
```

> fluentd 로깅

``` docker    
```

> 아마존 클라우드워치 로그

``` docker    
```