# Docker API Doc

> docker API documentation (1.4v)

[docker API documentation](https://docs.docker.com/engine/api/v1.40/)	
		

> Container List 확인

	  [실행 컨테이너 리스트]
	  * ubuntu : docker ps
	  * GET 	  	   
	  	  IP:Port/containers/json 
		  IP:Port/containers/json?all=false

	  [전체 컨테이너 리스트]
	  * ubuntu : docker ps -a		   
	  * GET 
		  IP:Port/containers/json?all=true

	  ex)
	  	  http://172.27.193.70:2375/containers/json?all=true 	


> Container 실행

	  * ubuntu : docker start containerID
	  * POST 	  	  
	  	  IP:Port/containers/containerID/start

	  ex)
	  	  http://172.27.193.70:2375/containers/0483eaf/start	


> Container 재실행 (실행과 무슨차이(?))

	  * ubuntu : docker restart containerID
	  * POST 	  	  
	  	  IP:Port/containers/containerID/start
		  IP:Port/containers/containerID/restart?t=5
		  <t : 컨테이너 종료 대기 시간>

	  ex)
	  	  http://172.27.193.70:2375/containers/0483eaf/start	


> Container 중지 (하고있는 작업을 마무리하고 종료)

	  * ubuntu : docker stop containerID
	  * POST 	  	  
	  	  IP:Port/containers/containerID/stop

	  ex)
		  http://172.27.193.70:2375/containers/0483eaf9f2ce/stop


> Container Kill (작업 유무 상관없이 강제 종료)

	  * ubuntu : docker kill containerID
	  * POST 	  	  
	  	  IP:Port/containers/containerID/kill

	  ex)
		  http://172.27.193.70:2375/containers/0483eaf9f2ce/kill


> Container 삭제

	  * ubuntu : docker rm containerID
	  * DELETE 	  	  
	  	  IP:Port/containers/containerID 

	  ex)
		  http://172.27.193.70:2375/containers/c6baf5a8





> Container 상세

	  * ubuntu : docker inspect containerID
	  * GET 	  	  
	  	  IP:Port/containers/containerID/json

	  ex)
		  http://172.27.193.70:2375/containers/c7fc/json  

> Container 생성

	  * ubuntu : docker create --name containerName image
	  * POST 	  	  
	  	  IP:Port/containers/create 
		  IP:Port/containers/create?name=containerName
	  [body]
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

> Container Logs
	 
	  * ubuntu : docker logs containerID
	  * GET 	  	  
	  	  IP:Port/containers/containerID/logs

	  ex)
		  http://172.27.193.70:2375/containers/0483eaf9f2ce/logs


> Container 파일시스템 변경사항 가져오기
	 
	  * GET 
	  	  IP:Port/containers/containerID/changes

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