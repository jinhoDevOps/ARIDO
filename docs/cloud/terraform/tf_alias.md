# terraform 명령을 tf로 단축하기
### 1. 별칭 설정
VSCode에서 PowerShell 터미널을 사용하고 있다면, `terraform` 명령어를 `tf`로 단축하여 사용할 수 있습니다. 이를 위해서는 별칭(alias)을 설정해야 합니다.

```powershell
#터미널을 닫으면 설정이 사라집니다. #윈도우 파워셸 #이것만해도편함
Set-Alias -Name tf -Value terraform
```

- 현재 사용자에 대한 모든 호스트: `~\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1`
- 모든 사용자에 대한 모든 호스트: `$PSHOME\profile.ps1`

```markdown
아래 내용은 보안이슈로 뭐가 안됨
1. **본인의 사용자 프로필 스크립트 확인**

    ```powershell
    Test-Path $PROFILE
    ```

    만약 이 명령이 `False`를 반환한다면, 프로필 파일이 없는 것이므로 생성해야 합니다.

2. **프로필 스크립트 생성 (없을 경우)**

    ```powershell
    New-Item -path $PROFILE -type file -force
    ```

3. **프로필 스크립트에 별칭 추가**

    `Set-Alias -Name tf -Value terraform`을 복사한 뒤 프로필 스크립트에 붙여넣기 합니다.

    이 작업은 텍스트 에디터를 열어서 수동으로 할 수 있으며, Visual Studio Code는 좋은 선택입니다:

    ```powershell
    code $PROFILE
    ```

4. **변경사항 적용**

    프로필 스크립트를 저장하고 닫은 후, 새로운 PowerShell 세션을 열거나 현재 세션에서 다음 명령을 실행합니다:

    ```powershell
    . $PROFILE
    ```
```

이제 `tf` 별칭을 사용하여 `terraform` 명령을 실행할 수 있어야 합니다.

### 0. 별칭 설정
 `terraform` 명령어를 `tf`로 단축하여 사용할 수 있습니다. 이를 위해서는 별칭(alias)을 설정해야 합니다.

```powershell
###윈도우 파워셸
#터미널을 닫으면 설정이 사라집니다.
Set-Alias -Name tf -Value terraform
```
---
```bash
### CentOS
echo "alias tf='terraform'" >> ~/.bashrc
source ~/.bashrc
```

```bash
### Ubuntu
echo "alias tf='terraform'" >> ~/.bashrc
source ~/.bashrc
```

이제 `tf`를 입력하여 `terraform` 명령을 실행할 수 있습니다.

**참고**: 이러한 변경은 해당 사용자에게만 적용됩니다. 시스템 전체에 별칭을 설정하려면 `/etc/bash.bashrc` 또는 `/etc/profile` 파일을 수정해야 하며, 이 작업에는 관리자 권한이 필요합니다.
