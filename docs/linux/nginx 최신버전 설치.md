---
tags: linux, nginx, 신규인프라
---
[Nginx 공식 설치가이드](https://nginx.org/en/linux_packages.html#instructions)
[Nginx버전확인](https://nginx.org/en/download.html)
## 설치 전 준비
새로운 머신에서 Nginx를 처음 설치하기 전에 Nginx 패키지 저장소를 설정해야 합니다. 저장소를 설정한 후에는 이를 통해 Nginx를 설치하고 업데이트할 수 있습니다.

RHEL 7과 SLES 12에서는 OpenSSL이 TLSv1.3을 지원하지 않기 때문에 HTTP/3 지원 없이 Nginx 패키지가 생성됩니다.

---
# Ubuntu

### 필수 패키지 설치

```bash
sudo apt install curl gnupg2 ca-certificates lsb-release ubuntu-keyring
```

### Nginx 서명 키 가져오기

```bash
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor \
    | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
```

### 키 검증

```bash
gpg --dry-run --quiet --no-keyring --import --import-options import-show /usr/share/keyrings/nginx-archive-keyring.gpg
```

### Apt 저장소 설정
`메인라인`과 `안정 버전` 중 한 가지만 선택해 설치해야 한다.
- 안정 버전
    ```bash
    echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
    http://nginx.org/packages/ubuntu `lsb_release -cs` nginx" \
        | sudo tee /etc/apt/sources.list.d/nginx.list
    ```

- 메인라인 버전
    ```bash
    echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
    http://nginx.org/packages/mainline/ubuntu `lsb_release -cs` nginx" \
        | sudo tee /etc/apt/sources.list.d/nginx.list
    ```

### 저장소 핀 설정

```bash
echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" \
    | sudo tee /etc/apt/preferences.d/99nginx
```

### Nginx 설치

```bash
sudo apt update
sudo apt install nginx
```

이로써 Debian과 Ubuntu에서 Nginx의 설치가 완료됩니다. 필요에 따라 추가적인 설정을 할 수 있습니다.

---


## RHEL 및 파생 버전
이 섹션은 Red Hat Enterprise Linux 및 그 파생 버전(CentOS, Oracle Linux, Rocky Linux, AlmaLinux 등)에 적용됩니다.

### 필수 패키지 설치
```bash
sudo yum install yum-utils -y
```

### Yum 저장소 설정
`/etc/yum.repos.d/nginx.repo` 파일을 생성하고 다음과 같이 내용을 작성합니다:

```bash
cat << EOF > /etc/yum.repos.d/nginx.repo

[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/\$releasever/\$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/\$releasever/\$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
EOF
```

기본적으로 안정적인 Nginx 패키지의 저장소가 사용됩니다. 만약 stable 버전을 사용하고 싶다면, 다음 명령어를 실행하세요:

```bash
sudo yum-config-manager --enable nginx-stable
```

### Nginx 설치
다음 명령어를 실행하여 Nginx를 설치합니다:

```bash
sudo yum install nginx -y
```

GPG 키를 수락하라는 메시지가 표시되면, 지문이 `573B FD6B 3D8F BC64 1079 A6AB ABF5 BD82 7BD9 BF62`와 일치하는지 확인한 후 수락합니다.

---
## nginx: Linux packages
[Supported distributions and versions](https://nginx.org/en/linux_packages.html#distributions)  <br>[Installation instructions](https://nginx.org/en/linux_packages.html#instructions)  <br>     [RHEL and derivatives](https://nginx.org/en/linux_packages.html#RHEL)  <br>     [Debian](https://nginx.org/en/linux_packages.html#Debian)  <br>     [Ubuntu](https://nginx.org/en/linux_packages.html#Ubuntu)  <br>     [SLES](https://nginx.org/en/linux_packages.html#SLES)  <br>     [Alpine](https://nginx.org/en/linux_packages.html#Alpine)  <br>     [Amazon Linux](https://nginx.org/en/linux_packages.html#Amazon-Linux)  <br>[Source Packages](https://nginx.org/en/linux_packages.html#sourcepackages)  <br>[Dynamic Modules](https://nginx.org/en/linux_packages.html#dynmodules)  <br>[Signatures](https://nginx.org/en/linux_packages.html#signatures)|