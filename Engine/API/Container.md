# Docker API Doc

> docker API documentation (1.4v)

[docker API documentation](https://docs.docker.com/engine/api/v1.40/)	
		

> Container List 확인

	  [실행 컨테이너 리스트]
	  * ubuntu : docker ps

	  * GET 	  	   
	  	  IP:Port/containers/json 
		  IP:Port/containers/json?all=false

      * QUERY PARAMETERS
	      all : 모든 컨테이너(true) / 실행중인 컨테이너(false) (Default : false)
		  limit : 모든 컨테이너 중 가장 최근에 생성된 컨테이너 수 (정수표기)
		  size : 컨테이너 크기를 반환 (Default : false)
		  filters : 확인..
		  
	  ex)
	  	  http://172.27.193.70:2375/containers/json?all=true 	


> Container 실행

	  * ubuntu : docker start containerID

	  * POST 	  	  
	  	  IP:Port/containers/containerID/start

	  * QUERY PARAMETERS
	      detachKeys : 컨테이너 분리를 위한 정의 (a-Z, ctrl-value 등)
		  
	  ex)
	  	  http://172.27.193.70:2375/containers/0483eaf/start	


> Container 재실행 (실행과 무슨차이(?))

	  * ubuntu : docker restart containerID

	  * POST 	  	  
	  	  IP:Port/containers/containerID/restart		  
	   
	  * QUERY PARAMETERS
	      t : 컨테이너 종료 전 대기 시간 지정 (정수)

	  ex)
	  	  http://172.27.193.70:2375/containers/0483eaf/restart	


> Container 중지 (하고있는 작업을 마무리하고 종료)

	  * ubuntu : docker stop containerID

	  * POST 	  	  
	  	  IP:Port/containers/containerID/stop

	  * QUERY PARAMETERS
	      t : 컨테이너 종료 전 대기 시간 지정 (정수)

	  ex)
		  http://172.27.193.70:2375/containers/0483eaf9f2ce/stop


> Container Kill (작업 유무 상관없이 강제 종료)

	  * ubuntu : docker kill containerID

	  * POST 	  	  
	  	  IP:Port/containers/containerID/kill

	  * QUERY PARAMETERS
	      signal : 컨테이너에 보내는 신호 (정수/문자열) (Default : "SIGKILL")

	  ex)
		  http://172.27.193.70:2375/containers/0483eaf9f2ce/kill


> Container 삭제

	  * ubuntu : docker rm containerID

	  * DELETE 	  	  
	  	  IP:Port/containers/containerID 

	  * QUERY PARAMETERS
	      v : 컨테이너와 연결된 볼륨 제거 여부 (Default : "false")
	      force : 실행중인 컨테이너 종료 여부 (Default : "false")
	      link : 컨테이너와 연결된 지정된 링크 제거 여부 (Default : "false")

	  ex)
		  http://172.27.193.70:2375/containers/c6baf5a8


> Container 이름 변경
	 	  
	  * ubuntu : docker rename [old]containerName [new]containerName
	    [옵션]
		: -a, -all : 모든 컨테이너 출력
		: --no-stream : 실시간 스트리밍이 아닌 한번 출력

	  * POST 	  
	  	  IP:Port/containers/containerID/rename

	  * QUERY PARAMETERS
	      name <필수> : 변경할 컨테이와 이름

	  ex)
		  http://172.27.193.70:2375/containers/0483eaf9f2ce/rename?name=postgres 


> Container 일시중지 / 재가동
	 	  
	  * ubuntu 
	    [일시중지] : docker pause containerID / containerName
	    [재가동] : docker unpause containerID / containerName		

	  * POST 	  
	  	  IP:Port/containers/containerID/rename?name=변경할이름

	  ex)
		  [일시중지] : http://172.27.193.70:2375/containers/0483eaf9f2ce/pause
		  [재가동] : http://172.27.193.70:2375/containers/0483eaf9f2ce/unpause


> Container 상세

	  * ubuntu : docker inspect containerID

	  * GET 	  	  
	  	  IP:Port/containers/containerID/json
		
	  * QUERY PARAMETERS
	  	  size : 컨테이너 크기 반환 (Default : "false")

	  ex)
		  http://172.27.193.70:2375/containers/c7fc/json  

