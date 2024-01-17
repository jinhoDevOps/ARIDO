---
tags: linux
---
lsync test # 8월4주


https://blog.jiniworld.me/112  
https://heuristing.net/posts/31/%EB%8C%80%EC%9A%A9%EB%9F%89-%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80-%EC%84%9C%EB%B2%84-%EC%9D%B4%EC%A0%84-%EC%9E%91%EC%97%85

https://docs.rockylinux.org/ko/guides/backup/rsync_ssh/
https://yeajs.tistory.com/31

리눅스 커널의 inotify로 파일시스템의 변경사항을 체크
inotify(Linux Kernel 2.6.13 이상)는 리눅스 커널에 포함된 기능으로, 파일시스템에 변경사항이 발생할 때 이벤트를 통보


### rsync 는 현재 폴더의 모든 파일 리스트를 서버로 전송한다. 
추가/수정된 파일이 존재하면 수정된 파일을 전송한다.
  - 현재 폴더에 1.txt, 2.txt 가 존재하면 서버에 1.txt, 2.txt 의 파일 리스트를 전송한 후, 이에 대한 추가/수정 여부가 확인되면 추가/수정된 파일을 전송한다.
  - 현재 폴더의 모든 파일 리스트를 서버로 전송하기 때문에 폴더에 파일이 많이 존재하는 경우 동기화 시간이 많이 소요될 수 있다.

### lsyncd 는 추가/수정된 파일이 존재하면 추가/수정된 파일만 전송한다.
  - 현재 폴더에 1.txt 가 추가/수정되면 서버에 1.txt 만 파일 리스트로 전송한 후, 이에 대한 추가/수정 여부가 확인되면 추가/수정된 파일을 전송한다.
  - 추가/수정된 파일만 동기화하기 때문에 rsync 로 모든 파일 리스트를 전송하는 것보다는 효과적인 것 같다.


https://ko.linux-console.net/?p=2784#gsc.tab=0
ubuntu

시나리오
---
1. rsync 설치 확인 명령 
```bash
yum list installed | grep rsync
systemctl start rsyncd
```

2. lsyncd 설치
```bash
yum list installed | grep lsyncd
yum install epel-release -y
yum install lsyncd -y
```

3. rsync 명령

```bash
rsync -azvh src/ dst/ #src폴더에 있는 내용을 dst폴더에 동기화
rsync -e 'ssh -p 7002' -avz 5_test/ root@192.168.123.4:/data 
```


4. lsyncd 환경설정 

```bash
vi /etc/lsyncd.conf

---

-- User configuration file for lsyncd.
--
-- Simple example for default rsync, but executing moves through on the target.
--
-- For more examples, see /usr/share/doc/lsyncd*/examples/
-- 
-- sync{default.rsyncssh, source="/var/www/html", host="localhost", targetdir="/tmp/htmlcopy/"}

settings {
     logfile = "/var/log/lsyncd/lsyncd.log",
     statusFile = "/var/log/lsyncd/lsyncd-status.log"
}

sync {
    default.rsyncssh,
    source="/data", -- 소스 서버 디렉토리
    host="root@192.168.123.163", -- 타겟 서버 아이피 or 도메인
    targetdir="/data", -- 타겟 서버 디렉토리
    delete=true, -- 삭제시 타겟 서버에 반영하기(동기화 하기)
    delay=1, -- 1초
    rsync={
        archive=true, -- 사용자와 퍼미션 동기화 하기
        compress=false,
        verbose=false
    },
    ssh = {
        port = 7002
    }
}

* 참고 
디폴트 값 : 8126
echo 65536 > /proc/sys/fs/inotify/max_user_watches
```

### 5.1 원본 서버에서 SSH 키 생성

원본 서버에서 새로운 SSH 키를 생성하려면 다음 명령어를 실행합니다:

```bash
ssh-keygen -t rsa
```

- 명령어 실행 후, 키를 저장할 경로와 비밀구를 입력할 수 있습니다.
- 기본 경로는 사용자의 홈 디렉토리 내 `.ssh/id_rsa` 입니다.
- 비밀구를 설정하지 않으려면 Enter 키를 누르세요.

### 5.2 대상 서버에 SSH 키 복사

원본 서버에서 생성한 공개 키를 대상 서버의 `authorized_keys` 파일에 추가하려면 `ssh-copy-id` 명령어를 사용할 수 있습니다. 다음 명령어를 원본 서버에서 실행하세요:

```bash
ssh-copy-id username@target_server_ip
```

- 여기서 `username`은 대상 서버의 사용자 이름이고, `target_server_ip`는 대상 서버의 IP 주소입니다.
- 처음 실행할 때, 대상 서버의 비밀번호를 입력해야 합니다.

