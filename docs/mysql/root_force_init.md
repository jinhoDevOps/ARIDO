
MySQL의 `root` 사용자 암호가 잘못 설정된 것 같습니다.   
이 문제를 해결하기 위해 MySQL 서버에 `root` 사용자로 로그인하지 않고 암호를 재설정해야 할 수 있습니다.

## MySQL 루트 패스워드 재설정 (필요한 경우)

MySQL 서버가 실행되지 않는 경우, 루트 패스워드 재설정을 위해 MySQL 설정 파일을 수정해야 할 수도 있습니다.

1. MySQL 설정 파일을 열어주세요 (일반적인 경로는 `/etc/my.cnf` 또는 `/etc/mysql/my.cnf`입니다).
2. `[mysqld]` 섹션에 다음 줄을 추가하세요:

   ```ini
   skip-grant-tables
   ```

3. MySQL 서비스를 재시작하세요:

   ```bash
   systemctl restart mysqld
   ```

4. 이제 암호 없이 루트 사용자로 로그인할 수 있습니다:

   ```bash
   mysql -u root
   ```

5. MySQL 프롬프트에서 루트 패스워드를 재설정하세요:

   ```sql
   FLUSH PRIVILEGES;
   ALTER USER 'root'@'localhost' IDENTIFIED BY '[NEW_PASSWORD]';
   ```

6. 설정 파일에서 `skip-grant-tables` 줄을 제거하고 MySQL 서비스를 다시 시작하세요:

   ```bash
   systemctl restart mysqld
   ```

