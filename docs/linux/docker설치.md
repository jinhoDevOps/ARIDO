---
tags: linux, docker
---
# CentOS 7에 도커를 설치하는 기본 가이드

1. **필요한 패키지 설치**

    `yum-utils` 패키지를 설치합니다. 이 패키지는 `yum-config-manager` 명령어를 포함하고 있습니다.

    ```bash
    yum install -y yum-utils
    ```

2. **Docker 저장소 추가**

    Docker의 공식 CentOS 저장소를 추가합니다.

    ```bash
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    ```

3. **이전 Docker 삭제(선택적)**

    이미 시스템에 설치된 이전 버전의 Docker를 삭제합니다. (선택적)

    ```bash
    yum remove -y docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
    ```

    **주의:** 이 작업은 기존에 실행 중인 Docker 컨테이너나 이미지를 중지하고 삭제할 수 있습니다. 먼저 백업을 하세요.

4. **Docker CE 설치**

    이제 Docker CE (Community Edition), CLI, 그리고 `containerd.io`를 설치할 수 있습니다.

    ```bash
    yum install -y docker-ce docker-ce-cli containerd.io
    ```

5. **Docker 서비스 시작 & 활성화**

    Docker 서비스를 시작합니다.

    ```bash
    systemctl enable docker
    systemctl start docker
    ```


이제 Docker가 성공적으로 설치되고 실행되어야 합니다. 설치가 완료되면, 다음 명령어로 Docker가 제대로 설치되었는지 확인할 수 있습니다:

```bash
docker --version
```

또는 간단한 테스트를 실행해볼 수도 있습니다:

```bash
#이 명령은 Docker Hub에서 `hello-world` 이미지를 다운로드 받고 실행합니다.
docker run hello-world
```

