---
tags: java, spring, mysql
---
# 스프링부트 mysql 연결에러
```
4.4.1.3 Changes in the Connector/J API
The name of the class that implements java.sql.Driver in MySQL Connector/J has changed 
from com.mysql.jdbc.Driver to com.mysql.cj.jdbc.Driver. The old class name has been deprecated.
```


## application.properties 사용
application.properties 을 사용할 경우 아래와 같이 변경한다.

```
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```
 

## application.yml 사용
application.yml 을 사용할 경우 아래와 같이 변경한다.
```
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver

spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
```

> 추가자료 https://victorydntmd.tistory.com/321 \
_스프링부트에서 밀고있는 h2 데이터베이스를 사용할 경우에는 AutoConfigure에서 설정을 잡아주기 때문에 별도의 설정이 필요없습니다._ \
_하지만 MySQL을 사용한다면 src/main/resources/application.properties 파일에서 커넥션 정보를 작성하여 설정을 잡아줘야 합니다._