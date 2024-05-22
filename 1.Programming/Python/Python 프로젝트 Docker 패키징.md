Python 프로젝트를 Docker 이미지로 패키징하고, 실행 시 환경변수를 외부에서 설정하는 방법을 포함한 상세 가이드라인을 제공합니다. 이 과정은 프로젝트의 루트 디렉토리에 `Dockerfile`이 위치할 때의 설정을 기준으로 합니다.

### 1. Dockerfile 작성

```Dockerfile
# Python 3.11 기반의 이미지 사용
FROM python:3.11-slim

# 작업 디렉토리 설정
WORKDIR /app

# 현재 디렉토리의 모든 파일을 컨테이너의 작업 디렉토리로 복사
COPY . /app

# requirements.txt를 사용하여 필요한 패키지 설치
RUN pip install --no-cache-dir -r requirements.txt

# 환경변수 설정 (테스트/개발용 기본값, 운영 환경에서는 외부에서 제공)
ENV OBS_ACCESS_KEY=your_obs_access_value
ENV OBS_SECRET_KEY=your_obs_secret_value

# 프로그램 실행 명령
CMD ["python", "main.py"]
```

### 2. Docker 이미지 빌드
프로젝트의 루트 디렉토리에서 아래의 명령어를 사용하여 Docker 이미지를 빌드합니다.
```bash
docker build -t your-project-name .
```
여기서 `your-project-name`는 원하는 프로젝트의 이름으로 대체합니다.

### 3. Docker 컨테이너 실행
빌드된 이미지를 기반으로 Docker 컨테이너를 실행할 때, 환경변수를 명령어로 전달하여 보안을 강화하고 유연성을 제공합니다. 아래의 명령어를 사용하여 컨테이너를 실행합니다:

```bash
docker run -d \
-e OBS_ACCESS_KEY=actual_obs_access_key \
-e OBS_SECRET_KEY=actual_obs_secret_key \
your-project-name
```

이렇게 환경변수를 `-e` 옵션으로 전달하면, Dockerfile 내에서 설정한 기본값을 오버라이드할 수 있습니다. 이는 특히 운영 환경에서 중요한 설정 정보를 외부에서 안전하게 제공하고 관리할 수 있게 해줍니다.

### 4. 관리 및 운영
- **로그 관리**: 컨테이너의 로그를 확인하려면 `docker logs [컨테이너 ID 또는 이름]` 명령을 사용합니다.
- **업데이트 및 배포**: 이미지를 업데이트하려면 Dockerfile을 수정한 후 이미지를 다시 빌드하고 컨테이너를 재시작해야 합니다.
