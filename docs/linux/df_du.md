## 1. 디스크 별 용량 확인
### `df` 명령어
- `df -h` 디스크 별 용량 확인
- `df -Th` 디스크 별 용량, 확장자 확인

## 2. 디렉토리 및 파일 용량 확인
### `du` 명령어
- `du -hs <folder>` 특정 디렉토리 용량 확인 (사람이 읽을 수 있는 형태로 출력) 
- `du -hs *` 현재 폴더에 있는 폴더 및 파일 용량 출력
-  `du -h --max-depth=1 | sort -hr` 현재 폴더에서 파일 용량이 큰 순서대로 출력
- `sudo du -hsx * | sort -rh | head -n 10` 현재 디렉토리에서 상위 10개 폴더의 용량 보기
> `du -hsx <folder/*> | sort -rh | head -n 10`  특정 디렉토리에서 상위 10개 폴더의 용량 보기(`<folder/*> 예시:`  /root/jinhok/org_kjh_centos_simple/*)