이제 원본 서버에서 대상 서버로 SSH 키를 사용하여 비밀번호 없이 연결할 수 있습니다. 이 설정은 `rsync`와 같은 명령어에서 원격 서버와의 연결을 더 편리하게 만듭니다.

### 주의 사항

- SSH 키는 중요한 인증 정보이므로 안전하게 관리해야 합니다.
- 공개 키를 잘못된 서버나 사용자에게 전송하지 않도록 주의하세요.
- 필요한 경우, SSH 키에 비밀구를 설정하여 추가 보안을 제공할 수 있습니다.

6. lsync 실행
systemctl enable lsyncd
systemctl start lsyncd

### 7.1 rsync로 복제

#### 7.1.1 원본에 있는 폴더 전송
원본 디렉토리에서 대상 디렉토리로 파일 및 폴더를 전송합니다.

```bash
rsync -azvh src/ dst/
```

#### 7.1.2 원본에 파일 만들어서 재전송
원본 디렉토리에 새 파일을 만든 후, 다시 전송합니다.

```bash
echo "새 파일 내용" > src/newfile.txt
rsync -azvh src/ dst/
```

#### 7.1.3 원본에 있는 파일에 내용을 적어서 재전송
원본 디렉토리의 기존 파일에 내용을 추가하거나 수정한 후, 다시 전송합니다.

```bash
echo "추가 내용" >> src/existingfile.txt
rsync -azvh src/ dst/
```

#### 7.1.4 대상에 신규 파일 만든 상태에서 재전송
대상 디렉토리에 신규 파일을 만든 후 원본과 동기화합니다. `--delete` 옵션을 추가하면 원본에 없는 대상 디렉토리의 파일을 삭제합니다.

```bash
rsync -azvh --delete src/ dst/
```

#### 7.1.5 대상에 신규로 만든 파일과 같은 이름으로 파일 만든 후 내용 적어서 재전송
대상 디렉토리에 있는 파일과 동일한 이름으로 원본 디렉토리에 파일을 만들고 내용을 적은 후 전송합니다.

```bash
echo "원본 파일 내용" > src/samefile.txt
rsync -azvh src/ dst/
```

### 원격 서버로 전송
원격 서버로 전송하려면 SSH를 통해 연결합니다. 포트 번호와 사용자 이름, IP 주소를 지정합니다.

```bash
rsync -e 'ssh -p 7002' -avz src/ root@192.168.123.x:/data
```

### 주의 사항
- 원격 서버로 전송할 때는 대상 서버에 SSH 접근 권한이 있어야 하며, 필요한 경우 키 인증을 설정해야 할 수 있습니다.
- `rsync` 명령을 사용할 때 파일 및 디렉토리 경로를 정확하게 지정해야 합니다. 
- `--delete` 옵션은 주의하여 사용해야 합니다. 원본에 없는 대상 디렉토리의 파일을 삭제하므로, 실수로 중요한 파일을 삭제할 수도 있습니다.

### 7.1.6 원본에서 파일 및 소유자 변경 후 전송
원본 서버에서 파일의 소유자를 변경하고 그룹 및 사용자 계정을 생성한 후 전송하는 과정입니다.

#### 1. test 그룹 및 test 사용자 계정 생성
```bash
sudo groupadd test
sudo useradd -g test test
```

#### 2. 원본 디렉토리에서 파일 소유자 변경
```bash
sudo chown -R test:test src/
```

#### 3. rsync로 전송
```bash
rsync -azvh src/ dst/
```

### 7.1.7 sudo 계정으로 전송

#### 1. 원본 서버에서 root로 실행
```bash
sudo rsync -e 'ssh -p 7002' -avz src/ root@192.168.123.x:/data
```

#### 2. 대상 서버에서 폴더 계정 권한 변경
```bash
sudo chown -R desired_user:desired_group /data
```

원본 서버에서 root로 실행하면 대상 서버의 `/data` 디렉토리에 모든 파일과 디렉토리가 root 소유로 전송됩니다. 대상 서버에서 원하는 사용자와 그룹의 소유로 변경하려면 위의 `chown` 명령을 사용합니다.

### 주의 사항
- `rsync`와 관련된 명령은 파일과 디렉토리의 소유권 및 권한을 변경할 수 있으므로 주의가 필요합니다.
- 원격 서버와의 연결에는 SSH 키 인증이나 적절한 권한 설정이 필요할 수 있습니다.
- 파일과 디렉토리의 소유자나 권한을 변경할 때는 신중하게 수행해야 하며, 중요한 시스템 파일이나 설정에 영향을 주지 않도록 주의해야 합니다.