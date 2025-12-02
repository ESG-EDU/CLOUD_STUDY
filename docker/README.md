# Docker 기초 교육

## 기본 명령어

| 명령어(CLI) | 설명 |
| :---: | :---: |
| `docker -v` | 도커 버젼 확인 |
| `docker search [KEYWORD]` | 이미지 검색 (공식이미지 : ***OFFICIAL***)  |
| `docker pull [이미지:태그]` | 이미지 받기 (태그 생략시 : ***latest***) |
| `docker images` | 이미지 목록 |
| `docker create [이미지:태그]` | 컨테이너 생성 |
| `docker start [컨테이너]` | 컨테이너 시작 |
| `docker stop [컨테이너]` | 컨테이너 정지 |
| `docker run [이미지:태그]` | 컨테이너 실행 |
| `docker rm [컨테이너]` | 컨테이너 삭제 (옵션: ***-f***) |
| `docker ps` | 프로세스 확인 (옵션: ***-a***) |
| `docker build -t [이미지:태그] [path]` | 이미지 생성 (***dockerfile*** 필수) |
| `docker rmi [이미지:태그]` | 이미지 삭제 (이미지 사용 시 삭제 불가) |

## 네트워크 명령어

| 명령어(CLI) | 설명 |
| :---: | :---: |
| `docker network ls` | 도커 네트워크 목록 |
| `docker network create [네트워크]` | 도커 네트워크 생성 (기본값: ***bridge***) |
| `docker network inspect [네트워크ID]` | 도커 네트워크 정보 |
| `docker network rm [네트워크ID]` | 도커 네트워크 삭제 |

### 볼륨 명령어

| 명령어(CLI) | 설명 |
| :---: | :---: |
| `docker volume ls` | 도커 전용 스토리지 목록 |
| `docker volume create [볼륨]` | 도커 전용 스토리지 생성 |
| `docker volume inspect [볼륨]` | 도커 전용 스토리지 정보 |
| `docker volume rm [볼륨]` | 도커 전용 스토리지 삭제 |

### 이미지 배포 명령어

| 명령어(CLI) | 설명 |
| :---: | :---: |
| `docker login` | docker hub 로그인 |
| `docker tag [이미지:태그] [계정명/이미지:태그]` | docker hub 저장소 네이밍 규칙 적용 |
| `docker push [계정명/이미지:태그]` | docker hub 저장소 올리기 |

### 실습

1. 도커 `이미지` 확인
```bash
docker image list
docker image ls
docker images
```

2. 도커 컨테이너 `생성` 하기
```bash
docker create -i --name ubt ubuntu:24.04
```

3. 도커 컨테이너 `시작` 하기
```bash
docker start ubt
```

4. 도커 컨테이너 `정지` 하기
```bash
docker stop ubt
```

5. 도커 컨테이너 `삭제` 하기
```bash
docker rm ubt
```

6. 도커 컨테이너 `생성 및 실행` 하기
```
docker run -d -it --name ubt ubuntu:24.04
```
---

### 이미지 생성

1. `dockerfile` 만들기
```
FROM ubuntu:24.04

RUN apt-get update
RUN apt-get install -y apache2

EXPOSE 80

CMD ["apache2ctl", "-D", "FOREGROUND"]
```

2. 도커 이미지 생성 (`build`) 하기
```
docker build -t web:1 .
```

3. 생성한 이미지 실행 하기
```
docker run -d -p 80:80 --name web web:1
```

> Dockerfile 설명
```
FROM        : 기본 대상 이미지를 정의 하는 속성

MAINTAINER  : 작성자의 정보를 기록하는 속성

RUN         : FROM의 기반 이미지 위에서 실행될 명령어 정의

COPY        : 도커 컨테이너의 경로로 파일을 복사 할때 사용하는 속성
COPY 로컬:컨테이너 
COPY c:\IDE\works\vscode\index.html:/var/www/html/index.html

ENV         : 도커 컨테이너의 환경변수를 정의 하는 속성

EXPOSE      : 연결할 포트 번호 정의

ENTRYPOINT  : 도커 컨테이너 생성 후 실행될 명령어 (1회 실행)

CMD         : 도커 컨테이너 시작 이후 실행될 명령어
```

### `docker run` 옵션
```
-i          : 컨터이너와 상호 입출력 활성화 정의
-t          : tty 활성화. 주로 -i 옵션과 함께 이용
-it         : -i와 -t를 한번에 정의하는 옵션

-p          : 포트포워딩 옵션   (ex 로컬포트:컨테이너포트)

-e          : 환경변수를 지정하거나 값을 변경 하는 옵션

-v          : 저장소 연경 또는 공유 하는 옵션
> 도커의 저장소 (도커 내부의 `Volumes` 영역 공간)
> 로컬의 저장소 (컴퓨터의 HDD 또는 SSD)
```

- 도커 컨테이너 `실행` 할때 ***network*** 연결해주기
```
docker network create myNet
docker run -d -p 80:80 --network=myNet --name web nginx:latest
```

---

## `docker compose` 알아보기

> 예시: 데이터베이스 컨테이너

1. `docker run`
```
docker run -d -p 3306:3306 -e MARIADB_ROOT_PASSWORD=1234 mariadb:11.5.2
```

2. `docker compose`

- 실행 명령어 (`compose.yml` 파일 존재 필요)
```
docker compose up -d
```

- `compose.yml` 내부 내용
```yml
services:
  db:
    container_name: db
    image: mariadb:latest
    restart: always
    ports: 
      - 23306:3306
    environment:
      - MARIADB_ROOT_PASSWORD=4321
    networks:
      myNet:
        ipv4_address: 192.168.100.10

networks:
  myNet:
    driver: bridge
    ipam:
      config:
        - subnet: 192.168.100.0/24
          gateway: 192.168.100.254
```
