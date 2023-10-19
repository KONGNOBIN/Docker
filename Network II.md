em # 시작하세요! 도커/쿠버네티스

### 친절한 설명으로 쉽게 이해하는 컨테이너 관리

> 네트워크 구조
``` docker
* 브리지 네트워크와 --net-alias
    --net-alias 옵션을 함께 사용하면
    특정 호스트 이름으로 컨테이너 여러 개에 접근할 수 있다.
  
    EX) 
        docker run -i -t -d \
        --name mynetwork_alias_container1 \ 
        --net mybridge \ 
        --net-alias alicek106 \
        ubuntu:14.04 

        docker run -i -t -d \
        --name mynetwork_alias_container2 \ 
        --net mybridge \ 
        --net-alias alicek106 \
        ubuntu:14.04 

        docker run -i -t -d \
        --name mynetwork_alias_container3 \ 
        --net mybridge \ 
        --net-alias alicek106 \
        ubuntu:14.04 

        3개의 컨테이너에 접근할 컨테이너 생성
        docker run -i -t -d \
        --name mynetwork_alias_ping \ 
        --net mybridge \ 
        ubuntu:14.04 

    컨테이너 내부에서 ping을 요청하면 컨테이너 3개의 IP로 ping이 전송된 것을 확인.
    요청할때마다 IP가 달라지는데 이건 라운드 로빈 방식이다. (우선순위 없이 돌아가며 할당받아 실행되는 방식)
    이것이 가능한 이유는 도커엔진에 내장된 DNS 때문.

    dig : DNS로 도메인 이름에 대응하는 IP조회할때 사용하는 도구
    * 도구 사용이 안될경우 아래 명령어 입력
    apt-get update
    apt-get install dnsutils

    컨테이너 내부에 아래처럼 입력 시 호스트 이름이 변환되는 IP 확인 가능
    [명령어]
       dig alicek106

    EX)  
        ...
        alicek106.              600     IN      A       172.18.0.2
        alicek106.              600     IN      A       172.18.0.4
        alicek106.              600     IN      A       172.18.0.3
        ...

* MacVLAN 네트워크
```
