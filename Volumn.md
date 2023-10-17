em # 시작하세요! 도커/쿠버네티스

### 친절한 설명으로 쉽게 이해하는 컨테이너 관리

> 호스트 볼륨 공유
``` docker
-v [호스트의 공유 디렉터리]:[컨테이너의 공유 디렉터리]

EX) 
    docker run -d \
	> -e MYSQL_ROOT_PASSWORD=gk \
	> -e MYSQL_DATABASE=wordpress \
	> -v /home/wordpress_db:/var/lib/mysql \      --mysql의 기본 디렉터리 : /var/lib/mysql
	> mysql:5.7

	host에 /home/wordpress_db 디렉터리를 생성하지 않아도 자동으로 생성해준다.
	데이터베이스에 관련파일이 있는지 확인
	ls /home/wordpress_db
	
	* 호스트와 컨테이너에 디렉터리가 존재하는 경우
	docker run -i -t --name volumn_dummy alicek106/volume_test
	/# ls /home/testdir_2/
	확인하면 test라는 파일 존재
	
	docker run -i -t --name volume -v /home/wordpress_db/:/home/testdir_2 alicek106/volume_test	
	/# ls /home/testdir_2/	
	확인하면 호스트의 디렉터리룰 컨테이너 디렉터리에 마운트됨.
	즉 호스트의 디렉터리로 덮어씌어진다.	
	
```

> 볼륨 컨테이너

``` docker
-v옵션으로 볼륨을 사용하는 컨테이너를 다른 컨테이너와 공유하는 것
--volume-from 옵션

EX)     
    docker run -i -t \
	> --name volumes_from \
	> --volumes-from volume \
	> ubuntu:14.04    
	
	* 여러 컨테이너가 동일한 컨테이너에 --volume-from 옵션을 사용함으로써 볼륨을 공유해 사용할 수도 있다.
	이러한 구조를 활용하여 '볼륨 컨테이너'를 활용하는 방안도 가능하다.
	
```

> 도커 볼륨
``` docker
도커 자체에서 제공하는 볼륨기능 활용
[볼륨생성]
docker volume

[볼륨황용]
-v volume이름:경로


EX) 
    docker volume create --name myvolume
	
	* 생성된 불륨 확인
	docker volume ls
		
	* /root 디렉터리에 volume 파일 생성
	docker run -i -t --name myvolume1 -v myvolume:/root/ ubuntu:14.04
	/# echo hello, volume! >> /root/volume	
	
	* 동일한 볼륨 사용시 위 해당 컨테이너 볼륨에도 파일 존재
	docker run -i -t --name myvolume2 -v myvolume:/root/ ubuntu:14.04
	/# cat /root/volume
	
	볼륨은 디렉터리 하나에 상응하는 단위로써 도커 엔진에서 관리한다.
	
	* 볼륨 저장소 확인 (inspect : 컨테이너, 이미지, 볼륨 등 도커의 모든 구성단위 정보 확인 가능)
	docker volume inspect myvolume
	docker inspect --type volume myvolume
	
	{
        "CreatedAt": "2023-10-12T22:50:53+09:00",
        "Driver": "local",  -- 볼륨 사용 드라이버
        "Labels": null,     -- 볼륨 구분 라벨
        "Mountpoint": "/var/lib/docker/volumes/myvolume/_data",   -- 호스트 저장 경로 (알 필요 없음)
        "Name": "myvolume",
        "Options": null,
        "Scope": "local"
    }
	
	*mount 옵션 사용방법
	--mount type:volume,source=myvolume,target=/root
	
	*호스트의 디렉터리 컨테이너에 마운트 하는 방법
	--mount type=bind,source=/home/wordpress_db,target=/home/testdir
    
```
