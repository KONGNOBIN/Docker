# Docker API Doc

> docker API documentation (1.4v)

[docker API documentation](https://docs.docker.com/engine/api/v1.40/)	
		

> Container List 확인
	
	  * GET 
	  	  [실행 컨테이너 리스트] (= docker ps)
	  	  IP:Port/containers/json 
		  IP:Port/containers/json?all=false

		  [전체 컨테이너 리스트] (= docker ps -a)
		  IP:Port/containers/json?all=true

	  ex)
	  	  http://172.27.193.70:2375/containers/json?all=true 	

> Container 실행
	
	  * POST 
	  	  [컨테이너 실행] (= docker start containerID)
	  	  IP:Port/containers/containerID/start

	  ex)
	  	  http://172.27.193.70:2375/containers/0483eaf/start	


> Container 실행
	
	  * POST 
	  	  [컨테이너 실행] (= docker start containerID)
	  	  IP:Port/containers/containerID/start

	  ex)
	  	  http://172.27.193.70:2375/containers/0483eaf/start


> Container 삭제
	 
	  * DELETE 
	  	  [컨테이너 삭제] (= docker rm containerID)
	  	  IP:Port/containers/containerID 

	  ex)
		  http://172.27.193.70:2375/containers/c6baf5a8


> Container 상세
	 
	  * GET 
	  	  [컨테이너 상세] (= docker inspect containerID)
	  	  IP:Port/containers/containerID/json

	  ex)
		  http://172.27.193.70:2375/containers/c7fc/json  

> Container 생성
	 
	  * POST 
	  	  [실행 컨테이너 리스트] (= docker create --name containerName image)
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