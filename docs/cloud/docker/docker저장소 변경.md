
# Docker 홈 디렉토리 변경 가이드라인

## 목차
1. [서론](#서론)
2. [데이터 이동](#데이터-이동)
8. [재부팅 후 문제 해결](#재부팅-후-문제-해결)

---

## 서론
이 가이드라인은 Docker의 데이터 디렉토리를 `/home/docker_data`로 변경하는 방법에 대해 설명합니다. 우분투와 CentOS에서 동작합니다.

## Docker 서비스 중지

```bash
systemctl stop docker
```

## 데이터 이동
기존 Docker 데이터를 `/home/docker_data`로 이동합니다.

```bash
rsync -aP /var/lib/docker/ /home/docker_data
```

## 설정 파일 수정
Docker의 설정 파일 `/etc/docker/daemon.json`을 수정합니다. 파일이 없다면 새로 생성합니다.

```bash
{
  "data-root": "/home/docker_data"
}
```
설정 변경 후 시스템 데몬을 리로드합니다.

```bash
systemctl daemon-reload
```


## Docker 서비스 재시작

```bash
systemctl start docker
```
설정이 제대로 적용되었는지 확인합니다.
```bash
docker info | grep "Docker Root Dir"
```

---

## 재부팅 후 문제 해결
재부팅 이후 위 방법이 안될 경우 다음과 같이 해결할 수 있습니다.
블로그 내용 첨부임 이 다음 내용은 테스트 X

### 1. 데이터 복제
```bash
cp -r /var/lib/docker /home/docker_data
```
이후 정상 작동하면 `/var/lib/docker`은 삭제 후 `vi /usr/lib/systemd/system/docker.service`로 다시 변경합니다.

### 2. daemon.json 생성 또는 추가
```bash
vi /etc/docker/daemon.json
```
내용:
```json
{
  "graph": "/home/docker_data"
}
```

### 3. 도커 재시작 및 적용 확인
```bash
service docker stop
systemctl daemon-reload
service docker start
```

---
> https://info-lab.tistory.com/313