> Container 생성

	  * ubuntu : docker create --name containerName image

	  * POST 	  	  
	  	  IP:Port/containers/create 
		  IP:Port/containers/create?name=containerName
		
	  * QUERY PARAMETERS
	  	  name : 컨테이너 이름 지정

	  * BODY SCHEMA
	  		{
			    "Hostname": "",      
			    "Domainname": "",
			    "User": "",
			    "AttachStdin": false,
			    "AttachStdout": false,
			    "AttachStderr": false,
			    "Tty": true,
			    "OpenStdin": true,
			    "StdinOnce": false,  
			    "Image": "postgres",
			    "Env": [
			               "POSTGRES_PASSWORD=gk"
			           ],
			    "Cmd": [
			             "postgres"
			           ],
			    "Entrypoint": [
			              "docker-entrypoint.sh"
			            ],
			    //"Volumes": { "/volumes/data": {}  },
			    //"WorkingDir": "",
			    //"NetworkDisabled": false,
			    "MacAddress": "",
			    "ExposedPorts": {
			        "5432/tcp": {}
			    },
			    "HostConfig": {
			        "PortBindings": {
			        "5432/tcp": [
			                    {
			                        "HostIp": "",
			                        "HostPort": "5432"
			                    }
			                  ]
			        },
			        "RestartPolicy": {
			                    "Name": "no",
			                    "MaximumRetryCount": 0
			                  },
			         "ConsoleSize": [
			                        49,
			                        188
			                  ]        
			    },    
			    "NetworkSettings": {
			        "Ports": {
			                "5432/tcp": [
			                    {
			                        "HostIp": "0.0.0.0",
			                        "HostPort": "5432"
			                    },
			                    {
			                        "HostIp": "::",
			                        "HostPort": "5432"
			                    }
			                ]
			            }
			    }
			}

	  ex)
	  	  http://172.27.193.70:2375/containers/create?name=postgres_test


> Container 내부 실행중인 프로세스 리스트
	 
	  * ubuntu : docker top containerID

	  * GET 	  	  
	  	  IP:Port/containers/containerID/top

	  * QUERY PARAMETERS
	      ps_args : 전달할 인수 (Default : -ef)
		  	        -e : 로그인한 사용자 관계없이 모든 프로세스
			        -f : 전체형식으로
					-aux : 모든 사용자의 모든 프로세스를 자세히 보여줄것
					-l : Long Format으로 출력
					-o : 사용자 지정 출력 형식

	  ex)
		  http://172.27.193.70:2375/containers/postgres/top


> Container Logs
	 
	  * ubuntu : docker logs containerID

	  * GET 	  	  
	  	  IP:Port/containers/containerID/logs

	  * QUERY PARAMETERS
	      follow : Log 반환 후 연결 유지 여부 (Default : false)
		  stdout : 출력 여부 (Default : false)
		  follow : 에러 여부 (Default : false)
		  since : 해당 시간 이후 로그 (Default : 0)
		  until : 해당 시간 이전 로그  (Default : 0)
		  timestamps : 모든 로그마다 시간 정보 출력 여부 (Default : false)
		  tail : 로그 마지막부터 해당 로그 줄수까지 출력 (정수표기) (Default : all)

	  ex)
		  http://172.27.193.70:2375/containers/postgres/logs?stdout=true


> Container 파일시스템 변경사항 가져오기
	 
	  * ubuntu : docker diff containerID
	  [참고]
	  	  C : 수정
		  A : 추가
		   : 삭제

	  * GET 
	  	  IP:Port/containers/containerID/changes
	  [참고]
	  	  0 : 수정 (C)
		  1 : 추가 (A)
		  2 : 삭제

	  ex)
		  http://172.27.193.70:2375/containers/0483eaf9f2ce/changes  


> Container 내보내기
	 
	  * ubuntu : docker export [옵션] containerID

	  * GET 	  
	  	  IP:Port/containers/containerID/export

	  ex)
		  http://172.27.193.70:2375/containers/0483eaf9f2ce/export 


> Container 실시간 구동 상태
	 	  
	  * ubuntu : docker stats [옵션] containerID
	    [옵션]
		: -a, -all : 모든 컨테이너 출력
		: --no-stream : 실시간 스트리밍이 아닌 한번 출력

	  * GET 	  
	  	  IP:Port/containers/containerID/stats?stream=true/false

	  ex)
		  http://172.27.193.70:2375/containers/0483eaf9f2ce/stats?stream=false 


> Container 파일 정보 가져오기
	 	  
	  * ubuntu : 

	  * HEAD 	  
	  	  IP:Port/containers/containerID/archive

	  * QUERY PARAMETERS
	      path : 컨테이너 파일 시스템의 리소스

	  ex)
		  http://172.27.193.70:2375/containers/postgres/archive