리눅스 CLI에서 JSON 형태의 데이터를 보기 편하게 출력하는 데에는 여러 도구를 사용할 수 있습니다. 가장 일반적으로 사용되는 도구는 jq입니다. jq는 JSON 데이터를 다루기 위한 경량이면서도 유연한 명령줄 JSON 처리기입니다.

#### jq 사용 예시

기본 사용법:


```Shell
cat asdf.log | jq . #아래와 같이 복잡한 파이프라인 에서도 마지막에 붙여서 사용 가능합니다. 
cat agent.log | grep "host.id" | awk -F DEBUG: '{print $3}' | tail -n 1 | jq
```

​

이 명령은 JSON 데이터를 예쁘게 포맷해서 출력합니다.

cat asdf.log | jq '.fieldname'

​

특정 필드만 선택하여 출력할 수 있습니다.

색상 및 들여쓰기 지정:

Shell

복사

cat asdf.log | jq -C '.'

​

C 옵션을 사용하면 출력에 색상을 추가할 수 있습니다.

#### 설치 방법

jq는 대부분의 리눅스 배포판에서 사용할 수 있으며, 대개 패키지 매니저를 통해 쉽게 설치할 수 있습니다.

Ubuntu/Debian 계열:

Shell

복사

sudo apt-get install jq

​

Red Hat/Fedora 계열:

Shell

복사

sudo yum install jq