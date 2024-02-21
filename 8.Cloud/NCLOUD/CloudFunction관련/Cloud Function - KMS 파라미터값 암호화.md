- [1. 클라우드 펑션 KMS 사용 테스트](#1-클라우드-펑션-kms-사용-테스트)
	- [1-1. 민간존 테스트](#1-1-민간존-테스트)
	- [1-2. 금융존 테스트](#1-2-금융존-테스트)
	- [1-3. 공공존 테스트](#1-3-공공존-테스트) - 클라우드 펑션없음 - 민간에서 명령하면될것같음
- [2. KMS 확인](#2-KMS-확인)
- [3. 부록](#3-부록)
---
NCP Cloud Function - Default Parameter 암호화 설정을 이용하여 파라미터값 암호화.
# 1. 클라우드 펑션 kms 사용 테스트

**결론**
kms 키를 생성하고, Cloud Function에서 디폴트 파라미터로 입력하는 데이터에 대한 암호화가 쉽게 가능하다.

**Cloud Function 특징**
- 액션당 하나의 vpc 만 선택가능
- 선택한 vpc의 private subnet만 선택 가능 - 인터넷 연결이 필요한 경우 NATGW 설정 필요
- 액션 설정에서 런타임을 고를 수 있는데 코드템플릿으로 다양한 샘플케이스 제공중
- 생성한 액션은 외부 연결주소 생성을 하여 APIGW 통한 외부 호출 가능
- 트리거: 테스트를 위해 미설정이 가능하며 cron, github. Cloud Insight, OBS 등 다양함
- 액션을 삭제하더라도 대시보드에 기록이 남아있음
- ⚠️ 금융존은 왜인지 FKR-2존의 private subnet만 설정 가능

## 1-1. 민간존 테스트 

1. 액션 생성
	1. 런타임 python 3.11 로 설정
	2. 파라미터로 넘어오는 값은 args 로 전달되는 것으로보임
	3. 넘어온 파라미터를 리턴해주는 파이썬 스크립트 작성
	4. 디폴트 파라미터 - 암호화 설정 - ON
		4-1. 디폴트 파라미터에 평문을 입력
		4-2. 입력 후 파라미터 키 선택 - 적용 누르면 해당 키의 밸류가 암호문으로 변경됨
![[Pasted image 20240207134341.png]]
- vpc 연결 정보의 경우 추후  vpc / subnet | zone 모두 수정가능합니다.
![[Pasted image 20240207134357.png]]
2. 실행
- 클라우드 펑션 파라미터 설정에는 암호화 되어있는 값이 설정되어있으나 응답 결과에는 정상적으로 복호화된 모습.
![[Pasted image 20240207134240.png]]
- Cloud Functions -> Action -> [모니터링]탭에서 실행 결과 확인 가능
![[Pasted image 20240207144643.png]]
![[Pasted image 20240207144657.png]]

---
## 1-2. 금융존 테스트

가시적인 테스트 결과 메세지를 위한 디폴트 파라미터 추가
⚠️ 금융존은 FKR-2존의 private subnet만 설정 가능
### Cloud Functions / Action 설정 내용

![[Pasted image 20240207154756.png]]

## 1-3. 공공존 테스트

공공존은 수도권/남부권 리전 모두 Cloud Function 서비스가 없습니다.

---
# 2. KMS 확인
kms 키 사용이력이 남는 모습.
키 이력을 내보낼수있는지 문의 필요 (없는 것으로 보임)
![[Pasted image 20240207145342.png]]
키 회전으로 kms 키 태그 변경되어도 정상 동작.
![[Pasted image 20240207160611.png]]
### 금융존에서 키 삭제테스트
![[Pasted image 20240207160805.png]]
- 연결된 리소스가 있으면 삭제 불가능
- NCF 액션 수정으로 암호화 설정 해제 (연결 제거) -> 삭제 요청 -> 삭제 성공
![[Pasted image 20240207160931.png]]
- 실수로 삭제하는 경우를 방지하기 위한 장치가 되어있는 것 확인
- kms 와 cloud function 작업 관련내용은 cloud activity tracer 에 모두 기록됨
- ![[Pasted image 20240207161154.png]]

---

# 3.부록

### 테스트 코드 (Cloud Function, runtime:python:3.11)

```
import os
def main(args):
    name = args.get("name", "World")
    place  = args.get("place", "Naver")
    #access_key = os.environ['NCLOUD_ACCESS_KEY']
    #secret_key = os.environ['NCLOUD_SECRET_KEY']
    access_key = args['NCLOUD_ACCESS_KEY']
    secret_key = args['NCLOUD_SECRET_KEY']
    
    return {
        "payload": f"Hello, {name} in {place}!",
        "test_msg": f"access_key: {access_key}\n secret_key: {secret_key}",
        "설명":"NCLOUD_SECRET_KEY에만 KMS 적용"
    }
```
### 디폴트 파라미터
```json
{"NCLOUD_ACCESS_KEY":"6DCCDF76FE0B0EC047B3","NCLOUD_SECRET_KEY":"1FB2B64F6F067C942E5237CEE56D022C5F3EDF36","OBJ_DIR":"OBJECTSTRORAGE_DIR","OBJ_NAME":"OBJECTSTRORAGE_NAME"}

// `NCLOUD_SECRET_KEY` kms 암호화 적용
{"NCLOUD_ACCESS_KEY":"6DCCDF76FE0B0EC047B3","NCLOUD_SECRET_KEY":"ncpkms:v1:iv+KFdopuG1szKWToM9JBDiarJihIQ1g2KvHtN+d4wvxE0YZygVkmkkz4XwCBcy9voqh2N199UprrWTpGEv/oNEyTzY=","OBJ_DIR":"OBJECTSTRORAGE_DIR","OBJ_NAME":"OBJECTSTRORAGE_NAME"}
```


---
금융존 설정내용

소스코드
```python
def main(args):
    name = args.get("name", "World")
    place  = args.get("place", "Naver")
    access_key = args['NCLOUD_ACCESS_KEY']
    secret_key = args['NCLOUD_SECRET_KEY']
    origin_param={"name":"testWORLD","OBJ_NAME":"OBJECTSTRORAGE_NAME","OBJ_DIR":"OBJECTSTRORAGE_DIR","NCLOUD_ACCESS_KEY":"22D3764A3DF3365C82C9","NCLOUD_SECRET_KEY":"ncpkms:v1:QG2UILfZu4MfXQEU/LPosSo3QjxaX41fAzSdzXLWnyP35wiOzxttrtDApjXmOkzTpYrZPYPR5gSQC3mM4QiJ6lB98Ho="}
    
    return {
        "original_parameter":f"{origin_param}",
        "payload": f"Hello, {name} in {place}!",
        "test_msg": f"access_key: {access_key}\n secret_key: {secret_key}",
        "설명":"NCLOUD_SECRET_KEY에만 KMS 적용"
    }
```
디폴트 파라미터
```json
{"name":"testWORLD","OBJ_NAME":"OBJECTSTRORAGE_NAME","OBJ_DIR":"OBJECTSTRORAGE_DIR","NCLOUD_ACCESS_KEY":"22D3764A3DF3365C82C9","NCLOUD_SECRET_KEY":"ncpkms:v1:QG2UILfZu4MfXQEU/LPosSo3QjxaX41fAzSdzXLWnyP35wiOzxttrtDApjXmOkzTpYrZPYPR5gSQC3mM4QiJ6lB98Ho="}
```
