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