# Docker API


> Rest API 통신방법 I

``` bash

	[Rest API 도커제어방법]

	Docker client (docker) : Docker deamon 에 서비스를 요청하며, REST API, UNIX socket, network interface를 이용하여, 통신.
	Docker deamon(dockerd) : image, container, network, volume과 같은 Docker object들을 관리하고 Docker client로 부터 들어오는 요청을 수행하는 역할을 한다. 
							 또한 다른 Docker deamon과 통신 가능

	따라서, API 요청을 받기 위해 Docker deamon은 Socket 방식의 3가지 타입을 가진다. (unix, tcp, fd)

	ps aux | grep docker

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

> Rest API 통신방법 II
```bash
	1. docker.service파일의 TCP소켓 접속허용 설정

	   docker.service 파일을 수정하여 처리 [관리자 권한을 실행]
	   [명령어]
	   	   sudo systemctl stop docker.service
		   sudo vi /lib/systemd/system/docker.service

	 	수정	
		   ExecStart=/usr/bin/dockerd 
		   -H unix:///var/run/docker.sock 
		   -H fd:// --containerd=/run/containerd/containerd.sock 
		   -H tcp://0.0.0.0:2375

		sudo systemctl daemon-reload
		sudo systemctl start docker.service

```
> 그림 1
![alt text](./images/docker%20remote%20vi.png)


> docker API documentation (1.4v)

[docker API documentation](https://docs.docker.com/engine/api/v1.40/)	
		

> Container List 확인

	IP주소:2375/containers/json (=docker ps)
![alt text](./images/docker%20API%20Test%20I.png)


> Container 생성

	IP주소:2375/containers/containerID/start (=docker start containerID)
![alt text](./images/docker%20API%20Test%20II.png)

이외 REST API 자료는 별도 파일 정리.


> 원격 PC 도커 제어 방법
``` bash
    WSL2 고정 IP 할당하기.
    원격 호스트 IP와 로컬 호스트 도커 내부 IP 포트포워딩 처리.
	
	*포트 포워딩 처리
	netsh interface portproxy add v4tov4 listenport=$port listenaddress=$원격호스트IP connectport=$port connectaddress=$WSL2IP
	* 이전 정보 제거
	netsh interface portproxy delete v4tov4 listenport=$port listenaddress=$addr";

	[Powershell 아래 스크립트 입력]
	netsh interface portproxy add v4tov4 listenport='2375' listenaddress='192.168.253.136' connectport='2375' connectaddress='172.27.193.70'

	[상세방법]
	1. WSL2 머신의 IP주소 확인
	2. 이전포트 전달 규칙 제거
	3. 포트 전달 규칙 추가
	4. 이전에 추가한 방화벽 규칙 제거
	5. 새로운 방화벽 규칙 추가

	# WSL2 머신의 IP주소 출력
	$remoteport = bash.exe -c "ifconfig eth0 | grep 'inet '"
	
	# WSL2 IP 추출
	$found = $remoteport -match '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}';

	if( $found )
	{
  		$remoteport = $found[0];
	} 
	else
	{
  		echo "WSL2 iP주소 없음";
  		exit;
	}

	# 추가할 Ports	
	$ports=@(80,443,2375,3000,5000);

	# 원격 호스트 IP	
	$addr='192.168.253.136';
	$ports_a = $ports -join ",";

	# 방화벽 규칙 제거 
	iex "Remove-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' ";

	# 방화벽 규칙 추가
	iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Outbound -LocalPort $ports_a -Action Allow -Protocol TCP";
	iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Inbound -LocalPort $ports_a -Action Allow -Protocol TCP";

	for( $i = 0; $i -lt $ports.length; $i++ )
	{
  		$port = $ports[$i];
  		iex "netsh interface portproxy delete v4tov4 listenport=$port listenaddress=$addr";
  		iex "netsh interface portproxy add v4tov4 listenport=$port listenaddress=$addr connectport=$port connectaddress=$remoteport";
	}	
```

> 그림 1
![alt text](./images/PortForwording.gif)