# Linux 기초 교육

- [리눅스 알아보기](https://github.com/ESG-EDU/Docs/tree/main/linux)

## Ubuntu 운영체제

### APT (Advanced Packaging Tool)

| 명령어 | 설명 |
| :---: | :---: |
| `apt-get update` | 패키지 인덱스 정보를 업데이트 |
| `apt-get upgrade` | 설치된 패키지 업그래이드 |
| `apt-get dist-upgrade` | 패키지 설치 |
| `apt-get install 패키지이름` | 패키지 설치 : 의존성검사하며 설치하기 |
| `apt-get --reinstall install 패키지이름` | 패키지 재설치 |
| `apt-get remove 패키지이름` | 패키지 삭제 : 설정파일은 지우지 않음 |
| `apt-get --purge remove 패키지이름` | 패키지 삭제 : 설정파일까지 모두 지움 |
| `apt-get source 패키지이름` | 패키지 소스코드 다운로드 |
| `apt-get build-dep 패키지이름` | 패키지 소스코드 다운로드 : 의존성있게 빌드 |
| `apt-cache  search 패키지이름` | 패키지 검색 |
| `apt-cache show 패키지이름` | 패키지 정보 보기 |

- 접속 사용자 확인

```bash
whoami
```

- 비밀번호 생성 및 변경

```bash
passwd
```

- 사용자 생성

```bash
useradd -m -s /bin/bash -c "홍길동" hong
```

> `hong` 사용자 비밀번호 설정 (root 권한 필요)

```bash
passwd hong
```

- 로그인(사용자 이동)

```bash
su hong
```

- 로그아웃(사용자 종료)

```bash
exit
```

