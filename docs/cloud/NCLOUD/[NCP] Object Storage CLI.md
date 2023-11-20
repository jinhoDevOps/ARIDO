#ncp #object_storage #s3
Amazon S3 CLI와 호환되는 Object Storage CLI의 초기 설정 방법부터 간단한 사용 방법 등을 확인합니다.
https://cli.ncloud-docs.com/docs/guide-objectstorage

### 파이썬 버전이 여러개 일경우

만약 `C:\Users\LENOVO\AppData\Local\Programs\Python\Python36-32` 디렉토리에 Python 3.6이 설치되어 있고, 이 경로에 `python.exe`가 존재한다면, 다음과 같이 가상 환경을 생성할 수 있습니다:

0. ( ~~_가상 환경 제거: 기존에 잘못 생성된 가상 환경을 제거합니다._~~ )

   ```shell
   Deactivate # 현재 가상 환경 비활성화
   Remove-Item D:\jhkSandBox\mytest -Recurse # 가상 환경 폴더 제거
   ```

1. **새 가상 환경 생성**: Python 3.6을 사용하여 새 가상 환경을 생성합니다.

   ```shell
   C:\Users\LENOVO\AppData\Local\Programs\Python\Python36-32\python.exe -m venv D:\jhkSandBox\mytest
   ```

2. **가상 환경 활성화**: 가상 환경을 활성화합니다.

   ```shell
   D:\jhkSandBox\mytest\Scripts\Activate.ps1
   ```

3. **Python 버전 확인**: 활성화된 가상 환경의 Python 버전을 확인합니다.
   ```python
   python --version
   # 그리고 작업을 이어나갑니다.
   pip uninstall botocore s3transfer
   pip install awscli==1.15.85 #botocore s3transfer가 다시 설치됨
   ```

```powershell
#가상환경의 경우 이런식으로 cli 사용해야 확실
(mytest) PS D:\jhkSandBox> .\obj-aws-sync.ps1
```

---

# 0. 초기설정

```python
pip install awscli==1.15.85
```

```sh
~$ aws configure
AWS Access Key ID [****************leLy]: NCP Access Key ID
AWS Secret Access Key [None]: NCP Secret Access Key
Default region name [None]: [Enter]
Default output format [None]: [Enter]
```

# 1. 명령어

### sync

```shell
# 로컬에 백업된 데이터를 Object Storage에 백업-동기화하는 명령어입니다.
aws --endpoint-url=https://kr.object.ncloudstorage.com s3 sync D:\jhkSandBox\tmp\logs s3://test-jhk/tmp/logs
```

aws --endpoint-url=https://kr.object.ncloudstorage.com s3 sync C:\Windows\System32\winevt\Logs\obj-test s3://mag-log/init

### 업로드

```shell
aws --endpoint-url=https://kr.object.ncloudstorage.com s3 cp <local_file_path> s3://<bucket_name>[/<object_name>]

aws --endpoint-url=https://kr.object.ncloudstorage.com s3 cp "D:\jhkSandBox\tmp\logs" s3://test-jhk/tmp/logs
```

# S3 버킷 내의 대상 경로에 이미 동일한 이름을 가진 파일이 있으면 덮어쓰기 됩니다.
    # 이로 인해 데이터 손실이 발생할 수 있으므로, 주의가 필요합니다.

# 2. 파워셸 스크립트

> 윈도우 환경에서 작성되었습니다.

스크립트 실행 전에 몇 가지 확인할 점이 있습니다:

# Python 3.8 이하 버전과 AWS CLI 버전 1.15.85가 설치되어 있어야 합니다.
    #`aws configure`를 통해 AWS 자격 증명과 구성 설정이 완료되어야 합니다.
실행하기 전에 `aws s3 cp` 명령어가 실제로 올바르게 작동하는지 확인하려면, 단일 파일에 대해 명령어를 수동으로 실행해 보는 것이 좋습니다. 예를 들어:

```powershell
aws --endpoint-url=https://kr.object.ncloudstorage.com s3 cp "C:\Windows\System32\winevt\Logs\Example.evtx" s3://mag-log/etc/
```

이렇게 수동 실행을 통해 실제 업로드가 성공하는지 확인한 후, 전체 스크립트를 실행하면 됩니다.

