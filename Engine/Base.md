# 시작하세요! 도커/쿠버네티스

### 친절한 설명으로 쉽게 이해하는 컨테이너 관리

> 도커 버전 확인

* docker -v
>이미지 다운
``` docker
docker pull ImageName:ImageVersion

EX)     
    docker pull ubuntu:14.04
    docker pull ubuntu       -- latest버전 생성
```

>컨테이너 생성

``` docker
docker container create ImageName:ImageVersion

EX)     
    docker container create ubuntu:14.04
    docker create ubuntu:14.04       - container 생략 가능 
    docker create ubuntu             - latest버전 생성

    * 이미지가 없는경우 pull 이후 create 실행
```

>컨테이너 시작
``` docker
docker container start containerID

EX) 
    docker container start 207e70515e01
    docker start 207e70515e01      - container 생략 가능 
    docker start 207               - Container ID 축약 가능

    * 앞자리가 동일한 컨테이너가 존재하면 에러발생
```

>컨테이너 접근
``` docker
docker container attach containerID

EX) 
    docker container attach 207e70515e01
    docker attach 207e70515e01      - container 생략 가능 
    docker attach 207               - Container ID 축약 가능

    * 앞자리가 동일한 컨테이너가 존재하면 에러발생
    * 컨테이너 내부에서 도커환경으로 돌아오는 방법
      1. 셸에 'exit' 입력   (컨테이너 종료 O)
      2. Ctrl + D 입력      (컨테이너 종료 O)
      3. Ctrl + P, Q 입력   (컨테이너 종료 X)
```


><mark>[중요]</mark> 컨테이너 생성 및 시작
``` docker
docker container run ImageName:ImageVersion

EX) 
    docker container run ubuntu:14.04
    docker run ubuntu:14.04      - container 생략 가능 
    docker run ubuntu            - latest버전 생성
    
    * run : pull(이미지가 없는경우) > create > start > attach(경우에따라) 
```

>이미지 목록확인
``` docker
docker images
```

>컨테이너 목록확인
``` docker
docker ps : 정지되지 않은 컨테이너 출력
docker ps -a : 모든 컨테이너 출력
```
>컨테이너 상세정보
``` docker
docker inspect containerID or containerName

EX) 
    docker inspect 207e70515e01   (containerID)
    docker inspect eager_borg     (containerName)
    
    docker inspect eager_borg | grep Id   (containerName의 'Id'단어가 포함되는 내용 출력)
        결과값 : "Id": "207e70515e016c36f35c80714bfefba016c9ea3d6872039a298ecc09a40fd172",
```
> 컨테이너 삭제
``` docker
docker rm containerID or containerName  (정지된 container 삭제)
docker rm -f containerID or containerName  (실행중인 container 삭제)
docker container prune (모든 컨테이너 삭제)

EX) 
    docker rm 207e70515e01      (containerID)
    docker rm eager_borg        (containerName)
    docker rm -f eager_borg     (containerName)	
```