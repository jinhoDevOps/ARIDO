
개요 정도만 적어놓고 상세 항목은 링크 걸어서 관리?


---
# NCLOUD API 개요

- 기본 API: 
	네이버 클라우드 플랫폼의 일부 서비스를 활용할 수 있는 NAVER Cloud Platform API(Ncloud API)로 호환 API와 연동 API를 제외한 모든 API를 의미
- 호환 API
	타사 API와 호환되는 네이버 클라우드 플랫폼의 API. AWS S3와 호환되는 Object Storage API, OpenStack Swift와 호환되는 Archive Storage API가 있음
- 연동 API
	네이버의 인공 지능 서비스, 애플리케이션 서비스를 간소화된 형태의 호출로 손쉽게 활용할 수 있는 API. CLOVA, Papago, Maps API 등이 있음

기본API 
네이버 클라우드 api 라고 쓰던게 이거임

오브젝트 스토리지는
호환 API
S3 API 

# AI services (CLOVA)
## CLOVA OCR
https://api.ncloud-docs.com/docs/ai-application-service-ocr
OCR(Optical character recognition, 광학 문자 인식)은 이미지(사진) 속 글자 위치를 찾고 어떤 글자인지 자동으로 알아내는 기술입니다. CLOVA OCR은 다양한 형태의 글자를 이해하기 위해 독자적인 글자 영역 검출 및 인식 기술을 보유하고 있습니다.  
또한 손쉽게 템플릿을 만들고 원하는 영역을 지정한 뒤, 필요한 글자만 빠르게 추출하는 기능을 제공합니다.  
CLOVA OCR API는 CLOVA OCR 빌더에서 설정한 Template을 기반으로 문자 인식을 제공하는 API로, 인식에 사용할 언어와 이미지 데이터를 입력받고, 그에 맞는 인식 결과를 텍스트로 반환합니다.

## 공통 설정

### API URL
| Method | Request URI |
| ---- | ---- |
| POST | CLOVA OCR 빌더에서 생성된 API Gateway의 InvokeURL로 호출함<br>각 도메인마다 고유의 호출 URL이 생성됨 |

### TEXT OCR 인식과 Template OCR 인식 요청
|구분|설명|Path|Request|Response|
|---|---|---|---|---|
|TEXT OCR|템플릿 정의없이 이미지의 모든 텍스트 인식|**/general**|이미지 인식 요청 형식을 따름  <br>설정 가능한 언어값은 'ko'/'ja'/'zh-TW' 이며, lang 필드가 설정되지 않은 경우, 'ko'가 default로 설정됨|이미지 인식 결과 형식을 따름  <br>matchedTemplate, title and validationResult 값은 전달되지 않음|
|Template OCR|도메인에 배포된 템플릿이 포함 된 이미지를 인식|**/infer**|이미지 인식 요청 형식을 따름  <br>lang 필드가 설정되지 않은 경우, 도메인의 언어 설정값이 default로 설정됨|이미지 인식 결과 형식을 따름|

## CLOVA OCR Document API

![[Pasted image 20240119105112.png]]