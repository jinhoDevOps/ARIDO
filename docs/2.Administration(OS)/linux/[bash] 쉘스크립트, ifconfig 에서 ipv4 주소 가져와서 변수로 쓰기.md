
`ifconfig`를 사용하여 `eth0` 인터페이스의 IPv4 주소를 자동으로 가져와서 이 값을 사용하는 방법.
예를 들어, 아래와 같은 스크립트를 작성할 수 있습니다:

```bash
#!/bin/bash

# eth0의 IPv4 주소를 추출
IP_ADDRESS=$(ifconfig eth0 | grep 'inet ' | awk '{print $2}')
echo $IP_ADDRESS
# /etc/default/minio에 설정 추가
cat <<EOT >> /etc/default/minio
MINIO_VOLUMES="/data/minio/"
MINIO_OPTS="--address $IP_ADDRESS:9000 --console-address $IP_ADDRESS:36419"
MINIO_ACCESS_KEY=minioadmin
MINIO_SECRET_KEY=minioadmin
EOT
```

이 스크립트는 `eth0`의 IP 주소를 찾아 `MINIO_OPTS`에 사용합니다. `grep`과 `awk`를 사용하여 `inet` 라인에서 IP 주소만 추출합니다. 이렇게 해서, 스크립트 실행 시 `eth0`의 현재 IP 주소로 자동으로 설정됩니다.