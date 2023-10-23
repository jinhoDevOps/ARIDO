---
tags: java, spring
---
### 프로젝트와 테스트 환경 요약

#### 프로젝트 환경

- **개발 환경**: 윈도우, IntelliJ, Spring Boot 2.7.x, Maven or Gradle, Java 11
- **데이터베이스**: MySQL 8.0.34
- **기타**: Thymeleaf 템플릿 엔진 사용

#### 테스트 환경

- **운영체제**: CentOS 7 리눅스 서버
- **컨테이너**: Docker, Docker Compose
  - 웹 서버 컨테이너 (톰캣 포트 8081)
  - 데이터베이스 컨테이너 (MySQL 포트 13306)

#### 중요 설정 파일

- **application.properties**: JDBC URL과 다른 데이터베이스 설정 포함
- **pom.xml**: 의존성 정보, MySQL JDBC 드라이버 포함
- **HTML 템플릿**: Thymeleaf 문법으로 작성

---

1. **윈도우와 리눅스 간의 동작 차이**

2. **데이터베이스 연결 문제**

   #### 문제 상황

   - **환경**: CentOS 7 리눅스 서버에 Docker로 구성한 테스트 환경. MySQL은 8.0.34 버전을 사용하며, 포트는 13306으로 설정.
   - **증상**: 윈도우 환경에서는 정상적으로 데이터베이스 정보를 불러오지만, 동일한 설정으로 리눅스 서버에 올린 도커 컨테이너에서는 데이터베이스 정보를 불러올 수 없음. log에 찍히는 오류는 없는상황
   - **오류 메시지**: `ERROR 2059 (HY000): Authentication plugin 'sha256_password' cannot be loaded: /usr/lib64/mysql/plugin/sha256_password.so: cannot open shared object file: No such file or directory`

   #### 해결 방안

   1. **MySQL 사용자 암호 알고리즘 변경**: 현재 사용 중인 `sha256_password`를 `mysql_native_password`로 변경하여 MySQL의 인증 문제를 해결할 수 있습니다.

      ```sql
        ALTER USER 'your_username'@'your_host' IDENTIFIED WITH 'mysql_native_password' BY 'your_password';
        alter user 'username'@'%' identified with mysql_native_password by 'your_password';

        FLUSH PRIVILEGES;
      ```

      MySQL 서비스를 재시작해야 할 수 있습니다.

   2. **JDBC URL 수정**: `application.properties` 파일에서 JDBC URL을 업데이트하여 인증 문제를 해결할 수 있습니다.

      ```properties
      #기존
      spring.datasource.url=jdbc:mysql://mysql.db.com:13306/example?useSSL=false&characterEncoding=UTF-8&serverTimezone=Asia/Seoul
      #수정
      spring.datasource.url=jdbc:mysql://mysql.db.com:13306/example?useSSL=false&characterEncoding=UTF-8&serverTimezone=Asia/Seoul&allowPublicKeyRetrieval=true&useLegacyDatetimeCode=false
      ```

   3. **Docker 네트워크 확인**: 도커 컨테이너 간의 네트워크 연결이 올바른지 확인하고, 필요하다면 `docker-compose.yml` 파일을 수정합니다.

   4. **서버와 데이터베이스 시간대 맞추기**: 서버와 데이터베이스의 시간대가 다르면 문제가 발생할 수 있습니다. `application.properties`와 MySQL 설정을 확인하세요.

3. **기타 설정 문제**
   - 문제 상황
   - 해결 방안
