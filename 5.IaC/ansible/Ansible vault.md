출처
[악분블로그](https://malwareanalysis.tistory.com/692)
[유튭](https://youtu.be/N2wzqgkUYD0)


Ansible Vault는 Ansible에서 사용되는 민감한 데이터를 암호화하는 도구입니다. 이를 통해 비밀번호, 개인 키, 또는 기타 중요 정보를 안전하게 관리할 수 있으며, 이 데이터를 포함하는 Ansible Playbooks을 공개된 소스 코드 저장소에 저장할 때 유용하게 사용됩니다.

**Ansible Vault 사용법 개요**:

- **암호화**: `ansible-vault encrypt` 명령을 사용하여 기존 파일을 암호화합니다. 암호화 시 설정한 비밀번호는 나중에 파일을 복호화할 때 필요합니다.
- **파일 생성 및 암호화**: `ansible-vault create` 명령을 사용하여 새로운 암호화된 파일을 만들고, 이 파일에 민감한 정보를 저장할 수 있습니다.
- **복호화**: `ansible-vault decrypt` 명령으로 암호화된 파일을 복호화할 수 있지만, 실수로 복호화된 내용이 저장소에 업로드되는 것을 방지하기 위해 권장되지 않습니다.
- **보기**: `ansible-vault view` 명령을 사용하여 암호화된 파일의 내용을 볼 수 있습니다.
- **수정**: `ansible-vault edit` 명령을 사용하여 암호화된 파일의 내용을 안전하게 수정할 수 있습니다.