⚠️주의: 동일한 이름을 가진 파일이 여러 폴더에 걸쳐 존재하는 경우, S3 버킷 내의 대상 경로에 이미 파일이 있으면 덮어쓰기 됩니다. 이로 인해 데이터 손실이 발생할 수 있으므로, 중복을 피하거나 버전 관리 정책을 사용하는 것이 좋습니다.

## 2-1. obj-aws-cp-with-dir

```sh
# 기본 설명:
# 이 스크립트는 로컬 디렉토리의 파일 중 년도와 월을 추출하여 분류한 후
# 해당 연월에 맞는 S3 버킷 경로로 업로드합니다.

# 로컬 디렉토리 정의
$LOCAL_DIRECTORY = "D:\jhkSandBox\tmp\logs"

# S3 버킷 기본 경로
$S3_BASE_PATH = "s3://test-jhk/tmp/logs"

# 사용자 확인 메시지
$userConfirmation = Read-Host "Are you sure you want to sync $LOCAL_DIRECTORY to $S3_BASE_PATH? (y/yes to confirm)"

if ($userConfirmation -eq 'y' -or $userConfirmation -eq 'yes') {
    Get-ChildItem -Path $LOCAL_DIRECTORY -Recurse | ForEach-Object {
        # 파일 이름에서 년도와 월 추출 (예: Archive-Security-2023-11-05-14)
        if ($_.Name -match '\d{4}-\d{2}') {
            $yearMonth = $Matches[0] # 연도와 월 (예: 2023-11)
        } else {
            $yearMonth = "etc"
        }

        # S3 경로 생성
        $S3_PATH = "$S3_BASE_PATH/$yearMonth/"

        # 객체가 디렉토리인 경우와 파일인 경우를 구분하여 처리
        if ($_.PSIsContainer) {
            # AWS S3로 디렉토리 업로드 (재귀적)
            aws --endpoint-url=https://kr.object.ncloudstorage.com s3 cp $_.FullName $S3_PATH --recursive
        } else {
            # AWS S3로 단일 파일 업로드
            aws --endpoint-url=https://kr.object.ncloudstorage.com s3 cp $_.FullName $S3_PATH
        }
    }
} else {
    Write-Host "User didn't confirm. Exiting script."
}
```

## 2-2. obj-aws-cp

```sh
# 기본 설명:
# 이 스크립트는 로컬 디렉토리의 파일 중 년도와 월을 추출하여 분류한 후
# 해당 연월에 맞는 S3 버킷 경로로 업로드합니다.
# 로컬 디렉토리 정의
$LOCAL_DIRECTORY = "D:\jhkSandBox\tmp\logs"

# S3 버킷 기본 경로
$S3_BASE_PATH = "s3://test-jhk/tmp/logs"

# 사용자확인 메세지
$userConfirmation = Read-Host "Are you sure you want to sync $LOCAL_DIRECTORY to $S3_BASE_PATH? (y/yes to confirm)"

if ($userConfirmation -eq 'y' -or $userConfirmation -eq 'yes') {
    Get-ChildItem -Path $LOCAL_DIRECTORY | ForEach-Object {
        # 파일 이름에서 년도와 월 추출 (예: Archive-Security-2023-11-05-14)
        if ($_.Name -match '\d{4}-\d{2}') {
            $yearMonth = $Matches[0] # 연도와 월 (예: 2023-11)
        } else {
            $yearMonth = "etc"
        }

        # S3 경로 생성
        $S3_PATH = "$S3_BASE_PATH/$yearMonth/"

        # AWS S3로 파일 업로드
        aws --endpoint-url=https://kr.object.ncloudstorage.com s3 cp $_.FullName $S3_PATH
    }
} else {
    Write-Host "User didn't confirm. Exiting script."
}
```


---

# 실제 작업한경우

⚠️ 오브젝트 스토리지 업로드는 처음 한경우라서 작업내용을 남겨놓는 글입니다.
⚠️ 이 메뉴얼은 확정된것이 아닙니다.

1. **aws cli파일 삭제하기** AWS 구성 파일과 자격 증명 파일을 직접 삭제합니다. 이 파일들은 일반적으로 다음 경로에 있습니다:
   - 자격 증명 파일: `~/.aws/credentials`
   - 구성 파일: `~/.aws/config`
     Windows의 경우 경로는 보통 `C:\Users\USERNAME\.aws\`입니다 (`USERNAME`은 실제 사용자 이름으로 대체).
     python과 awscli 는 따로 삭제하지 않음
2. 실행에 사용한 스크립트 삭제

---
