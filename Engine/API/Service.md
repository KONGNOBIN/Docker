# Docker API Doc

> docker API documentation (1.4v)

[docker API documentation](https://docs.docker.com/engine/api/v1.40/)	
		
> Service 정보 가져오기
	 	  
	  * ubuntu : docker service ls

	  * HEAD 	  
	  	  IP:Port/services

	  * QUERY PARAMETERS
	      filters : id=<service id>
					label=<service label>	
					mode=["replicated"|"global"]
					name=<service name>

	  ex)
		  http://172.27.193.70:2375/services