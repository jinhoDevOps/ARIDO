---
tags:
  - ncp
  - nas
---
??how to add NAS
https://guide.ncloud-docs.com/docs/nas-use-linux-vpc
1. 볼륨을 만든다
2. 서버에 필요한 패키지들을 받는다
```
yum install nfs-utils -y  //얘도os별로 다름
systemctl start rpcbind 
systemctl enable rpcbind
systemctl status rpcbind  //상태확인
systemctl restart rpcbind // stop 후에 재시작?
```

3. 마운트 
```
mkdir /mnt/nas
mount -t nfs {마운트정보} /mnt/nas
mount -t nfs 169.254.84.49:/n2955718_insighttest /mnt/nas
```
	nfs: network file sys
	@@@: 마운트 정보에있는 경로
	`df -h`  //마운트 확인
169.254.84.49:/n2955718_insighttest
1. fstab 설정
`vi /etc/fstab`
`@@@ /mntnas nfs defaults 0 0`

`169.254.84.49:/n2955718_insighttest /mnt/nas nfs defaults 0 0`
