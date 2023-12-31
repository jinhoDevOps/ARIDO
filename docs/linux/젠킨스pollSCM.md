---
tags: linux, jenkins, cron
---
크론탭과 관련된 내용을 "젠킨스 Poll SCM 스케쥴 설정" 주제로 마크다운 형식에 맞게 추가 및 정리하겠습니다.

---

# 젠킨스 Poll SCM 스케쥴 설정

## 목차
- [크론탭 기본 구조](#크론탭-기본-구조)
- [스케쥴 예시](#스케쥴-예시)

---

## 크론탭 기본 구조

크론탭(Crontab)은 유닉스 계열의 운영체제에서 사용되는 작업 스케쥴러입니다. 원하는 시간에 정해진 작업을 자동으로 실행할 수 있도록 해줍니다.

```sh
* * * * * : 각 필드는 순서대로 분, 시, 일, 월, 요일을 의미합니다.
```
### 크론탭 구성 요소

크론탭에서 스케줄을 설정할 때 사용되는 각 필드의 의미는 다음과 같습니다:

1. **분 (Minute)**: 0 - 59
2. **시 (Hour)**: 0 - 23
3. **일 (Day of month)**: 1 - 31
4. **월 (Month)**: 1 - 12
5. **요일 (Day of week)**: 0 - 7 (0과 7은 일요일)

### 특수 문자

- **\***: 모든 값
- **,**: 값 리스트 (예: `1,5,10`)
- **-**: 범위 (예: `1-6`)
- **/**: 주기 (예: `*/3`)

### 예시

- `5 * * * *`: 매시 5분에 작업을 실행
- `*/10 1 * * *`: 오전 1시부터 1시 59분까지 10분마다 작업을 실행
- `0 22 * * 1-5`: 주중(월~금) 오후 10시에 작업을 실행


---

## 스케쥴 예시

### 매 초마다 실행
```sh
* * * * * : 매 초마다 SCM을 확인하여 빌드를 실행합니다.
```

### 1분마다 확인
```sh
H/1 * * * * : 1분마다 SCM을 확인하여 빌드를 실행합니다.
```

### 10분 간격으로 빌드 작업 수행
```sh
H/10 * * * * : 10분 간격으로 SCM을 확인하여 빌드를 실행합니다.
```

### 모든 시간의 첫 30분 동안에 10분 간격으로 빌드
```sh
H(0-29)/10 * * * * : 모든 시간의 첫 30분 동안에 10분 간격으로 SCM을 확인하여 빌드를 실행합니다.
```

### 12월 달은 제외하고 매달 1일과 15일에 한 번씩 빌드
```sh
H H 1,15 1-11 * : 12월 달은 제외하고 매달 1일과 15일에 한 번씩 SCM을 확인하여 빌드를 실행합니다.
```

---
